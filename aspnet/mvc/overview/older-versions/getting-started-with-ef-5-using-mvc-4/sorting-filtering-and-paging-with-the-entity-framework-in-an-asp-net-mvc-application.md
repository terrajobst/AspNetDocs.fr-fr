---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Tri, filtrage et pagination avec le Entity Framework dans une application MVC ASP.NET (3 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b1ddb70805dcb07fb60eea895ff572c054bde5c6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595225"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Tri, filtrage et pagination avec le Entity Framework dans une application MVC ASP.NET (3 sur 10)

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels depuis le début ou [Télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et démarrer ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [Téléchargez le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire le problème. Vous pouvez généralement trouver la solution au problème en comparant votre code au code terminé. Pour obtenir des erreurs courantes et comment les résoudre, consultez [Erreurs et solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Dans le didacticiel précédent, vous avez implémenté un ensemble de pages Web pour les opérations CRUD de base pour les entités `Student`. Dans ce didacticiel, vous allez ajouter des fonctionnalités de tri, de filtrage et de pagination à la page d’index des **étudiants** . Vous allez également créer une page qui effectue un regroupement simple.

L’illustration suivante montre à quoi ressemblera la page quand vous aurez terminé. Les en-têtes des colonnes sont des liens sur lesquels l’utilisateur peut cliquer pour trier selon les colonnes. Cliquer de façon répétée sur un en-tête de colonne permet de changer l’ordre de tri (croissant ou décroissant).

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Ajouter des liens de tri de colonne à la page d’index des étudiants

Pour ajouter le tri à la page d’index des étudiants, vous allez modifier la méthode `Index` du contrôleur `Student` et ajouter du code à la vue `Student` index.

### <a name="add-sorting-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de tri à la méthode index

Dans *Controllers\StudentController.cs*, remplacez la méthode `Index` par le code suivant :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ce code reçoit un paramètre `sortOrder` à partir de la chaîne de requête dans l’URL. La valeur de la chaîne de requête est fournie par ASP.NET MVC en tant que paramètre à la méthode d’action. Le paramètre sera la chaîne « Name » ou « Date », éventuellement suivie d’un trait de soulignement et de la chaîne « desc » pour spécifier l’ordre décroissant. L'ordre de tri par défaut est le tri croissant.

La première fois que la page d’index est demandée, il n’y a pas de chaîne de requête. Les étudiants sont affichés dans l’ordre croissant par `LastName`, qui est la valeur par défaut établie par le cas de chute dans l’instruction `switch`. Quand l’utilisateur clique sur un lien hypertexte d’en-tête de colonne, la valeur `sortOrder` appropriée est fournie dans la chaîne de requête.

Les deux variables `ViewBag` sont utilisées afin que la vue puisse configurer les liens hypertexte d’en-tête de colonne avec les valeurs de chaîne de requête appropriées :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il s’agit d’instructions ternaires. La première spécifie que si le paramètre `sortOrder` a la valeur null ou est vide, `ViewBag.NameSortParm` doit avoir la valeur « Name\_DESC ». dans le cas contraire, elle doit être définie sur une chaîne vide. Ces deux instructions permettent à l’affichage de définir les liens hypertexte d’en-tête de colonne comme suit :

| Ordre de tri actuel | Lien hypertexte Nom de famille | Lien hypertexte Date |
| --- | --- | --- |
| Nom de famille croissant | descending | ascending |
| Nom de famille décroissant | ascending | ascending |
| Date croissante | ascending | descending |
| Date décroissante | ascending | ascending |

La méthode utilise [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) pour spécifier la colonne à utiliser pour le tri. Le code crée une variable [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) avant l’instruction `switch`, la modifie dans l’instruction `switch` et appelle la méthode `ToList` après l’instruction `switch`. Lorsque vous créez et modifiez des variables `IQueryable`, aucune requête n’est envoyée à la base de données. La requête n’est pas exécutée tant que vous n’avez pas converti l’objet `IQueryable` en collection en appelant une méthode telle que `ToList`. Par conséquent, ce code génère une requête unique qui n’est pas exécutée jusqu’à l’instruction `return View`.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Ajouter des liens hypertexte d’en-tête de colonne à la vue d’index des étudiants

Dans *Views\Student\Index.cshtml*, remplacez les éléments `<tr>` et `<th>` de la ligne d’en-tête par le code en surbrillance :

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Ce code utilise les informations contenues dans les propriétés de `ViewBag` pour définir des liens hypertexte avec les valeurs de chaîne de requête appropriées.

Exécutez la page et cliquez sur les en-têtes de colonne **Last Name** et **Date d’inscription** pour vérifier que le tri fonctionne.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Une fois que vous avez cliqué sur l’en-tête **nom** , les étudiants sont affichés dans l’ordre décroissant des noms.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Ajouter une zone de recherche à la page d’index des étudiants

Pour ajouter le filtrage à la page d’index des étudiants, vous allez ajouter une zone de texte et un bouton d’envoi à la vue et apporter les modifications correspondantes dans la méthode `Index`. La zone de texte vous permet d’entrer une chaîne à rechercher dans les champs de prénom et de nom.

### <a name="add-filtering-functionality-to-the-index-method"></a>Ajout d’une fonctionnalité de filtrage à la méthode index

Dans *Controllers\StudentController.cs*, remplacez la méthode `Index` par le code suivant (les modifications sont mises en surbrillance) :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Vous avez ajouté un paramètre `searchString` à la méthode `Index`. Vous avez également ajouté à l’instruction LINQ une clause `where` qui sélectionne uniquement les étudiants dont le prénom ou le nom contient la chaîne recherchée. La valeur de la chaîne de recherche est reçue à partir d’une zone de texte que vous ajouterez à la vue index. L’instruction qui ajoute la clause [Where](https://msdn.microsoft.com/library/bb535040.aspx) est exécutée uniquement s’il existe une valeur à rechercher.

> [!NOTE]
> Dans de nombreux cas, vous pouvez appeler la même méthode sur un jeu d’entités Entity Framework ou en tant que méthode d’extension sur une collection en mémoire. Les résultats sont normalement identiques, mais dans certains cas, ils peuvent être différents. Par exemple, l’implémentation .NET Framework de la méthode `Contains` retourne toutes les lignes lorsque vous lui transmettez une chaîne vide, mais le fournisseur Entity Framework pour SQL Server Compact 4,0 retourne zéro ligne pour les chaînes vides. Par conséquent, le code de l’exemple (en plaçant l’instruction `Where` à l’intérieur d’une instruction `if`) garantit que vous obteniez les mêmes résultats pour toutes les versions de SQL Server. En outre, l’implémentation .NET Framework de la méthode `Contains` effectue une comparaison respectant la casse par défaut, mais Entity Framework fournisseurs de SQL Server effectuent des comparaisons qui ne respectent pas la casse par défaut. Par conséquent, l’appel de la méthode `ToUpper` pour que le test ne respecte pas explicitement la casse, garantit que les résultats ne changent pas lorsque vous modifiez ultérieurement le code pour utiliser un référentiel, qui retourne une collection `IEnumerable` au lieu d’un objet `IQueryable`. (Lorsque vous appelez la méthode `Contains` sur une collection `IEnumerable`, vous obtenez l’implémentation du .NET Framework ; lorsque vous l’appelez sur un objet `IQueryable`, vous obtenez l’implémentation du fournisseur de base de données.)

### <a name="add-a-search-box-to-the-student-index-view"></a>Ajouter une zone de recherche à l’affichage d’index des étudiants

Dans *Views\Student\Index.cshtml*, ajoutez le code en surbrillance juste avant la balise d’ouverture `table` pour créer une légende, une zone de texte et un bouton de **recherche** .

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Exécutez la page, entrez une chaîne de recherche, puis cliquez sur **Rechercher** pour vérifier que le filtrage fonctionne.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Notez que l’URL ne contient pas la chaîne de recherche « an », ce qui signifie que si vous ajoutez cette page à un signet, vous n’obtiendrez pas la liste filtrée lorsque vous utiliserez le signet. Vous allez modifier le bouton de **recherche** afin d’utiliser des chaînes de requête pour les critères de filtre plus loin dans le didacticiel.

## <a name="add-paging-to-the-students-index-page"></a>Ajouter la pagination à la page d’index des étudiants

Pour ajouter la pagination à la page d’index des étudiants, vous allez commencer par installer le package NuGet **PagedList. Mvc** . Ensuite, vous allez apporter des modifications supplémentaires à la méthode `Index` et ajouter des liens de pagination à la vue `Index`. **PagedList. Mvc** est l’un des nombreux bons packages de pagination et de tri pour ASP.NET MVC, et son utilisation ici est uniquement prévue comme un exemple, et non comme une recommandation pour celui-ci sur d’autres options. L’illustration suivante montre les liens de pagination.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installer le package NuGet PagedList. MVC

Le package NuGet **PagedList. Mvc** installe automatiquement le package **PagedList** en tant que dépendance. Le package **PagedList** installe un type de collection `PagedList` et des méthodes d’extension pour les collections `IQueryable` et `IEnumerable`. Les méthodes d’extension créent une seule page de données dans une collection `PagedList` à partir de votre `IQueryable` ou `IEnumerable`, et la collection `PagedList` fournit plusieurs propriétés et méthodes qui facilitent la pagination. Le package **PagedList. Mvc** installe un programme d’assistance de pagination qui affiche les boutons de pagination.

Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** , puis **gérer les packages NuGet pour la solution**.

Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur l’onglet **en ligne** sur la gauche, puis entrez « paginé » dans la zone de recherche. Quand vous voyez le package **PagedList. Mvc** , cliquez sur **installer**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Dans la zone **Sélectionner des projets** , cliquez sur **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de pagination à la méthode index

Dans *Controllers\StudentController.cs*, ajoutez une instruction `using` pour l’espace de noms `PagedList` :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Remplacez la méthode `Index` par le code suivant :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ce code ajoute un paramètre `page`, un paramètre d’ordre de tri actuel et un paramètre de filtre actuel à la signature de méthode, comme illustré ici :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La première fois que la page s’affiche, ou si l’utilisateur n’a pas cliqué sur un lien de changement de page ni de tri, tous les paramètres sont Null. Si l’utilisateur clique sur un lien de pagination, la variable `page` contient le numéro de page à afficher.

`A ViewBag` propriété fournit la vue avec l’ordre de tri actuel, car elle doit être incluse dans les liens de pagination afin de conserver l’ordre de tri identique lors de la pagination :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Une autre propriété, `ViewBag.CurrentFilter`, fournit la vue avec la chaîne de filtre actuelle. Cette valeur doit être incluse dans les liens de changement de page pour que les paramètres de filtre soient conservés lors du changement de page, et elle doit être restaurée dans la zone de texte lorsque la page est réaffichée. Si la chaîne de recherche est modifiée au cours du changement de page, la page doit être réinitialisée à 1, car le nouveau filtre peut entraîner l’affichage de données différentes. La chaîne de recherche est modifiée lorsqu’une valeur est entrée dans la zone de texte et que le bouton Envoyer est enfoncé. Dans ce cas, le paramètre `searchString` n’a pas la valeur null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

À la fin de la méthode, la méthode d’extension `ToPagedList` sur l’objet des élèves `IQueryable` convertit la requête Student en une seule page d’élèves dans un type de collection qui prend en charge la pagination. Cette seule page d’élèves est ensuite transmise à la vue :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

La méthode `ToPagedList` accepte un numéro de page. Les deux points d’interrogation représentent l' [opérateur de fusion Null](https://msdn.microsoft.com/library/ms173224.aspx). L’opérateur de fusion Null définit une valeur par défaut pour un type nullable ; l’expression `(page ?? 1)` indique de renvoyer la valeur de `page` si elle a une valeur, ou de renvoyer 1 si `page` a la valeur Null.

### <a name="add-paging-links-to-the-student-index-view"></a>Ajouter des liens de pagination à la vue d’index des étudiants

Dans *Views\Student\Index.cshtml*, remplacez le code existant par le code suivant :

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

L’instruction `@model` en haut de la page spécifie que la vue obtient désormais un objet `PagedList` à la place d’un objet `List`.

L’instruction `using` pour `PagedList.Mvc` donne accès au programme d’assistance MVC pour les boutons de pagination.

Le code utilise une surcharge de [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) qui lui permet de spécifier [FormMethod. obtient](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Le [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) par défaut soumet les données de formulaire avec une publication, ce qui signifie que les paramètres sont transmis dans le corps du message http et non dans l’URL en tant que chaînes de requête. Lorsque vous spécifiez HTTP GET, les données de formulaire sont transmises dans l’URL sous forme de chaînes de requête, ce qui permet aux utilisateurs d’ajouter l’URL aux favoris. Les [recommandations du W3C relatives à l’utilisation de http](http://www.w3.org/2001/tag/doc/whenToUseGet.html) -spécifient que vous devez utiliser l’instruction obtenir quand l’action n’entraîne pas de mise à jour.

La zone de texte est initialisée avec la chaîne de recherche actuelle. ainsi, lorsque vous cliquez sur une nouvelle page, vous pouvez voir la chaîne de recherche actuelle.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Les liens d’en-tête de colonne utilisent la chaîne de requête pour transmettre la chaîne de recherche actuelle au contrôleur afin que l’utilisateur puisse trier les résultats de filtrage :

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

La page actuelle et le nombre total de pages sont affichées.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

S’il n’y a aucune page à afficher, « la page 0 de 0 » s’affiche. (Dans ce cas, le numéro de page est supérieur au nombre de pages, car `Model.PageNumber` a la valeur 1, et `Model.PageCount` est égal à 0.)

Les boutons de pagination sont affichés par le programme d’assistance `PagedListPager` :

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Le programme d’assistance `PagedListPager` fournit un certain nombre d’options que vous pouvez personnaliser, y compris les URL et les styles. Pour plus d’informations, consultez [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) sur le site github.

Exécutez la page.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Cliquez sur les liens de changement de page dans différents ordres de tri pour vérifier que le changement de page fonctionne. Ensuite, entrez une chaîne de recherche et essayez de changer de page à nouveau pour vérifier que le changement de page fonctionne correctement avec le tri et le filtrage.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Créer une page about qui affiche les statistiques des élèves

Pour la page à propos de du site Web Contoso University, vous allez afficher le nombre d’étudiants inscrits pour chaque date d’inscription. Cela nécessite un regroupement et des calculs simples sur les groupes. Pour ce faire, vous devez effectuer les opérations suivantes :

- Créez une classe de modèle de vue pour les données que vous devez transmettre à la vue.
- Modifiez la méthode `About` dans le contrôleur de `Home`.
- Modifiez la vue `About`.

### <a name="create-the-view-model"></a>Créer le modèle de vue

Créez un dossier *ViewModels* . Dans ce dossier, ajoutez un fichier de classe *EnrollmentDateGroup.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modifier le contrôleur Home

Dans *HomeController.cs*, ajoutez les instructions `using` suivantes en haut du fichier :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Ajoutez une variable de classe pour le contexte de base de données immédiatement après l’accolade ouvrante de la classe :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Remplacez la méthode `About` par le code suivant :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

L’instruction LINQ regroupe les entités student par date d’inscription, calcule le nombre d’entités dans chaque groupe, et stocke les résultats dans une collection d’objets de modèle d’affichage `EnrollmentDateGroup`.

Ajoutez une méthode de `Dispose` :

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modifier la vue About

Remplacez le code du fichier *Views\Home\About.cshtml* par le code suivant :

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Exécutez l’application et cliquez sur le lien **à propos** de. Le nombre d’étudiants pour chaque date d’inscription s’affiche dans une table.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Facultatif : déployer l’application sur Windows Azure

Jusqu’à présent, votre application s’exécutait localement dans IIS Express sur votre ordinateur de développement. Pour qu’il soit disponible pour d’autres personnes à utiliser sur Internet, vous devez le déployer sur un fournisseur d’hébergement Web. Dans cette section facultative du didacticiel, vous allez le déployer sur un site Web Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Utilisation de Migrations Code First pour déployer la base de données

Pour déployer la base de données, vous utiliserez Migrations Code First. Lorsque vous créez le profil de publication que vous utilisez pour configurer les paramètres de déploiement à partir de Visual Studio, vous activez une case à cocher intitulée **exécuter migrations code First (s’exécute au démarrage de l’application)** . Ce paramètre fait que le processus de déploiement configure automatiquement le fichier *Web. config* de l’application sur le serveur de destination afin que code First utilise la classe d’initialiseur `MigrateDatabaseToLatestVersion`.

Visual Studio ne fait rien avec la base de données pendant le processus de déploiement. Lorsque l’application déployée accède à la base de données pour la première fois après le déploiement, Code First crée automatiquement la base de données ou met à jour le schéma de base de données vers la dernière version. Si l’application implémente une méthode de migration `Seed`, la méthode s’exécute après la création de la base de données ou lors de la mise à jour du schéma.

Votre méthode de migration `Seed` insère les données de test. Si vous déployez dans un environnement de production, vous devez modifier la méthode `Seed` afin qu’elle insère uniquement les données que vous souhaitez insérer dans votre base de données de production. Par exemple, dans votre modèle de données actuel, vous souhaiterez peut-être avoir des cours réels, mais des étudiants fictifs dans la base de données de développement. Vous pouvez écrire une méthode de `Seed` pour charger les deux en développement, puis commenter les étudiants fictifs avant de procéder au déploiement en production. Ou vous pouvez écrire une méthode de `Seed` pour charger uniquement les cours, puis entrer manuellement les étudiants fictifs dans la base de données de test à l’aide de l’interface utilisateur de l’application.

### <a name="get-a-windows-azure-account"></a>Obtenir un compte Windows Azure

Vous aurez besoin d’un compte Windows Azure. Si vous n’en avez pas déjà un, vous pouvez créer un compte d’évaluation gratuit en quelques minutes. Pour plus d’informations, consultez [version d’évaluation gratuite de Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Créer un site Web et une base de données SQL dans Windows Azure

Votre site Web Windows Azure s’exécute dans un environnement d’hébergement partagé, ce qui signifie qu’il s’exécute sur des machines virtuelles partagées avec d’autres clients Windows Azure. Un environnement d’hébergement partagé est un moyen économique de commencer dans le Cloud. Plus tard, si votre trafic Web augmente, l’application peut évoluer pour répondre à la nécessité en exécutant sur des machines virtuelles dédiées. Si vous avez besoin d’une architecture plus complexe, vous pouvez migrer vers un service Cloud Windows Azure. Les services Cloud s’exécutent sur des machines virtuelles dédiées que vous pouvez configurer en fonction de vos besoins.

Windows Azure SQL Database est un service de base de données relationnelle basé sur le Cloud qui repose sur les technologies SQL Server. Les outils et les applications qui fonctionnent avec SQL Server fonctionnent également avec SQL Database.

1. Dans le [portail de gestion Windows Azure](https://manage.windowsazure.com/), cliquez sur **sites Web** dans l’onglet de gauche, puis sur **nouveau**.

    ![Bouton nouveau dans Portail de gestion](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Cliquez sur **création personnalisée**.

    ![Créer avec le lien de base de données dans Portail de gestion](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   Le **nouveau site Web-Assistant Création personnalisée** s’ouvre.
3. Dans l’étape **nouveau site Web** de l’Assistant, entrez une chaîne dans la zone **URL** à utiliser comme URL unique pour votre application. L’URL complète se compose de ce que vous entrez ici et du suffixe qui s’affiche en regard de la zone de texte. L’illustration montre « ConU », mais cette URL est probablement utilisée. vous devrez donc en choisir une autre.

    ![Créer avec le lien de base de données dans Portail de gestion](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. Dans la liste déroulante **région** , choisissez une région proche de vous. Ce paramètre spécifie le centre de données dans lequel votre site Web s’exécutera.
5. Dans la liste déroulante **base de données** , choisissez **créer une base de données SQL gratuite de 20 Mo**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. Dans le **nom**de la chaîne de connexion de base de base, entrez *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Cliquez sur la flèche qui pointe vers la droite au bas de la zone. L’Assistant passe à l’étape **paramètres de base de données** .
8. Dans la zone **nom** , entrez *ContosoUniversityDB*.
9. Dans la zone **serveur** , sélectionnez **nouveau SQL Database serveur**. Si vous avez déjà créé un serveur, vous pouvez également sélectionner ce serveur dans la liste déroulante.
10. Entrez un **nom de connexion** et un **mot de passe**d’administrateur. Si vous avez sélectionné **nouveau SQL Database serveur** , vous n’entrez pas de nom et de mot de passe existants ici, vous entrez un nouveau nom et un mot de passe que vous définissez maintenant pour une utilisation ultérieure lorsque vous accédez à la base de données. Si vous avez sélectionné un serveur que vous avez créé précédemment, vous devez entrer les informations d’identification de ce serveur. Pour ce didacticiel, vous n’activez pas la case à cocher ***avancé*** . Les options ***avancées*** vous permettent de définir le [classement](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)de base de données.
11. Choisissez la même **région** que celle que vous avez choisie pour le site Web.
12. Cliquez sur la coche en bas à droite de la zone pour indiquer que vous avez terminé.   
  
    ![Étape des paramètres de base de données du nouveau site Web-Assistant créer avec la base de données](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    L’illustration suivante montre l’utilisation d’un SQL Server et d’une connexion existants.   
  
    ![Étape des paramètres de base de données du nouveau site Web-Assistant créer avec la base de données](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    La Portail de gestion retourne à la page sites Web, et la colonne **État** indique que le site est en cours de création. Après un certain temps (généralement inférieur à une minute), la colonne **État** indique que le site a été créé avec succès. Dans la barre de navigation de gauche, le nombre de sites que vous avez dans votre compte apparaît en regard de l’icône **sites Web** , et le nombre de bases de données s’affiche en regard de l’icône **bases de données SQL** .

## <a name="deploy-the-application-to-windows-azure"></a>Déployer l’application sur Windows Azure

1. Dans Visual Studio, cliquez avec le bouton droit sur le projet dans **Explorateur de solutions** puis sélectionnez **publier** dans le menu contextuel.  
  
    ![Publier dans le menu contextuel du projet](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. Sous l’onglet **Profil** de l’Assistant **publier le site Web** , cliquez sur **Importer**.  
  
    ![Importation de paramètres de publication](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Si vous n’avez pas encore ajouté votre abonnement Windows Azure dans Visual Studio, procédez comme suit. Dans ces étapes, vous ajoutez votre abonnement afin que la liste déroulante sous **Importer à partir d’un site Web Windows Azure** inclue votre site Web.

    a. Dans la boîte de dialogue **Importer un profil de publication** , cliquez sur **Importer à partir d’un site Web Windows Azure**, puis cliquez sur **Ajouter un abonnement Windows Azure**.

    ![Ajouter un abonnement Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. Dans la boîte de dialogue **importer des abonnements Windows Azure** , cliquez sur **Télécharger le fichier d’abonnement**.

    ![Télécharger le fichier d’abonnement](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Dans la fenêtre de votre navigateur, enregistrez le fichier *. publishsettings* .

    ![Télécharger le fichier. publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Sécurité : le fichier *publishsettings* contient vos informations d’identification (non codées) utilisées pour gérer vos abonnements et services Windows Azure. La meilleure pratique de sécurité pour ce fichier est de le stocker temporairement en dehors de vos répertoires sources (par exemple, dans le dossier *bibliothèques \ Documents* ), puis de le supprimer une fois l’importation terminée. Un utilisateur malveillant qui accède au fichier `.publishsettings` peut modifier, créer et supprimer vos services Windows Azure.

    d. Dans la boîte de dialogue **importer des abonnements Windows Azure** , cliquez sur **Parcourir** et accédez au fichier *. publishsettings* .

    ![Télécharger Sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Cliquez sur **Importer**.

    ![importer](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. Dans la boîte de dialogue **Importer un profil de publication** , sélectionnez **Importer à partir d’un site Web Windows Azure**, sélectionnez votre site Web dans la liste déroulante, puis cliquez sur **OK**.  
  
    ![Importer le profil de publication](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. Dans l’onglet **connexion** , cliquez sur **valider la connexion** pour vous assurer que les paramètres sont corrects.  
  
    ![Valider la connexion](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Une fois la connexion validée, une coche verte s’affiche en regard du bouton **valider la connexion** . Cliquez sur **Next**.  
  
    ![Connexion correctement validée](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Ouvrez la liste déroulante **chaîne de connexion distante** sous **SchoolContext** et sélectionnez la chaîne de connexion pour la base de données que vous avez créée.
8. Sélectionnez **exécuter migrations code First (s’exécute au démarrage de l’application)** .
9. Désactivez l’option **utiliser cette chaîne de connexion au moment** de l’exécution pour le **userContext (DefaultConnection)** , car cette application n’utilise pas la base de données d’appartenance.   
  
    ![Onglet Paramètres](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Cliquez sur **Next**.
11. Dans l’onglet **Aperçu** , cliquez sur **Démarrer l’aperçu**.  
  
    ![Bouton StartPreview dans l’onglet d’aperçu](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    L’onglet affiche une liste des fichiers qui seront copiés sur le serveur. L’affichage de l’aperçu n’est pas nécessaire pour publier l’application, mais il s’agit d’une fonction utile à connaître. Dans ce cas, vous n’avez rien à faire avec la liste des fichiers affichés. La prochaine fois que vous déployez cette application, seuls les fichiers qui ont été modifiés seront répertoriés dans cette liste.  
  
    ![Sortie du fichier StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Cliquez sur **Publier**.  
    Visual Studio commence le processus de copie des fichiers sur le serveur Windows Azure.
13. La fenêtre **sortie** indique les actions de déploiement effectuées et signale la réussite du déploiement.  
  
    ![Génération de rapports de la fenêtre Sortie réussie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Une fois le déploiement réussi, le navigateur par défaut s’ouvre automatiquement à l’URL du site Web déployé.  
    L’application que vous avez créée s’exécute maintenant dans le Cloud. Cliquez sur l’onglet students.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

À ce stade, votre base de données *SchoolContext* a été créée dans le Azure SQL Database Windows, car vous avez sélectionné **exécuter migrations code First (s’exécute au démarrage de l’application)** . Le fichier *Web. config* du site Web déployé a été modifié afin que l’initialiseur [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) s’exécute la première fois que votre code lit ou écrit des données dans la base de données (qui s’est produite lorsque vous avez sélectionné l’onglet **students** ) :

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Le processus de déploiement a également créé une nouvelle chaîne de connexion *(SchoolContext\_DatabasePublish*) pour migrations code First à utiliser pour la mise à jour du schéma de base de données et l’amorçage de la base de données.

![Chaîne de connexion Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

La chaîne de connexion *DefaultConnection* est pour la base de données d’appartenance (que nous n’utilisons pas dans ce didacticiel). La chaîne de connexion *SchoolContext* est pour la base de données ContosoUniversity.

Vous pouvez trouver la version déployée du fichier Web. config sur votre propre ordinateur dans *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Vous pouvez accéder au fichier *Web. config* déployé lui-même à l’aide de FTP. Pour obtenir des instructions, consultez [déploiement Web ASP.net à l’aide de Visual Studio : déploiement d’une mise à jour de code](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Suivez les instructions qui commencent par « pour utiliser un outil FTP, vous avez besoin de trois choses : l’URL FTP, le nom d’utilisateur et le mot de passe ».

> [!NOTE]
> L’application Web n’implémente pas la sécurité, de sorte que toute personne qui trouve l’URL peut modifier les données. Pour obtenir des instructions sur la façon de sécuriser le site Web, consultez [déployer une application ASP.NET MVC sécurisée avec appartenance, OAuth et SQL Database sur un site Web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Vous pouvez empêcher d’autres personnes d’utiliser le site à l’aide de Windows Azure Portail de gestion ou **Explorateur de serveurs** dans Visual Studio pour arrêter le site.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Initialiseurs Code First

Dans la section déploiement, vous avez vu l’initialiseur [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) utilisé. Code First fournit également d’autres initialiseurs que vous pouvez utiliser, y compris [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (valeur par défaut), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) et [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). L’initialiseur de `DropCreateAlways` peut être utile pour configurer des conditions pour les tests unitaires. Vous pouvez également écrire vos propres initialiseurs, et vous pouvez appeler un initialiseur de manière explicite si vous ne souhaitez pas attendre la lecture ou l’écriture de l’application dans la base de données. Pour obtenir une explication complète des initialiseurs, consultez le chapitre 6 de la [Entity Framework de programmation livre : code First](http://shop.oreilly.com/product/0636920022220.do) par Julie Lerman et Rowan Miller.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez vu comment créer un modèle de données et implémenter des fonctionnalités de base de CRUD, de tri, de filtrage, de pagination et de regroupement. Dans le didacticiel suivant, vous allez commencer à examiner des rubriques plus avancées en développant le modèle de données.

Vous trouverez des liens vers d’autres ressources de Entity Framework dans le [plan de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Suivant](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
