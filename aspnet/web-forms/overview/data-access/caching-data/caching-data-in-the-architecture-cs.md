---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: Mise en cache de données dansC#l’architecture () | Microsoft Docs
author: rick-anderson
description: Dans le didacticiel précédent, nous avons appris à appliquer la mise en cache au niveau de la couche de présentation. Dans ce didacticiel, nous allons apprendre à tirer parti de notre architecte superposé...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 192cadb8e2f862ac2a97a36b375e247b281ece93
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78551169"
---
# <a name="caching-data-in-the-architecture-c"></a>Mise en cache de données dans l’architecture (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) ou [Télécharger le PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> Dans le didacticiel précédent, nous avons appris à appliquer la mise en cache au niveau de la couche de présentation. Dans ce didacticiel, nous allons apprendre à tirer parti de notre architecture en couches pour mettre en cache des données au niveau de la couche de logique métier. Pour ce faire, nous étendons l’architecture de façon à inclure une couche de mise en cache.

## <a name="introduction"></a>Introduction

Comme nous l’avons vu dans le didacticiel précédent, la mise en cache des données ObjectDataSource s est aussi simple que la définition de deux propriétés. Malheureusement, ObjectDataSource applique la mise en cache au niveau de la couche de présentation, ce qui couple étroitement les stratégies de mise en cache à la page ASP.NET. L’une des raisons justifiant la création d’une architecture en couches est de permettre la rupture de ces couplages. La couche de logique métier, par exemple, dissocie la logique métier des pages ASP.NET, tandis que la couche d’accès aux données dissocie les détails d’accès aux données. Ce découplage de la logique métier et des détails d’accès aux données est préféré, en partie, car il rend le système plus lisible, plus facile à gérer et plus flexible à modifier. Il permet également la connaissance du domaine et la Division du travail qu’un développeur travaillant sur la couche de présentation n’a pas besoin de connaître les détails de la base de données pour effectuer son travail. Le découplage de la stratégie de mise en cache de la couche présentation présente des avantages similaires.

Dans ce didacticiel, nous allons augmenter notre architecture pour inclure une *couche de mise en cache* (ou CL pour Short) qui utilise notre stratégie de mise en cache. La couche de mise en cache inclut une classe `ProductsCL` qui fournit l’accès aux informations sur les produits avec des méthodes comme `GetProducts()`, `GetProductsByCategoryID(categoryID)`, etc., qui, lorsqu’elles sont appelées, tentent d’abord de récupérer les données à partir du cache. Si le cache est vide, ces méthodes appellent la méthode `ProductsBLL` appropriée dans la couche BLL, qui, à son tour, obtient les données de la couche DAL. La méthode `ProductsCL` met en cache les données récupérées à partir de la couche BLL avant de la retourner.

Comme le montre la figure 1, le CL réside entre les couches de la présentation et de la logique métier.

![La couche de mise en cache (CL) est une autre couche de notre architecture](caching-data-in-the-architecture-cs/_static/image1.png)

**Figure 1**: la couche de mise en cache (CL) est une autre couche dans notre architecture

## <a name="step-1-creating-the-caching-layer-classes"></a>Étape 1 : création des classes de couche de mise en cache

Dans ce didacticiel, nous allons créer un CL très simple avec une seule classe `ProductsCL` qui n’a que quelques méthodes. La création d’une couche de mise en cache complète pour l’application entière nécessiterait la création de classes `CategoriesCL`, `EmployeesCL`et `SuppliersCL`, et la fourniture d’une méthode dans ces classes de couche de mise en cache pour chaque méthode d’accès aux données ou de modification dans la couche BLL. Comme avec les couches BLL et DAL, la couche de mise en cache doit idéalement être implémentée en tant que projet de bibliothèque de classes distinct ; Toutefois, nous l’implémenterons en tant que classe dans le dossier `App_Code`.

Pour séparer plus correctement les classes CL des classes DAL et BLL, créez un sous-dossier dans le dossier `App_Code`. Cliquez avec le bouton droit sur le dossier `App_Code` dans le Explorateur de solutions, choisissez nouveau dossier, puis nommez le nouveau dossier `CL`. Après avoir créé ce dossier, ajoutez-y une nouvelle classe nommée `ProductsCL.cs`.

![Ajoutez un nouveau dossier nommé CL et une classe nommée ProductsCL.cs](caching-data-in-the-architecture-cs/_static/image2.png)

**Figure 2**: ajouter un nouveau dossier nommé `CL` et une classe nommée `ProductsCL.cs`

La classe `ProductsCL` doit inclure le même jeu de méthodes d’accès aux données et de modification que dans sa classe de couche de logique métier correspondante (`ProductsBLL`). Au lieu de créer toutes ces méthodes, n’hésitez pas à créer un couple ici pour avoir une idée des modèles utilisés par le CL. En particulier, nous allons ajouter les méthodes `GetProducts()` et `GetProductsByCategoryID(categoryID)` à l’étape 3 et une surcharge `UpdateProduct` à l’étape 4. Vous pouvez ajouter les autres méthodes de `ProductsCL` et les classes `CategoriesCL`, `EmployeesCL`et `SuppliersCL` à votre convenance.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Étape 2 : lecture et écriture dans le cache de données

La fonctionnalité de mise en cache ObjectDataSource explorée dans le didacticiel précédent utilise en interne le cache de données ASP.NET pour stocker les données extraites de la couche BLL. Le cache de données est également accessible par programme à partir des classes code-behind ASP.NET pages ou des classes dans l’architecture de l’application Web. Pour lire et écrire dans le cache de données à partir d’une classe code-behind de page ASP.NET, utilisez le modèle suivant :

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

La méthode de la [classe`Cache`](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [`Insert`](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) a un nombre de surcharges. `Cache["key"] = value` et `Cache.Insert(key, value)` sont synonymes et les deux ajoutent un élément au cache à l’aide de la clé spécifiée sans expiration définie. En règle générale, nous souhaitons spécifier un délai d’expiration lors de l’ajout d’un élément au cache, en tant que dépendance, expiration de temps ou les deux. Utilisez l’une des autres surcharges de la méthode `Insert` pour fournir des informations d’expiration basées sur la dépendance ou le temps.

Les méthodes de la couche s de mise en cache doivent d’abord vérifier si les données demandées se trouvent dans le cache et, le cas échéant, les retourner à partir de là. Si les données demandées ne se trouvent pas dans le cache, la méthode BLL appropriée doit être appelée. Sa valeur de retour doit être mise en cache puis retournée, comme le montre le diagramme de séquence suivant.

![Les méthodes de la couche de mise en cache s retournent des données du cache si elles sont disponibles](caching-data-in-the-architecture-cs/_static/image3.png)

**Figure 3**: les méthodes de la couche de mise en cache retournent des données du cache si elles sont disponibles

La séquence illustrée à la figure 3 s’effectue dans les classes CL à l’aide du modèle suivant :

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

Ici, *type* est le type de données stockées dans le cache `Northwind.ProductsDataTable`, par exemple, la *clé* est la clé qui identifie de façon unique l’élément de cache. Si l’élément avec la *clé* spécifiée n’est pas dans le cache, l' *instance* est `null` et les données sont récupérées à partir de la méthode BLL appropriée et ajoutées au cache. Au moment où `return instance` est atteint, l' *instance* contient une référence aux données, soit à partir du cache, soit extraites de la couche BLL.

Veillez à utiliser le modèle ci-dessus lors de l’accès aux données à partir du cache. Le modèle suivant, qui, à première vue, semble équivalent, contient une différence subtile qui introduit une condition de concurrence. Les conditions de concurrence sont difficiles à déboguer, car elles se révèlent sporadiques et difficiles à reproduire.

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

La différence dans cette seconde, extrait de code incorrect, est qu’au lieu de stocker une référence à l’élément mis en cache dans une variable locale, le cache de données est accessible directement dans l’instruction conditionnelle *et* dans le `return`. Imaginez que lorsque ce code est atteint, `Cache["key"]` n’est pas`null`, mais avant que l’instruction `return` soit atteinte, le système supprime la *clé* du cache. Dans ce cas rare, le code retourne une valeur `null` plutôt qu’un objet du type attendu.

> [!NOTE]
> Le cache de données étant thread-safe, vous ne devez pas synchroniser l’accès aux threads pour des opérations de lecture ou d’écriture simples. Toutefois, si vous devez effectuer plusieurs opérations sur les données du cache qui doivent être atomiques, vous êtes responsable de l’implémentation d’un verrou ou d’un autre mécanisme pour garantir la sécurité des threads. Pour plus d’informations, consultez [synchronisation de l’accès au Cache ASP.net](http://www.ddj.com/184406369) .

Un élément peut être supprimé par programme du cache de données à l’aide de la [méthode`Remove`](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) comme suit :

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Étape 3 : retour des informations sur le produit à partir de la classe`ProductsCL`

Pour ce didacticiel, vous allez implémenter deux méthodes pour retourner les informations sur les produits à partir de la classe `ProductsCL` : `GetProducts()` et `GetProductsByCategoryID(categoryID)`. À l’instar de la classe `ProductsBL` dans la couche de logique métier, la méthode `GetProducts()` dans CL retourne des informations sur tous les produits sous la forme d’un objet `Northwind.ProductsDataTable`, tandis que `GetProductsByCategoryID(categoryID)` retourne tous les produits d’une catégorie spécifiée.

Le code suivant montre une partie des méthodes de la classe `ProductsCL` :

[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

Tout d’abord, notez les attributs `DataObject` et `DataObjectMethodAttribute` appliqués à la classe et aux méthodes. Ces attributs fournissent des informations à l’Assistant ObjectDataSource s, indiquant quelles classes et méthodes doivent apparaître dans les étapes de l’Assistant. Étant donné que les classes et les méthodes CL sont accessibles à partir d’un ObjectDataSource dans la couche de présentation, j’ai ajouté ces attributs pour améliorer l’expérience au moment de la conception. Reportez-vous au didacticiel [création d’une couche de logique métier](../introduction/creating-a-business-logic-layer-cs.md) pour obtenir une description plus complète de ces attributs et de leurs effets.

Dans les méthodes `GetProducts()` et `GetProductsByCategoryID(categoryID)`, les données retournées à partir de la méthode `GetCacheItem(key)` sont assignées à une variable locale. La méthode `GetCacheItem(key)`, que nous examinerons bientôt, retourne un élément particulier du cache en fonction de la *clé*spécifiée. Si ces données sont introuvables dans le cache, elles sont récupérées à partir de la méthode de la classe `ProductsBLL` correspondante, puis ajoutées au cache à l’aide de la méthode `AddCacheItem(key, value)`.

Les méthodes `GetCacheItem(key)` et `AddCacheItem(key, value)` interface avec le cache de données, en lisant et en écrivant des valeurs, respectivement. La méthode `GetCacheItem(key)` est la plus simple des deux. Elle retourne simplement la valeur de la classe cache à l’aide de la *clé*passée :

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` n’utilise pas la valeur de *clé* fournie, mais appelle à la place la méthode `GetCacheKey(key)`, qui retourne la *clé* ajoutée avec ProductsCache-. Le `MasterCacheKeyArray`, qui contient la chaîne ProductsCache, est également utilisé par la méthode `AddCacheItem(key, value)`, comme nous le verrons momentanément.

À partir d’une classe code-behind de la page ASP.NET, le cache de données est accessible à l’aide de la propriété `Page` Class s [`Cache`](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)et permet une syntaxe comme `Cache["key"] = value`, comme indiqué à l’étape 2. À partir d’une classe dans l’architecture, le cache de données est accessible à l’aide de `HttpRuntime.Cache` ou `HttpContext.Current.Cache`. L’entrée de blog de [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx) [httpRuntime. cache et HttpContext. Current. cache présente](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) les légères avantages en matière de performances lors de l’utilisation de `HttpRuntime` au lieu de `HttpContext.Current`; par conséquent, `ProductsCL` utilise `HttpRuntime`.

> [!NOTE]
> Si votre architecture est implémentée à l’aide de projets de bibliothèque de classes, vous devrez ajouter une référence à l’assembly `System.Web` pour pouvoir utiliser les classes [httpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) et [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) .

Si l’élément est introuvable dans le cache, les méthodes de la classe `ProductsCL` obtiennent les données de la couche BLL et les ajoutent au cache à l’aide de la méthode `AddCacheItem(key, value)`. Pour ajouter de la *valeur* au cache, nous pourrions utiliser le code suivant, qui utilise un délai d’expiration de 60 seconde :

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` spécifie la durée d’expiration à partir de 60 secondes dans le futur, alors que [`System.Web.Caching.Cache.NoSlidingExpiration`](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) indique qu’il n’y a pas d’expiration décalée. Bien que cette `Insert` surcharge de méthode ait des paramètres d’entrée pour une expiration absolue et décalée, vous ne pouvez fournir qu’une seule des deux. Si vous tentez de spécifier à la fois une heure absolue et un intervalle de temps, la méthode `Insert` lèvera une exception `ArgumentException`.

> [!NOTE]
> Cette implémentation de la méthode `AddCacheItem(key, value)` présente actuellement des lacunes. Nous allons résoudre ces problèmes à l’étape 4.

## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Étape 4 : invalidation du cache lorsque les données sont modifiées via l’architecture

Outre les méthodes de récupération des données, la couche de mise en cache doit fournir les mêmes méthodes que la couche BLL pour l’insertion, la mise à jour et la suppression des données. Les méthodes de modification des données CL ne modifient pas les données mises en cache, mais appellent plutôt la méthode de modification de données BLL s correspondante, puis invalident le cache. Comme nous l’avons vu dans le didacticiel précédent, il s’agit du même comportement que l’ObjectDataSource s’applique lorsque ses fonctionnalités de mise en cache sont activées et que ses méthodes `Insert`, `Update`ou `Delete` sont appelées.

La surcharge de `UpdateProduct` suivante montre comment implémenter les méthodes de modification des données dans le CL :

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

La méthode de couche de logique métier de modification de données appropriée est appelée, mais avant que sa réponse soit renvoyée, nous devons invalider le cache. Malheureusement, l’invalidation du cache n’est pas simple, car les méthodes de `ProductsCL` classe s `GetProducts()` et de `GetProductsByCategoryID(categoryID)` ajoutent des éléments au cache avec des clés différentes, et la méthode `GetProductsByCategoryID(categoryID)` ajoute un élément de cache différent pour chaque *CategoryID*unique.

Lors de l’invalidation du cache, nous devons supprimer *tous* les éléments qui ont pu être ajoutés par la classe `ProductsCL`. Pour ce faire, vous pouvez associer une *dépendance de cache* à chaque élément ajouté au cache dans la méthode `AddCacheItem(key, value)`. En général, une dépendance de cache peut être un autre élément dans le cache, un fichier sur le système de fichiers ou des données d’une base de données Microsoft SQL Server. Lorsque la dépendance change ou est supprimée du cache, les éléments du cache auquel elle est associée sont supprimés automatiquement du cache. Pour ce didacticiel, nous voulons créer un élément supplémentaire dans le cache qui sert de dépendance de cache pour tous les éléments ajoutés via la classe `ProductsCL`. De cette façon, tous ces éléments peuvent être supprimés du cache en supprimant simplement la dépendance du cache.

Nous allons mettre à jour la méthode `AddCacheItem(key, value)` pour que chaque élément ajouté au cache par le biais de cette méthode soit associé à une seule dépendance de cache :

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` est un tableau de chaînes qui contient une valeur unique, ProductsCache. Tout d’abord, un élément de cache est ajouté au cache et affecté la date et l’heure actuelles. Si l’élément de cache existe déjà, il est mis à jour. Une dépendance de cache est ensuite créée. Le constructeur de la [classe`CacheDependency`](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) a un nombre de surcharges, mais celui qui est utilisé ici attend deux entrées de tableau `string`. La première spécifie le jeu de fichiers à utiliser comme dépendances. Étant donné que nous ne souhaitons pas utiliser de dépendances basées sur des fichiers, la valeur `null` est utilisée pour le premier paramètre d’entrée. Le deuxième paramètre d’entrée spécifie le jeu de clés de cache à utiliser comme dépendances. Ici, nous spécifions notre seule dépendance, `MasterCacheKeyArray`. La `CacheDependency` est ensuite transmise à la méthode `Insert`.

Une fois cette modification apportée à `AddCacheItem(key, value)`, l’invalidation du cache est aussi simple que la suppression de la dépendance.

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Étape 5 : appel de la couche de mise en cache à partir de la couche de présentation

Les classes et les méthodes de la couche de mise en cache peuvent être utilisées pour travailler avec des données à l’aide des techniques que nous avons examinées dans ces didacticiels. Pour illustrer l’utilisation des données mises en cache, enregistrez vos modifications dans la classe `ProductsCL`, puis ouvrez la page `FromTheArchitecture.aspx` dans le dossier `Caching` et ajoutez un contrôle GridView. À partir de la balise active GridView s, créez un nouvel ObjectDataSource. Dans la première étape de l’Assistant, vous devriez voir la classe `ProductsCL` comme l’une des options de la liste déroulante.

[![la classe ProductsCL est incluse dans la liste déroulante des objets métier](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Figure 4**: la classe `ProductsCL` est incluse dans la liste déroulante objet métier ([cliquez pour afficher l’image en taille réelle](caching-data-in-the-architecture-cs/_static/image6.png))

Après avoir sélectionné `ProductsCL`, cliquez sur suivant. La liste déroulante de l’onglet sélectionner comporte deux éléments : `GetProducts()` et `GetProductsByCategoryID(categoryID)` et l’onglet mettre à jour a la seule surcharge de `UpdateProduct`. Choisissez la méthode `GetProducts()` dans l’onglet sélectionner et la méthode `UpdateProducts` sous l’onglet mettre à jour, puis cliquez sur Terminer.

[![les méthodes de la classe ProductsCL s sont répertoriées dans les listes déroulantes](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Figure 5**: les méthodes de la classe `ProductsCL` s sont répertoriées dans les listes déroulantes ([cliquez pour afficher l’image en taille réelle](caching-data-in-the-architecture-cs/_static/image9.png))

Une fois l’Assistant terminé, Visual Studio définit la propriété ObjectDataSource s `OldValuesParameterFormatString` sur `original_{0}` et ajoute les champs appropriés au GridView. Remplacez la valeur par défaut de la propriété `OldValuesParameterFormatString`, `{0}`et configurez le contrôle GridView pour prendre en charge la pagination, le tri et la modification. Étant donné que la surcharge `UploadProducts` utilisée par le CL accepte uniquement le nom et le prix du produit modifié, limitez le GridView afin que seuls ces champs soient modifiables.

Dans le didacticiel précédent, nous avons défini un GridView pour inclure des champs pour les champs `ProductName`, `CategoryName`et `UnitPrice`. N’hésitez pas à répliquer cette mise en forme et cette structure. dans ce cas, vos balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

À ce stade, nous disposons d’une page qui utilise la couche de mise en cache. Pour voir le cache en action, définissez des points d’arrêt dans la `ProductsCL` classe s `GetProducts()` et `UpdateProduct` méthodes. Accédez à la page dans un navigateur et parcourez le code lors du tri et de la pagination pour voir les données extraites du cache. Ensuite, mettez à jour un enregistrement et notez que le cache est invalidé et, par conséquent, il est récupéré de la couche BLL lorsque les données sont reliées au contrôle GridView.

> [!NOTE]
> La couche de mise en cache fournie dans le téléchargement accompagnant cet article n’est pas complète. Il ne contient qu’une seule classe, `ProductsCL`, qui arbore uniquement un petit nombre de méthodes. En outre, une seule page ASP.NET utilise la CL (`~/Caching/FromTheArchitecture.aspx`). toutes les autres font toujours référence à la couche BLL directement. Si vous envisagez d’utiliser un CL dans votre application, tous les appels de la couche de présentation doivent accéder à la CL, ce qui nécessiterait que les classes et méthodes CL décrivaient ces classes et méthodes dans la couche BLL actuellement utilisée par la couche de présentation.

## <a name="summary"></a>Récapitulatif

Si la mise en cache peut être appliquée au niveau de la couche de présentation avec les contrôles SqlDataSource et ObjectDataSource de ASP.NET 2,0 s, les responsabilités de mise en cache dans l’idéal sont déléguées à une couche distincte dans l’architecture. Dans ce didacticiel, nous avons créé une couche de mise en cache qui réside entre la couche de présentation et la couche de logique métier. La couche de mise en cache doit fournir le même ensemble de classes et de méthodes qui existent dans la couche BLL et sont appelées à partir de la couche de présentation.

Les exemples de couche de mise en cache que nous avons explorés dans cet exemple et les didacticiels précédents présentent le *chargement réactif*. Avec le chargement réactif, les données sont chargées dans le cache uniquement lorsqu’une demande de données est effectuée et que des données sont manquantes dans le cache. Les données peuvent également être *chargées de manière proactive* dans le cache, une technique qui charge les données dans le cache avant qu’elles ne soient réellement nécessaires. Dans le didacticiel suivant, nous verrons un exemple de chargement proactif lorsque nous examinerons comment stocker des valeurs statiques dans le cache au démarrage de l’application.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Teresa Murph. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](caching-data-with-the-objectdatasource-cs.md)
> [Suivant](caching-data-at-application-startup-cs.md)
