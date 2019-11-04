---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Didacticiel : lire les données associées avec EF dans une application ASP.NET MVC'
description: Dans ce didacticiel, vous allez lire et afficher les données associées, c’est-à-dire les données que le Entity Framework charge dans les propriétés de navigation.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d804c8dd45ad131949260c85d9d9c6683bfe9646
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445655"
---
# <a name="tutorial-read-related-data-with-ef-in-an-aspnet-mvc-app"></a>Didacticiel : lire les données associées avec EF dans une application ASP.NET MVC

Dans le didacticiel précédent, vous avez terminé le modèle de données School. Dans ce didacticiel, vous allez lire et afficher les données associées, c’est-à-dire les données que le Entity Framework charge dans les propriétés de navigation.

Les illustrations suivantes montrent les pages que vous allez utiliser.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de la Entity Framework 6 Code First et Visual Studio. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Découvrir comment charger les données associées
> * Créer une page Courses
> * Créer une page Instructors

## <a name="prerequisites"></a>Configuration requise

* [Créer un modèle de données plus complexe](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="learn-how-to-load-related-data"></a>Découvrir comment charger les données associées

La Entity Framework peut charger des données associées de plusieurs façons dans les propriétés de navigation d’une entité :

- *Chargement différé*. Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées. Toutefois, la première fois que vous essayez d’accéder à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement. Cela entraîne l’envoi de plusieurs requêtes à la base de données : une pour l’entité elle-même et une autre chaque fois que les données associées pour l’entité doivent être récupérées. La classe `DbContext` permet le chargement différé par défaut.

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Chargement hâtif*. Quand l’entité est lue, ses données associées sont également récupérées. Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires. Vous spécifiez le chargement hâtif à l’aide de la méthode `Include`.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Chargement explicite*. Cela est similaire au chargement différé, à ceci près que vous récupérez explicitement les données associées dans le code ; elle ne se produit pas automatiquement lorsque vous accédez à une propriété de navigation. Vous chargez les données associées manuellement en obtenant l’entrée du gestionnaire d’état d’objet pour une entité et en appelant la méthode [collection. Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) pour les collections ou la méthode [Reference. Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) pour les propriétés qui contiennent une seule entité. (Dans l’exemple suivant, si vous souhaitez charger la propriété de navigation administrateur, vous devez remplacer `Collection(x => x.Courses)` par `Reference(x => x.Administrator)`.) En général, vous utilisez le chargement explicite uniquement lorsque vous avez désactivé le chargement différé.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Étant donné qu’ils ne récupèrent pas immédiatement les valeurs de propriété, le chargement différé et le chargement explicite sont également connus sous le nom de *chargement différé*.

### <a name="performance-considerations"></a>Considérations sur les performances

Si vous savez que vous avez besoin des données associées pour toutes les entités récupérées, le chargement hâtif souvent offre des performances optimales, car une seule requête envoyée à la base de données est généralement plus efficace que les requêtes distinctes pour chaque entité récupérée. Par exemple, dans les exemples ci-dessus, supposons que chaque service comporte dix cours associés. L’exemple de chargement hâtif entraînerait une seule requête (Join) et un seul aller-retour vers la base de données. Les exemples de chargement différé et de chargement explicite produiront onze requêtes et onze allers-retours à la base de données. Les allers-retours supplémentaires à la base de données sont particulièrement nuisibles pour les performances lorsque la latence est élevée.

En revanche, dans certains scénarios, le chargement différé est plus efficace. Le chargement hâtif peut entraîner la génération d’une jointure très complexe, ce qui SQL Server ne peut pas être traité efficacement. Ou, si vous avez besoin d’accéder aux propriétés de navigation d’une entité uniquement pour un sous-ensemble d’un ensemble d’entités que vous traitez, le chargement différé peut être plus performant, car le chargement hâtif récupérerait plus de données que nécessaire. Si les performances sont essentielles, il est préférable de tester les performances des deux façons afin d’effectuer le meilleur choix.

Le chargement différé peut masquer le code qui provoque des problèmes de performances. Par exemple, le code qui ne spécifie pas le chargement hâtif ou explicite, mais qui traite un volume élevé d’entités et utilise plusieurs propriétés de navigation dans chaque itération peut être très inefficace (en raison de nombreux allers-retours vers la base de données). Une application qui fonctionne correctement dans le développement à l’aide d’un serveur SQL Server local peut présenter des problèmes de performances lorsqu’elle est déplacée vers Azure SQL Database en raison de l’augmentation de la latence et du chargement différé. Le profilage des requêtes de base de données avec une charge de test réaliste vous permet de déterminer si le chargement différé est approprié. Pour plus d’informations, consultez [démystification des stratégies de Entity Framework : chargement des données associées](https://msdn.microsoft.com/magazine/hh205756.aspx) et [utilisation du Entity Framework pour réduire la latence du réseau à SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Désactiver le chargement différé avant la sérialisation

Si vous laissez le chargement différé activé au cours de la sérialisation, vous pouvez finir par interroger beaucoup plus de données que prévu. La sérialisation fonctionne généralement en accédant à chaque propriété sur une instance d’un type. L’accès aux propriétés déclenche un chargement différé, et ces entités chargées en différé sont sérialisées. Le processus de sérialisation accède ensuite à chaque propriété des entités chargées tardivement, ce qui peut entraîner un chargement et une sérialisation encore plus paresseux. Pour éviter cette réaction à la chaîne d’exécution, désactivez le chargement différé avant de sérialiser une entité.

La sérialisation peut également être compliquée par les classes proxy utilisées par le Entity Framework, comme expliqué dans le didacticiel sur les [scénarios avancés](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Pour éviter les problèmes de sérialisation, il est possible de sérialiser des objets DTO (Data Transfer Objects) au lieu d’objets d’entité, comme indiqué dans le didacticiel utilisation de l' [API Web avec Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) .

Si vous n’utilisez pas les DTO, vous pouvez désactiver le chargement différé et éviter les problèmes de proxy en [désactivant la création du proxy](https://msdn.microsoft.com/data/jj592886.aspx).

Voici [d’autres façons de désactiver le chargement différé](https://msdn.microsoft.com/data/jj574232):

- Pour les propriétés de navigation spécifiques, omettez le mot clé `virtual` lorsque vous déclarez la propriété.
- Pour toutes les propriétés de navigation, définissez `LazyLoadingEnabled` sur `false`, placez le code suivant dans le constructeur de votre classe de contexte :

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page"></a>Créer une page Courses

L’entité `Course` inclut une propriété de navigation qui contient l’entité `Department` du département auquel le cours est affecté. Pour afficher le nom du service affecté dans une liste de cours, vous devez obtenir la propriété `Name` à partir de l’entité `Department` qui est dans la propriété de navigation `Course.Department`.

Créez un contrôleur nommé `CourseController` (et non CoursesController) pour le type d’entité `Course`, en utilisant les mêmes options pour le **contrôleur MVC 5 avec vues, en utilisant Entity Framework** générateur de modèles automatique que vous avez fait précédemment pour le contrôleur de `Student` :

| Paramètre | valeur |
| ------- | ----- |
| Classe de modèle | Sélectionnez **cours (ContosoUniversity. Models)** . |
| Classe de contexte de données | Sélectionnez **SchoolContext (ContosoUniversity. dal)** . |
| Nom du contrôleur | Entrez *CourseController*. Là encore, pas *CoursesController* avec un *s*. Quand vous avez sélectionné le **cours (ContosoUniversity. Models)** , la valeur du **nom du contrôleur** a été automatiquement renseignée. Vous devez modifier la valeur. |

Laissez les autres valeurs par défaut et ajoutez le contrôleur.

Ouvrez *Controllers\CourseController.cs* et examinez la méthode `Index` :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

La génération de modèles automatique a spécifié un chargement hâtif pour la propriété de navigation `Department` à l’aide de la méthode `Include`.

Ouvrez *Views\Course\Index.cshtml* et remplacez le code du modèle par le code suivant. Les modifications sont mises en surbrillance :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Vous avez apporté les modifications suivantes au code généré automatiquement :

- Modification de l’en-tête de l' **index** aux **cours**.
- Ajout d’une colonne **Number** qui affiche la valeur de la propriété `CourseID`. Par défaut, les clés primaires ne sont pas échafaudées, car elles ne sont normalement pas compréhensibles pour les utilisateurs finaux. Toutefois, dans le cas présent, la clé primaire est significative et vous voulez l’afficher.
- Déplacement de la colonne **Department** sur le côté droit et modification de son titre. L’échafaudage a choisi correctement d’afficher la propriété `Name` à partir de l’entité `Department`, mais ici, dans la page course, l’en-tête de colonne doit être **Department** et non **Name**.

Notez que pour la colonne Department, le code de génération de modèles automatique affiche la propriété `Name` de l’entité `Department` qui est chargée dans la propriété de navigation `Department` :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Exécutez la page (sélectionnez l’onglet **courses** sur la page d’hébergement de Contoso University) pour afficher la liste des noms de service.

## <a name="create-an-instructors-page"></a>Créer une page Instructors

Dans cette section, vous allez créer un contrôleur et une vue pour l’entité `Instructor`, afin d’afficher la page des formateurs. Cette page lit et affiche les données associées comme suit :

- La liste des instructeurs affiche les données associées de l’entité `OfficeAssignment`. Il existe une relation un-à-zéro-ou-un entre les entités `Instructor` et `OfficeAssignment`. Vous allez utiliser le chargement hâtif pour les entités `OfficeAssignment`. Comme expliqué précédemment, le chargement hâtif est généralement plus efficace lorsque vous avez besoin des données associées pour toutes les lignes extraites de la table primaire. Dans ce cas, vous souhaitez afficher les affectations de bureaux pour tous les formateurs affichés.
- Quand l’utilisateur sélectionne un formateur, les entités `Course` associées sont affichées. Il existe une relation plusieurs-à-plusieurs entre les entités `Instructor` et `Course`. Vous allez utiliser le chargement hâtif pour les entités `Course` et leurs entités `Department` associées. Dans ce cas, le chargement différé peut être plus efficace, car vous avez besoin de cours uniquement pour l’instructeur sélectionné. Toutefois, cet exemple montre comment utiliser le chargement hâtif pour des propriétés de navigation dans des entités qui se trouvent elles-mêmes dans des propriétés de navigation.
- Lorsque l’utilisateur sélectionne un cours, les données associées à partir du jeu d’entités `Enrollments` s’affichent. Il existe une relation un-à-plusieurs entre les entités `Course` et `Enrollment`. Vous allez ajouter un chargement explicite pour les entités `Enrollment` et leurs entités `Student` associées. (Le chargement explicite n’est pas nécessaire, car le chargement différé est activé, mais il montre comment effectuer un chargement explicite.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Créer un modèle de vue pour la vue d’index de l’instructeur

La page enseignants affiche trois tables différentes. Par conséquent, vous allez créer un modèle de vue qui comprend trois propriétés, chacune contenant les données d’une des tables.

Dans le dossier *ViewModels* , créez *InstructorIndexData.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Créer le contrôleur et les vues du formateur

Créer un contrôleur `InstructorController` (non InstructorsController) avec une action de lecture/écriture EF :

| Paramètre | valeur |
| ------- | ----- |
| Classe de modèle | Sélectionnez **Instructor (ContosoUniversity. Models)** . |
| Classe de contexte de données | Sélectionnez **SchoolContext (ContosoUniversity. dal)** . |
| Nom du contrôleur | Entrez *InstructorController*. Là encore, pas *InstructorsController* avec un *s*. Quand vous avez sélectionné le **cours (ContosoUniversity. Models)** , la valeur du **nom du contrôleur** a été automatiquement renseignée. Vous devez modifier la valeur. |

Laissez les autres valeurs par défaut et ajoutez le contrôleur.

Ouvrez *Controllers\InstructorController.cs* et ajoutez une instruction `using` pour l’espace de noms `ViewModels` :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Le code de génération de modèles automatique dans la méthode `Index` spécifie le chargement hâtif uniquement pour la propriété de navigation `OfficeAssignment` :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Remplacez la méthode `Index` par le code suivant pour charger des données connexes supplémentaires et les placer dans le modèle de vue :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La méthode accepte les données d’itinéraire facultatives (`id`) et un paramètre de chaîne de requête (`courseID`) qui fournissent les valeurs d’ID de l’instructeur sélectionné et du cours sélectionné, et transmet toutes les données requises à la vue. Ces paramètres sont fournis par les liens hypertexte **Select** dans la page.

Le code commence par créer une instance du modèle de vue et la placer dans la liste des formateurs. Le code spécifie le chargement hâtif pour l' `Instructor.OfficeAssignment` et la propriété de navigation `Instructor.Courses`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

La deuxième `Include` méthode charge des cours et, pour chaque cours chargé, elle effectue un chargement hâtif pour la propriété de navigation `Course.Department`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Comme mentionné précédemment, le chargement hâtif n’est pas obligatoire, mais il est effectué pour améliorer les performances. Étant donné que la vue requiert toujours l’entité `OfficeAssignment`, il est plus efficace de l’extraire dans la même requête. `Course` les entités sont requises lorsqu’un formateur est sélectionné dans la page Web, de sorte que le chargement hâtif est préférable au chargement différé uniquement si la page s’affiche plus souvent avec un cours sélectionné que sans.

Si un ID d’instructeur a été sélectionné, l’instructeur sélectionné est récupéré dans la liste des instructeurs du modèle de vue. La propriété `Courses` du modèle de vue est ensuite chargée avec les entités `Course` à partir de la propriété de navigation `Courses` de ce formateur.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

La méthode `Where` retourne une collection, mais dans ce cas, les critères passés à cette méthode entraînent le retour d’une seule entité `Instructor`. La méthode `Single` convertit la collection en une seule entité `Instructor`, ce qui vous permet d’accéder à la propriété `Courses` de cette entité.

Vous utilisez la méthode [unique](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) sur une collection lorsque vous savez que la collection ne comportera qu’un seul élément. La méthode `Single` lève une exception si la collection qui lui est passée est vide ou s’il y a plusieurs éléments. Une alternative est [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), qui retourne une valeur par défaut (`null` dans ce cas) si la collection est vide. Toutefois, dans ce cas, il en résulterait une exception (de la tentative de recherche d’une propriété `Courses` sur une référence `null`) et le message d’exception indiquera moins clairement la cause du problème. Lorsque vous appelez la méthode `Single`, vous pouvez également transmettre la condition `Where` au lieu d’appeler la méthode `Where` séparément :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

À la place de :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Ensuite, si un cours a été sélectionné, le cours sélectionné est récupéré à partir de la liste des cours dans le modèle de vue. Ensuite, la propriété `Enrollments` du modèle de vue est chargée avec les entités `Enrollment` à partir de la propriété de navigation `Enrollments` de ce cours.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Modifier la vue d’index de l’instructeur

Dans *Views\Instructor\Index.cshtml*, remplacez le code du modèle par le code suivant. Les modifications sont mises en surbrillance :

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Vous avez apporté les modifications suivantes au code existant :

- Vous avez changé la classe de modèle en `InstructorIndexData`.
- Vous avez changé le titre de la page en remplaçant **Index** par **Instructors**.
- Ajout d’une colonne **Office** qui affiche `item.OfficeAssignment.Location` uniquement si `item.OfficeAssignment` n’a pas la valeur null. (Comme il s’agit d’une relation un-à-zéro-ou-un, il se peut qu’il n’y ait pas d’entité `OfficeAssignment` associée.)

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Ajout de code qui ajoute dynamiquement `class="success"` à l’élément `tr` de l’instructeur sélectionné. Cela définit une couleur d’arrière-plan pour la ligne sélectionnée à l’aide d’une classe [bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) .

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Ajout d’une nouvelle `ActionLink` étiquetée **Select** juste avant les autres liens de chaque ligne, ce qui entraîne l’envoi de l’ID d’instructeur sélectionné à la méthode `Index`.

Exécutez l’application et sélectionnez l’onglet **Instructors** . La page affiche la propriété `Location` des entités `OfficeAssignment` associées et une cellule de table vide lorsqu’il n’existe aucune entité `OfficeAssignment` associée.

Dans le fichier *Views\Instructor\Index.cshtml* , après l’élément `table` fermant (à la fin du fichier), ajoutez le code suivant. Ce code affiche la liste des cours associés à un formateur quand un formateur est sélectionné.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Ce code lit la propriété `Courses` du modèle de vue pour afficher la liste des cours. Il fournit également un lien hypertexte `Select` qui envoie l’ID du cours sélectionné à la méthode d’action `Index`.

Exécutez la page et sélectionnez un formateur. Vous voyez à présent une grille qui affiche les cours affectés au formateur sélectionné et, pour chaque cours, vous voyez le nom du département affecté.

Après le bloc de code que vous venez d’ajouter, ajoutez le code suivant. Ceci affiche la liste des étudiants qui sont inscrits à un cours quand ce cours est sélectionné.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Ce code lit la propriété `Enrollments` du modèle de vue pour afficher la liste des étudiants inscrits au cours.

Exécutez la page et sélectionnez un formateur. Ensuite, sélectionnez un cours pour afficher la liste des étudiants inscrits et leurs notes.

### <a name="adding-explicit-loading"></a>Ajout d’un chargement explicite

Ouvrez *InstructorController.cs* et examinez comment la méthode `Index` obtient la liste des inscriptions pour un cours sélectionné :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Lorsque vous avez récupéré la liste des instructeurs, vous avez spécifié le chargement hâtif pour la propriété de navigation `Courses` et pour la propriété `Department` de chaque cours. Vous placez ensuite la collection `Courses` dans le modèle de vue et vous accédez maintenant à la propriété de navigation `Enrollments` à partir d’une entité de cette collection. Étant donné que vous n’avez pas spécifié le chargement hâtif pour la propriété de navigation `Course.Enrollments`, les données de cette propriété s’affichent dans la page suite à un chargement différé.

Si vous désactivez le chargement différé sans modifier le code d’une autre façon, la propriété `Enrollments` serait null, quel que soit le nombre d’inscriptions que le cours avait réellement. Dans ce cas, pour charger la propriété `Enrollments`, vous devez spécifier un chargement hâtif ou explicite. Vous avez déjà vu comment effectuer un chargement hâtif. Pour voir un exemple de chargement explicite, remplacez la méthode `Index` par le code suivant, qui charge explicitement la propriété `Enrollments`. Le code modifié est mis en surbrillance.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Après avoir obtenu l’entité de `Course` sélectionnée, le nouveau code charge explicitement la propriété de navigation `Enrollments` de ce cours :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Ensuite, il charge explicitement l’entité `Student` associée à chaque `Enrollment` d’entité :

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Notez que vous utilisez la méthode `Collection` pour charger une propriété de collection, mais pour une propriété qui contient une seule entité, vous utilisez la méthode `Reference`.

Exécutez la page d’index du formateur maintenant. vous ne verrez aucune différence dans ce qui est affiché sur la page, bien que vous ayez modifié la manière dont les données sont récupérées.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Vous trouverez des liens vers d’autres ressources de Entity Framework dans l' [ASP.net accès aux données-ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Chargement des données associées découvert
> * Page Courses créée
> * Page Instructors créée

Passez à l’article suivant pour découvrir comment mettre à jour les données associées.

> [!div class="nextstepaction"]
> [Mettre à jour les données associées](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
