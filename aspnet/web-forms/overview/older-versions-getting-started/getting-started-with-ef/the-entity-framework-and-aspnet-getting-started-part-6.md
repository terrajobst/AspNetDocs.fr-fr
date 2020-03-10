---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 6 | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications de Web Forms ASP.NET à l’aide de l’Entity Framework. L’exemple d’application est...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 8bfbe74f90eb03ea9aab7610842ef2578e80d113
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564224"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-partie 6

par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="implementing-table-per-hierarchy-inheritance"></a>Implémentation de l'héritage TPH (table par hiérarchie)

Dans le didacticiel précédent, vous travailliez avec des données associées en ajoutant et en supprimant des relations, et en ajoutant une nouvelle entité qui avait une relation avec une entité existante. Ce didacticiel vous indiquera comment implémenter l’héritage dans le modèle de données.

Dans la programmation orientée objet, vous pouvez utiliser l’héritage pour faciliter l’utilisation des classes associées. Par exemple, vous pouvez créer des classes `Instructor` et `Student` qui dérivent d’une classe de base `Person`. Vous pouvez créer les mêmes types de structures d’héritage parmi les entités du Entity Framework.

Dans cette partie du didacticiel, vous ne créez pas de nouvelles pages Web. Au lieu de cela, vous allez ajouter des entités dérivées au modèle de données et modifier les pages existantes pour utiliser les nouvelles entités.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Héritage TPH (table par hiérarchie) et table par type

Une base de données peut stocker des informations sur des objets connexes dans une table ou dans plusieurs tables. Par exemple, dans la base de données `School`, la table `Person` contient des informations sur les étudiants et les instructeurs dans une table unique. Certaines des colonnes s’appliquent uniquement aux formateurs (`HireDate`), certains uniquement aux élèves (`EnrollmentDate`) et d’autres aux deux (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Vous pouvez configurer la Entity Framework pour créer des entités `Instructor` et `Student` qui héritent de l’entité `Person`. Ce modèle de génération d’une structure d’héritage d’entité à partir d’une seule table de base de données est appelé l’héritage TPH ( *table par hiérarchie* ).

Pour les cours, la base de données `School` utilise un modèle différent. Les cours en ligne et les cours sur site sont stockés dans des tables distinctes, chacune ayant une clé étrangère qui pointe vers la table `Course`. Les informations communes aux deux types de cours sont stockées uniquement dans la table `Course`.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Vous pouvez configurer le modèle de données Entity Framework afin que les entités `OnlineCourse` et `OnsiteCourse` héritent de l’entité `Course`. Ce modèle de génération d’une structure d’héritage d’entité à partir de tables distinctes pour chaque type, avec chaque table distincte faisant référence à une table qui stocke les données communes à tous les types, est appelé l’héritage de *table par type* (TPT).

Les modèles d’héritage TPH offrent généralement de meilleures performances dans le Entity Framework que les modèles d’héritage TPT, car les modèles TPT peuvent entraîner des requêtes de jointure complexes. Cette procédure pas à pas montre comment implémenter l’héritage TPH. Pour ce faire, procédez comme suit :

- Créez `Instructor` et les types d’entités `Student` qui dérivent de `Person`.
- Déplacez les propriétés relatives aux entités dérivées de l’entité `Person` vers les entités dérivées.
- Définir des contraintes sur les propriétés dans les types dérivés.
- Faites de l’entité `Person` une entité abstraite.
- Mappez chaque entité dérivée à la table `Person` avec une condition qui spécifie comment déterminer si une ligne `Person` représente ce type dérivé.

## <a name="adding-instructor-and-student-entities"></a>Ajout d’entités de formateur et d’étudiant

Ouvrez le fichier <em>SchoolModel. edmx</em> , cliquez avec le bouton droit sur une zone non occupée dans le concepteur, sélectionnez <strong>Ajouter</strong>, puis sélectionnez <strong>entité</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

Dans la boîte de dialogue **Ajouter une entité** , nommez l’entité `Instructor` et définissez son **type de base** sur `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Cliquez sur **OK**. Le concepteur crée une entité `Instructor` qui dérive de l’entité `Person`. La nouvelle entité n’a pas encore de propriétés.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Répétez la procédure pour créer une entité `Student` qui dérive également de `Person`.

Seuls les formateurs ont des dates d’embauche. vous devez donc déplacer cette propriété de l’entité `Person` vers l’entité `Instructor`. Dans l’entité `Person`, cliquez avec le bouton droit sur la propriété `HireDate`, puis cliquez sur **couper**. Cliquez ensuite avec le bouton droit sur **Propriétés** dans l’entité `Instructor`, puis cliquez sur **coller**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

La date d’embauche d’une entité de `Instructor` ne peut pas être null. Cliquez avec le bouton droit sur la propriété `HireDate`, cliquez sur **Propriétés**, puis, dans la fenêtre **propriétés** , remplacez `Nullable` par `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Répétez la procédure pour déplacer la propriété `EnrollmentDate` de l’entité `Person` vers l’entité `Student`. Assurez-vous que vous définissez également `Nullable` sur `False` pour la propriété `EnrollmentDate`.

Maintenant qu’une entité `Person` a uniquement les propriétés qui sont communes aux entités `Instructor` et `Student` (à l’exception des propriétés de navigation que vous ne déplacez pas), l’entité ne peut être utilisée qu’en tant qu’entité de base dans la structure d’héritage. Par conséquent, vous devez vous assurer qu’elle n’est jamais traitée comme une entité indépendante. Cliquez avec le bouton droit sur l’entité `Person`, sélectionnez **Propriétés**, puis, dans la fenêtre **Propriétés** , affectez la valeur **true**à la propriété **abstract** .

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mappage des entités Instructor et Student à la table Person

À présent, vous devez dire à la Entity Framework comment faire la différence entre les entités `Instructor` et `Student` dans la base de données.

Cliquez avec le bouton droit sur l’entité `Instructor`, puis sélectionnez **mappage de table**. Dans la fenêtre **Détails de mappage** , cliquez sur **Ajouter une table ou une vue** , puis sélectionnez **personne**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Cliquez sur **Ajouter une condition**, puis sélectionnez **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Remplacez l' **opérateur** par **is** et **Value/Property** par **not null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Répétez la procédure pour l’entité `Students`, en spécifiant que cette entité est mappée à la table `Person` lorsque la colonne `EnrollmentDate` n’a pas la valeur null. Ensuite, enregistrez et fermez le modèle de données.

Générez le projet afin de créer les nouvelles entités en tant que classes et les rendre disponibles dans le concepteur.

## <a name="using-the-instructor-and-student-entities"></a>Utilisation des entités Instructor et Student

Lorsque vous avez créé les pages Web qui fonctionnent avec les données des étudiants et des enseignants, vous les affectez au jeu d’entités `Person`, et vous filtrez sur la propriété `HireDate` ou `EnrollmentDate` pour limiter les données renvoyées aux élèves ou aux formateurs. Toutefois, lorsque vous liez chaque contrôle de source de données au jeu d’entités `Person`, vous pouvez spécifier que seuls les types d’entités `Student` ou `Instructor` doivent être sélectionnés. Étant donné que le Entity Framework sait comment différencier les élèves et les formateurs dans le jeu d’entités `Person`, vous pouvez supprimer les paramètres de propriété `Where` que vous avez entrés manuellement.

Dans le concepteur Visual Studio, vous pouvez spécifier le type d’entité qu’un contrôle de `EntityDataSource` doit sélectionner dans la zone de liste déroulante **EntityTypeFilter** de l’Assistant `Configure Data Source`, comme indiqué dans l’exemple suivant.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Et dans la fenêtre **Propriétés** , vous pouvez supprimer `Where` valeurs de clause qui ne sont plus nécessaires, comme illustré dans l’exemple suivant.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Toutefois, étant donné que vous avez modifié le balisage des contrôles `EntityDataSource` pour utiliser l’attribut `ContextTypeName`, vous ne pouvez pas exécuter l’Assistant Configuration de la **source de données** sur `EntityDataSource` contrôles que vous avez déjà créés. Par conséquent, vous effectuerez les modifications requises en modifiant le balisage à la place.

Ouvrez la page *students. aspx* . Dans le contrôle `StudentsEntityDataSource`, supprimez l’attribut `Where` et ajoutez un attribut `EntityTypeFilter="Student"`. Le balisage ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

La définition de l’attribut `EntityTypeFilter` permet de s’assurer que le contrôle `EntityDataSource` sélectionne uniquement le type d’entité spécifié. Si vous souhaitez récupérer à la fois les `Student` et les types d’entités `Instructor`, vous ne devez pas définir cet attribut. (Vous avez la possibilité de récupérer plusieurs types d’entité avec un contrôle `EntityDataSource` uniquement si vous utilisez le contrôle pour l’accès aux données en lecture seule. Si vous utilisez un contrôle de `EntityDataSource` pour insérer, mettre à jour ou supprimer des entités, et si le jeu d’entités auquel il est lié peut contenir plusieurs types, vous ne pouvez utiliser qu’un seul type d’entité, et vous devez définir cet attribut.)

Répétez la procédure pour le contrôle `SearchEntityDataSource`, sauf si vous supprimez uniquement la partie de l’attribut `Where` qui sélectionne `Student` entités au lieu de supprimer complètement la propriété. La balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Exécutez la page pour vérifier qu’elle fonctionne toujours comme auparavant.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Mettez à jour les pages suivantes que vous avez créées dans les didacticiels précédents afin qu’elles utilisent les nouvelles `Student` et `Instructor` entités au lieu des entités `Person`, puis exécutez-les pour vérifier qu’elles fonctionnent comme avant :

- Dans *StudentsAdd. aspx*, ajoutez `EntityTypeFilter="Student"` au contrôle `StudentsEntityDataSource`. Le balisage ressemble maintenant à l’exemple suivant : 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- Dans *à propos de. aspx*, ajoutez `EntityTypeFilter="Student"` au contrôle `StudentStatisticsEntityDataSource` et supprimez `Where="it.EnrollmentDate is not null"`. Le balisage ressemble maintenant à l’exemple suivant : 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- Dans les *enseignants. aspx* et *InstructorsCourses. aspx*, ajoutez `EntityTypeFilter="Instructor"` au contrôle `InstructorsEntityDataSource` et supprimez `Where="it.HireDate is not null"`. Le balisage dans *Pastructors. aspx* ressemble maintenant à l’exemple suivant : 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Le balisage dans *InstructorsCourses. aspx* ressemble maintenant à l’exemple suivant :

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![Image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Suite à ces modifications, vous avez amélioré la maintenabilité de l’application Contoso University de plusieurs façons. Vous avez déplacé la logique de sélection et de validation hors de la couche d’interface utilisateur (balisage *. aspx* ) et nous l’avons fait partie intégrante de la couche d’accès aux données. Cela permet d’isoler votre code d’application des modifications que vous pouvez apporter à l’avenir au schéma de base de données ou au modèle de données. Par exemple, vous pouvez décider que les élèves peuvent être recrutés en tant qu’aides aux enseignants et donc obtenir une date d’embauche. Vous pouvez ensuite ajouter une nouvelle propriété pour différencier les élèves des enseignants et mettre à jour le modèle de données. Aucun code de l’application Web ne doit changer, sauf si vous souhaitez afficher une date d’embauche pour les élèves. Un autre avantage de l’ajout d’entités `Instructor` et `Student` est que votre code est plus facile à comprendre que lorsqu’il est référencé `Person` objets qui étaient en fait des étudiants ou des formateurs.

Vous avez maintenant vu un moyen d’implémenter un modèle d’héritage dans le Entity Framework. Dans le didacticiel suivant, vous allez apprendre à utiliser des procédures stockées afin d’avoir davantage de contrôle sur la façon dont le Entity Framework accède à la base de données.

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-7.md)
