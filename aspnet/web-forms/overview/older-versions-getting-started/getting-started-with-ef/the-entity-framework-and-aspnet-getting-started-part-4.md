---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 4 | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications de Web Forms ASP.NET à l’aide de l’Entity Framework. L’exemple d’application est...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: eb75a76038466bf30738387ed4739687de1df944
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638165"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 4

par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="working-with-related-data"></a>Utilisation des données associées

Dans le didacticiel précédent, vous avez utilisé le contrôle `EntityDataSource` pour filtrer, trier et regrouper des données. Dans ce didacticiel, vous allez afficher et mettre à jour les données associées.

Vous allez créer la page des instructeurs qui affiche une liste d’instructeurs. Lorsque vous sélectionnez un formateur, vous voyez une liste des cours traités par ce formateur. Lorsque vous sélectionnez un cours, vous voyez les détails relatifs au cours et à la liste des étudiants inscrits au cours. Vous pouvez modifier le nom de l’instructeur, la date d’embauche et l’affectation Office. L’attribution de bureau est un jeu d’entités distinct auquel vous accédez via une propriété de navigation.

Vous pouvez lier des données de référence à des données de détail dans le balisage ou dans le code. Dans cette partie du didacticiel, vous allez utiliser les deux méthodes.

[![image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Affichage et mise à jour des entités associées dans un contrôle GridView

Créez une nouvelle page Web nommée *Instructors. aspx* qui utilise la page maître *site. Master* , puis ajoutez le balisage suivant au contrôle `Content` nommé `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Ce balisage crée un contrôle de `EntityDataSource` qui sélectionne les instructeurs et active les mises à jour. L’élément `div` configure la balise à afficher à gauche pour que vous puissiez ajouter une colonne à droite ultérieurement.

Entre le balisage `EntityDataSource` et la balise de `</div>` de fermeture, ajoutez la balise suivante qui crée un contrôle `GridView` et un `Label` contrôle que vous allez utiliser pour les messages d’erreur :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Ce contrôle de `GridView` permet de sélectionner la ligne, met en surbrillance la ligne sélectionnée avec une couleur d’arrière-plan gris clair et spécifie les gestionnaires (que vous créerez ultérieurement) pour les événements `SelectedIndexChanged` et `Updating`. Il spécifie également `PersonID` pour la propriété `DataKeyNames`, afin que la valeur de clé de la ligne sélectionnée puisse être passée à un autre contrôle que vous ajouterez ultérieurement.

La dernière colonne contient l’attribution de bureau de l’instructeur, qui est stockée dans une propriété de navigation de l’entité `Person`, car elle provient d’une entité associée. Notez que l’élément `EditItemTemplate` spécifie `Eval` au lieu de `Bind`, car le contrôle `GridView` ne peut pas effectuer de liaison directe avec les propriétés de navigation pour les mettre à jour. Vous allez mettre à jour l’attribution de bureau dans le code. Pour ce faire, vous avez besoin d’une référence au contrôle `TextBox`, et vous pouvez l’obtenir et l’enregistrer dans l’événement `Init` du contrôle `TextBox`.

Le contrôle de `GridView` est un contrôle `Label` utilisé pour les messages d’erreur. La propriété `Visible` du contrôle est `false`et l’état d’affichage est désactivé, afin que l’étiquette s’affiche uniquement lorsque le code le rend visible en réponse à une erreur.

Ouvrez le fichier *Instructors.aspx.cs* et ajoutez l’instruction `using` suivante :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Ajoutez un champ de classe privé immédiatement après la déclaration de nom de classe partielle pour contenir une référence à la zone de texte Assignment Office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Ajoutez un stub pour le gestionnaire d’événements `SelectedIndexChanged` auquel vous ajouterez du code pour une version ultérieure. Ajoutez également un gestionnaire pour l’événement `Init` du contrôle de `TextBox` d’affectation Office afin que vous puissiez stocker une référence au contrôle `TextBox`. Vous utiliserez cette référence pour récupérer la valeur entrée par l’utilisateur afin de mettre à jour l’entité associée à la propriété de navigation.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Vous utiliserez l’événement `Updating` du contrôle `GridView` pour mettre à jour la propriété `Location` de l’entité `OfficeAssignment` associée. Ajoutez le gestionnaire suivant pour l’événement `Updating` :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Ce code est exécuté lorsque l’utilisateur clique sur **mettre à jour** dans une ligne `GridView`. Le code utilise LINQ to Entities pour récupérer l’entité `OfficeAssignment` qui est associée à l’entité `Person` actuelle, à l’aide de la `PersonID` de la ligne sélectionnée à partir de l’argument d’événement.

Le code effectue ensuite l’une des actions suivantes en fonction de la valeur du contrôle `InstructorOfficeTextBox` :

- Si la zone de texte a une valeur et qu’il n’existe aucune entité `OfficeAssignment` à mettre à jour, elle en crée une.
- Si la zone de texte a une valeur et qu’il existe une entité `OfficeAssignment`, elle met à jour la valeur de la propriété `Location`.
- Si la zone de texte est vide et qu’une entité `OfficeAssignment` existe, elle supprime l’entité.

Après cela, il enregistre les modifications dans la base de données. Si une exception se produit, elle affiche un message d’erreur.

Exécutez la page.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Cliquez sur **modifier** pour que tous les champs soient modifiés en zones de texte.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Modifiez l’une de ces valeurs, y compris l' **attribution de bureau**. Cliquez sur **mettre à jour** pour voir les modifications reflétées dans la liste.

## <a name="displaying-related-entities-in-a-separate-control"></a>Affichage des entités associées dans un contrôle distinct

Chaque formateur peut enseigner un ou plusieurs cours. vous allez donc ajouter un contrôle de `EntityDataSource` et un contrôle `GridView` pour répertorier les cours associés à l’option Instructor sélectionné dans le contrôle `GridView` des enseignants. Pour créer un en-tête et le contrôle de `EntityDataSource` pour les entités de cours, ajoutez le balisage suivant entre le message d’erreur `Label` contrôle et la balise de `</div>` de fermeture :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

Le paramètre `Where` contient la valeur de la `PersonID` de l’instructeur dont la ligne est sélectionnée dans le contrôle `InstructorsGridView`. La propriété `Where` contient une commande de sous-sélection qui obtient toutes les entités `Person` associées de la propriété de navigation `People` d’une entité `Course` et sélectionne l’entité `Course` uniquement si l’une des entités `Person` associées contient la valeur `PersonID` sélectionnée.

Pour créer le contrôle `GridView`, ajoutez le balisage suivant immédiatement après le contrôle `CoursesEntityDataSource` (avant la balise de `</div>` de fermeture) :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Étant donné qu’aucun cours ne sera affiché si aucun formateur n’est sélectionné, un élément `EmptyDataTemplate` est inclus.

Exécutez la page.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Sélectionnez un formateur qui a un ou plusieurs cours attribués, et le ou les cours apparaissent dans la liste. (Remarque : bien que le schéma de base de données autorise plusieurs cours, les données de test fournies avec la base de données ne comportent pas plus d’un cours. Vous pouvez ajouter vous-même des cours à la base de données à l’aide de la fenêtre **Explorateur de serveurs** ou de la page *CoursesAdd. aspx* , que vous ajouterez dans un didacticiel ultérieur.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

Le contrôle `CoursesGridView` affiche uniquement quelques champs de cours. Pour afficher tous les détails d’un cours, vous utiliserez un contrôle de `DetailsView` pour le cours que l’utilisateur sélectionne. Dans *Instructors. aspx*, ajoutez le balisage suivant après la balise `</div>` fermante (veillez à placer ce balisage **après** la balise div de fermeture, et non avant) :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Ce balisage crée un contrôle `EntityDataSource` qui est lié au jeu d’entités `Courses`. La propriété `Where` sélectionne un cours à l’aide de la valeur `CourseID` de la ligne sélectionnée dans le contrôle `GridView` des cours. Le balisage spécifie un gestionnaire pour l’événement `Selected`, que vous utiliserez ultérieurement pour afficher les notes des étudiants, qui est un autre niveau plus bas dans la hiérarchie.

Dans *Instructors.aspx.cs*, créez le stub suivant pour la méthode `CourseDetailsEntityDataSource_Selected`. (Vous allez remplir ce talon plus loin dans le didacticiel. pour l’instant, vous en aurez besoin pour que la page soit compilée et exécutée.)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Exécutez la page.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Initialement, il n’y a pas de détails sur les cours, car aucun cours n’est sélectionné. Sélectionnez un formateur qui a un cours affecté, puis sélectionnez un cours pour afficher les détails.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Utilisation de l’événement « sélectionné » EntityDataSource pour afficher les données associées

Enfin, vous souhaitez afficher tous les étudiants inscrits et leurs notes pour le cours sélectionné. Pour ce faire, vous allez utiliser l’événement `Selected` du contrôle `EntityDataSource` lié à la `DetailsView`course.

Dans *Instructors. aspx*, ajoutez le balisage suivant après le contrôle `DetailsView` :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Ce balisage crée un contrôle de `ListView` qui affiche une liste d’étudiants et leurs notes pour le cours sélectionné. Aucune source de données n’est spécifiée, car vous allez créer un DataBind du contrôle dans le code. L’élément `EmptyDataTemplate` fournit un message à afficher quand aucun cours n’est sélectionné. dans ce cas, il n’y a aucun étudiant à afficher. L’élément `LayoutTemplate` crée une table HTML pour afficher la liste, tandis que le `ItemTemplate` spécifie les colonnes à afficher. L’ID de l’étudiant et le grade des étudiants proviennent de l’entité `StudentGrade`, et le nom de l’étudiant provient de l’entité `Person` que le Entity Framework met à disposition dans la propriété de navigation `Person` de l’entité `StudentGrade`.

Dans *Instructors.aspx.cs*, remplacez la méthode extraite `CourseDetailsEntityDataSource_Selected` par le code suivant :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

L’argument d’événement de cet événement fournit les données sélectionnées sous la forme d’une collection, qui n’aura aucun élément si rien n’est sélectionné ou un élément si une `Course` entité est sélectionnée. Si une entité de `Course` est sélectionnée, le code utilise la méthode `First` pour convertir la collection en un seul objet. Il obtient ensuite `StudentGrade` entités de la propriété de navigation, les convertit en une collection et lie le contrôle `GradesListView` à la collection.

Cela suffit pour afficher les notes, mais vous voulez vous assurer que le message dans le modèle de données vide s’affiche la première fois que la page est affichée et chaque fois qu’un cours n’est pas sélectionné. Pour ce faire, créez la méthode suivante, que vous allez appeler à partir de deux emplacements :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Appelez cette nouvelle méthode à partir de la méthode `Page_Load` pour afficher le modèle de données vide la première fois que la page est affichée. Et l’appellent à partir de la méthode `InstructorsGridView_SelectedIndexChanged`, car cet événement est déclenché lorsqu’un formateur est sélectionné, ce qui signifie que les nouveaux cours sont chargés dans les cours `GridView` contrôle et qu’aucun n’est encore sélectionné. Voici les deux appels :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Exécutez la page.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Sélectionnez un formateur auquel un cours est affecté, puis sélectionnez le cours.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Vous avez maintenant vu quelques façons d’utiliser les données associées. Dans le didacticiel suivant, vous allez apprendre à ajouter des relations entre des entités existantes, à supprimer des relations et à ajouter une nouvelle entité qui a une relation à une entité existante.

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-5.md)
