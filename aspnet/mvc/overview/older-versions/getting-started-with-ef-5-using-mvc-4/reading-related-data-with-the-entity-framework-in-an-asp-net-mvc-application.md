---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lecture de données associées avec l’Entity Framework dans une application ASP.NET MVC (5 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e1752022b66952783039fbea0c2880e2f9be6ef8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595208"
---
# <a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Lecture de données associées avec l’Entity Framework dans une application ASP.NET MVC (5 sur 10)

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels depuis le début ou [Télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et démarrer ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [Téléchargez le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire le problème. Vous pouvez généralement trouver la solution au problème en comparant votre code au code terminé. Pour obtenir des erreurs courantes et comment les résoudre, consultez [Erreurs et solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Dans le didacticiel précédent, vous avez terminé le modèle de données School. Dans ce didacticiel, vous allez lire et afficher les données associées, c’est-à-dire les données que le Entity Framework charge dans les propriétés de navigation.

Les illustrations suivantes montrent les pages que vous allez utiliser.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Chargement différé, hâtif et explicite de données associées

La Entity Framework peut charger des données associées de plusieurs façons dans les propriétés de navigation d’une entité :

- *Chargement différé*. Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées. Toutefois, la première fois que vous essayez d’accéder à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement. Cela entraîne l’envoi de plusieurs requêtes à la base de données : une pour l’entité elle-même et une autre chaque fois que les données associées pour l’entité doivent être récupérées. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Chargement hâtif*. Quand l’entité est lue, ses données associées sont également récupérées. Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires. Vous spécifiez le chargement hâtif à l’aide de la méthode `Include`.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Chargement explicite*. Cela est similaire au chargement différé, à ceci près que vous récupérez explicitement les données associées dans le code ; elle ne se produit pas automatiquement lorsque vous accédez à une propriété de navigation. Vous chargez les données associées manuellement en obtenant l’entrée du gestionnaire d’état d’objet pour une entité et en appelant la méthode `Collection.Load` pour les collections ou la méthode `Reference.Load` pour les propriétés qui contiennent une seule entité. (Dans l’exemple suivant, si vous souhaitez charger la propriété de navigation administrateur, vous devez remplacer `Collection(x => x.Courses)` par `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Étant donné qu’ils ne récupèrent pas immédiatement les valeurs de propriété, le chargement différé et le chargement explicite sont également connus sous le nom de *chargement différé*.

En général, si vous savez que vous avez besoin de données associées pour chaque entité Récupérée, le chargement hâtif offre les meilleures performances, car une seule requête envoyée à la base de données est généralement plus efficace que les requêtes distinctes pour chaque entité récupérée. Par exemple, dans les exemples ci-dessus, supposons que chaque service comporte dix cours associés. L’exemple de chargement hâtif entraînerait une seule requête (Join) et un seul aller-retour vers la base de données. Les exemples de chargement différé et de chargement explicite produiront onze requêtes et onze allers-retours à la base de données. Les allers-retours supplémentaires à la base de données sont particulièrement nuisibles pour les performances lorsque la latence est élevée.

En revanche, dans certains scénarios, le chargement différé est plus efficace. Le chargement hâtif peut entraîner la génération d’une jointure très complexe, ce qui SQL Server ne peut pas être traité efficacement. Ou, si vous avez besoin d’accéder aux propriétés de navigation d’une entité uniquement pour un sous-ensemble d’un ensemble d’entités que vous traitez, le chargement différé peut être plus efficace, car le chargement hâtif récupère plus de données que nécessaire. Si les performances sont essentielles, il est préférable de tester les performances des deux façons afin d’effectuer le meilleur choix.

En général, vous utilisez le chargement explicite uniquement lorsque vous avez désactivé le chargement différé. Un scénario où vous devez désactiver le chargement différé est lors de la sérialisation. Le chargement différé et la sérialisation ne sont pas bien confondus. Si vous n’êtes pas prudent, vous pouvez terminer l’interrogation de beaucoup plus de données que prévu lorsque le chargement différé est activé. La sérialisation fonctionne généralement en accédant à chaque propriété sur une instance d’un type. L’accès aux propriétés déclenche un chargement différé, et ces entités chargées en différé sont sérialisées. Le processus de sérialisation accède ensuite à chaque propriété des entités chargées tardivement, ce qui peut entraîner un chargement et une sérialisation encore plus paresseux. Pour éviter cette réaction à la chaîne d’exécution, désactivez le chargement différé avant de sérialiser une entité.

La classe de contexte de base de données effectue un chargement différé par défaut. Il existe deux façons de désactiver le chargement différé :

- Pour les propriétés de navigation spécifiques, omettez le mot clé `virtual` lorsque vous déclarez la propriété.
- Pour toutes les propriétés de navigation, définissez `LazyLoadingEnabled` sur `false`. Par exemple, vous pouvez placer le code suivant dans le constructeur de votre classe de contexte : 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Le chargement différé peut masquer le code qui provoque des problèmes de performances. Par exemple, le code qui ne spécifie pas le chargement hâtif ou explicite, mais qui traite un volume élevé d’entités et utilise plusieurs propriétés de navigation dans chaque itération peut être très inefficace (en raison de nombreux allers-retours vers la base de données). Une application qui fonctionne correctement dans le développement à l’aide d’un serveur SQL Server local peut présenter des problèmes de performances lorsqu’elle est déplacée vers Azure SQL Database en raison de l’augmentation de la latence et du chargement différé. Le profilage des requêtes de base de données avec une charge de test réaliste vous permet de déterminer si le chargement différé est approprié. Pour plus d’informations, consultez [démystification des stratégies de Entity Framework : chargement des données associées](https://msdn.microsoft.com/magazine/hh205756.aspx) et [utilisation du Entity Framework pour réduire la latence du réseau à SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Créer une page d’index des cours qui affiche le nom du service

L’entité `Course` inclut une propriété de navigation qui contient l’entité `Department` du département auquel le cours est affecté. Pour afficher le nom du service affecté dans une liste de cours, vous devez obtenir la propriété `Name` à partir de l’entité `Department` qui est dans la propriété de navigation `Course.Department`.

Créez un contrôleur nommé `CourseController` pour le type d’entité `Course`, en utilisant les mêmes options que celles que vous avez effectuées précédemment pour le contrôleur `Student`, comme indiqué dans l’illustration suivante (sauf à la différence de l’image, votre classe de contexte se trouve dans l’espace de noms DAL et non dans l’espace de noms Models) :

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Ouvrez *Controllers\CourseController.cs* et examinez la méthode `Index` :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

La génération de modèles automatique a spécifié un chargement hâtif pour la propriété de navigation `Department` à l’aide de la méthode `Include`.

Ouvrez *Views\Course\Index.cshtml* et remplacez le code existant par le code suivant. Les modifications sont mises en surbrillance :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Vous avez apporté les modifications suivantes au code généré automatiquement :

- Modification de l’en-tête de l' **index** aux **cours**.
- Déplacement des liens de ligne vers la gauche.
- Ajout d’une colonne sous le **numéro** de titre qui affiche la valeur de la propriété `CourseID`. (Par défaut, les clés primaires ne sont pas échafaudées, car elles ne sont normalement pas compréhensibles pour les utilisateurs finaux. Toutefois, dans ce cas, la clé primaire est significative et vous souhaitez l’afficher.)
- Modification du dernier en-tête de colonne de **DepartmentID** (nom de la clé étrangère à l’entité `Department`) en **Department**.

Notez que pour la dernière colonne, le code de génération de modèles automatique affiche la propriété `Name` de l’entité `Department` qui est chargée dans la propriété de navigation `Department` :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Exécutez la page (sélectionnez l’onglet **courses** sur la page d’hébergement de Contoso University) pour afficher la liste des noms de service.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Créer une page d’index des enseignants qui affiche les cours et les inscriptions

Dans cette section, vous allez créer un contrôleur et une vue pour l’entité `Instructor`, afin d’afficher la page d’index des formateurs :

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Cette page lit et affiche les données associées comme suit :

- La liste des instructeurs affiche les données associées de l’entité `OfficeAssignment`. Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`. Vous allez utiliser le chargement hâtif pour les entités `OfficeAssignment`. Comme expliqué précédemment, le chargement hâtif est généralement plus efficace lorsque vous avez besoin des données associées pour toutes les lignes extraites de la table primaire. Dans ce cas, vous souhaitez afficher les affectations de bureaux pour tous les formateurs affichés.
- Quand l’utilisateur sélectionne un formateur, les entités `Course` associées sont affichées. Il existe une relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`. Vous allez utiliser le chargement hâtif pour les entités `Course` et leurs entités `Department` associées. Dans ce cas, le chargement différé peut être plus efficace, car vous avez besoin de cours uniquement pour l’instructeur sélectionné. Toutefois, cet exemple montre comment utiliser le chargement hâtif pour des propriétés de navigation dans des entités qui se trouvent elles-mêmes dans des propriétés de navigation.
- Lorsque l’utilisateur sélectionne un cours, les données associées à partir du jeu d’entités `Enrollments` s’affichent. Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`. Vous allez ajouter un chargement explicite pour les entités `Enrollment` et leurs entités `Student` associées. (Le chargement explicite n’est pas nécessaire, car le chargement différé est activé, mais il montre comment effectuer un chargement explicite.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Créer un modèle de vue pour la vue d’index de l’instructeur

La page d’index de l’instructeur affiche trois tables différentes. Par conséquent, vous allez créer un modèle de vue qui comprend trois propriétés, chacune contenant les données d’une des tables.

Dans le dossier *ViewModels* , créez *InstructorIndexData.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Ajout d’un style pour les lignes sélectionnées

Pour marquer des lignes sélectionnées, vous avez besoin d’une couleur d’arrière-plan différente. Pour fournir un style pour cette interface utilisateur, ajoutez le code en surbrillance suivant à la section `/* info and errors */` dans *Content\Site.CSS*, comme indiqué ci-dessous :

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Création du contrôleur et des vues du formateur

Créez un contrôleur de `InstructorController` comme indiqué dans l’illustration suivante :

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Ouvrez *Controllers\InstructorController.cs* et ajoutez une instruction `using` pour l’espace de noms `ViewModels` :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Le code de génération de modèles automatique dans la méthode `Index` spécifie le chargement hâtif uniquement pour la propriété de navigation `OfficeAssignment` :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Remplacez la méthode `Index` par le code suivant pour charger des données connexes supplémentaires et les placer dans le modèle de vue :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

La méthode accepte les données d’itinéraire facultatives (`id`) et un paramètre de chaîne de requête (`courseID`) qui fournissent les valeurs d’ID de l’instructeur sélectionné et du cours sélectionné, et transmet toutes les données requises à la vue. Ces paramètres sont fournis par les liens hypertexte **Select** dans la page.

> [!TIP]
> 
> **Données d’itinéraire**
> 
> Les données de routage sont des données que le Binder de modèle a trouvées dans un segment d’URL spécifié dans la table de routage. Par exemple, l’itinéraire par défaut spécifie les segments `controller`, `action`et `id` :
> 
> ```csharp
> routes.MapRoute(  
>  name: "Default",  
>  url: "{controller}/{action}/{id}",  
>  defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }  
> );
> ```
> 
> Dans l’URL suivante, l’itinéraire par défaut mappe `Instructor` comme `controller`, `Index` comme `action` et 1 comme `id`; Il s’agit des valeurs de données d’itinéraire.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> " ? courseID = 2021" est une valeur de chaîne de requête. Le Binder de modèle fonctionne également si vous transmettez le `id` en tant que valeur de chaîne de requête :
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Les URL sont créées par des instructions `ActionLink` dans la vue Razor. Dans le code suivant, le paramètre `id` correspond à l’itinéraire par défaut, de sorte que `id` est ajouté aux données d’itinéraire.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> Dans le code suivant, `courseID` ne correspond pas à un paramètre de l’itinéraire par défaut, il est donc ajouté en tant que chaîne de requête.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

Le code commence par créer une instance du modèle de vue et la placer dans la liste des formateurs. Le code spécifie le chargement hâtif pour l' `Instructor.OfficeAssignment` et la propriété de navigation `Instructor.Courses`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

La deuxième `Include` méthode charge des cours et, pour chaque cours chargé, elle effectue un chargement hâtif pour la propriété de navigation `Course.Department`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Comme mentionné précédemment, le chargement hâtif n’est pas obligatoire, mais il est effectué pour améliorer les performances. Étant donné que la vue requiert toujours l’entité `OfficeAssignment`, il est plus efficace de l’extraire dans la même requête. `Course` les entités sont requises lorsqu’un formateur est sélectionné dans la page Web, de sorte que le chargement hâtif est préférable au chargement différé uniquement si la page s’affiche plus souvent avec un cours sélectionné que sans.

Si un ID d’instructeur a été sélectionné, l’instructeur sélectionné est récupéré dans la liste des instructeurs du modèle de vue. La propriété `Courses` du modèle de vue est ensuite chargée avec les entités `Course` à partir de la propriété de navigation `Courses` de ce formateur.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

La méthode `Where` retourne une collection, mais dans ce cas, les critères passés à cette méthode entraînent le retour d’une seule entité `Instructor`. La méthode `Single` convertit la collection en une seule entité `Instructor`, ce qui vous permet d’accéder à la propriété `Courses` de cette entité.

Vous utilisez la méthode [unique](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) sur une collection lorsque vous savez que la collection ne comportera qu’un seul élément. La méthode `Single` lève une exception si la collection qui lui est passée est vide ou s’il y a plusieurs éléments. Une alternative est [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), qui retourne une valeur par défaut (`null` dans ce cas) si la collection est vide. Toutefois, dans ce cas, il en résulterait une exception (de la tentative de recherche d’une propriété `Courses` sur une référence `null`) et le message d’exception indiquera moins clairement la cause du problème. Lorsque vous appelez la méthode `Single`, vous pouvez également transmettre la condition `Where` au lieu d’appeler la méthode `Where` séparément :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

À la place de :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Ensuite, si un cours a été sélectionné, le cours sélectionné est récupéré à partir de la liste des cours dans le modèle de vue. Ensuite, la propriété `Enrollments` du modèle de vue est chargée avec les entités `Enrollment` à partir de la propriété de navigation `Enrollments` de ce cours.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modification de la vue d’index du formateur

Dans *Views\Instructor\Index.cshtml*, remplacez le code existant par le code suivant. Les modifications sont mises en surbrillance :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Vous avez apporté les modifications suivantes au code existant :

- Vous avez changé la classe de modèle en `InstructorIndexData`.
- Vous avez changé le titre de la page en remplaçant **Index** par **Instructors**.
- Déplacement des colonnes de lien de ligne vers la gauche.
- Suppression de la colonne **FullName** .
- Ajout d’une colonne **Office** qui affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’a pas la valeur null. (Comme il s’agit d’une relation un-à-zéro-ou-un, il se peut qu’il n’y ait pas d’entité `OfficeAssignment` associée.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Ajout de code qui ajoute dynamiquement `class="selectedrow"` à l’élément `tr` de l’instructeur sélectionné. Cela définit une couleur d’arrière-plan pour la ligne sélectionnée à l’aide de la classe CSS que vous avez créée précédemment. (L’attribut `valign` sera utile dans le didacticiel suivant lorsque vous ajoutez une colonne à plusieurs lignes à la table.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Ajout d’une nouvelle `ActionLink` étiquetée **Select** juste avant les autres liens de chaque ligne, ce qui entraîne l’envoi de l’ID d’instructeur sélectionné à la méthode `Index`.

Exécutez l’application et sélectionnez l’onglet **Instructors** . La page affiche la propriété `Location` des entités `OfficeAssignment` associées et une cellule de table vide lorsqu’il n’existe aucune entité `OfficeAssignment` associée.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Dans le fichier *Views\Instructor\Index.cshtml* , après l’élément `table` fermant (à la fin du fichier), ajoutez le code en surbrillance suivant. Cela permet d’afficher une liste des cours liés à un formateur quand un formateur est sélectionné.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Ce code lit la propriété `Courses` du modèle de vue pour afficher la liste des cours. Il fournit également un lien hypertexte `Select` qui envoie l’ID du cours sélectionné à la méthode d’action `Index`.

> [!NOTE]
> Le fichier *. CSS* est mis en cache par les navigateurs. Si les modifications ne s’affichent pas lors de l’exécution de l’application, effectuez une actualisation matérielle (maintenez la touche CTRL enfoncée tout en cliquant sur le bouton **Actualiser** ou appuyez sur CTRL + F5).

Exécutez la page et sélectionnez un formateur. Vous voyez à présent une grille qui affiche les cours affectés au formateur sélectionné et, pour chaque cours, vous voyez le nom du département affecté.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Après le bloc de code que vous venez d’ajouter, ajoutez le code suivant. Ceci affiche la liste des étudiants qui sont inscrits à un cours quand ce cours est sélectionné.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Ce code lit la propriété `Enrollments` du modèle de vue pour afficher la liste des étudiants inscrits au cours.

Exécutez la page et sélectionnez un formateur. Ensuite, sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs notes.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Ajout d’un chargement explicite

Ouvrez *InstructorController.cs* et examinez comment la méthode `Index` obtient la liste des inscriptions pour un cours sélectionné :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Lorsque vous avez récupéré la liste des instructeurs, vous avez spécifié le chargement hâtif pour la propriété de navigation `Courses` et pour la propriété `Department` de chaque cours. Vous placez ensuite la collection `Courses` dans le modèle de vue et vous accédez maintenant à la propriété de navigation `Enrollments` à partir d’une entité de cette collection. Étant donné que vous n’avez pas spécifié le chargement hâtif pour la propriété de navigation `Course.Enrollments`, les données de cette propriété s’affichent dans la page suite à un chargement différé.

Si vous désactivez le chargement différé sans modifier le code d’une autre façon, la propriété `Enrollments` serait null, quel que soit le nombre d’inscriptions que le cours avait réellement. Dans ce cas, pour charger la propriété `Enrollments`, vous devez spécifier un chargement hâtif ou explicite. Vous avez déjà vu comment effectuer un chargement hâtif. Pour voir un exemple de chargement explicite, remplacez la méthode `Index` par le code suivant, qui charge explicitement la propriété `Enrollments`. Le code modifié est mis en surbrillance.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Après avoir obtenu l’entité de `Course` sélectionnée, le nouveau code charge explicitement la propriété de navigation `Enrollments` de ce cours :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Ensuite, il charge explicitement l’entité `Student` associée à chaque `Enrollment` d’entité :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Notez que vous utilisez la méthode `Collection` pour charger une propriété de collection, mais pour une propriété qui contient une seule entité, vous utilisez la méthode `Reference`. Vous pouvez exécuter la page d’index du formateur maintenant et vous ne verrez aucune différence dans ce qui est affiché sur la page, bien que vous ayez modifié la manière dont les données sont récupérées.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant utilisé les trois méthodes (Lazy, hâtif et Explicit) pour charger les données associées dans les propriétés de navigation. Dans le didacticiel suivant, vous apprendrez à mettre à jour les données associées.

Vous trouverez des liens vers d’autres ressources de Entity Framework dans le [plan de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Suivant](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
