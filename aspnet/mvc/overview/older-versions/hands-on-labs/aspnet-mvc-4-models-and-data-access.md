---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Modèles ASP.NET MVC 4 et accès aux données | Microsoft Docs
author: rick-anderson
description: 'Remarque : ce laboratoire pratique suppose que vous avez une connaissance de base de ASP.NET MVC. Si vous n’avez pas encore utilisé ASP.NET MVC, nous vous recommandons de passer à ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560199"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>Modèles ASP.NET MVC 4 et accès aux données

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

Ce laboratoire pratique suppose que vous avez une connaissance de base de **ASP.NET MVC**. Si vous n’avez pas encore utilisé **ASP.NET MVC** , nous vous recommandons de passer au-dessus d’un laboratoire pratique de **notions de base sur ASP.NET MVC 4** .

Ce laboratoire vous guide tout au long des améliorations et des nouvelles fonctionnalités décrites précédemment en appliquant des modifications mineures à un exemple d’application Web fournie dans le dossier source.

> [!NOTE]
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet propre à ce laboratoire est disponible sur les [modèles ASP.NET MVC 4 et l’accès aux données](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

Dans le laboratoire pratique **ASP.NET MVC notions de base** , vous avez passé des données codées en dur des contrôleurs aux modèles de vue. Toutefois, pour générer une application Web réelle, vous souhaiterez peut-être utiliser une véritable base de données.

Ce laboratoire pratique vous montrera comment utiliser un moteur de base de données pour stocker et récupérer les données nécessaires à l’application du Store musical. Pour ce faire, vous allez démarrer avec une base de données existante et créer l’Entity Data Model à partir de celle-ci. Tout au long de ce laboratoire, vous allez rencontrer l’approche **Database First** , ainsi que l’approche **Code First** .

Toutefois, vous pouvez également utiliser l’approche **Model First** , créer le même modèle à l’aide des outils, puis générer la base de données à partir de celle-ci.

![Database First et Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First et Model First")

*Database First et Model First*

Après avoir généré le modèle, vous allez effectuer les ajustements appropriés dans le StoreController pour fournir les vues du magasin avec les données extraites de la base de données, au lieu d’utiliser des données codées en dur. Vous n’avez pas besoin d’apporter des modifications aux modèles de vue, car le StoreController retourne les mêmes ViewModels aux modèles de vue, bien que cette fois-ci, les données proviennent de la base de données.

**Approche Code First**

L’approche Code First nous permet de définir le modèle à partir du code sans générer des classes qui sont généralement associées à l’infrastructure.

Dans code First, les objets de modèle sont définis avec les POCO, &quot;les objets CLR plain&quot;. Les POCO sont des classes simples simples qui n’ont pas d’héritage et n’implémentent pas d’interfaces. Nous pouvons générer automatiquement la base de données à partir de ces bases de données ou utiliser une base de données existante et générer le mappage de la classe à partir du code.

L’utilisation de cette approche présente les avantages suivants : le modèle reste indépendant de l’infrastructure de persistance (dans ce cas, Entity Framework), car les classes POCO ne sont pas associées à l’infrastructure de mappage.

> [!NOTE]
> Ce laboratoire est basé sur ASP.NET MVC 4 et une version de l’exemple d’application Music Store personnalisée et réduite pour s’adapter uniquement aux fonctionnalités présentées dans ce laboratoire pratique.
> 
> Si vous souhaitez explorer l’ensemble de l’application didacticiel du **Store Music** , vous pouvez la trouver dans [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Installation

**Installation d’extraits de code**

Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio. Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .

Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe C : utilisation d’extraits de Code](#AppendixC)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique est constitué des exercices suivants :

1. [Exercice 1 : ajout d’une base de données](#Exercise1)
2. [Exercice 2 : création d’une base de données à l’aide d’Code First](#Exercise2)
3. [Exercice 3 : interrogation de la base de données avec des paramètres](#Exercise3)

> [!NOTE]
> Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices. Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.

Durée estimée pour effectuer ce laboratoire : **35 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Exercice 1 : ajout d’une base de données

Dans cet exercice, vous allez apprendre à ajouter une base de données avec les tables de l’application MusicStore à la solution afin de consommer ses données. Une fois que la base de données est générée avec le modèle et ajoutée à la solution, vous allez modifier la classe StoreController pour fournir le modèle de vue avec les données extraites de la base de données, au lieu d’utiliser des valeurs codées en dur.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Tâche 1 : ajout d’une base de données

Dans cette tâche, vous allez ajouter une base de données déjà créée avec les tables principales de l’application MusicStore à la solution.

1. Ouvrez le **début** de la solution situé à l’emplacement **source/EX1-AddingADatabaseDBFirst/Begin/** Folder.

   1. Vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ajoutez un fichier de base de données **MvcMusicStore** . Dans ce laboratoire pratique, vous allez utiliser une base de données déjà créée appelée **MvcMusicStore. mdf**. Pour ce faire, cliquez avec le bouton droit sur le dossier de données de l' **application\_** , pointez sur **Ajouter** , puis cliquez sur **élément existant**. Accédez à **\Source\Assets** et sélectionnez le fichier **MvcMusicStore. mdf** .

    ![Ajout d’un élément existant](aspnet-mvc-4-models-and-data-access/_static/image2.png "Ajout d’un élément existant")

    *Ajout d’un élément existant*

    ![Fichier de base de données MvcMusicStore. mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "Fichier de base de données MvcMusicStore. mdf")

    *Fichier de base de données MvcMusicStore. mdf*

    La base de données a été ajoutée au projet. Même lorsque la base de données se trouve dans la solution, vous pouvez l’interroger et la mettre à jour en l’hébergeant sur un serveur de base de données différent.

    ![Base de données MvcMusicStore dans Explorateur de solutions](aspnet-mvc-4-models-and-data-access/_static/image4.png "Base de données MvcMusicStore dans Explorateur de solutions")

    *Base de données MvcMusicStore dans Explorateur de solutions*
3. Vérifiez la connexion à la base de données. Pour ce faire, double-cliquez sur **MvcMusicStore. mdf** pour établir une connexion.

    ![Connexion à MvcMusicStore. mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connexion à MvcMusicStore. mdf")

    *Connexion à MvcMusicStore. mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Tâche 2 : création d’un modèle de données

Dans cette tâche, vous allez créer un modèle de données pour interagir avec la base de données ajoutée dans la tâche précédente.

1. Créez un modèle de données qui représente la base de données. Pour ce faire, dans Explorateur de solutions cliquez avec le bouton droit sur le dossier **Models** , pointez sur **Ajouter** , puis cliquez sur **nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez le modèle de **données** , puis l’élément **ADO.NET Entity Data Model** . Modifiez le nom du modèle de données en **StoreDB. edmx** , puis cliquez sur **Ajouter**.

    ![Ajout de la Entity Data Model ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Ajout de la Entity Data Model ADO.NET StoreDB")

    *Ajout de la Entity Data Model ADO.NET StoreDB*
2. L' **assistant Entity Data Model** s’affiche. Cet Assistant va vous guider tout au long de la création de la couche de modèle. Étant donné que le modèle doit être créé en fonction de la base de données existante récemment ajoutée, sélectionnez **générer à partir de la base de données** , puis cliquez sur **suivant**.

    ![Choix du contenu du modèle](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choix du contenu du modèle")

    *Choix du contenu du modèle*
3. Étant donné que vous générez un modèle à partir d’une base de données, vous devrez spécifier la connexion à utiliser. Cliquez sur **nouvelle connexion**.
4. Sélectionnez **Microsoft SQL Server fichier de base de données** , puis cliquez sur **Continuer**.

    ![Choisir la source de données](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choisir la source de données")

    *Boîte de dialogue Choisir la source de données*
5. Cliquez sur **Parcourir** et sélectionnez la base de données **MvcMusicStore. mdf** que vous avez située dans le dossier **application\_données** , puis cliquez sur **OK**.

    ![Propriétés de connexion](aspnet-mvc-4-models-and-data-access/_static/image9.png "Propriétés de connexion")

    *Propriétés de connexion*
6. La classe générée doit avoir le même nom que la chaîne de connexion de l’entité. par conséquent, remplacez son nom par **MusicStoreEntities** , puis cliquez sur **suivant**.

    ![Choix de la connexion de données](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choix de la connexion de données")

    *Choix de la connexion de données*
7. Choisissez les objets de base de données à utiliser. Étant donné que le modèle d’entité utilisera uniquement les tables de la base de données, sélectionnez l’option **tables** et assurez-vous que les options **inclure les colonnes clés étrangères dans le modèle** et mettre en **pluriel ou au singulier les noms d’objets générés** sont également sélectionnées. Remplacez l’espace de noms du modèle par **MvcMusicStore. Model** , puis cliquez sur **Terminer**.

    ![Choix des objets de base de données](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choix des objets de base de données")

    *Choix des objets de base de données*

    > [!NOTE]
    > Si une boîte de dialogue d’avertissement de sécurité s’affiche, cliquez sur **OK** pour exécuter le modèle et générer les classes pour les entités du modèle.
8. Un diagramme d’entité pour la base de données s’affiche, tandis qu’une classe distincte qui mappe chaque table à la base de données est créée. Par exemple, la table **albums** sera représentée par une classe **album** , où chaque colonne de la table sera mappée à une propriété de classe. Cela vous permet d’interroger et d’utiliser des objets qui représentent des lignes dans la base de données.

    ![Diagramme d’entité](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagramme des entités")

    *Diagramme d’entité*

    > [!NOTE]
    > Les modèles T4 (. TT) exécutent du code pour générer les classes d’entités et remplacent les classes existantes portant le même nom. Dans cet exemple, les classes &quot;&quot;d’album, &quot;genre&quot; et &quot;artiste&quot; ont été remplacées par le code généré.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Tâche 3 : création de l’application

Dans cette tâche, vous allez vérifier que, bien que la génération de modèle ait supprimé les classes de modèle **album**, **genre** et **artiste** , le projet est généré avec succès à l’aide des nouvelles classes de modèle de données.

1. Générez le projet en sélectionnant l’élément de menu **générer** , puis **créez MvcMusicStore**.

    ![Génération du projet](aspnet-mvc-4-models-and-data-access/_static/image13.png "Génération du projet")

    *Génération du projet*
2. Le projet est correctement généré. Pourquoi fonctionne-t-il toujours ? Cela fonctionne parce que les tables de base de données ont des champs qui incluent les propriétés que vous utilisiez dans l' **album** et le **genre**des classes supprimées.

    ![Builds réussies](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds réussies")

    *Builds réussies*
3. Tandis que le concepteur affiche les entités dans un format de diagramme, C# il s’agit vraiment de classes. Développez le nœud **StoreDB. edmx** dans le Explorateur de solutions puis **StoreDB.TT**, vous verrez les nouvelles entités générées.

    ![Fichiers générés](aspnet-mvc-4-models-and-data-access/_static/image15.png "Fichiers générés")

    *Fichiers générés*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tâche 4 : interrogation de la base de données

Dans cette tâche, vous allez mettre à jour la classe StoreController afin que, au lieu d’utiliser des données codées en dur, elle interroge la base de données pour récupérer les informations.

1. Ouvrez **Controllers\StoreController.cs** et ajoutez le champ suivant à la classe pour contenir une instance de la classe **MusicStoreEntities** , nommée **storeDB**:

    (Extrait de code- *modèles et accès aux données-EX1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. La classe **MusicStoreEntities** expose une propriété de collection pour chaque table de la base de données. Mettez à jour la méthode d’action **Browse** pour récupérer un genre avec tous les **albums**.

    (Extrait de code- *modèles et accès aux données-navigation dans le magasin de EX1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Vous utilisez une fonctionnalité de .NET appelée **LINQ** (Language-Integrated Query) pour écrire des expressions de requête fortement typées sur ces collections, qui exécutent du code sur la base de données et retournent des objets que vous pouvez programmer.
    > 
    > Pour plus d’informations sur LINQ, consultez le [site MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Mettez à jour la méthode d’action d' **index** pour récupérer tous les genres.

    (Extrait de code- *modèles et accès aux données-index du magasin EX1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Mettez à jour la méthode d’action d' **index** pour récupérer tous les genres et transformer la collection en liste.

    (Extrait de code- *modèles et accès aux données-EX1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’application

Au cours de cette tâche, vous allez vérifier que la page d’index de magasin affiche désormais les genres stockés dans la base de données au lieu de ceux qui sont codés en dur. Il n’est pas nécessaire de modifier le modèle de vue, car **StoreController** retourne les mêmes entités qu’auparavant, bien que cette fois-ci, les données proviennent de la base de données.

1. Régénérez la solution et appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Vérifiez que le menu des **genres** n’est plus une liste codée en dur et que les données sont récupérées directement à partir de la base de données.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Exploration des genres de la base de données*
3. À présent, accédez à n’importe quel genre et vérifiez que les albums sont remplis à partir de la base de données.

    ![Exploration des albums de la base de données](aspnet-mvc-4-models-and-data-access/_static/image17.png "Exploration des albums de la base de données")

    *Exploration des albums de la base de données*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Exercice 2 : création d’une base de données à l’aide d’Code First

Dans cet exercice, vous allez apprendre à utiliser l’approche Code First pour créer une base de données avec les tables de l’application MusicStore et comment accéder à ses données.

Une fois le modèle généré, vous allez modifier le StoreController pour fournir le modèle de vue avec les données extraites de la base de données, au lieu d’utiliser des valeurs codées en dur.

> [!NOTE]
> Si vous avez terminé l’exercice 1 et que vous avez déjà travaillé avec l’approche Database First, vous allez maintenant apprendre à obtenir les mêmes résultats avec un processus différent. Les tâches qui sont en commun avec l’exercice 1 ont été marquées pour faciliter votre lecture. Si vous n’avez pas terminé l’exercice 1 mais que vous souhaitez connaître l’approche Code First, vous pouvez commencer par cet exercice et obtenir une couverture complète de la rubrique.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Tâche 1 : remplissage des exemples de données

Dans cette tâche, vous allez remplir la base de données avec des exemples de données lorsqu’elle est créée initialement à l’aide de code-First.

1. Ouvrez le **début** de la solution situé dans **source/EX2-CreatingADatabaseCodeFirst/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ajoutez le fichier **SampleData.cs** au dossier **Models** . Pour ce faire, cliquez avec le bouton droit sur le dossier **Models** , pointez sur **Ajouter** , puis cliquez sur **élément existant**. Accédez à **\Source\Assets** et sélectionnez le fichier **SampleData.cs** .

    ![Code de remplissage des exemples de données](aspnet-mvc-4-models-and-data-access/_static/image18.png "Code de remplissage des exemples de données")

    *Code de remplissage des exemples de données*
3. Ouvrez le fichier **global.asax.cs** et ajoutez les instructions *using* suivantes.

    (Extrait de code- *modèles et accès aux données-EX2 global asax using*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. Dans la méthode **Application\_Start ()** , ajoutez la ligne suivante pour définir l’initialiseur de base de données.

    (Extrait de code- *modèles et accès aux données-EX2 global asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Tâche 2 : configuration de la connexion à la base de données

Maintenant que vous avez déjà ajouté une base de données à notre projet, vous allez écrire dans le fichier **Web. config** la chaîne de connexion.

1. Ajoutez une chaîne de connexion à **Web. config**. Pour ce faire, ouvrez le **fichier Web. config** à la racine du projet et remplacez la chaîne de connexion nommée DefaultConnection par cette ligne dans la section **&lt;connectionStrings&gt;** :

    ![Emplacement du fichier Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Emplacement du fichier Web. config")

    *Emplacement du fichier Web. config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Tâche 3 : utilisation du modèle

Maintenant que vous avez déjà configuré la connexion à la base de données, vous allez lier le modèle aux tables de la base de données. Dans cette tâche, vous allez créer une classe qui sera liée à la base de données avec Code First. N’oubliez pas qu’il existe une classe de modèle POCO existante qui doit être modifiée.

> [!NOTE]
> Si vous avez terminé l’exercice 1, vous remarquerez que cette étape a été effectuée par un Assistant. En procédant Code First, vous allez créer manuellement des classes qui seront liées aux entités de données.

1. Ouvrez le **genre** de classe de modèle POCO à partir du dossier de **projet Models** et incluez un ID. Utilisez une propriété int portant le nom **GenreId**.

    (Extrait de code- *modèles et accès aux données-Ex2 code First genre*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Pour utiliser des conventions de Code First, le genre de classe doit avoir une propriété de clé primaire qui sera détectée automatiquement.
    > 
    > Pour plus d’informations sur les conventions de Code First, consultez cet [article MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. À présent, ouvrez l' **album** de classe de modèle POCO à partir du dossier de **projet Models** et incluez les clés étrangères, puis créez des propriétés avec les noms **GenreId** et **ArtistId**. Cette classe a déjà le **GenreId** pour la clé primaire.

    (Extrait de code- *modèles et accès aux données-Ex2 code First album*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Ouvrez la classe de modèle POCO **Artist** et incluez la propriété **ArtistId** .

    (Extrait de code- *modèles et accès aux données-Ex2 code First Artist*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Cliquez avec le bouton droit sur le dossier du projet **Models** , puis sélectionnez **Ajouter | Classe**. Nommez le fichier **MusicStoreEntities.cs**. Cliquez ensuite sur **Ajouter.**

    ![Ajout d’une classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "Ajout d’une classe")

    *Ajout d’un nouvel élément*

    ![Ajout d’un Class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Ajout d’un Class2")

    *Ajout d’une classe*
5. Ouvrez la classe que vous venez de créer, **MusicStoreEntities.cs**, et incluez les espaces de noms **System. Data. Entity** et **System. Data. Entity. infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Remplacez la déclaration de classe pour étendre la classe **DbContext** : déclarez une **DBSet** publique et substituez la méthode **OnModelCreating** . Après cette étape, vous obtiendrez une classe de domaine qui lie votre modèle au Entity Framework. Pour ce faire, remplacez le code de la classe par le code suivant :

    (Extrait de code- *modèles et accès aux données-Ex2 code First MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Avec Entity Framework **DbContext** et **DBSet** , vous serez en mesure d’interroger le genre de la classe POCO. En étendant la méthode **OnModelCreating** , vous spécifiez dans le **code** comment le genre sera mappé à une table de base de données. Vous trouverez plus d’informations sur DBContext et DBSet dans cet article MSDN : [lien](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tâche 4 : interrogation de la base de données

Dans cette tâche, vous allez mettre à jour la classe StoreController afin que, au lieu d’utiliser des données codées en dur, elle la récupère de la base de données.

> [!NOTE]
> Cette tâche est en commun avec l’exercice 1.
> 
> Si vous avez terminé l’exercice 1, vous noterez que ces étapes sont les mêmes dans les deux approches (Database First ou code First). Elles diffèrent dans la façon dont les données sont liées au modèle, mais l’accès aux entités de données est encore transparent à partir du contrôleur.

1. Ouvrez **Controllers\StoreController.cs** et ajoutez le champ suivant à la classe pour contenir une instance de la classe **MusicStoreEntities** , nommée **storeDB**:

    (Extrait de code- *modèles et accès aux données-EX1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. La classe **MusicStoreEntities** expose une propriété de collection pour chaque table de la base de données. Mettez à jour la méthode d’action **Browse** pour récupérer un genre avec tous les **albums**.

    (Extrait de code- *modèles et accès aux données-EX2 Store Browse*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Vous utilisez une fonctionnalité de .NET appelée **LINQ** (Language-Integrated Query) pour écrire des expressions de requête fortement typées sur ces collections, qui exécutent du code sur la base de données et retournent des objets que vous pouvez programmer.
    > 
    > Pour plus d’informations sur LINQ, consultez le [site MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Mettez à jour la méthode d’action d' **index** pour récupérer tous les genres.

    (Extrait de code- *modèles et accès aux données-index EX2 Store*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Mettez à jour la méthode d’action d' **index** pour récupérer tous les genres et transformer la collection en liste.

    (Extrait de code- *modèles et accès aux données-EX2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’application

Au cours de cette tâche, vous allez vérifier que la page d’index de magasin affiche désormais les genres stockés dans la base de données au lieu de ceux qui sont codés en dur. Il n’est pas nécessaire de modifier le modèle de vue, car **StoreController** retourne le même **StoreIndexViewModel** qu’auparavant, mais cette fois-ci, les données proviennent de la base de données.

1. Régénérez la solution et appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Vérifiez que le menu des **genres** n’est plus une liste codée en dur et que les données sont récupérées directement à partir de la base de données.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Exploration des genres de la base de données*
3. À présent, accédez à n’importe quel genre et vérifiez que les albums sont remplis à partir de la base de données.

    ![Exploration des albums de la base de données](aspnet-mvc-4-models-and-data-access/_static/image23.png "Exploration des albums de la base de données")

    *Exploration des albums de la base de données*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Exercice 3 : interrogation de la base de données avec des paramètres

Dans cet exercice, vous allez apprendre à interroger la base de données à l’aide de paramètres et à utiliser la mise en forme des résultats de requête, une fonctionnalité qui réduit le nombre d’accès à la base de données qui récupèrent les données de façon plus efficace.

> [!NOTE]
> Pour plus d’informations sur la mise en forme des résultats de la requête, consultez l' [article MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)suivant.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Tâche 1 : modification de StoreController pour récupérer des albums de la base de données

Dans cette tâche, vous allez modifier la classe **StoreController** pour accéder à la base de données afin de récupérer les albums d’un genre spécifique.

1. Ouvrez la solution **Begin** située dans le dossier **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** si vous souhaitez utiliser l’approche code-First ou le dossier **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** si vous souhaitez utiliser l’approche basée sur la première base de données. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la classe **StoreController** pour modifier la méthode d’action **Browse** . Pour ce faire, dans le **Explorateur de solutions**, développez le dossier **Controllers** , puis double-cliquez sur **StoreController.cs**.
3. Modifiez la méthode d’action **Parcourir** pour récupérer des albums pour un genre spécifique. Pour ce faire, remplacez le code suivant :

    (Extrait de code- *modèles et accès aux données-EX3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Pour remplir une collection de l’entité, vous devez utiliser la méthode **include** pour spécifier que vous souhaitez également récupérer les albums. Vous pouvez utiliser le. Extension **unique ()** dans LINQ, car dans ce cas, un seul genre est attendu pour un album. La méthode **Single ()** prend une expression lambda en tant que paramètre, qui, dans ce cas, spécifie un objet de genre unique, de sorte que son nom correspond à la valeur définie.
> 
> Vous pouvez tirer parti d’une fonctionnalité qui vous permet d’indiquer d’autres entités associées que vous souhaitez charger également lorsque l’objet genre est récupéré. Cette fonctionnalité est appelée mise en **forme des résultats**de la requête et vous permet de réduire le nombre de fois où l’accès à la base de données est nécessaire pour récupérer des informations. Dans ce scénario, vous souhaiterez pré-extraire les albums pour le genre que vous récupérez.
> 
> La requête inclut **genres. include (&quot;les albums&quot;)** pour indiquer que vous souhaitez également obtenir des albums associés. Cela aboutit à une application plus efficace, car elle récupère à la fois les données de genre et d’album dans une requête de base de données unique.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tâche 2 : exécution de l’application

Au cours de cette tâche, vous allez exécuter l’application et récupérer des albums d’un genre spécifique à partir de la base de données.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/Store/Browse ? genre = pop** pour vérifier que les résultats sont récupérés à partir de la base de données.

    ![Exploration par genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Exploration par genre")

    *Exploration de/Store/Browse ? genre = pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Tâche 3 : accès aux albums par ID

Dans cette tâche, vous allez répéter la procédure précédente pour récupérer les albums en fonction de leur ID.

1. Fermez le navigateur si nécessaire pour revenir à Visual Studio. Ouvrez la classe **StoreController** pour modifier la méthode d’action **Details** . Pour ce faire, dans le **Explorateur de solutions**, développez le dossier **Controllers** , puis double-cliquez sur **StoreController.cs**.
2. Modifiez la méthode d’action **Details** pour récupérer les détails des albums en fonction de leur **ID**. Pour ce faire, remplacez le code suivant :

    (Extrait de code- *modèles et accès aux données-EX3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’application

Dans cette tâche, vous allez exécuter l’application dans un navigateur Web et obtenir les détails de l’album en fonction de son ID.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/Store/Details/51** ou parcourez les genres et sélectionnez un album pour vérifier que les résultats sont récupérés à partir de la base de données.

    ![Exploration des détails](aspnet-mvc-4-models-and-data-access/_static/image25.png "Exploration des détails")

    *Navigation dans/Store/Details/51*

> [!NOTE]
> En outre, vous pouvez déployer cette application sur les sites Web Windows Azure à l' [annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En effectuant ce laboratoire pratique, vous avez appris les principes de base des modèles MVC ASP.NET et l’accès aux données, à l’aide de l’approche **Database First** , ainsi que de l’approche **Code First** :

- Comment ajouter une base de données à la solution afin de consommer ses données
- Comment mettre à jour des contrôleurs pour fournir des modèles d’affichage avec les données extraites de la base de données au lieu d’un en codage codé en dur
- Comment interroger la base de données à l’aide de paramètres
- Comment utiliser la mise en forme des résultats de la requête, une fonctionnalité qui réduit le nombre d’accès aux bases de données, la récupération des données de façon plus efficace
- Comment utiliser les Database First et les approches de Code First dans Microsoft Entity Framework pour lier la base de données au modèle

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe A : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    *Vignette VS Express pour le Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

Cette annexe vous indique comment créer un nouveau site Web à partir de Windows Azure Portail de gestion et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tâche 1 : création d’un nouveau site Web à partir du portail Windows Azure

1. Accédez au [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Windows Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente. Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).

    ![Ouvrir une session sur Windows Portail Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Ouvrir une session sur Windows Portail Azure")

    *Connectez-vous à Windows Azure Portail de gestion*
2. Cliquez sur **nouveau** dans la barre de commandes.

    ![Création d’un nouveau site Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "Création d’un nouveau site Web")

    *Création d’un nouveau site Web*
3. Cliquez sur **compute** | **site Web**. Sélectionnez ensuite l’option **création rapide** . Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.

    > [!NOTE]
    > Un site Web Windows Azure est l’hôte d’une application Web s’exécutant dans le Cloud que vous pouvez contrôler et gérer. L’option création rapide vous permet de déployer une application Web terminée sur le site Web Windows Azure depuis l’extérieur du portail. Elle n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un nouveau site Web à l’aide de la création rapide](aspnet-mvc-4-models-and-data-access/_static/image33.png "Création d’un nouveau site Web à l’aide de la création rapide")

    *Création d’un nouveau site Web à l’aide de la création rapide*
4. Patientez jusqu’à la création du nouveau **site Web** .
5. Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** . Vérifiez que le nouveau site Web fonctionne.

    ![Navigation vers le nouveau site Web](aspnet-mvc-4-models-and-data-access/_static/image34.png "Navigation vers le nouveau site Web")

    *Navigation vers le nouveau site Web*

    ![Site Web en cours d’exécution](aspnet-mvc-4-models-and-data-access/_static/image35.png "Site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.

    ![Ouverture des pages de gestion de site Web](aspnet-mvc-4-models-and-data-access/_static/image36.png "Ouverture des pages de gestion de site Web")

    *Ouverture des pages de gestion de site Web*
7. Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .

    > [!NOTE]
    > Le *profil de publication* contient toutes les informations requises pour publier une application Web sur un site Web Windows Azure pour chaque méthode de publication activée. Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur les sites Web Windows Azure.

    ![Téléchargement du profil de publication du site Web](aspnet-mvc-4-models-and-data-access/_static/image37.png "Téléchargement du profil de publication du site Web")

    *Téléchargement du profil de publication du site Web*
8. Téléchargez le fichier de profil de publication dans un emplacement connu. Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur un site Web Windows Azure à partir de Visual Studio.

    ![Enregistrement du fichier de profil de publication](aspnet-mvc-4-models-and-data-access/_static/image38.png "Enregistrement du profil de publication")

    *Enregistrement du fichier de profil de publication*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database. Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.

1. Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application. Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Windows Azure, à partir des **bases de données SQL** | **serveurs** | **tableau de bord du serveur**. Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes. Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.

    ![Tableau de bord du serveur SQL Database](aspnet-mvc-4-models-and-data-access/_static/image39.png "Tableau de bord du serveur SQL Database")

    *Tableau de bord du serveur SQL Database*
2. Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](aspnet-mvc-4-models-and-data-access/_static/image40.png).

    ![Ajout de l’adresse IP du client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Ajout de l’adresse IP du client*
3. Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Confirmer les modifications*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.

    ![Publication de l’application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publication de l'application")

    *Publication du site Web*
2. Importez le profil de publication que vous avez enregistré dans la première tâche.

    ![Importation du profil de publication](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la validation terminée, cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.

    ![Validation de la connexion](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validation de la connexion")

    *Validation de la connexion*
4. Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).

    ![Configuration de Web Deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Configuration de Web Deploy")

    *Configuration de Web Deploy*
5. Configurez la connexion de base de données comme suit :

   - Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .
   - Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.
   - Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.
   - Tapez un nouveau nom de base de données.

     ![Configuration de la chaîne de connexion de destination](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuration de la chaîne de connexion de destination")

     *Configuration de la chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](aspnet-mvc-4-models-and-data-access/_static/image48.png "Création de la chaîne de base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Chaîne de connexion pointant vers SQL Database")

    *Chaîne de connexion pointant vers SQL Database*
8. Dans la page **Aperçu** , cliquez sur **publier**.

    ![Publication de l’application Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publication de l’application Web")

    *Publication de l’application Web*
9. Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Annexe C : utilisation d’extraits de code

Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main. Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.

![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-models-and-data-access/_static/image51.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")

*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***

1. Placez le curseur à l’endroit où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).
3. Regarder comme IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).
5. Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-models-and-data-access/_static/image52.png "Commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-models-and-data-access/_static/image53.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*

![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-models-and-data-access/_static/image54.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")

*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*

***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0. Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.

1. Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.
2. Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.

![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-models-and-data-access/_static/image55.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")

*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*

![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-models-and-data-access/_static/image56.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")

*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*
