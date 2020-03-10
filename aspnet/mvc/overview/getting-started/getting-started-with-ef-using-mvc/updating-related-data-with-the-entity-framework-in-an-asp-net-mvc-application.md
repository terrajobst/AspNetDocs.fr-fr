---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Didacticiel : mettre à jour les données associées avec EF dans une application ASP.NET MVC'
description: Dans ce didacticiel, vous allez mettre à jour les données associées. Pour la plupart des relations, vous pouvez effectuer cette opération en mettant à jour les champs de clé étrangère ou les propriétés de navigation.
author: tdykstra
ms.author: riande
ms.date: 01/19/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4d5f6447fdccefdcdf9497a9e94f23243302a0e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616003"
---
# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a>Didacticiel : mettre à jour les données associées avec EF dans une application ASP.NET MVC

Dans le didacticiel précédent, vous avez affiché les données associées. Dans ce didacticiel, vous allez mettre à jour les données associées. Pour la plupart des relations, vous pouvez effectuer cette opération en mettant à jour les champs de clé étrangère ou les propriétés de navigation. Pour les relations plusieurs-à-plusieurs, la Entity Framework n’expose pas directement la table de jointure, vous ajoutez et supprimez des entités vers et à partir des propriétés de navigation appropriées.

Les illustrations suivantes montrent quelques-unes des pages que vous allez utiliser.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Modification des enseignants avec les cours](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Personnaliser les pages des cours
> * Page Ajouter Office aux enseignants
> * Page Ajouter des cours aux enseignants
> * Mettre à jour DeleteConfirmed
> * Ajouter des emplacements de bureau et des cours à la page Create

## <a name="prerequisites"></a>Conditions préalables requises

* [Lecture de données associées](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a>Personnaliser les pages des cours

Quand une entité Course est créée, elle doit avoir une relation avec un département existant. Pour faciliter cela, le code du modèle généré automatiquement inclut des méthodes de contrôleur, et des vues Create et Edit qui incluent une liste déroulante pour sélectionner le département. La liste déroulante définit la `Course.DepartmentID` propriété de clé étrangère, et c’est l’ensemble des Entity Framework nécessaires pour charger la propriété de navigation `Department` avec l’entité de `Department` appropriée. Vous utilisez le code du modèle généré automatiquement, mais que vous modifiez un peu pour ajouter la gestion des erreurs et trier la liste déroulante.

Dans *CourseController.cs*, supprimez les quatre méthodes `Create` et `Edit` et remplacez-les par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Ajoutez l’instruction `using` suivante au début du fichier :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

La méthode `PopulateDepartmentsDropDownList` obtient une liste de tous les services triés par nom, crée une collection `SelectList` pour une liste déroulante et passe la collection à la vue dans une propriété `ViewBag`. La méthode accepte le paramètre facultatif `selectedDepartment` qui permet au code appelant de spécifier l’élément sélectionné lors de l’affichage de la liste déroulante. La vue passe le nom `DepartmentID` à l’application auxiliaire [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) , et l’application d’assistance sait ensuite rechercher un `SelectList` nommé `DepartmentID`dans l’objet `ViewBag`.

La méthode `HttpGet` `Create` appelle la méthode `PopulateDepartmentsDropDownList` sans définir l’élément sélectionné, car pour un nouveau cours, le service n’est pas encore établi :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

La méthode `HttpGet` `Edit` définit l’élément sélectionné, en fonction de l’ID du service qui est déjà affecté au cours en cours de modification :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Les méthodes `HttpPost` pour les `Create` et `Edit` incluent également du code qui définit l’élément sélectionné lorsqu’il réaffiche la page après une erreur :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Ce code garantit que lorsque la page est réaffichée pour afficher le message d’erreur, quel que soit le service sélectionné reste sélectionné.

Les vues de cours sont déjà structurées avec des listes déroulantes pour le champ Department, mais vous ne souhaitez pas la légende DepartmentID pour ce champ. par conséquent, vous devez mettre en surbrillance la modification suivante du fichier *Views\Course\Create.cshtml* pour modifier la légende.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Apportez la même modification dans *Views\Course\Edit.cshtml*.

Normalement, le générateur de modèles automatique n’effectue pas de génération de modèles automatique pour une clé primaire, car la valeur de clé est générée par la base de données et ne peut pas être modifiée et n’est pas une valeur significative à afficher aux utilisateurs. Pour les entités course, le générateur de modèles automatique inclut une zone de texte pour le champ `CourseID`, car il comprend que l’attribut `DatabaseGeneratedOption.None` signifie que l’utilisateur doit pouvoir entrer la valeur de clé primaire. Mais cela ne comprend pas que, étant donné que le nombre est significatif que vous souhaitez voir dans les autres affichages, vous devez l’ajouter manuellement.

Dans *Views\Course\Edit.cshtml*, ajoutez un champ de numéro de cours avant le champ **titre** . Étant donné qu’il s’agit de la clé primaire, elle est affichée, mais elle ne peut pas être modifiée.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Il existe déjà un champ masqué (`Html.HiddenFor` Helper) pour le numéro de cours dans la vue Edit (édition). L’ajout d’un programme d’assistance *html. LabelFor* n’élimine pas le besoin du champ masqué, car il n’est pas inclus dans les données publiées lorsque l’utilisateur clique sur **Enregistrer** dans la page Modifier.

Dans *Views\Course\Delete.cshtml* et *Views\Course\Details.cshtml*, remplacez la légende « Name » du nom de service par « Department » et ajoutez un champ de numéro de cours avant le champ **title** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Exécutez la page **créer** (Affichez la page index du cours et cliquez sur **créer nouveau**), puis entrez les données pour un nouveau cours :

| Valeur | Paramètre |
| ----- | ------- |
| nombre | Entrez *1000*. |
| Titre | Entrez *algébrique*. |
| Crédits | Entrez *4*. |
|Department | Sélectionnez **mathématique**. |

Cliquez sur **Créer**. La page index du cours s’affiche avec le nouveau cours ajouté à la liste. Le nom du département dans la liste de la page Index provient de la propriété de navigation, ce qui montre que la relation a été établie correctement.

Exécutez la page **modifier** (Affichez la page index du cours et cliquez sur **modifier** sur un cours).

Modifiez les données dans la page et cliquez sur **Save**. La page index du cours s’affiche avec les données de cours mises à jour.

## <a name="add-office-to-instructors-page"></a>Page Ajouter Office aux enseignants

Quand vous modifiez un enregistrement de formateur, vous voulez avoir la possibilité de mettre à jour l’attribution du bureau du formateur. L’entité `Instructor` a une relation un-à-zéro-ou-un avec l’entité `OfficeAssignment`, ce qui signifie que vous devez gérer les situations suivantes :

- Si l’utilisateur efface l’attribution de bureau et qu’elle a initialement une valeur, vous devez supprimer et supprimer l’entité `OfficeAssignment`.
- Si l’utilisateur entre une valeur d’affectation de bureau et qu’elle était vide à l’origine, vous devez créer une entité de `OfficeAssignment`.
- Si l’utilisateur modifie la valeur d’une attribution de bureau, vous devez modifier la valeur dans une entité de `OfficeAssignment` existante.

Ouvrez *InstructorController.cs* et examinez la `HttpGet` méthode `Edit` :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Ici, le code généré automatiquement n’est pas ce que vous souhaitez. Il définit des données pour une liste déroulante, mais vous avez besoin d’une zone de texte. Remplacez cette méthode par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Ce code supprime l’instruction `ViewBag` et ajoute un chargement hâtif pour l’entité `OfficeAssignment` associée. Comme vous ne pouvez pas effectuer un chargement hâtif avec la méthode `Find`, les méthodes `Where` et `Single` sont utilisées à la place pour sélectionner l’instructeur.

Remplacez la méthode `HttpPost` `Edit` par le code suivant. qui gère les mises à jour des affectations Office :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

La référence à `RetryLimitExceededException` nécessite une instruction `using` ; pour l’ajouter, pointez votre souris sur `RetryLimitExceededException`. Le message suivant s’affiche : ![ message d’exception de nouvelle tentative](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Sélectionnez **afficher les corrections potentielles**, puis **utilisez System. Data. Entity. infrastructure**

![Résoudre l’exception de nouvelle tentative](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Le code effectue les actions suivantes :

- Remplace le nom de la méthode par `EditPost`, car la signature est désormais la même que la méthode `HttpGet` (l’attribut `ActionName` spécifie que l’URL/Edit/est toujours utilisée).
- Obtient l'entité `Instructor` en cours à partir de la base de données à l’aide d’un chargement hâtif de la propriété de navigation `OfficeAssignment`. C’est identique à ce que vous avez fait dans la méthode `HttpGet` `Edit`.
- Met à jour l’entité `Instructor` récupérée avec les valeurs du classeur de modèles. La surcharge [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) utilisée vous permet de *liste blanche* les propriétés que vous souhaitez inclure. Cela empêche la post-publication, comme expliqué dans [le deuxième didacticiel](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Si l’emplacement du Bureau est vide, affecte à la propriété `Instructor.OfficeAssignment` la valeur NULL afin que la ligne associée dans la table `OfficeAssignment` soit supprimée.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Il enregistre les modifications dans la base de données.

Dans *Views\Instructor\Edit.cshtml*, après les éléments `div` du champ **Hire date** , ajoutez un nouveau champ pour la modification de l’emplacement du Bureau :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Exécutez la page (sélectionnez l’onglet **Instructors** , puis cliquez sur **modifier** sur un formateur). Modifiez **Office Location** et cliquez sur **Save**.

## <a name="add-courses-to-instructors-page"></a>Page Ajouter des cours aux enseignants

Les formateurs peuvent donner un nombre quelconque de cours. Vous allez maintenant améliorer la page de modification du formateur en ajoutant la possibilité de modifier les attributions de cours à l’aide d’un groupe de cases à cocher.

La relation entre les entités `Course` et `Instructor` est plusieurs-à-plusieurs, ce qui signifie que vous n’avez pas d’accès direct aux propriétés de clé étrangère qui se trouvent dans la table de jointure. Au lieu de cela, vous ajoutez et supprimez des entités vers et à partir de la propriété de navigation `Instructor.Courses`.

L’interface utilisateur qui vous permet de changer les cours auxquels un formateur est affecté est un groupe de cases à cocher. Une case à cocher est affichée pour chaque cours de la base de données, et ceux auxquels le formateur est actuellement affecté sont sélectionnés. L’utilisateur peut cocher ou décocher les cases pour modifier les affectations de cours. Si le nombre de cours était bien plus important, vous souhaiterez probablement utiliser une méthode différente de présentation des données dans la vue, mais vous utiliseriez la même méthode de manipulation des propriétés de navigation pour créer ou supprimer des relations.

Pour fournir des données à la vue pour la liste de cases à cocher, vous utilisez une classe de modèle de vue. Créez *AssignedCourseData.cs* dans le dossier *ViewModels* et remplacez le code existant par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Dans *InstructorController.cs*, remplacez la méthode `HttpGet` `Edit` par le code suivant. Les modifications apparaissent en surbrillance.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Le code ajoute un chargement hâtif pour la propriété de navigation `Courses` et appelle la nouvelle méthode `PopulateAssignedCourseData` pour fournir des informations pour le tableau de cases à cocher avec la classe de modèle de vue `AssignedCourseData`.

Le code de la méthode `PopulateAssignedCourseData` lit toutes les entités `Course` afin de charger une liste de cours à l’aide de la classe de modèle de vue. Pour chaque cours, le code vérifie s’il existe dans la propriété de navigation `Courses` du formateur. Pour créer une recherche efficace en vérifiant si un cours est affecté à l’instructeur, les cours affectés à l’instructeur sont placés dans une collection [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) . La propriété `Assigned` est définie sur `true` pour les cours auxquels l’instructeur est affecté. La vue utilise cette propriété pour déterminer quelles cases doivent être affichées cochées. Enfin, la liste est passée à la vue dans une propriété `ViewBag`.

Ensuite, ajoutez le code qui est exécuté quand l’utilisateur clique sur **Save**. Remplacez la méthode `EditPost` par le code suivant, qui appelle une nouvelle méthode qui met à jour la propriété de navigation `Courses` de l’entité `Instructor`. Les modifications apparaissent en surbrillance.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

La signature de la méthode est maintenant différente de la méthode `HttpGet` `Edit`. par conséquent, le nom de la méthode passe de `EditPost` à `Edit`.

Étant donné que la vue n’a pas de collection d’entités `Course`, le Binder de modèle ne peut pas mettre automatiquement à jour la propriété de navigation `Courses`. Au lieu d’utiliser le classeur de modèles pour mettre à jour la propriété de navigation `Courses`, vous le ferez dans la nouvelle méthode `UpdateInstructorCourses`. Par conséquent, vous devez exclure la propriété `Courses` de la liaison de modèle. Cela ne nécessite aucune modification du code qui appelle [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) , car vous utilisez la surcharge de liste d’autorisation et `Courses` ne figure pas *dans la liste* d’inclusion.

Si aucune case à cocher n’a été activée, le code de `UpdateInstructorCourses` initialise la propriété de navigation `Courses` avec une collection vide :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Le code boucle ensuite à travers tous les cours dans la base de données, et vérifie chaque cours par rapport à ceux actuellement affectés au formateur relativement à ceux qui ont été sélectionnés dans la vue. Pour faciliter des recherches efficaces, les deux dernières collections sont stockées dans des objets `HashSet`.

Si la case pour un cours a été cochée mais que le cours n’est pas dans la propriété de navigation `Instructor.Courses`, le cours est ajouté à la collection dans la propriété de navigation.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Si la case pour un cours a été cochée mais que le cours est dans la propriété de navigation `Instructor.Courses`, le cours est supprimé de la propriété de navigation.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Dans *Views\Instructor\Edit.cshtml*, ajoutez un champ **courses** avec un tableau de cases à cocher en ajoutant le code suivant immédiatement après les éléments `div` pour le champ `OfficeAssignment` et avant l’élément `div` pour le bouton **Enregistrer** :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Après avoir collé le code, si les sauts de ligne et la mise en retrait n’ont pas l’aspect souhaité, corrigez manuellement tout afin qu’il ressemble à ce que vous voyez ici. L’indentation ne doit pas nécessairement être parfaite, mais les lignes `@</tr><tr>`, `@:<td>`, `@:</td>` et `@</tr>` doivent chacune tenir sur une seule ligne comme dans l’illustration, sinon vous recevrez une erreur d’exécution.

Ce code crée un tableau HTML qui a trois colonnes. Dans chaque colonne se trouve une case à cocher, suivie d’une légende qui est constituée du numéro et du titre du cours. Les cases à cocher ont toutes le même nom (« selectedCourses »), ce qui informe le Binder de modèle qu’il doit être traité comme un groupe. L’attribut `value` de chaque case à cocher est défini sur la valeur de `CourseID.` lors de la publication de la page, le classeur de modèles passe un tableau au contrôleur qui se compose des valeurs `CourseID` uniquement pour les cases à cocher sélectionnées.

Lorsque les cases à cocher sont initialement affichées, celles qui concernent des cours affectés à l’instructeur ont `checked` attributs, qui les sélectionne (les affiche cochées).

Après avoir modifié les attributions de cours, vous souhaiterez peut-être vérifier les modifications lorsque le site revient à la page `Index`. Par conséquent, vous devez ajouter une colonne à la table dans cette page. Dans ce cas, vous n’avez pas besoin d’utiliser l’objet `ViewBag`, car les informations que vous souhaitez afficher se trouvent déjà dans la `Courses` propriété de navigation de l’entité `Instructor` que vous passez à la page comme modèle.

Dans *Views\Instructor\Index.cshtml*, ajoutez un titre de **cours** immédiatement après l’en-tête **Office** , comme indiqué dans l’exemple suivant :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Ajoutez ensuite une nouvelle cellule de détail juste après la cellule de détail de l’emplacement du Bureau :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Exécutez la page d’index de l' **instructeur** pour voir les cours affectés à chaque formateur.

Cliquez sur **modifier** dans un formateur pour voir la page Modifier.

Modifiez certaines affectations de cours, puis cliquez sur **Enregistrer**. Les modifications que vous apportez sont reflétées dans la page Index.

 Remarque : l’approche adoptée ici pour modifier les données de cours de formateur fonctionne bien lorsqu’il existe un nombre limité de cours. Pour les collections qui sont beaucoup plus volumineuses, une autre interface utilisateur et une autre méthode de mise à jour seraient nécessaires.

## <a name="update-deleteconfirmed"></a>Mettre à jour DeleteConfirmed

Dans *InstructorController.cs*, supprimez la méthode `DeleteConfirmed` et insérez le code suivant à la place.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Ce code apporte les modifications suivantes :

- Si l’instructeur est affecté en tant qu’administrateur d’un service, supprime l’attribution de l’instructeur de ce service. Sans ce code, vous obtiendriez une erreur d’intégrité référentielle si vous avez essayé de supprimer un formateur qui a été affecté en tant qu’administrateur pour un service.

Ce code ne gère pas le scénario d’un formateur affecté en tant qu’administrateur pour plusieurs services. Dans le dernier didacticiel, vous allez ajouter du code qui empêche ce scénario de se produire.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Ajouter des emplacements de bureau et des cours à la page Create

Dans *InstructorController.cs*, supprimez les méthodes `HttpGet` et `HttpPost` `Create`, puis ajoutez le code suivant à leur place :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Ce code est similaire à ce que vous avez vu pour les méthodes Edit, à ceci près qu’aucun cours n’est sélectionné initialement. La méthode `HttpGet` `Create` appelle la méthode `PopulateAssignedCourseData` non parce qu’il peut y avoir des cours sélectionnés mais afin de fournir une collection vide pour la boucle `foreach` dans la vue (sinon, le code de la vue lève une exception de référence null).

La méthode de création HttpPost ajoute chaque cours sélectionné à la propriété de navigation courses avant le code du modèle qui vérifie les erreurs de validation et ajoute le nouvel instructeur à la base de données. Des cours sont ajoutés même s’il existe des erreurs de modèle. ainsi, en cas d’erreurs de modèle (par exemple, l’utilisateur a entré une date non valide) de sorte que, lorsque la page est réaffichée avec un message d’erreur, les sélections de cours effectuées sont automatiquement restaurées.

Notez que pour pouvoir ajouter des cours à la propriété de navigation `Courses`, vous devez initialiser la propriété en tant que collection vide :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Comme alternative à cette opération dans le code du contrôleur, vous pouvez l’effectuer dans le modèle Instructor en modifiant le getter de propriété pour créer automatiquement la collection si elle n’existe pas, comme le montre l’exemple suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Si vous modifiez la propriété `Courses` de cette façon, vous pouvez supprimer le code d’initialisation explicite de la propriété dans le contrôleur.

Dans *Views\Instructor\Create.cshtml*, ajoutez une zone de texte d’emplacement de bureau et des cases à cocher de cours après le champ Hire date et avant le bouton **Submit** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Après avoir collé le code, corrigez les sauts de ligne et la mise en retrait comme vous l’avez fait précédemment pour la page Modifier.

Exécutez la page créer et ajoutez un formateur.

<a id="transactions"></a>

## <a name="handling-transactions"></a>Gérer des transactions

Comme expliqué dans le [didacticiel de la fonctionnalité CRUD de base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), par défaut, le Entity Framework implémente implicitement des transactions. Pour les scénarios où vous avez besoin de davantage de contrôle, par exemple, si vous souhaitez inclure des opérations effectuées en dehors de Entity Framework dans une transaction, consultez [utilisation des transactions](https://msdn.microsoft.com/data/dn456843) sur MSDN.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Des liens vers d’autres ressources de Entity Framework sont disponibles dans [accès aux données ASP.net-ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-step"></a>Étape suivante

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Pages de cours personnalisées
> * Page Office ajoutée aux formateurs ajoutée
> * Page des cours ajoutés à l’instructeur
> * Mise à jour DeleteConfirmed
> * Ajout de l’emplacement et des cours Office à la page créer

Passez à l’article suivant pour savoir comment implémenter un modèle de programmation asynchrone.
> [!div class="nextstepaction"]
> [Modèle de programmation asynchrone](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
