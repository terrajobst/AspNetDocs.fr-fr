---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Mise à jour des données associées avec l’Entity Framework dans une application MVC ASP.NET (6 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d29cb172d642b67947b461d1a7e55d01872bb8c2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74592439"
---
# <a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Mise à jour des données associées avec l’Entity Framework dans une application MVC ASP.NET (6 sur 10)

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels depuis le début ou [Télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et démarrer ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [Téléchargez le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire le problème. Vous pouvez généralement trouver la solution au problème en comparant votre code au code terminé. Pour obtenir des erreurs courantes et comment les résoudre, consultez [Erreurs et solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Dans le didacticiel précédent, vous avez affiché les données associées. dans ce didacticiel, vous allez mettre à jour les données associées. Pour la plupart des relations, vous pouvez effectuer cette opération en mettant à jour les champs de clé étrangère appropriés. Pour les relations plusieurs-à-plusieurs, la Entity Framework n’expose pas directement la table de jointure. vous devez donc ajouter et supprimer explicitement des entités vers et à partir des propriétés de navigation appropriées.

Les illustrations suivantes montrent les pages que vous allez utiliser.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personnaliser les pages Create et Edit pour les cours

Quand une entité de cours est créée, elle doit avoir une relation à un département existant. Pour faciliter cela, le code du modèle généré automatiquement inclut des méthodes de contrôleur, et des vues Create et Edit qui incluent une liste déroulante pour sélectionner le département. La liste déroulante définit la `Course.DepartmentID` propriété de clé étrangère, et c’est l’ensemble des Entity Framework nécessaires pour charger la propriété de navigation `Department` avec l’entité de `Department` appropriée. Vous utilisez le code du modèle généré automatiquement, mais que vous modifiez un peu pour ajouter la gestion des erreurs et trier la liste déroulante.

Dans *CourseController.cs*, supprimez les quatre méthodes `Edit` et `Create` et remplacez-les par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

La méthode `PopulateDepartmentsDropDownList` obtient une liste de tous les services triés par nom, crée une collection `SelectList` pour une liste déroulante et passe la collection à la vue dans une propriété `ViewBag`. La méthode accepte le paramètre facultatif `selectedDepartment` qui permet au code appelant de spécifier l’élément sélectionné lors de l’affichage de la liste déroulante. La vue passe le nom `DepartmentID` à [l’application auxiliaire `DropDownList`](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), et l’application d’assistance sait ensuite rechercher un `SelectList` nommé `DepartmentID`dans l’objet `ViewBag`.

La méthode `HttpGet` `Create` appelle la méthode `PopulateDepartmentsDropDownList` sans définir l’élément sélectionné, car pour un nouveau cours, le service n’est pas encore établi :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

La méthode `HttpGet` `Edit` définit l’élément sélectionné, en fonction de l’ID du service qui est déjà affecté au cours en cours de modification :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Les méthodes `HttpPost` pour les `Create` et `Edit` incluent également du code qui définit l’élément sélectionné lorsqu’il réaffiche la page après une erreur :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Ce code garantit que lorsque la page est réaffichée pour afficher le message d’erreur, quel que soit le service sélectionné reste sélectionné.

Dans *Views\Course\Create.cshtml*, ajoutez le code en surbrillance pour créer un champ de numéro de cours avant le champ **titre** . Comme expliqué dans un didacticiel précédent, les champs de clé primaire ne sont pas générés par défaut, mais cette clé primaire est significative. par conséquent, vous souhaitez que l’utilisateur puisse entrer la valeur de clé.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

Dans *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*et *Views\Course\Details.cshtml*, ajoutez un champ de numéro de cours avant le champ **titre** . Étant donné qu’il s’agit de la clé primaire, elle est affichée, mais elle ne peut pas être modifiée.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Exécutez la page **créer** (Affichez la page index du cours et cliquez sur **créer nouveau**), puis entrez les données pour un nouveau cours :

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Cliquez sur **Créer**. La page index du cours s’affiche avec le nouveau cours ajouté à la liste. Le nom du département dans la liste de la page Index provient de la propriété de navigation, ce qui montre que la relation a été établie correctement.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Exécutez la page **modifier** (Affichez la page index du cours et cliquez sur **modifier** sur un cours).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Modifiez les données dans la page et cliquez sur **Save**. La page index du cours s’affiche avec les données de cours mises à jour.

## <a name="adding-an-edit-page-for-instructors"></a>Ajout d’une page de modification pour les enseignants

Quand vous modifiez un enregistrement de formateur, vous voulez avoir la possibilité de mettre à jour l’attribution du bureau du formateur. L’entité `Instructor` a une relation un-à-zéro-ou-un avec l’entité `OfficeAssignment`, ce qui signifie que vous devez gérer les situations suivantes :

- Si l’utilisateur efface l’attribution de bureau et qu’elle a initialement une valeur, vous devez supprimer et supprimer l’entité `OfficeAssignment`.
- Si l’utilisateur entre une valeur d’affectation de bureau et qu’elle était vide à l’origine, vous devez créer une entité de `OfficeAssignment`.
- Si l’utilisateur modifie la valeur d’une attribution de bureau, vous devez modifier la valeur dans une entité de `OfficeAssignment` existante.

Ouvrez *InstructorController.cs* et examinez la `HttpGet` méthode `Edit` :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ici, le code généré automatiquement n’est pas ce que vous souhaitez. Il définit des données pour une liste déroulante, mais vous avez besoin d’une zone de texte. Remplacez cette méthode par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Ce code supprime l’instruction `ViewBag` et ajoute un chargement hâtif pour l’entité `OfficeAssignment` associée. Comme vous ne pouvez pas effectuer un chargement hâtif avec la méthode `Find`, les méthodes `Where` et `Single` sont utilisées à la place pour sélectionner l’instructeur.

Remplacez la méthode `HttpPost` `Edit` par le code suivant. qui gère les mises à jour des affectations Office :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Le code effectue les actions suivantes :

- Obtient l'entité `Instructor` en cours à partir de la base de données à l’aide d’un chargement hâtif de la propriété de navigation `OfficeAssignment`. C’est identique à ce que vous avez fait dans la méthode `HttpGet` `Edit`.
- Met à jour l’entité `Instructor` récupérée avec les valeurs du classeur de modèles. La surcharge [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) utilisée vous permet de *liste blanche* les propriétés que vous souhaitez inclure. Cela empêche la post-publication, comme expliqué dans [le deuxième didacticiel](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Si l’emplacement du Bureau est vide, affecte à la propriété `Instructor.OfficeAssignment` la valeur NULL afin que la ligne associée dans la table `OfficeAssignment` soit supprimée.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Il enregistre les modifications dans la base de données.

Dans *Views\Instructor\Edit.cshtml*, après les éléments `div` du champ **Hire date** , ajoutez un nouveau champ pour la modification de l’emplacement du Bureau :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Exécutez la page (sélectionnez l’onglet **Instructors** , puis cliquez sur **modifier** sur un formateur). Modifiez **Office Location** et cliquez sur **Save**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Ajout d’affectations de cours à la page de modification de l’instructeur

Les formateurs peuvent donner un nombre quelconque de cours. Maintenant, vous allez améliorer la page de modification des formateurs en ajoutant la possibilité de modifier les affectations de cours avec un groupe de cases à cocher, comme le montre la capture d’écran suivante :

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

La relation entre les entités `Course` et `Instructor` est plusieurs-à-plusieurs, ce qui signifie que vous n’avez pas d’accès direct à la table de jointure. Au lieu de cela, vous allez ajouter et supprimer des entités dans la propriété de navigation `Instructor.Courses`.

L’interface utilisateur qui vous permet de changer les cours auxquels un formateur est affecté est un groupe de cases à cocher. Une case à cocher est affichée pour chaque cours de la base de données, et ceux auxquels le formateur est actuellement affecté sont sélectionnés. L’utilisateur peut cocher ou décocher les cases pour changer les affectations de cours. Si le nombre de cours était bien plus important, vous souhaiterez probablement utiliser une méthode différente de présentation des données dans la vue, mais vous utiliseriez la même méthode de manipulation des propriétés de navigation pour créer ou supprimer des relations.

Pour fournir des données à la vue pour la liste de cases à cocher, vous utilisez une classe de modèle de vue. Créez *AssignedCourseData.cs* dans le dossier *ViewModels* et remplacez le code existant par le code suivant :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Dans *InstructorController.cs*, remplacez la méthode `HttpGet` `Edit` par le code suivant. Les modifications sont mises en surbrillance.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Le code ajoute un chargement hâtif pour la propriété de navigation `Courses` et appelle la nouvelle méthode `PopulateAssignedCourseData` pour fournir des informations pour le tableau de cases à cocher avec la classe de modèle de vue `AssignedCourseData`.

Le code de la méthode `PopulateAssignedCourseData` lit toutes les entités `Course` afin de charger une liste de cours à l’aide de la classe de modèle de vue. Pour chaque cours, le code vérifie s’il existe dans la propriété de navigation `Courses` du formateur. Pour créer une recherche efficace en vérifiant si un cours est affecté à l’instructeur, les cours affectés à l’instructeur sont placés dans une collection [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) . La propriété `Assigned` est définie sur `true` pour les cours auxquels l’instructeur est affecté. La vue utilise cette propriété pour déterminer quelles cases doivent être affichées cochées. Enfin, la liste est passée à la vue dans une propriété `ViewBag`.

Ensuite, ajoutez le code qui est exécuté quand l’utilisateur clique sur **Save**. Remplacez la méthode `HttpPost` `Edit` par le code suivant, qui appelle une nouvelle méthode qui met à jour la propriété de navigation `Courses` de l’entité `Instructor`. Les modifications sont mises en surbrillance.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Étant donné que la vue n’a pas de collection d’entités `Course`, le Binder de modèle ne peut pas mettre automatiquement à jour la propriété de navigation `Courses`. Au lieu d’utiliser le classeur de modèles pour mettre à jour la propriété de navigation courses, vous le ferez dans la nouvelle méthode `UpdateInstructorCourses`. Par conséquent, vous devez exclure la propriété `Courses` de la liaison de modèle. Cela ne nécessite aucune modification du code qui appelle [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) , car vous utilisez la surcharge de liste d’autorisation et `Courses` ne figure pas *dans la liste* d’inclusion.

Si aucune case à cocher n’a été activée, le code de `UpdateInstructorCourses` initialise la propriété de navigation `Courses` avec une collection vide :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Le code boucle ensuite à travers tous les cours dans la base de données, et vérifie chaque cours par rapport à ceux actuellement affectés au formateur relativement à ceux qui ont été sélectionnés dans la vue. Pour faciliter des recherches efficaces, les deux dernières collections sont stockées dans des objets `HashSet`.

Si la case pour un cours a été cochée mais que le cours n’est pas dans la propriété de navigation `Instructor.Courses`, le cours est ajouté à la collection dans la propriété de navigation.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Si la case pour un cours a été cochée mais que le cours est dans la propriété de navigation `Instructor.Courses`, le cours est supprimé de la propriété de navigation.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Dans *Views\Instructor\Edit.cshtml*, ajoutez un champ **courses** avec un tableau de cases à cocher en ajoutant le code en surbrillance suivant immédiatement après les éléments `div` pour le champ `OfficeAssignment` :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Ce code crée un tableau HTML qui a trois colonnes. Dans chaque colonne se trouve une case à cocher, suivie d’une légende qui est constituée du numéro et du titre du cours. Les cases à cocher ont toutes le même nom (« selectedCourses »), ce qui informe le Binder de modèle qu’il doit être traité comme un groupe. L’attribut `value` de chaque case à cocher est défini sur la valeur de `CourseID.` lors de la publication de la page, le classeur de modèles passe un tableau au contrôleur qui se compose des valeurs `CourseID` uniquement pour les cases à cocher sélectionnées.

Lorsque les cases à cocher sont initialement affichées, celles qui concernent des cours affectés à l’instructeur ont `checked` attributs, qui les sélectionne (les affiche cochées).

Après avoir modifié les attributions de cours, vous souhaiterez peut-être vérifier les modifications lorsque le site revient à la page `Index`. Par conséquent, vous devez ajouter une colonne à la table dans cette page. Dans ce cas, vous n’avez pas besoin d’utiliser l’objet `ViewBag`, car les informations que vous souhaitez afficher se trouvent déjà dans la `Courses` propriété de navigation de l’entité `Instructor` que vous passez à la page comme modèle.

Dans *Views\Instructor\Index.cshtml*, ajoutez un titre de **cours** immédiatement après l’en-tête **Office** , comme indiqué dans l’exemple suivant :

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Ajoutez ensuite une nouvelle cellule de détail juste après la cellule de détail de l’emplacement du Bureau :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Exécutez la page d’index de l' **instructeur** pour voir les cours affectés à chaque formateur :

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Cliquez sur **modifier** dans un formateur pour voir la page Modifier.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Modifiez certaines affectations de cours, puis cliquez sur **Enregistrer**. Les modifications que vous apportez sont reflétées dans la page Index.

 Remarque : l’approche adoptée pour modifier les données de cours de formateur fonctionne bien lorsqu’il existe un nombre limité de cours. Pour les collections qui sont beaucoup plus volumineuses, une autre interface utilisateur et une autre méthode de mise à jour seraient nécessaires.  

## <a name="update-the-delete-method"></a>Mettre à jour la méthode Delete

Modifiez le code de la méthode de suppression HttpPost pour supprimer l’enregistrement d’affectation de bureau (le cas échéant) lors de la suppression de l’instructeur :

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]

Si vous essayez de supprimer un formateur affecté à un service en tant qu’administrateur, vous obtiendrez une erreur d’intégrité référentielle. Consultez [la version actuelle de ce didacticiel](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) pour obtenir du code supplémentaire qui supprimera automatiquement l’instructeur d’un service où l’instructeur est affecté en tant qu’administrateur.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant terminé cette présentation de l’utilisation des données associées. Jusqu’ici, dans ces didacticiels, vous avez effectué une gamme complète d’opérations CRUD, mais vous n’avez pas traité les problèmes d’accès concurrentiel. Le didacticiel suivant présente le sujet de l’accès concurrentiel, explique les options de gestion et ajoute la gestion de la concurrence au code CRUD que vous avez déjà écrit pour un type d’entité.

Vous trouverez des liens vers d’autres ressources Entity Framework à la fin du [dernier didacticiel de cette série](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Précédent](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
