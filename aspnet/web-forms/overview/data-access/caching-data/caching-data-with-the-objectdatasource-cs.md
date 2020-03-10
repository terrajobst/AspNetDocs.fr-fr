---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Mise en cache de données avecC#ObjectDataSource () | Microsoft Docs
author: rick-anderson
description: La mise en cache peut être la différence entre une application Web lente et rapide. Ce didacticiel est le premier d’une analyse détaillée de la mise en cache dans ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: c9883314d6153b9816d9bad2a281ab3c0a816448
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78524254"
---
# <a name="caching-data-with-the-objectdatasource-c"></a>Mise en cache de données avec ObjectDataSource (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) ou [Télécharger le PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> La mise en cache peut être la différence entre une application Web lente et rapide. Ce didacticiel est le premier d’une analyse détaillée de la mise en cache dans ASP.NET. Découvrez les concepts clés de la mise en cache et comment appliquer la mise en cache à la couche de présentation via le contrôle ObjectDataSource.

## <a name="introduction"></a>Introduction

En informatique, la *mise en cache* est le processus qui consiste à mettre des données ou des informations coûteuses à obtenir et à en stocker une copie dans un emplacement qui est plus rapide à accéder. Pour les applications pilotées par les données, les requêtes volumineuses et complexes consomment généralement la majorité du temps d’exécution de l’application. Les performances d’une telle application peuvent souvent être améliorées en stockant les résultats des requêtes de base de données coûteuses dans la mémoire de l’application.

ASP.NET 2,0 offre une variété d’options de mise en cache. Une page Web entière ou un contrôle utilisateur rendu du balisage peut être mis en cache via *la mise en cache de sortie*. Les contrôles ObjectDataSource et SqlDataSource offrent également des fonctionnalités de mise en cache, ce qui permet aux données d’être mises en cache au niveau du contrôle. Et le *cache de données* de ASP.NET fournit une API de mise en cache riche qui permet aux développeurs de pages de mettre en cache les objets par programmation. Dans ce didacticiel et les trois suivants, nous allons examiner l’utilisation des fonctionnalités de mise en cache de ObjectDataSource, ainsi que du cache de données. Nous allons également découvrir comment mettre en cache des données à l’ensemble de l’application au démarrage et comment conserver les données mises en cache à l’aide de dépendances de cache SQL. Ces didacticiels n’explorent pas la mise en cache de sortie. Pour plus d’informations sur la mise en cache de sortie, consultez [mise en cache de sortie dans ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

La mise en cache peut être appliquée à n’importe quel emplacement de l’architecture, de la couche d’accès aux données jusqu’à la couche de présentation. Dans ce didacticiel, nous allons examiner l’application de la mise en cache à la couche de présentation via le contrôle ObjectDataSource. Dans le didacticiel suivant, nous examinerons la mise en cache des données au niveau de la couche de logique métier.

## <a name="key-caching-concepts"></a>Principaux concepts de Caching

La mise en cache peut améliorer sensiblement les performances globales et l’évolutivité d’une application en prenant des données qui sont coûteuses à générer et à stocker une copie de celle-ci dans un emplacement qui peut être plus efficace. Étant donné que le cache contient uniquement une copie des données réelles sous-jacentes, il peut devenir obsolète, ou *périmé*, si les données sous-jacentes sont modifiées. Pour combattre cela, un développeur de pages peut indiquer des critères par lesquels l’élément du cache sera *expulsé* du cache, à l’aide de l’une des deux opérations suivantes :

- **Critères basés sur le temps** un élément peut être ajouté au cache pour une durée absolue ou glissante. Par exemple, un développeur de pages peut indiquer une durée de, par exemple, 60 secondes. Avec une durée absolue, l’élément mis en cache est expulsé de 60 secondes après son ajout au cache, quelle que soit la fréquence à laquelle il a fait l’objet d’un accès. Avec une durée glissante, l’élément mis en cache est supprimé 60 secondes après le dernier accès.
- **Critères basés sur la dépendance** une dépendance peut être associée à un élément lorsqu’il est ajouté au cache. En cas de modification de la dépendance de l’élément, il est expulsé du cache. La dépendance peut être un fichier, un autre élément de cache ou une combinaison des deux. ASP.NET 2,0 autorise également les dépendances de cache SQL, qui permettent aux développeurs d’ajouter un élément au cache et de le supprimer lorsque les données de la base de données sous-jacente sont modifiées. Nous examinerons les dépendances de cache SQL dans le didacticiel [à venir à l’aide des dépendances de cache SQL](using-sql-cache-dependencies-cs.md) .

Quels que soient les critères d’éviction spécifiés, un élément du cache peut être *nettoyé* avant que les critères basés sur le temps ou sur les dépendances soient atteints. Si le cache a atteint sa capacité maximale, les éléments existants doivent être supprimés avant de pouvoir ajouter de nouveaux éléments. Par conséquent, lorsque vous utilisez par programmation des données mises en cache, il est vital que vous partiez toujours du principe que les données mises en cache ne sont peut-être pas présentes. Nous allons examiner le modèle à utiliser pour accéder aux données à partir du cache par programmation dans le didacticiel suivant, *mise en cache des données dans l’architecture*.

La mise en cache offre un moyen économique d’augmenter les performances d’une application. À mesure que [Steven Smith](http://aspadvice.com/blogs/ssmith/) en est articulé dans son article [ASP.net Caching : techniques et meilleures pratiques](https://msdn.microsoft.com/library/aa478965.aspx):

La mise en cache peut être un bon moyen d’obtenir de bonnes performances sans nécessiter beaucoup de temps et d’analyses. La mémoire est peu coûteuse. par conséquent, si vous pouvez obtenir les performances dont vous avez besoin en mettant en cache la sortie pendant 30 secondes au lieu de passer un jour ou une semaine à essayer d’optimiser votre code ou votre base de données, effectuez la solution de mise en cache (en supposant que les données anciennes de 30 secondes sont correctes) et passez à la suite. Par la suite, une conception médiocre va probablement vous tenir à vous, et bien évidemment, vous devez essayer de concevoir correctement vos applications. Mais si vous avez simplement besoin d’obtenir de bonnes performances aujourd’hui, la mise en cache peut être une excellente [approche], vous permettant de refactoriser votre application à une date ultérieure lorsque vous avez le temps de le faire.

Bien que la mise en cache puisse apporter des améliorations significatives en matière de performances, elle ne s’applique pas dans toutes les situations, par exemple avec les applications qui utilisent des données en temps réel, les mises à jour fréquentes, ou où même les données obsolètes à durée de vie courte sont inacceptables. Mais pour la plupart des applications, la mise en cache doit être utilisée. Pour plus d’informations sur la mise en cache dans ASP.NET 2,0, reportez-vous à la section [Caching for performance](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) des [didacticiels de démarrage rapide ASP.NET 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Étape 1 : création des pages Web de mise en cache

Avant de commencer l’exploration des fonctionnalités de mise en cache de ObjectDataSource, commençons par un moment pour créer les pages ASP.NET dans notre projet de site Web dont nous aurons besoin pour ce didacticiel et les trois prochaines. Commencez par ajouter un nouveau dossier nommé `Caching`. Ajoutez ensuite les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`

![Ajouter les pages ASP.NET pour les didacticiels liés à la mise en cache](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Figure 1**: ajouter les pages ASP.net pour les didacticiels liés à la mise en cache

Comme dans les autres dossiers, `Default.aspx` dans le dossier `Caching` répertorie les didacticiels de la section. N’oubliez pas que le contrôle utilisateur `SectionLevelTutorialListing.ascx` fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur la page s Mode Création.

[![figure 2 : ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Figure 2**: figure 2 : ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-cs/_static/image4.png))

Enfin, ajoutez ces pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après l’utilisation de l' `<siteMapNode>`de données binaires :

[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels de mise en cache.

![Le plan de site contient maintenant des entrées pour les didacticiels de mise en cache](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Figure 3**: le plan de site contient maintenant des entrées pour les didacticiels de mise en cache

## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Étape 2 : affichage d’une liste de produits dans une page Web

Ce didacticiel explore l’utilisation des fonctionnalités de mise en cache intégrées du contrôle ObjectDataSource. Avant de pouvoir examiner ces fonctionnalités, nous avons tout d’abord besoin d’une page à partir de laquelle travailler. Nous allons créer une page Web qui utilise un GridView pour répertorier les informations sur les produits récupérées par un ObjectDataSource à partir de la classe `ProductsBLL`.

Commencez par ouvrir la page `ObjectDataSource.aspx` dans le dossier `Caching`. Faites glisser un contrôle GridView de la boîte à outils vers le concepteur, affectez à sa propriété `ID` la valeur `Products`et, à partir de sa balise active, choisissez de le lier à un nouveau contrôle ObjectDataSource nommé `ProductsDataSource`. Configurez l’ObjectDataSource pour qu’il fonctionne avec la classe `ProductsBLL`.

[![configurer ObjectDataSource pour utiliser la classe ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Figure 4**: configurer ObjectDataSource pour utiliser la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-cs/_static/image8.png))

Pour cette page, créons un GridView modifiable afin que nous puissions examiner ce qui se passe lorsque les données mises en cache dans ObjectDataSource sont modifiées par le biais de l’interface de GridView s. Laissez la liste déroulante de l’onglet sélectionner définie sur sa valeur par défaut, `GetProducts()`, mais remplacez l’élément sélectionné dans l’onglet mettre à jour par la surcharge `UpdateProduct` qui accepte `productName`, `unitPrice`et `productID` comme paramètres d’entrée.

[![définir la liste déroulante des onglets de mise à jour sur la surcharge UpdateProduct appropriée](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Figure 5**: définir la liste déroulante des onglets de mise à jour sur la surcharge de `UpdateProduct` appropriée ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-cs/_static/image11.png))

Enfin, définissez les listes déroulantes des onglets insérer et supprimer sur (aucun), puis cliquez sur Terminer. À la fin de l’Assistant Configuration de la source de données, Visual Studio définit la propriété ObjectDataSource s `OldValuesParameterFormatString` sur `original_{0}`. Comme nous l’avons vu dans le didacticiel sur l' [insertion, la mise à jour et la suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , cette propriété doit être supprimée de la syntaxe déclarative ou redéfinie sur sa valeur par défaut, `{0}`, pour que notre flux de travail de mise à jour puisse s’exécuter sans erreur.

En outre, à la fin de l’Assistant, Visual Studio ajoute un champ au GridView pour chacun des champs de données de produit. Supprimez tout sauf les `ProductName`, `CategoryName`et `UnitPrice` BoundFields. Ensuite, mettez à jour les propriétés de `HeaderText` de chacun de ces BoundFields de façon à ce qu’elles soient respectivement produit, Category et Price. Étant donné que le champ `ProductName` est obligatoire, convertissez le BoundField en TemplateField et ajoutez un RequiredFieldValidator à la `EditItemTemplate`. De même, convertissez le `UnitPrice` BoundField en TemplateField et ajoutez un CompareValidator pour vous assurer que la valeur entrée par l’utilisateur est une valeur monétaire valide qui est supérieure ou égale à zéro. En plus de ces modifications, n’hésitez pas à effectuer des modifications esthétiques, telles que l’alignement à droite de la valeur `UnitPrice`, ou la spécification de la mise en forme du texte `UnitPrice` dans ses interfaces en lecture seule et en modification.

Rendez le GridView modifiable en activant la case à cocher Activer la modification dans la balise active GridView s. Activez également les cases à cocher Activer la pagination et activer le tri.

> [!NOTE]
> Vous avez besoin d’un avis sur la personnalisation de l’interface de modification de GridView s ? Si c’est le cas, reportez-vous au didacticiel [Personnalisation de l’interface de modification des données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

[![activer la prise en charge de GridView pour la modification, le tri et la pagination](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Figure 6**: activer la prise en charge de GridView pour la modification, le tri et la pagination ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-cs/_static/image14.png))

Après avoir apporté ces modifications GridView, les balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Comme le montre la figure 7, le contrôle GridView modifiable répertorie le nom, la catégorie et le prix de chacun des produits de la base de données. Prenez un moment pour tester la fonctionnalité des pages trier les résultats, les parcourir et modifier un enregistrement.

[![chaque nom, catégorie et prix du produit est listé dans un GridView pouvant être trié, paginable et modifiable](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Figure 7**: le nom, la catégorie et le prix de chaque produit sont listés dans un GridView pouvant être trié, paginable et modifiable ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-cs/_static/image17.png))

## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Étape 3 : examen du moment où ObjectDataSource demande des données

Le `Products` GridView récupère ses données à afficher en appelant la méthode `Select` de l’ObjectDataSource `ProductsDataSource`. Cet ObjectDataSource crée une instance de la couche de logique métier `ProductsBLL` classe et appelle sa méthode `GetProducts()`, qui à son tour appelle la méthode de la couche d’accès aux données `ProductsTableAdapter` s `GetProducts()`. La méthode DAL se connecte à la base de données Northwind et émet la requête `SELECT` configurée. Ces données sont ensuite retournées à la couche DAL, qui les empaquette dans un `NorthwindDataTable`. L’objet DataTable est retourné à la couche BLL, qui le retourne à l’ObjectDataSource, qui le retourne au GridView. Le GridView crée ensuite un objet `GridViewRow` pour chaque `DataRow` dans le DataTable, et chaque `GridViewRow` est finalement rendue dans le code HTML renvoyé au client et affiché sur le navigateur du visiteur.

Cette séquence d’événements se produit chaque fois que le GridView doit effectuer une liaison à ses données sous-jacentes. Cela se produit lorsque la page est visitée pour la première fois, lorsque vous passez d’une page de données à une autre, lorsque vous triez le contrôle GridView ou lorsque vous modifiez les données de GridView par le biais de ses interfaces intégrées de modification ou de suppression. Si l’état d’affichage GridView s est désactivé, le GridView est relié à chaque publication (postback) et à chaque publication (postback). Le contrôle GridView peut également être explicitement relié à ses données en appelant sa méthode `DataBind()`.

Pour apprécier pleinement la fréquence à laquelle les données sont récupérées de la base de données, vous pouvez afficher un message indiquant le moment où les données sont récupérées à nouveau. Ajoutez un contrôle Web Label au-dessus du contrôle GridView nommé `ODSEvents`. Effacez sa propriété `Text` et affectez à sa propriété `EnableViewState` la valeur `false`. Sous l’étiquette, ajoutez un contrôle Web Button et affectez à sa propriété `Text` la valeur postback.

[![ajouter une étiquette et un bouton à la page au-dessus du contrôle GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Figure 8**: ajouter une étiquette et un bouton à la page au-dessus du contrôle GridView ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-cs/_static/image20.png))

Pendant le flux de travail d’accès aux données, l’événement ObjectDataSource s `Selecting` se déclenche avant la création de l’objet sous-jacent et la méthode de configuration appelée. Créez un gestionnaire d’événements pour cet événement et ajoutez le code suivant :

[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Chaque fois que l’ObjectDataSource envoie une requête à l’architecture pour les données, l’étiquette affiche l’événement de sélection de texte déclenché.

Visitez cette page dans un navigateur. Lorsque la page est visitée pour la première fois, l’événement de sélection de texte déclenché est affiché. Cliquez sur le bouton publication (postback) et notez que le texte disparaît (en supposant que la propriété GridView s `EnableViewState` est définie sur `true`, la valeur par défaut). Cela est dû au fait que, lors de la publication (postback), le GridView est reconstruit à partir de son état d’affichage et, par conséquent, il n’est pas converti en ObjectDataSource pour ses données. Toutefois, le tri, la pagination ou la modification des données entraîne la reliaison du GridView à sa source de données, et par conséquent, le texte événement de sélection réapparaît.

[![chaque fois que le GridView est relié à sa source de données, la sélection de l’événement déclenché s’affiche.](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Figure 9**: chaque fois que le GridView est relié à sa source de données, la sélection de l’événement déclenché s’affiche ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-cs/_static/image23.png))

[![lorsque vous cliquez sur le bouton publication, le contrôle GridView est reconstruit à partir de son état d’affichage](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Figure 10**: lorsque vous cliquez sur le bouton de publication, le contrôle GridView est reconstruit à partir de son état d’affichage ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-cs/_static/image26.png))

La récupération des données de la base de données peut paraître gaspiller chaque fois que les données sont paginées ou triées. Après tout, puisque nous utilisons à nouveau la pagination par défaut, ObjectDataSource a récupéré tous les enregistrements lors de l’affichage de la première page. Même si le GridView ne fournit pas de prise en charge du tri et de la pagination, les données doivent être récupérées de la base de données chaque fois que la page est visitée pour la première fois par n’importe quel utilisateur (et à chaque publication, si l’état d’affichage est désactivé). Toutefois, si le GridView présente les mêmes données à tous les utilisateurs, ces demandes de base de données supplémentaires sont superflues. Pourquoi ne pas mettre en cache les résultats retournés par la méthode `GetProducts()` et lier le contrôle GridView à ces résultats mis en cache ?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Étape 4 : mise en cache des données à l’aide de ObjectDataSource

En définissant simplement quelques propriétés, ObjectDataSource peut être configuré pour mettre automatiquement en cache ses données récupérées dans le cache de données ASP.NET. La liste suivante récapitule les propriétés liées au cache de l’ObjectDataSource :

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) doit avoir la valeur `true` pour activer la mise en cache. La valeur par défaut est `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) durée, en secondes, pendant laquelle les données sont mises en cache. La valeur par défaut est 0. ObjectDataSource met uniquement en cache les données si `EnableCaching` est `true` et `CacheDuration` est définie sur une valeur supérieure à zéro.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) peut avoir la valeur `Absolute` ou `Sliding`. Si `Absolute`, ObjectDataSource met en cache ses données extraites pendant `CacheDuration` secondes ; Si `Sliding`, les données expirent uniquement après qu’elles n’ont pas été consultées pendant `CacheDuration` secondes. La valeur par défaut est `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) Utilisez cette propriété pour associer les entrées du cache ObjectDataSource s à une dépendance de cache existante. Les entrées de données ObjectDataSource s peuvent être supprimées prématurément du cache en faisant expirer le `CacheKeyDependency`associé. Cette propriété est généralement utilisée pour associer une dépendance de cache SQL au cache de l’ObjectDataSource s, une rubrique que nous explorerons ultérieurement à l’aide du didacticiel sur les [dépendances de cache SQL](using-sql-cache-dependencies-cs.md) .

Configurez le `ProductsDataSource` ObjectDataSource pour mettre ses données en cache pendant 30 secondes sur une échelle absolue. Définissez la propriété ObjectDataSource s `EnableCaching` sur `true` et sa propriété `CacheDuration` sur 30. Laissez la propriété `CacheExpirationPolicy` définie sur sa valeur par défaut, `Absolute`.

[![configurer ObjectDataSource pour mettre en cache ses données pendant 30 secondes](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Figure 11**: configurer ObjectDataSource pour mettre en cache ses données pendant 30 secondes ([cliquez pour afficher l’image en taille réelle](caching-data-with-the-objectdatasource-cs/_static/image29.png))

Enregistrez vos modifications et réexaminez cette page dans un navigateur. Le texte de sélection de l’événement s’affiche lorsque vous accédez pour la première fois à la page, car les données ne se trouvent pas dans le cache initialement. Toutefois, les publications (postbacks) suivantes déclenchées par un clic sur le bouton de publication (postback), le tri, la pagination ou le clic sur les boutons modifier ou annuler ne réaffichent *pas* le texte de sélection de l’événement déclenché. Cela est dû au fait que l’événement `Selecting` se déclenche uniquement lorsque ObjectDataSource obtient ses données à partir de son objet sous-jacent ; l’événement `Selecting` ne se déclenche pas si les données sont extraites du cache de données.

Au bout de 30 secondes, les données sont supprimées du cache. Les données seront également supprimées du cache si les méthodes d' `Insert`ObjectDataSource, `Update`ou `Delete` sont appelées. Par conséquent, au bout de 30 secondes ou lorsque vous avez cliqué sur le bouton mettre à jour, le tri, la pagination ou le clic sur les boutons modifier ou annuler entraîne l’affichage de ses données par ObjectDataSource pour l’objet sous-jacent, ce qui affiche le texte de sélection déclenché lorsque l’événement `Selecting` se déclenche. Ces résultats retournés sont remis dans le cache de données.

> [!NOTE]
> Si vous voyez fréquemment le texte de sélection de l’événement déclenché, même si vous vous attendez à ce que ObjectDataSource utilise les données mises en cache, cela peut être dû à des contraintes de mémoire. Si la mémoire disponible est insuffisante, les données ajoutées au cache par ObjectDataSource peuvent avoir été nettoyées. Si le ObjectDataSource ne semble pas mettre en cache correctement les données ou met en cache les données de manière sporadique, fermez certaines applications pour libérer de la mémoire, puis réessayez.

La figure 12 illustre le flux de travail de mise en cache de ObjectDataSource s. Lorsque le texte de sélection de l’événement déclenché s’affiche sur votre écran, c’est parce que les données n’étaient pas dans le cache et devaient être récupérées à partir de l’objet sous-jacent. Toutefois, lorsque ce texte est manquant, il s’agit du fait que les données étaient disponibles à partir du cache. Lorsque les données sont retournées à partir du cache, il n’y a aucun appel à l’objet sous-jacent et, par conséquent, aucune requête de base de données n’est exécutée.

![ObjectDataSource stocke et récupère ses données à partir du cache de données](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Figure 12**: l’ObjectDataSource stocke et récupère ses données à partir du cache de données

Chaque application ASP.NET a sa propre instance de cache de données qui est partagée entre toutes les pages et les visiteurs. Cela signifie que les données stockées dans le cache de données par l’ObjectDataSource sont également partagées entre tous les utilisateurs qui visitent la page. Pour vérifier cela, ouvrez la page `ObjectDataSource.aspx` dans un navigateur. Lors de la première visite de la page, le texte de sélection de l’événement sélectionné s’affiche (en supposant que les données ajoutées au cache par les tests précédents ont été supprimées, par la suite). Ouvrez une deuxième instance du navigateur et copiez et collez l’URL de la première instance du navigateur à la seconde. Dans la deuxième instance du navigateur, le texte de sélection de l’événement qui est déclenché n’est pas affiché parce qu’il utilise les mêmes données mises en cache que la première.

Lors de l’insertion de ses données récupérées dans le cache, ObjectDataSource utilise une valeur de clé de cache qui comprend : les valeurs de propriété `CacheDuration` et `CacheExpirationPolicy` ; type de l’objet métier sous-jacent utilisé par l’ObjectDataSource, qui est spécifié via la [propriété`TypeName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, dans cet exemple); la valeur de la propriété `SelectMethod` et le nom et les valeurs des paramètres dans la collection `SelectParameters` ; et les valeurs de ses propriétés `StartRowIndex` et `MaximumRows`, qui sont utilisées lors de l’implémentation de la [pagination personnalisée.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

L’élaboration de la valeur de clé de cache en tant que combinaison de ces propriétés garantit une entrée de cache unique à mesure que ces valeurs changent. Par exemple, dans les didacticiels précédents, nous avons examiné à l’aide de la classe `ProductsBLL` s `GetProductsByCategoryID(categoryID)`, qui retourne tous les produits pour une catégorie spécifiée. Un utilisateur peut accéder à la page et afficher les boissons, qui a une `CategoryID` de 1. Si le ObjectDataSource a mis en cache ses résultats sans tenir compte des valeurs de `SelectParameters`, lorsqu’un autre utilisateur a accédé à la page pour afficher les condiments alors que les produits de boissons se trouvaient dans le cache, ils voient les produits de boisson mis en cache plutôt que les condiments. En faisant varier la clé de cache par ces propriétés, qui incluent les valeurs du `SelectParameters`, ObjectDataSource conserve une entrée de cache distincte pour les boissons et les condiments.

## <a name="stale-data-concerns"></a>Problèmes liés aux données périmées

ObjectDataSource supprime automatiquement ses éléments du cache lorsque l’une de ses méthodes `Insert`, `Update`ou `Delete` est appelée. Cela permet de se protéger contre les données périmées en effaçant les entrées du cache lorsque les données sont modifiées via la page. Toutefois, il est possible pour un ObjectDataSource qui utilise la mise en cache de toujours afficher des données obsolètes. Dans le cas le plus simple, cela peut être dû à la modification directe des données dans la base de données. Peut-être qu’un administrateur de base de données a exécuté un script qui modifie certains des enregistrements de la base de données.

Ce scénario peut également être dérouler de façon plus subtile. Alors que ObjectDataSource supprime ses éléments du cache quand l’une de ses méthodes de modification de données est appelée, les éléments mis en cache supprimés concernent la combinaison de valeurs de propriété de l’ObjectDataSource s (`CacheDuration`, `TypeName`, `SelectMethod`, etc.). Si vous avez deux ObjectDataSources qui utilisent des `SelectMethods` ou des `SelectParameters`différents, mais que vous pouvez toujours mettre à jour les mêmes données, un ObjectDataSource peut mettre à jour une ligne et invalider ses propres entrées de cache, mais la ligne correspondante pour la deuxième ObjectDataSource sera toujours traitée à partir du cache. Je vous encourage à créer des pages pour présenter cette fonctionnalité. Créez une page qui affiche un GridView modifiable qui extrait ses données d’un ObjectDataSource qui utilise la mise en cache et qui est configurée pour obtenir des données de la méthode de `GetProducts()` de la classe `ProductsBLL`. Ajoutez un autre GridView modifiable et ObjectDataSource à cette page (ou un autre), mais pour ce deuxième ObjectDataSource, utilisez la méthode `GetProductsByCategoryID(categoryID)`. Étant donné que les deux propriétés de `SelectMethod` ObjectDataSources diffèrent, elles ont chacune leurs propres valeurs mises en cache. Si vous modifiez un produit dans une grille, la prochaine fois que vous lierez à nouveau les données à l’autre grille (par pagination, tri, etc.), il servira toujours les anciennes données mises en cache et ne reflétera pas la modification apportée à partir de l’autre grille.

En bref, utilisez uniquement des expirations basées sur le temps si vous souhaitez avoir le potentiel des données périmées et utiliser des expirations plus courtes pour les scénarios où l’actualisation des données est importante. Si les données périmées ne sont pas acceptables, renoncez à la mise en cache ou utilisez des dépendances de cache SQL (en supposant que les données de base de données que vous mettez à nouveau en cache Nous allons explorer les dépendances de cache SQL dans un prochain didacticiel.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons examiné les fonctionnalités de mise en cache intégrées à ObjectDataSource. En définissant simplement quelques propriétés, nous pouvons demander à ObjectDataSource de mettre en cache les résultats retournés par le `SelectMethod` spécifié dans le cache de données ASP.NET. Les propriétés `CacheDuration` et `CacheExpirationPolicy` indiquent la durée de mise en cache de l’élément et s’il s’agit d’une expiration absolue ou décalée. La propriété `CacheKeyDependency` associe toutes les entrées de cache ObjectDataSource dans une dépendance de cache existante. Cela peut être utilisé pour supprimer les entrées ObjectDataSource du cache avant que l’expiration basée sur la durée soit atteinte, et est généralement utilisé avec les dépendances de cache SQL.

Dans la mesure où ObjectDataSource met simplement en cache ses valeurs dans le cache de données, nous pourrions répliquer les fonctionnalités ObjectDataSource s intégrées par programmation. Il n’a pas de sens pour effectuer cette opération au niveau de la couche de présentation, puisque l’ObjectDataSource offre cette fonctionnalité dès le moment, mais nous pouvons implémenter des fonctionnalités de mise en cache dans une couche distincte de l’architecture. Pour ce faire, nous devons répéter la même logique que celle utilisée par ObjectDataSource. Nous allons découvrir comment travailler par programmation avec le cache de données dans l’architecture de notre prochain didacticiel.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Mise en cache ASP.NET : techniques et meilleures pratiques](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guide d’architecture de mise en cache pour les applications .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Mise en cache de sortie dans ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](caching-data-in-the-architecture-cs.md)
