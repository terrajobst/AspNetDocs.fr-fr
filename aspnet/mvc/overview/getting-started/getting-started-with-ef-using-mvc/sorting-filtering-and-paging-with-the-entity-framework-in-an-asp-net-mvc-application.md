---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutoriel : Ajouter le tri, le filtrage et la pagination avec le Entity Framework dans une application MVC ASP.NET | Microsoft Docs'
author: tdykstra
description: Dans ce didacticiel, vous allez ajouter des fonctionnalités de tri, de filtrage et de pagination à la page d’index des **étudiants** . Vous créez également une page de regroupement simple.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: d9fadc12aa83a8095f364cf39e5376243a7d0670
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810754"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Tutoriel : Ajouter le tri, le filtrage et la pagination avec le Entity Framework dans une application MVC ASP.NET

Dans le [didacticiel précédent](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), vous avez implémenté un ensemble de pages Web pour les opérations CRUD `Student` de base pour les entités. Dans ce didacticiel, vous allez ajouter des fonctionnalités de tri, de filtrage et de pagination à la page d’index des **étudiants** . Vous créez également une page de regroupement simple.

L’illustration suivante montre à quoi ressemblera la page une fois que vous aurez terminé. Les en-têtes des colonnes sont des liens sur lesquels l’utilisateur peut cliquer pour trier selon les colonnes. Cliquer de façon répétée sur un en-tête de colonne permet de changer l’ordre de tri (croissant ou décroissant).

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter des liens de tri de colonne
> * Ajouter une zone Rechercher
> * Ajouter la pagination
> * Créer une page À propos

## <a name="prerequisites"></a>Prérequis

* [Implémentation de la fonctionnalité CRUD de base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Ajouter des liens de tri de colonne

Pour ajouter le tri à la page `Index` `Student` d’index des étudiants, vous allez modifier la méthode du contrôleur et ajouter du `Student` code à la vue index.

### <a name="add-sorting-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de tri à la méthode index

- Dans *Controllers\StudentController.cs*, remplacez la `Index` méthode par le code suivant:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ce code reçoit un paramètre `sortOrder` à partir de la chaîne de requête dans l’URL. La valeur de la chaîne de requête est fournie par ASP.NET MVC en tant que paramètre à la méthode d’action. Le paramètre est une chaîne «Name» ou «date», éventuellement suivie d’un trait de soulignement et de la chaîne «desc» pour spécifier l’ordre décroissant. L'ordre de tri par défaut est le tri croissant.

La première fois que la page d’index est demandée, il n’y a pas de chaîne de requête. Les étudiants sont affichés dans l’ordre croissant par `LastName`, qui est la valeur par défaut établie par le cas de chute dans l' `switch` instruction. Quand l’utilisateur clique sur un lien hypertexte d’en-tête de colonne, la valeur `sortOrder` appropriée est fournie dans la chaîne de requête.

Les deux `ViewBag` variables sont utilisées afin que la vue puisse configurer les liens hypertexte d’en-tête de colonne avec les valeurs de chaîne de requête appropriées:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il s’agit d’instructions ternaires. La première spécifie que si le `sortOrder` paramètre est null ou vide, `ViewBag.NameSortParm` doit être défini sur «nom\_DESC»; sinon, il doit être défini sur une chaîne vide. Ces deux instructions permettent à la vue de définir les liens hypertexte d’en-tête de colonne comme suit :

| Ordre de tri actuel | Lien hypertexte Nom de famille | Lien hypertexte Date |
| --- | --- | --- |
| Nom de famille croissant | descending | ascending |
| Nom de famille décroissant | croissant | croissant |
| Date par ordre croissant | croissant | décroissant |
| Date par ordre décroissant | croissant | ascending |

La méthode utilise [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) pour spécifier la colonne à utiliser pour le tri. Le code crée une <xref:System.Linq.IQueryable%601> variable avant l' `switch` instruction, la modifie dans l' `switch` instruction et appelle la `ToList` méthode après l' `switch` instruction. Lorsque vous créez et modifiez des variables `IQueryable`, aucune requête n’est envoyée à la base de données. La requête n’est pas exécutée tant que `IQueryable` vous n’avez pas converti l’objet en collection en `ToList`appelant une méthode telle que. Par conséquent, ce code génère une requête unique qui n’est pas exécutée `return View` jusqu’à l’instruction.

Comme alternative à l’écriture d’instructions LINQ différentes pour chaque ordre de tri, vous pouvez créer dynamiquement une instruction LINQ. Pour plus d’informations sur Dynamic LINQ, consultez [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Ajouter des liens hypertexte d’en-tête de colonne à la vue d’index des étudiants

1. Dans *Views\Student\Index.cshtml*, remplacez les `<tr>` éléments `<th>` et pour la ligne d’en-tête par le code en surbrillance:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Ce code utilise les informations dans les `ViewBag` propriétés pour définir des liens hypertexte avec les valeurs de chaîne de requête appropriées.

2. Exécutez la page et cliquez sur les en-têtes de colonne **Last Name** et **Date d’inscription** pour vérifier que le tri fonctionne.

   Une fois que vous avez cliqué sur l’en-tête **nom** , les étudiants sont affichés dans l’ordre décroissant des noms.

## <a name="add-a-search-box"></a>Ajouter une zone Rechercher

Pour ajouter le filtrage à la page d’index des étudiants, vous allez ajouter une zone de texte et un bouton Envoyer à la vue et apporter `Index` les modifications correspondantes dans la méthode. La zone de texte vous permet d’entrer une chaîne à rechercher dans les champs prénom et nom.

### <a name="add-filtering-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de filtrage à la méthode Index

- Dans *Controllers\StudentController.cs*, remplacez la `Index` méthode par le code suivant (les modifications sont mises en surbrillance):

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Le code ajoute un `searchString` paramètre à la `Index` méthode. La valeur de chaîne de recherche est reçue à partir d’une zone de texte que vous ajouterez à la vue Index. Elle ajoute également une `where` clause à l’instruction LINQ qui sélectionne uniquement les étudiants dont le prénom ou le nom contient la chaîne recherchée. L’instruction qui ajoute la <xref:System.Linq.Queryable.Where%2A> clause s’exécute uniquement s’il existe une valeur à rechercher.

> [!NOTE]
> Dans de nombreux cas, vous pouvez appeler la même méthode sur un jeu d’entités Entity Framework ou en tant que méthode d’extension sur une collection en mémoire. Les résultats sont normalement identiques, mais dans certains cas, ils peuvent être différents.
>
> Par exemple, l’implémentation .NET Framework de la `Contains` méthode retourne toutes les lignes quand vous lui transmettez une chaîne vide, mais le fournisseur Entity Framework pour SQL Server Compact 4,0 retourne zéro ligne pour les chaînes vides. Par conséquent, le code de l’exemple ( `Where` en plaçant l' `if` instruction à l’intérieur d’une instruction) permet de s’assurer que vous recevez les mêmes résultats pour toutes les versions de SQL Server. En outre, l’implémentation .NET Framework de `Contains` la méthode effectue une comparaison respectant la casse par défaut, mais Entity Framework les fournisseurs SQL Server effectuent des comparaisons qui ne respectent pas la casse par défaut. Par conséquent, l' `ToUpper` appel de la méthode pour que le test ne respecte pas la casse explicitement garantit que les résultats ne changent pas lorsque vous modifiez ultérieurement le code pour utiliser un référentiel, qui `IEnumerable` retourne une collection au `IQueryable` lieu d’un objet. (Lorsque vous appelez la méthode `Contains` sur une collection `IEnumerable`, vous obtenez l’implémentation du .NET Framework ; lorsque vous l’appelez sur un objet `IQueryable`, vous obtenez l’implémentation du fournisseur de base de données.)
>
> La gestion NULL peut également être différente pour différents fournisseurs de bases de données ou `IQueryable` lorsque vous utilisez un objet comparé à `IEnumerable` quand vous utilisez une collection. Par exemple, dans certains scénarios, `Where` une condition telle `table.Column != 0` que peut `null` ne pas retourner des colonnes ayant comme valeur. Par défaut, EF génère des opérateurs SQL supplémentaires pour faire l’égalité entre les valeurs NULL et le travail dans la base de données, comme il fonctionne en mémoire, mais vous pouvez définir l’indicateur [UseDatabaseNullSemantics](https://docs.microsoft.com/dotnet/api/system.data.entity.infrastructure.dbcontextconfiguration.usedatabasenullsemantics) dans EF6 ou appeler la méthode [UseRelationalNulls](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.infrastructure.relationaldbcontextoptionsbuilder-2.userelationalnulls) dans EF Core à configurez ce comportement.

### <a name="add-a-search-box-to-the-student-index-view"></a>Ajouter une zone de recherche à la vue d’index des étudiants

1. Dans *Views\Student\Index.cshtml*, ajoutez le code en surbrillance juste `table` avant la balise d’ouverture afin de créer une légende, une zone de texte et un bouton de **recherche** .

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Exécutez la page, entrez une chaîne de recherche, puis cliquez sur **Rechercher** pour vérifier que le filtrage fonctionne.

   Notez que l’URL ne contient pas la chaîne de recherche «an», ce qui signifie que si vous ajoutez cette page à un signet, vous n’obtiendrez pas la liste filtrée lorsque vous utiliserez le signet. Cela s’applique également aux liens de tri de colonne, car ils trieront la liste entière. Vous allez modifier le bouton de **recherche** afin d’utiliser des chaînes de requête pour les critères de filtre plus loin dans le didacticiel.

## <a name="add-paging"></a>Ajouter la pagination

Pour ajouter la pagination à la page d’index des étudiants, vous allez commencer par installer le package NuGet **PagedList. Mvc** . Ensuite, vous allez apporter des modifications supplémentaires `Index` à la méthode et ajouter des liens `Index` de pagination à la vue. **PagedList. Mvc** est l’un des nombreux bons packages de pagination et de tri pour ASP.NET MVC, et son utilisation ici est uniquement prévue comme un exemple, et non comme une recommandation pour celui-ci sur d’autres options.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installer le package NuGet PagedList. MVC

Le package NuGet **PagedList. Mvc** installe automatiquement le package **PagedList** en tant que dépendance. Le package **PagedList** installe un `PagedList` type de collection et des méthodes d' `IEnumerable` extension pour les `IQueryable` collections et. Les méthodes d’extension créent une seule page de données dans `PagedList` une collection à partir `IQueryable` de `IEnumerable`votre ou, `PagedList` et la collection fournit plusieurs propriétés et méthodes qui facilitent la pagination. Le package **PagedList. Mvc** installe un programme d’assistance de pagination qui affiche les boutons de pagination.

1. Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** , puis **console du gestionnaire de package**.

2. Dans la fenêtre de la **console du gestionnaire de package** , assurez-vous que la source du **package** est **NuGet.org** et que le **projet par défaut** est **ContosoUniversity**, puis entrez la commande suivante:

   ```text
   Install-Package PagedList.Mvc
   ```

3. Générez le projet.

### <a name="add-paging-functionality-to-the-index-method"></a>Ajouter la fonctionnalité de changement de page à la méthode Index

1. Dans *Controllers\StudentController.cs*, ajoutez une `using` instruction pour l' `PagedList` espace de noms:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Remplacez la méthode `Index` par le code suivant :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Ce code ajoute un `page` paramètre, un paramètre d’ordre de tri actuel et un paramètre de filtre actuel à la signature de méthode:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   La première fois que la page est affichée, ou si l’utilisateur n’a pas cliqué sur un lien de pagination ou de tri, tous les paramètres ont la valeur null. Si l’utilisateur clique sur un lien de `page` pagination, la variable contient le numéro de page à afficher.

   Une `ViewBag` propriété fournit la vue avec l’ordre de tri actuel, car elle doit être incluse dans les liens de pagination afin de conserver l’ordre de tri identique lors de la pagination:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Une autre propriété `ViewBag.CurrentFilter`,, fournit la vue avec la chaîne de filtre actuelle. Cette valeur doit être incluse dans les liens de changement de page pour que les paramètres de filtre soient conservés lors du changement de page, et elle doit être restaurée dans la zone de texte lorsque la page est réaffichée. Si la chaîne de recherche est modifiée au cours du changement de page, la page doit être réinitialisée à 1, car le nouveau filtre peut entraîner l’affichage de données différentes. La chaîne de recherche est modifiée lorsqu’une valeur est entrée dans la zone de texte et que le bouton Envoyer est enfoncé. Dans ce cas, le `searchString` paramètre n’a pas la valeur null.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   À la fin de la méthode, la `ToPagedList` méthode d’extension de l' `IQueryable` objet Students convertit la requête Student en une seule page d’élèves dans un type de collection qui prend en charge la pagination. Cette seule page d’élèves est ensuite transmise à la vue:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   La méthode `ToPagedList` accepte un numéro de page. Les deux points d’interrogation représentent l' [opérateur de fusion Null](/dotnet/csharp/language-reference/operators/null-coalescing-operator). L’opérateur de fusion Null définit une valeur par défaut pour un type nullable ; l’expression `(page ?? 1)` indique de renvoyer la valeur de `page` si elle a une valeur, ou de renvoyer 1 si `page` a la valeur Null.

### <a name="add-paging-links-to-the-student-index-view"></a>Ajouter des liens de pagination à la vue d’index des étudiants

1. Dans *Views\Student\Index.cshtml*, remplacez le code existant par le code suivant. Les modifications apparaissent en surbrillance.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   L’instruction `@model` en haut de la page spécifie que la vue obtient désormais un objet `PagedList` à la place d’un objet `List`.

   L' `using` instruction pour `PagedList.Mvc` donne accès au programme d’assistance MVC pour les boutons de pagination.

   Le code utilise une surcharge de [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) qui lui permet de spécifier [FormMethod. obtient](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   Le [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) par défaut soumet les données de formulaire avec une publication, ce qui signifie que les paramètres sont transmis dans le corps du message http et non dans l’URL en tant que chaînes de requête. Lorsque vous spécifiez HTTP GET, les données de formulaire sont transmises dans l’URL sous forme de chaînes de requête, ce qui permet aux utilisateurs d’ajouter l’URL aux favoris. Les [recommandations du W3C relatives à l’utilisation de http sont](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommandées si vous devez utiliser la commande obtenir quand l’action n’entraîne pas de mise à jour.

   La zone de texte est initialisée avec la chaîne de recherche actuelle. ainsi, lorsque vous cliquez sur une nouvelle page, vous pouvez voir la chaîne de recherche actuelle.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   Les liens d’en-tête de colonne utilisent la chaîne de requête pour transmettre la chaîne de recherche actuelle au contrôleur afin que l’utilisateur puisse trier les résultats de filtrage :

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   La page actuelle et le nombre total de pages sont affichées.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   S’il n’y a aucune page à afficher, «la page 0 de 0» s’affiche. (Dans ce cas, le numéro de page est supérieur au nombre de `Model.PageNumber` pages, car est `Model.PageCount` 1 et est égal à 0.)

   Les boutons de pagination sont affichés par `PagedListPager` le programme d’assistance:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   L' `PagedListPager` application auxiliaire fournit un certain nombre d’options que vous pouvez personnaliser, y compris les URL et les styles. Pour plus d’informations, consultez [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) sur le site github.

2. Exécutez la page.

   Cliquez sur les liens de changement de page dans différents ordres de tri pour vérifier que le changement de page fonctionne. Ensuite, entrez une chaîne de recherche et essayez de changer de page à nouveau pour vérifier que le changement de page fonctionne correctement avec le tri et le filtrage.

## <a name="create-an-about-page"></a>Créer une page À propos

Pour la page à propos de du site Web Contoso University, vous allez afficher le nombre d’étudiants inscrits pour chaque date d’inscription. Cela nécessite un regroupement et des calculs simples sur les groupes. Pour ce faire, vous devez effectuer les opérations suivantes :

- Créez une classe de modèle de vue pour les données que vous devez transmettre à la vue.
- Modifiez la `About` méthode dans le `Home` contrôleur.
- Modifiez la `About` vue.

### <a name="create-the-view-model"></a>Créer le modèle de vue

Créez un dossier *ViewModels* dans le dossier du projet. Dans ce dossier, ajoutez un fichier de classe *EnrollmentDateGroup.cs* et remplacez le code du modèle par le code suivant:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modifier le contrôleur Home

1. Dans *HomeController.cs*, ajoutez les instructions `using` suivantes en haut du fichier:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Ajoutez une variable de classe pour le contexte de base de données immédiatement après l’accolade ouvrante de la classe:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Remplacez la méthode `About` par le code suivant :

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   L’instruction LINQ regroupe les entités Student par date d’inscription, calcule le nombre d’entités dans chaque groupe et stocke les résultats dans une collection d’objets de modèle de vue `EnrollmentDateGroup`.

4. Ajoutez une `Dispose` méthode:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modifier la vue About

1. Remplacez le code du fichier *Views\Home\About.cshtml* par le code suivant:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Exécutez l’application et cliquez sur le lien **à propos** de.

   Le nombre d’étudiants pour chaque date d’inscription s’affiche dans un tableau.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Des liens vers d’autres ressources de Entity Framework sont disponibles dans [accès aux données ASP.net-ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter des liens de tri de colonne
> * Ajouter une zone Rechercher
> * Ajouter la pagination
> * Créer une page À propos

Passez à l’article suivant pour apprendre à utiliser la résilience des connexions et l’interception des commandes.
> [!div class="nextstepaction"]
> [Résilience des connexions et interception des commandes](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
