---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: Mise en cache des données auC#démarrage de l’application () | Microsoft Docs
author: rick-anderson
description: Dans toutes les applications Web, certaines données seront fréquemment utilisées et certaines données seront rarement utilisées. Nous pouvons améliorer les performances de notre application ASP.NET b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b55b0df1b7843120de284891e16178df23fabe
ms.sourcegitcommit: fe5c7512383a9b0a05d321ff10d3cca1611556f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70386543"
---
# <a name="caching-data-at-application-startup-c"></a>Mise en cache de données au démarrage de l’application (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> Dans toutes les applications Web, certaines données seront fréquemment utilisées et certaines données seront rarement utilisées. Nous pouvons améliorer les performances de notre application ASP.NET en chargeant à l’avance les données fréquemment utilisées, une technique appelée Caching. Ce didacticiel présente une approche du chargement proactif, qui consiste à charger des données dans le cache au démarrage de l’application.

## <a name="introduction"></a>Introduction

Les deux didacticiels précédents ont examiné la mise en cache des données dans les couches de présentation et de mise en cache. Lors de la [mise en cache des données avec ObjectDataSource](caching-data-with-the-objectdatasource-cs.md), nous avons examiné l’utilisation des fonctionnalités de mise en cache d’ObjectDataSource pour mettre en cache les données dans la couche de présentation. [La mise en cache des données dans l’architecture](caching-data-in-the-architecture-cs.md) a examiné la mise en cache dans une nouvelle couche de mise en cache distincte. Ces deux didacticiels ont utilisé le *chargement réactif* dans l’utilisation du cache de données. Avec le chargement réactif, chaque fois que les données sont demandées, le système vérifie d’abord s’il se trouve dans le cache. Si ce n’est pas le cas, il récupère les données de la source d’origine, telles que la base de données, puis les stocke dans le cache. Le principal avantage du chargement réactif est sa facilité d’implémentation. L’un de ses inconvénients est sa performance inégale entre les requêtes. Imaginez une page qui utilise la couche de mise en cache du didacticiel précédent pour afficher des informations sur le produit. Lorsque cette page est visitée pour la première fois, ou lorsqu’elle est visitée pour la première fois après la suppression des données mises en cache en raison de contraintes de mémoire ou si l’expiration spécifiée a été atteinte, les données doivent être récupérées de la base de données. Par conséquent, les demandes des utilisateurs prendront plus de temps que les demandes des utilisateurs qui peuvent être traitées par le cache.

Le *chargement proactif* offre une autre stratégie de gestion du cache qui permet de lisser les performances entre les demandes en chargeant les données mises en cache avant qu’elles ne soient nécessaires. En règle générale, le chargement proactif utilise un processus qui vérifie périodiquement ou est averti lorsqu’une mise à jour a été effectuée sur les données sous-jacentes. Ce processus met ensuite à jour le cache pour le conserver à nouveau. Le chargement proactif est particulièrement utile si les données sous-jacentes proviennent d’une connexion lente à la base de données, d’un service Web ou d’une source de données particulièrement lente. Toutefois, cette approche du chargement proactif est plus difficile à implémenter, car elle nécessite la création, la gestion et le déploiement d’un processus pour vérifier les modifications et mettre à jour le cache.

Une autre version du chargement proactif, et le type que nous allons explorer dans ce didacticiel, chargent les données dans le cache au démarrage de l’application. Cette approche est particulièrement utile pour mettre en cache des données statiques, telles que les enregistrements dans les tables de recherche de base de données.

> [!NOTE]
> Pour plus d’informations sur les différences entre le chargement proactif et réactif, ainsi que sur les listes des avantages, les inconvénients et les recommandations d’implémentation, reportez-vous à la section [gestion du contenu d’un cache](https://msdn.microsoft.com/library/ms978503.aspx) du [Guide d’architecture de mise en cache pour .net Applications d’infrastructure](https://msdn.microsoft.com/library/ms978498.aspx).

## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Étape 1 : Détermination des données à mettre en cache au démarrage de l’application

Les exemples de mise en cache qui utilisent le chargement réactif que nous avons examinés dans les deux didacticiels précédents fonctionnent bien avec les données qui peuvent changer régulièrement et ne prend pas exorbitantly de temps à générer. Toutefois, si les données mises en cache ne changent jamais, l’expiration utilisée par le chargement réactif est superflue. De même, si la génération des données mises en cache prend beaucoup de temps, alors les utilisateurs dont les demandes recherchent le cache vide devront supporter une longue attente pendant l’extraction des données sous-jacentes. Envisagez de mettre en cache des données et des données statiques qui prennent un temps exceptionnellement long pour la génération au démarrage de l’application.

Bien que les bases de données aient de nombreuses valeurs dynamiques et changeantes, elles ont également une quantité de données statiques équitable. Par exemple, presque tous les modèles de données ont une ou plusieurs colonnes qui contiennent une valeur particulière d’un ensemble fixe de choix. Une `Patients` table de base de données `PrimaryLanguage` peut avoir une colonne, dont l’ensemble de valeurs peut être l’anglais, l’espagnol, le français, le russe, le japonais, etc. Ces types de colonnes sont souvent implémentés à l’aide de *tables de choix*. Au lieu de stocker la chaîne en anglais ou en `Patients` français dans la table, une deuxième table est créée avec, généralement, deux colonnes : un identificateur unique et une description de chaîne, avec un enregistrement pour chaque valeur possible. La `PrimaryLanguage` colonne de la `Patients` table stocke l’identificateur unique correspondant dans la table de recherche. Dans la figure 1, la langue principale du patient John Doe est l’anglais, tandis que Ed Johnson est russe.

![La table de langues est une table de recherche utilisée par la table patients](caching-data-at-application-startup-cs/_static/image1.png)

**Figure 1**: La `Languages` table est une table de recherche utilisée par `Patients` la table.

L’interface utilisateur pour la modification ou la création d’un nouveau patient inclut une liste déroulante des langues autorisées remplies par les enregistrements dans `Languages` la table. Sans mise en cache, chaque fois que cette interface est visitée, `Languages` le système doit interroger la table. C’est un gaspillage et inutile, car les valeurs de la table de recherche changent très rarement, si jamais.

Nous pourrions mettre en `Languages` cache les données à l’aide des mêmes techniques de chargement réactives que celles étudiées dans les didacticiels précédents. Toutefois, le chargement réactif utilise une expiration basée sur la durée, ce qui n’est pas nécessaire pour les données de table de recherche statique. Bien que la mise en cache à l’aide du chargement réactif soit meilleure qu’aucune mise en cache, la meilleure approche consiste à charger de manière proactive les données de la table de recherche dans le cache au démarrage de l’application.

Dans ce didacticiel, nous allons examiner comment mettre en cache les données de table de recherche et d’autres informations statiques.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Étape 2 : Examen des différentes façons de mettre en cache des données

Les informations peuvent être mises en cache par programmation dans une application ASP.NET à l’aide d’une variété d’approches. Nous avons déjà vu comment utiliser le cache de données dans les didacticiels précédents. Les objets peuvent également être mis en cache par programmation à l’aide de *membres statiques* ou de l’état de l' *application*.

Lorsque vous utilisez une classe, en général, la classe doit d’abord être instanciée pour pouvoir accéder à ses membres. Par exemple, pour appeler une méthode à partir de l’une des classes de notre couche de logique métier, nous devons d’abord créer une instance de la classe :

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

Avant de pouvoir appeler *SomeMethod* ou Work with *SomeProperty*, nous devons d’abord créer une instance de la classe à `new` l’aide du mot clé. *SomeMethod* et *SomeProperty* sont associés à une instance particulière. La durée de vie de ces membres est liée à la durée de vie de l’objet associé. Les *membres statiques*, quant à eux, sont des variables, des propriétés et des méthodes qui sont partagées entre *toutes les* instances de la classe et, par conséquent, ont une durée de vie aussi longue que la classe. Les membres statiques sont dénotés par `static`le mot clé.

En plus des membres statiques, les données peuvent être mises en cache à l’aide de l’état de l’application. Chaque application ASP.NET gère une collection nom/valeur qui est partagée entre tous les utilisateurs et les pages de l’application. Cette collection est accessible à l’aide de la [ `Application` propriété](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)de la [ `HttpContext` classe](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)et utilisée à partir de la classe code-behind d’une page ASP.net, comme suit :

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

Le cache de données fournit une API beaucoup plus riche pour la mise en cache des données, en fournissant des mécanismes pour les expirations basées sur le temps et la dépendance, les priorités des éléments de cache, etc. Avec les membres statiques et l’état de l’application, ces fonctionnalités doivent être ajoutées manuellement par le développeur de pages. Toutefois, lors de la mise en cache des données au démarrage de l’application pendant la durée de vie de l’application, les avantages du cache de données sont discutable. Dans ce didacticiel, nous allons examiner le code qui utilise les trois techniques de mise en cache des données statiques.

## <a name="step-3-caching-thesupplierstable-data"></a>Étape 3 : Mise en`Suppliers`cache des données de table

Les tables de base de données Northwind que nous avons implémentées jusqu’à ce jour n’incluent pas les tables de recherche traditionnelles. Les quatre DataTables implémentés dans notre couche DAL toutes les tables de modèle dont les valeurs sont non statiques. Plutôt que de passer le temps d’ajouter un nouveau DataTable à la couche DAL, puis une nouvelle classe et de nouvelles méthodes à la couche BLL, pour ce didacticiel, `Suppliers` nous supposons simplement que les données de la table sont statiques. Par conséquent, nous pourrions mettre en cache ces données au démarrage de l’application.

Pour commencer, créez une nouvelle classe nommée `StaticCache.cs` dans le `CL` dossier.

![Créer la classe StaticCache.cs dans le dossier CL](caching-data-at-application-startup-cs/_static/image2.png)

**Figure 2**: Créer la `StaticCache.cs` classe dans le `CL` dossier

Nous devons ajouter une méthode qui charge les données au démarrage dans le magasin de cache approprié, ainsi que les méthodes qui retournent des données à partir de ce cache.

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

Le code ci-dessus utilise une variable membre `suppliers`statique,, pour contenir les résultats `SuppliersBLL` de la `GetSuppliers()` méthode de la classe, qui est `LoadStaticCache()` appelée à partir de la méthode. La `LoadStaticCache()` méthode est censée être appelée pendant le démarrage de l’application. Une fois que ces données ont été chargées au démarrage de l’application, toute page devant fonctionner avec des données `StaticCache` de fournisseur `GetSuppliers()` peut appeler la méthode de la classe. Par conséquent, l’appel à la base de données pour récupérer les fournisseurs ne se produit qu’une seule fois, au démarrage de l’application.

Au lieu d’utiliser une variable de membre statique comme stockage de cache, nous aurions pu utiliser l’état de l’application ou le cache de données. Le code suivant illustre la classe reoutils pour utiliser l’état de l’application :

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

Dans `LoadStaticCache()`, les informations sur le fournisseur sont stockées dans la *clé*de la variable d’application. Elle est retournée en tant que type`Northwind.SuppliersDataTable`approprié ( `GetSuppliers()`) à partir de. L’état de l’application est accessible dans les classes code-behind des pages ASP.NET `Application["key"]`à l’aide de, dans l' `HttpContext.Current.Application["key"]` architecture que nous devons utiliser pour `HttpContext`récupérer le actuel.

De même, le cache de données peut être utilisé comme un magasin de cache, comme le montre le code suivant :

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

Pour ajouter un élément au cache de données sans expiration basée sur la durée, utilisez les `System.Web.Caching.Cache.NoAbsoluteExpiration` valeurs et `System.Web.Caching.Cache.NoSlidingExpiration` comme paramètres d’entrée. Cette surcharge particulière de la `Insert` méthode du cache de données a été sélectionnée afin que nous puissions spécifier la *priorité* de l’élément de cache. La priorité est utilisée pour déterminer les éléments à nettoyer à partir du cache lorsque la mémoire disponible est faible. Ici, nous utilisons la `NotRemovable`priorité, qui garantit que cet élément de cache ne sera pas nettoyé.

> [!NOTE]
> Le téléchargement de ce didacticiel implémente `StaticCache` la classe à l’aide de l’approche de variable de membre statique. Le code pour les techniques d’État et de cache de données de l’application est disponible dans les commentaires du fichier de classe.

## <a name="step-4-executing-code-at-application-startup"></a>Étape 4 : Exécution du code au démarrage de l’application

Pour exécuter du code lors du premier démarrage d’une application Web, nous devons créer un fichier `Global.asax`spécial nommé. Ce fichier peut contenir des gestionnaires d’événements pour les événements application, session et au niveau de la demande, et c’est ici que nous pouvons ajouter du code qui sera exécuté à chaque démarrage de l’application.

Ajoutez le `Global.asax` fichier au répertoire racine de votre application Web en cliquant avec le bouton droit sur le nom du projet de site Web dans le Explorateur de solutions de Visual Studio, puis en choisissant Ajouter un nouvel élément. Dans la boîte de dialogue Ajouter un nouvel élément, sélectionnez le type d’élément classe d’application globale, puis cliquez sur le bouton Ajouter.

> [!NOTE]
> Si vous disposez déjà d' `Global.asax` un fichier dans votre projet, le type d’élément de classe d’application globale ne figurera pas dans la boîte de dialogue Ajouter un nouvel élément.

[![Ajoutez le fichier global. asax au répertoire racine de votre application Web.](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**Figure 3**: Ajoutez le `Global.asax` fichier au répertoire racine de votre application Web ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-cs/_static/image5.png))

Le modèle `Global.asax` de fichier par défaut comprend cinq méthodes au sein d' `<script>` une balise côté serveur :

- **`Application_Start`** s’exécute lors du premier démarrage de l’application Web
- **`Application_End`** s’exécute lorsque l’application s’arrête
- **`Application_Error`** s’exécute chaque fois qu’une exception non gérée atteint l’application
- **`Session_Start`** s’exécute lors de la création d’une nouvelle session
- **`Session_End`** s’exécute lorsqu’une session a expiré ou est abandonnée

Le `Application_Start` gestionnaire d’événements est appelé une seule fois pendant le cycle de vie d’une application. L’application démarre la première fois qu’une ressource ASP.net est demandée à partir de l’application et continue de s’exécuter jusqu’à ce que l’application soit redémarrée, ce qui peut `/Bin` se produire en modifiant `Global.asax`le contenu du dossier, en modifiant, en modifiant le contenu dans le `App_Code` dossier ou modification du `Web.config` fichier, entre autres causes. Pour plus d’informations sur le cycle de vie de l’application, consultez [vue d’ensemble du cycle de vie des applications ASP.net](https://msdn.microsoft.com/library/ms178473.aspx) .

Pour ces didacticiels, nous devons uniquement ajouter du code `Application_Start` à la méthode. n’hésitez donc pas à supprimer les autres. Dans `Application_Start`, il vous suffit `StaticCache` d’appeler `LoadStaticCache()` la méthode de la classe, qui chargera et mettra en cache les informations sur le fournisseur :

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

C’est aussi simple que cela ! Au démarrage de l’application `LoadStaticCache()` , la méthode récupère les informations de fournisseur de la couche BLL et les stocke dans une variable membre statique (ou dans la Banque de cache que `StaticCache` vous avez terminée à l’aide de dans la classe). Pour vérifier ce comportement, définissez un point d’arrêt `Application_Start` dans la méthode et exécutez votre application. Notez que le point d’arrêt est atteint lors du démarrage de l’application. Toutefois, les requêtes suivantes n’entraînent pas `Application_Start` l’exécution de la méthode.

[![Utilisez un point d’arrêt pour vérifier que le gestionnaire d’événements Application_Start est en cours d’exécution](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**Figure 4**: Utilisez un point d’arrêt pour vérifier `Application_Start` que le gestionnaire d’événements est en cours d’exécution ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-cs/_static/image8.png))

> [!NOTE]
> Si vous n’atteignez pas `Application_Start` le point d’arrêt lorsque vous démarrez le débogage pour la première fois, cela est dû au fait que votre application a déjà démarré. Forcez l’application à redémarrer en modifiant vos `Global.asax` fichiers `Web.config` ou, puis réessayez. Vous pouvez simplement ajouter (ou supprimer) une ligne vide à la fin de l’un de ces fichiers pour redémarrer rapidement l’application.

## <a name="step-5-displaying-the-cached-data"></a>Étape 5 : Affichage des données mises en cache

À ce stade, `StaticCache` la classe a une version des données du fournisseur mise en cache au démarrage de l’application, accessible `GetSuppliers()` par le biais de sa méthode. Pour utiliser ces données à partir de la couche de présentation, nous pouvons utiliser un ObjectDataSource ou appeler par programmation `StaticCache` la `GetSuppliers()` méthode de la classe à partir de la classe code-behind d’une page ASP.net. Examinons l’utilisation des contrôles ObjectDataSource et GridView pour afficher les informations sur les fournisseurs mises en cache.

Commencez par ouvrir la `AtApplicationStartup.aspx` page dans le `Caching` dossier. Faites glisser un contrôle GridView de la boîte à outils vers le `ID` concepteur, `Suppliers`en affectant à sa propriété la valeur. Ensuite, à partir de la balise active de GridView, choisissez de créer `SuppliersCachedDataSource`un ObjectDataSource nommé. Configurez l’ObjectDataSource pour `StaticCache` utiliser la `GetSuppliers()` méthode de la classe.

[![Configurer ObjectDataSource pour utiliser la classe StaticCache](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**Figure 5**: Configurer ObjectDataSource pour utiliser la classe `StaticCache` ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-cs/_static/image11.png))

[![Utilisez la méthode GetSuppliers () pour récupérer les données des fournisseurs mises en cache](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**Figure 6**: Utilisez la `GetSuppliers()` méthode pour récupérer les données des fournisseurs mises en cache ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-cs/_static/image14.png))

Une fois l’Assistant terminé, Visual Studio ajoute automatiquement BoundFields pour chacun des champs de données dans `SuppliersDataTable`. Le balisage déclaratif de GridView et de ObjectDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

La figure 7 illustre la page affichée dans un navigateur. La sortie est la même que nous avons extrait les données de la classe `SuppliersBLL` de la couche BLL, `StaticCache` mais l’utilisation de la classe retourne les données des fournisseurs mises en cache au démarrage de l’application. Vous pouvez définir des points d' `StaticCache` arrêt dans `GetSuppliers()` la méthode de la classe pour vérifier ce comportement.

[![Les données des fournisseurs mises en cache sont affichées dans un GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**Figure 7**: Les données des fournisseurs mises en cache sont affichées dans un GridView ([cliquez pour afficher l’image en taille réelle](caching-data-at-application-startup-cs/_static/image17.png))

## <a name="summary"></a>Récapitulatif

La plupart des modèles de données contiennent une quantité équitable de données statiques, généralement implémentées sous la forme de tables de choix. Étant donné que ces informations sont statiques, il n’y a aucune raison d’accéder continuellement à la base de données chaque fois que ces informations doivent être affichées. En outre, en raison de sa nature statique, lors de la mise en cache des données, il n’est pas nécessaire d’effectuer une expiration. Dans ce didacticiel, nous avons vu comment prendre ces données et les mettre en cache dans le cache de données, l’état de l’application et par le biais d’une variable de membre statique. Ces informations sont mises en cache au démarrage de l’application et restent dans le cache tout au long de la durée de vie de l’application.

Dans ce didacticiel et les deux dernières, nous avons examiné la mise en cache des données pendant la durée de vie de l’application, ainsi que l’utilisation d’expirations temporelles. Toutefois, lors de la mise en cache des données de base de données, une expiration basée sur la durée peut être inférieure à la solution idéale. Au lieu de vider régulièrement le cache, il est préférable de supprimer uniquement l’élément mis en cache lorsque les données de la base de données sous-jacente sont modifiées. Cette solution idéale est possible grâce à l’utilisation de dépendances de cache SQL, que nous examinerons dans le prochain didacticiel.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à l’adresse [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à l' [http://ScottOnWriting.NET](http://ScottOnWriting.NET)adresse.

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Teresa Murphy et Zack Jones. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, déposez-moi une ligne à l’adresse [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](caching-data-in-the-architecture-cs.md)
> [Suivant](using-sql-cache-dependencies-cs.md)
