---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implémentation de l’héritage avec l’Entity Framework dans une application ASP.NET MVC (8 sur 10) | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9507cba71b976825257cc9948d54f2651355959d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595318"
---
# <a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implémentation de l’héritage avec l’Entity Framework dans une application ASP.NET MVC (8 sur 10)

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 Code First et de Visual Studio 2012. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Vous pouvez démarrer la série de didacticiels depuis le début ou [Télécharger un projet de démarrage pour ce chapitre](building-the-ef5-mvc4-chapter-downloads.md) et démarrer ici.
> 
> > [!NOTE] 
> > 
> > Si vous rencontrez un problème que vous ne pouvez pas résoudre, [Téléchargez le chapitre terminé](building-the-ef5-mvc4-chapter-downloads.md) et essayez de reproduire le problème. Vous pouvez généralement trouver la solution au problème en comparant votre code au code terminé. Pour obtenir des erreurs courantes et comment les résoudre, consultez [Erreurs et solutions de contournement.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

Dans le didacticiel précédent, vous avez géré les exceptions d’accès concurrentiel. Ce didacticiel vous indiquera comment implémenter l’héritage dans le modèle de données.

Dans la programmation orientée objet, vous pouvez utiliser l’héritage pour éliminer le code redondant. Dans ce didacticiel, vous allez modifier les classes `Instructor` et `Student` afin qu’elles dérivent d’une classe de base `Person` qui contient des propriétés telles que `LastName`, communes aux formateurs et aux étudiants. Vous n’ajouterez ni ne modifierez aucune page web, mais vous modifierez une partie du code et ces modifications seront automatiquement répercutées dans la base de données.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Héritage TPH (table par hiérarchie) et table par type

Dans la programmation orientée objet, vous pouvez utiliser l’héritage pour faciliter l’utilisation des classes associées. Par exemple, les classes `Instructor` et `Student` dans le modèle de données `School` partagent plusieurs propriétés, ce qui aboutit à du code redondant :

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Supposons que vous souhaitez éliminer le code redondant pour les propriétés partagées par les entités `Instructor` et `Student`. Vous pouvez créer une classe de base `Person` qui contient uniquement les propriétés partagées, puis faire en sorte que les entités `Instructor` et `Student` héritent de cette classe de base, comme le montre l’illustration suivante :

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Il existe plusieurs façons de représenter cette structure d’héritage dans la base de données. Vous pouvez avoir une table `Person` qui contient des informations sur les étudiants et les formateurs dans une seule table. Certaines des colonnes ne peuvent s’appliquer qu’aux formateurs (`HireDate`), certains uniquement aux élèves (`EnrollmentDate`), certains aux deux (`LastName`, `FirstName`). En règle générale, vous avez une colonne de *discriminateur* pour indiquer le type représenté par chaque ligne. Par exemple, la colonne de discriminateur peut avoir « Instructor » pour les formateurs et « Student » pour les étudiants.

![Table par hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Ce modèle de génération d’une structure d’héritage d’entité à partir d’une seule table de base de données est appelé l’héritage TPH ( *table par hiérarchie* ).

Une alternative consiste à faire en sorte que la base de données ressemble plus à la structure d’héritage. Par exemple, vous pouvez avoir uniquement les champs nom dans la table `Person` et avoir des tables `Instructor` et `Student` distinctes avec les champs de date.

![Table par type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Ce modèle de création d’une table de base de données pour chaque classe d’entité est appelé l’héritage de *table par type* (TPT).

Les modèles d’héritage TPH offrent généralement de meilleures performances dans le Entity Framework que les modèles d’héritage TPT, car les modèles TPT peuvent entraîner des requêtes de jointure complexes. Ce didacticiel montre comment implémenter l’héritage TPH. Pour ce faire, procédez comme suit :

- Créez une classe `Person` et modifiez les classes `Instructor` et `Student` à dériver de `Person`.
- Ajoutez le code de mappage modèle-à-base de données à la classe de contexte de base de données.
- Modifiez `InstructorID` et `StudentID` références dans le projet à `PersonID`.

## <a name="creating-the-person-class"></a>Création de la classe Person

 Remarque : vous ne pourrez pas compiler le projet après avoir créé les classes ci-dessous tant que vous n’aurez pas mis à jour les contrôleurs qui utilisent ces classes. 

Dans le dossier *Models* , créez *Person.cs* et remplacez le code du modèle par le code suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Dans *Instructor.cs*, dérivez la classe `Instructor` de la classe `Person` et supprimez les champs clé et nom. Le code ressemblera à l’exemple suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Apportez des modifications similaires à *Student.cs*. La classe `Student` se présente comme dans l’exemple suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Ajout du type d’entité Person au modèle

Dans *SchoolContext.cs*, ajoutez une propriété `DbSet` pour le type d’entité `Person` :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

C’est là tout ce dont Entity Framework a besoin pour configurer l’héritage TPH (table par hiérarchie). Comme vous le verrez, lorsque la base de données est recréée, elle aura une table `Person` à la place des tables `Student` et `Instructor`.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Modification de InstructorID et StudentID en PersonID

Dans *SchoolContext.cs*, dans l’instruction de mappage Instructor-course, remplacez `MapRightKey("InstructorID")` par `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Cette modification n’est pas obligatoire. elle modifie simplement le nom de la colonne InstructorID dans la table de jointure plusieurs-à-plusieurs. Si vous avez laissé le nom en tant que InstructorID, l’application fonctionnera toujours correctement. Voici le *SchoolContext.cs*terminé :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Ensuite, vous devez modifier `InstructorID` en `PersonID` et `StudentID` pour `PersonID` tout le projet ***, sauf*** dans les fichiers de migrations horodatés dans le dossier *migrations* . Pour ce faire, vous devez rechercher et ouvrir uniquement les fichiers qui doivent être modifiés, puis effectuer une modification globale sur les fichiers ouverts. Le seul fichier du dossier *migrations* que vous devez modifier est *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Commencez par fermer tous les fichiers ouverts dans Visual Studio.
2. Cliquez sur **Rechercher et remplacer--Rechercher tous les fichiers** dans le menu **Edition** , puis recherchez tous les fichiers du projet qui contiennent des `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Ouvrez chaque fichier dans la fenêtre **résultats** de la recherche, ***à l’exception*** des &lt;horodatage&gt;\_fichiers de migration *. cs* dans le dossier *migrations* , en double-cliquant sur une ligne pour chaque fichier.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Ouvrez la boîte de dialogue **remplacer dans les fichiers** et remplacez **regarder dans** par **tous les documents ouverts**.
5. Utilisez la boîte de dialogue **remplacer dans les fichiers** pour remplacer tous les `InstructorID` par `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Recherchez tous les fichiers du projet qui contiennent `StudentID`.
7. Ouvrez chaque fichier dans la fenêtre résultats de la **recherche** , ***à l’exception*** des &lt;horodatage&gt;\_fichiers de migration *\*. cs* dans le dossier *migrations* , en double-cliquant sur une ligne pour chaque fichier.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Ouvrez la boîte de dialogue **remplacer dans les fichiers** et remplacez **regarder dans** par **tous les documents ouverts**.
9. Utilisez la boîte de dialogue **remplacer dans les fichiers** pour remplacer tous les `StudentID` par des `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. créer le projet ;

(Notez que cela démontre un *inconvénient* du modèle de `classnameID` pour nommer des clés primaires. Si vous aviez nommé ID des clés primaires sans préfixer le nom de la classe, *aucun* changement de nom n’est nécessaire maintenant.)

## <a name="create-and-update-a-migrations-file"></a>Créer et mettre à jour un fichier de migration

Dans la console du gestionnaire de package (PMC), entrez la commande suivante :

`Add-Migration Inheritance`

Exécutez la commande `Update-Database` dans le PMC. La commande échoue à ce stade, car nous avons des données existantes que les migrations ne savent pas gérer. Vous recevez l’erreur suivante :

*L’instruction ALTER TABLE est en conflit avec la contrainte de clé étrangère «FK\_dbo. Service\_dbo. Person\_PersonID». Le conflit s’est produit dans la base de données « ContosoUniversity », table «dbo. Person», colonne « PersonID ».*

Ouvrez *migrations\&lt ; timestamp&gt;\_Inheritance.cs* et remplacez la méthode `Up` par le code suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Réexécutez la commande `update-database`.

> [!NOTE]
> Il est possible d’établir d’autres erreurs lors de la migration des données et d’apporter des modifications au schéma. Si vous recevez des erreurs de migration que vous ne pouvez pas résoudre, vous pouvez poursuivre le didacticiel en modifiant la chaîne de connexion dans le fichier *Web. config* ou en supprimant la base de données. L’approche la plus simple consiste à renommer la base de données dans le fichier *Web. config* . Par exemple, remplacez le nom de la base de données par CU\_test comme indiqué dans l’exemple suivant :
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Avec une nouvelle base de données, il n’y a pas de données à migrer et la commande `update-database` est bien plus susceptible de s’exécuter sans erreur. Pour obtenir des instructions sur la suppression de la base de données, consultez Suppression d' [une base de données de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Si vous adoptez cette approche pour poursuivre le didacticiel, ignorez l’étape de déploiement à la fin de ce didacticiel, puisque le site déployé obtiendrait la même erreur lors de l’exécution automatique des migrations. Si vous souhaitez résoudre les problèmes liés à une erreur de migration, la meilleure ressource est l’un des Entity Framework forums ou StackOverflow.com.

## <a name="testing"></a>Test

Exécutez le site et essayez différentes pages. Tout fonctionne comme avant.

Dans **Explorateur de serveurs,** développez **SchoolContext** , puis **tables**, et vous constatez que les tables **Student** et **Instructor** ont été remplacées par une table **Person** . Développez la table **Person** . vous constatez qu’elle contient toutes les colonnes qui étaient utilisées dans les tables **Student** et **Instructor** .

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Cliquez avec le bouton droit sur la table Person, puis cliquez sur **Afficher les données de la table** pour voir la colonne de discriminateur.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Le diagramme suivant illustre la structure de la nouvelle base de données School :

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Récapitulatif

L’héritage TPH (table par hiérarchie) est maintenant implémenté pour les classes `Person`, `Student`et `Instructor`. Pour plus d’informations à ce sujet et d’autres structures d’héritage, consultez [stratégies de mappage d’héritage](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) sur le blog de Morteza Manavi. Dans le didacticiel suivant, vous verrez des méthodes pour implémenter le référentiel et les modèles d’unité de travail.

Vous trouverez des liens vers d’autres ressources de Entity Framework dans le [plan de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
