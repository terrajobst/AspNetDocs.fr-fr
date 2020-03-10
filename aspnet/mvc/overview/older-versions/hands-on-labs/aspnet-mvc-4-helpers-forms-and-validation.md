---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET, formulaires et validation de MVC 4 | Microsoft Docs
author: rick-anderson
description: Dans les modèles ASP.NET MVC 4 et l’atelier pratique d’accès aux données, vous avez chargé et affiché les données de la base de données. Dans ce laboratoire pratique, vous allez ajouter à...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539577"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Helpers, formulaires et validation d’ASP.NET MVC 4

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

Dans les **modèles ASP.NET MVC 4 et** l’atelier pratique d’accès aux données, vous avez chargé et affiché les données de la base de données. Dans ce laboratoire pratique, vous allez ajouter à l’application du **Store musical** la possibilité de modifier ces données.

Avec cet objectif à l’esprit, vous allez d’abord créer le contrôleur qui prendra en charge les actions de création, lecture, mise à jour et suppression (CRUD) des albums. Vous allez générer un modèle d’affichage d’index en tirant parti de la fonctionnalité de génération de modèles automatique de ASP.NET MVC pour afficher les propriétés des albums dans un tableau HTML. Pour améliorer cette vue, vous allez ajouter un programme d’assistance HTML personnalisé qui va tronquer les descriptions longues.

Ensuite, vous allez ajouter les affichages modifier et créer qui vous permettront de modifier les albums dans la base de données, à l’aide d’éléments de formulaire tels que les listes déroulantes.

Enfin, vous permettez aux utilisateurs de supprimer un album. vous les empêchera également d’entrer des données erronées en validant leur entrée.

Ce laboratoire pratique suppose que vous avez une connaissance de base de **ASP.NET MVC**. Si vous n’avez pas encore utilisé **ASP.NET MVC** , nous vous recommandons de passer au-dessus d’un laboratoire pratique de **notions de base sur ASP.NET MVC** .

Ce laboratoire vous guide tout au long des améliorations et des nouvelles fonctionnalités décrites précédemment en appliquant des modifications mineures à un exemple d’application Web fournie dans le dossier source.

> [!NOTE]
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet propre à ce laboratoire est disponible à l’adresse [ASP.net, formulaires et validation de MVC 4](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Créer un contrôleur pour prendre en charge les opérations CRUD
- Générer une vue d’index pour afficher les propriétés d’entité dans une table HTML
- Ajouter un programme d’assistance HTML personnalisé
- Créer et personnaliser un affichage de modification
- Faire la différence entre les méthodes d’action qui réagissent à des appels HTTP-is ou HTTP-POSTCONNEXION
- Ajouter et personnaliser une vue Create
- Gérer la suppression d’une entité
- Valider les entrées utilisateur

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

Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe B : utilisation d’extraits de Code](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Les exercices suivants composent ce laboratoire pratique :

1. [Création du contrôleur Store Manager et de sa vue index](#Exercise1)
2. [Ajout d’un programme d’assistance HTML](#Exercise2)
3. [Création de la vue Edit](#Exercise3)
4. [Ajout d’une vue Create](#Exercise4)
5. [Gestion de la suppression](#Exercise5)
6. [Ajout de la validation](#Exercise6)
7. [Utilisation de jQuery discrète côté client](#Exercise7)

> [!NOTE]
> Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices. Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.

Durée estimée pour effectuer ce laboratoire : **60 minutes**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Exercice 1 : création du contrôleur Store Manager et de sa vue index

Dans cet exercice, vous allez apprendre à créer un nouveau contrôleur pour prendre en charge les opérations CRUD, à personnaliser sa méthode d’action d’index pour retourner une liste d’albums de la base de données et enfin à générer un modèle de vue d’index tirant parti de la génération de modèles automatique de ASP.NET MVC. pour afficher les propriétés des albums dans un tableau HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Tâche 1 : création de la StoreManagerController

Dans cette tâche, vous allez créer un nouveau contrôleur appelé **StoreManagerController** pour prendre en charge les opérations CRUD.

1. Ouvrez le **début** de la solution situé à l’emplacement **source/EX1-CreatingTheStoreManagerController/Begin/** Folder.

   1. Vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ajoutez un nouveau contrôleur. Pour ce faire, cliquez avec le bouton droit sur le dossier **Controllers** dans le Explorateur de solutions, sélectionnez **Ajouter** , puis la commande **contrôleur** . Remplacez le **nom** du contrôleur par **StoreManagerController** et assurez-vous que l’option **contrôleur MVC avec actions de lecture/écriture vides** est sélectionnée. Cliquez sur **Ajouter**.

    ![Boîte de dialogue Ajouter un contrôleur](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Boîte de dialogue Ajouter un contrôleur")

    *Boîte de dialogue Ajouter un contrôleur*

    Une nouvelle classe de contrôleur est générée. Étant donné que vous avez indiqué d’ajouter des actions en lecture/écriture, des méthodes stub pour celles-ci, les actions CRUD courantes sont créées avec des commentaires TODO remplis, invitant à inclure la logique spécifique de l’application.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Tâche 2 : personnalisation de l’index StoreManager

Dans cette tâche, vous allez personnaliser la méthode d’action d’index StoreManager pour retourner une vue avec la liste des albums de la base de données.

1. Dans la classe StoreManagerController, ajoutez les directives *using* suivantes.

    (Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX1 using MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Ajoutez un champ à **StoreManagerController** pour contenir une instance de **MusicStoreEntities.**

    (Extrait de code- *ASP.net et Forms de l’aide de MVC 4-EX1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implémentez l’action d’index StoreManagerController pour retourner une vue avec la liste des albums.

    La logique d’action du contrôleur sera très similaire à l’action d’index de StoreController écrite précédemment. Utilisez LINQ pour récupérer tous les albums, y compris les informations relatives au genre et à l’artiste à afficher.

    (Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX1 StoreManagerController index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Tâche 3 : création de la vue index

Dans cette tâche, vous allez créer le modèle de vue d’index pour afficher la liste des albums retournés par le contrôleur **StoreManager** .

1. Avant de créer le modèle de vue, vous devez générer le projet afin que la **boîte de dialogue Ajouter une vue** sache la classe d' **album** à utiliser. Sélectionner une **Build | Générez MvcMusicStore** pour générer le projet.
2. Cliquez avec le bouton droit à l’intérieur de la méthode d’action **index** et sélectionnez **Ajouter une vue**. La boîte de dialogue **Ajouter une vue s’affiche** .

    ![Ajouter une vue](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Ajoute une vue")

    *Ajout d’une vue à partir de la méthode index*
3. Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **index**. Sélectionnez l’option **créer un affichage fortement typé** , puis sélectionnez **album (MvcMusicStore. Models)** dans la liste déroulante **classe de modèle** . Sélectionnez **liste** dans la liste déroulante **modèle de structure** . Laissez le **moteur** d’affichage **Razor** et les autres champs avec leur valeur par défaut, puis cliquez sur **Ajouter**.

    ![Ajout d’une vue d’index](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Ajout d’une vue d’index")

    *Ajout d’une vue d’index*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Tâche 4 : personnalisation de la structure de la vue index

Dans cette tâche, vous allez ajuster le modèle d’affichage simple créé avec la fonctionnalité de génération de modèles automatique ASP.NET MVC pour qu’il affiche les champs souhaités.

> [!NOTE]
> La prise en charge de la **génération de modèles** automatique dans ASP.NET MVC génère un modèle de vue simple qui répertorie tous les champs du modèle d’album. La **génération de modèles** automatique offre un moyen rapide de commencer à utiliser une vue fortement typée : au lieu d’avoir à écrire le modèle de vue manuellement, la génération de modèles automatique génère rapidement un modèle par défaut, puis vous pouvez modifier le code généré.

1. Passez en revue le code créé. La liste de champs générée fait partie du tableau HTML suivant que la **génération de modèles** automatique utilise pour afficher des données tabulaires.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Remplacez la **table&lt;&gt;** code par le code suivant pour afficher uniquement les champs **genre**, **artiste**, titre de l' **album**et **prix** . Cela supprime les colonnes **AlbumId** et de l’URL de la pochette de l' **album** . En outre, il modifie les colonnes GenreId et ArtistId pour afficher les propriétés de classe liées de **Artist.Name** et **genre.Name**, et supprime le lien **Détails** .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Modifiez les descriptions suivantes.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’application

Dans cette tâche, vous allez tester que le modèle de vue d' **index** **StoreManager** affiche une liste d’albums en fonction de la conception des étapes précédentes.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager** pour vérifier qu’une liste d’albums s’affiche, en affichant son **titre**, son **artiste** et son **genre**.

    ![Exploration de la liste des albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Exploration de la liste des albums")

    *Exploration de la liste des albums*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Exercice 2 : ajout d’un programme d’assistance HTML

La page d’index StoreManager présente un problème potentiel : les propriétés Title et Artist Name peuvent être suffisamment longues pour lever la mise en forme de la table. Dans cet exercice, vous allez apprendre à ajouter un programme d’assistance HTML personnalisé pour tronquer ce texte.

Dans l’illustration suivante, vous pouvez voir comment le format est modifié en raison de la longueur du texte lorsque vous utilisez une petite taille de navigateur.

![Exploration de la liste des albums avec du texte non tronqué](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Exploration de la liste des albums avec du texte non tronqué")

*Exploration de la liste des albums avec du texte non tronqué*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Tâche 1 : extension du programme d’assistance HTML

Dans cette tâche, vous allez ajouter une nouvelle méthode **tronquer** à l’objet **HTML** exposé dans les vues ASP.NET MVC. Pour ce faire, vous allez implémenter une **méthode d’extension** à la classe **System. Web. Mvc. HtmlHelper** intégrée fournie par ASP.NET MVC.

> [!NOTE]
> Pour en savoir plus sur les **méthodes d’extension**, consultez cet article MSDN. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).

1. Ouvrez le **début** de la solution situé dans **source/EX2-AddingAnHTMLHelper/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la vue d’index de StoreManager. Pour ce faire, dans le Explorateur de solutions développez le dossier **views** , puis le **StoreManager** et ouvrez le fichier **index. cshtml** .
3. Ajoutez le code suivant sous la directive <strong>@model</strong> pour définir la méthode d’assistance <strong>TRUNCATE</strong> .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Tâche 2 : troncation du texte dans la page

Dans cette tâche, vous allez utiliser la méthode **TRUNCATE** pour tronquer le texte dans le modèle de vue.

1. Ouvrez la vue d’index de StoreManager. Pour ce faire, dans le Explorateur de solutions développez le dossier **views** , puis le **StoreManager** et ouvrez le fichier **index. cshtml** .
2. Remplacez les lignes qui affichent le nom de l' **artiste** et le **titre**de l’album. Pour ce faire, remplacez les lignes suivantes.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’application

Dans cette tâche, vous allez vérifier que le modèle de vue d' **index** **StoreManager** tronque le titre et le nom de l’artiste de l’album.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager** pour vérifier que les longs textes de la colonne **title** et **Artist** sont tronqués.

    ![Noms des titres et artistes tronqués](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Noms des titres et artistes tronqués")

    *Titres et noms d’artistes tronqués*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Exercice 3 : création de la vue Edit

Dans cet exercice, vous allez apprendre à créer un formulaire pour permettre aux responsables du magasin de modifier un album. Ils parcourent l’URL **/StoreManager/Edit/ID** (**ID** étant l’ID unique de l’album à modifier), en effectuant un appel http-to sur le serveur.

La méthode d’action de modification du contrôleur récupère l’album approprié à partir de la base de données, crée un objet **StoreManagerViewModel** pour l’encapsuler (avec une liste d’artistes et de genres), puis le transmet à un modèle de vue pour restituer la page HTML à l’utilisateur. Cette page contient une **&lt;formulaire&gt;** élément avec des zones de texte et des listes déroulantes pour modifier les propriétés de l’album.

Une fois que l’utilisateur a mis à jour les valeurs de formulaire de l’album et clique sur le bouton **Enregistrer** , les modifications sont soumises via un rappel http-postconnexion à **/StoreManager/Edit/ID**. Bien que l’URL reste la même que dans le dernier appel, ASP.NET MVC identifie cette fois qu’il s’agit d’un HTTP-HTTP et exécute donc une méthode d’action de modification différente (une décorée avec **[HttpPost]** ).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Tâche 1 : implémentation de la méthode d’action de modification HTTP-obtenue

Dans cette tâche, vous allez implémenter la version HTTP-obtenir de la méthode Edit action pour récupérer l’album approprié de la base de données, ainsi qu’une liste de tous les genres et artistes. Il empaquette ces données dans l’objet **StoreManagerViewModel** défini à la dernière étape, qui est ensuite transmis à un modèle de vue pour afficher la réponse.

1. Ouvrez le **début** de la solution situé dans **source/EX3-CreatingTheEditView/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la classe **StoreManagerController** . Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.
3. Remplacez la méthode d’action **http-obtenir Edit** par le code suivant pour récupérer l' **album** approprié ainsi que les listes **genres** et **artistes** .

    (Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX3 STOREMANAGERCONTROLLER http-obtenir Edit action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Vous utilisez **System. Web. Mvc** **SelectList** pour les artistes et les genres à la place de la liste **System. Collections. Generic** .
    > 
    > **SelectList** est un moyen plus clair de remplir les listes déroulantes html et de gérer des éléments tels que la sélection actuelle. L’instanciation et la configuration ultérieure de ces objets ViewModel dans l’action du contrôleur feront du scénario de modification de formulaire.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Tâche 2 : création de la vue de modification

Dans cette tâche, vous allez créer un modèle de vue de modification qui affichera ultérieurement les propriétés de l’album.

1. Créez la vue Edit (édition). Pour ce faire, cliquez avec le bouton droit dans la méthode d’action **modifier** , puis sélectionnez **Ajouter une vue**.
2. Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **modifier**. Cochez la case **créer une vue fortement typée** et sélectionnez **album (MvcMusicStore. Models)** dans la liste déroulante **afficher la classe de données** . Sélectionnez **modifier** dans la liste déroulante **modèle de structure** . Laissez les autres champs avec leur valeur par défaut, puis cliquez sur **Ajouter**.

    ![Ajout d’une vue Edit](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Ajout d’une vue Edit")

    *Ajout d’une vue Edit*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’application

Dans cette tâche, vous allez tester que la page de la vue **Edit** **StoreManager** affiche les valeurs des propriétés pour l’album passé comme paramètre.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager/Edit/1** pour vérifier que les valeurs des propriétés de l’album sont affichées.

    ![Exploration de l’affichage des modifications de l’album](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Exploration de l’affichage des modifications de l’album")

    *Exploration de l’affichage des modifications de l’album*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Tâche 4 : implémentation de listes déroulantes sur le modèle éditeur d’albums

Dans cette tâche, vous allez ajouter des listes déroulantes au modèle de vue créé dans la dernière tâche, afin que l’utilisateur puisse sélectionner dans une liste d’artistes et de genres.

1. Remplacez tout le code du jeu de champs de l' **album** par ce qui suit :

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Un programme d’assistance **html. DropDownList** a été ajouté pour afficher les listes déroulantes permettant de choisir des artistes et des genres. Les paramètres passés à **html. DropDownList** sont les suivants :
    > 
    > 1. Nom du champ de formulaire ( **&quot;ArtistId&quot;** ).
    > 2. **SelectList** de valeurs de la liste déroulante.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’application

Dans cette tâche, vous allez tester que la page de la vue **Edit** **StoreManager** affiche des listes déroulantes à la place des champs de texte artiste et genre ID.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager/Edit/1** pour vérifier qu’elle affiche les listes déroulantes à la place des champs de texte artiste et genre ID.

    ![Exploration de l’affichage des modifications de l’album avec des listes déroulantes](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Exploration de l’affichage des modifications de l’album avec des listes déroulantes")

    *Exploration de l’affichage des modifications de l’album, cette fois avec des listes déroulantes*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Tâche 6 : implémentation de la méthode d’action HTTP-après modification

Maintenant que la vue Edit s’affiche comme prévu, vous devez implémenter la méthode d’action HTTP-après modification pour enregistrer les modifications apportées à l’album.

1. Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio. Ouvrez **StoreManagerController** dans le dossier **Controllers** .
2. Remplacez le code de méthode d’action **http-postérieur** à la modification par le code suivant (Notez que la méthode qui doit être remplacée est une version surchargée qui reçoit deux paramètres) :

    (Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX3 STOREMANAGERCONTROLLER http-après modification*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Cette méthode est exécutée lorsque l’utilisateur clique sur le bouton **Enregistrer** de la vue et effectue une opération http-poster des valeurs de formulaire sur le serveur pour les rendre persistantes dans la base de données. L’élément Decorator **[HttpPost]** indique que la méthode doit être utilisée pour ces scénarios http-poster. La méthode prend un objet **album** . ASP.NET MVC crée automatiquement l’objet album à partir des valeurs de&gt; de formulaire &lt;publiées.
    > 
    > La méthode effectue les étapes suivantes :
    > 
    > 1. Si le modèle est valide :
    > 
    >     1. Mettez à jour l’entrée de l’album dans le contexte pour la marquer comme un objet modifié.
    >     2. Enregistrez les modifications et redirigez vers la vue index.
    > 2. Si le modèle n’est pas valide, il remplit le ViewBag avec **GenreId** et **ArtistId**, puis il retourne la vue avec l’objet album reçu pour permettre à l’utilisateur d’effectuer toutes les mises à jour requises.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tâche 7 : exécution de l’application

Au cours de cette tâche, vous allez tester que la page de la vue de **modification StoreManager** enregistre en fait les données d’album mises à jour dans la base de données.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager/Edit/1**. Remplacez le titre de l’album par **charger** , puis cliquez sur **Enregistrer**. Vérifiez que le titre de l’album a effectivement été modifié dans la liste des albums.

    ![Mise à jour d’un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Mise à jour d’un album")

    *Mise à jour d’un album*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Exercice 4 : ajout d’une vue Create

Maintenant que **StoreManagerController** prend en charge la fonctionnalité de **modification** , dans cet exercice, vous allez apprendre à ajouter un modèle de vue Create pour permettre aux responsables du magasin d’ajouter de nouveaux albums à l’application.

Comme vous l’avez fait avec la fonctionnalité d’édition, vous allez implémenter le scénario de création à l’aide de deux méthodes distinctes au sein de la classe **StoreManagerController** :

1. Une méthode d’action affichera un formulaire vide lorsque les responsables des boutiques accèdent d’abord à l’URL **/StoreManager/Create** .
2. Une deuxième méthode d’action gère le scénario dans lequel le responsable du magasin clique sur le bouton **Enregistrer** dans le formulaire et renvoie les valeurs à l’URL **/StoreManager/Create** sous la forme d’une requête http-http.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Tâche 1 : implémentation de la méthode d’action de création HTTP-obtien

Dans cette tâche, vous allez implémenter la version HTTP-obtenir de la méthode Create action pour récupérer une liste de tous les genres et artistes, empaquetez ces données dans un objet **StoreManagerViewModel** , qui sera ensuite passé à un modèle de vue.

1. Ouvrez le **début** de la solution situé dans **source/EX4-AddingACreateView/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la classe **StoreManagerController** . Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.
3. Remplacez le code de la méthode de **création** d’action par ce qui suit :

    (Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX4 STOREMANAGERCONTROLLER http-obtenir Create action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Tâche 2 : ajout de la vue Create

Au cours de cette tâche, vous allez ajouter le modèle de vue créer qui affichera un nouveau formulaire d’album (vide).

1. Cliquez avec le bouton droit à l’intérieur de la méthode **Create** action et sélectionnez **Ajouter une vue**. La boîte de dialogue Ajouter une vue s’affiche.
2. Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **créer**. Sélectionnez l’option **créer un affichage fortement typé** et sélectionnez **album (MvcMusicStore. Models)** dans la liste déroulante **classe de modèle** , puis **créez** à partir de la liste déroulante **modèle de structure** . Laissez les autres champs avec leur valeur par défaut, puis cliquez sur **Ajouter**.

    ![Ajout d’une vue Create](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-Create-View. png")

    *Ajout de la vue Create*
3. Mettez à jour les champs **GenreId** et **ArtistId** pour utiliser une liste déroulante comme indiqué ci-dessous :

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’application

Dans cette tâche, vous allez tester que la page **créer** un affichage de **StoreManager** affiche un formulaire d’album vide.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager/Create**. Vérifiez qu’un formulaire vide s’affiche pour remplir les nouvelles propriétés de l’album.

    ![Créer une vue avec un formulaire vide](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Créer une vue avec un formulaire vide")

    *Créer une vue avec un formulaire vide*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Tâche 4 : implémentation de la méthode d’action HTTP-Après création

Dans cette tâche, vous allez implémenter la version HTTP-POSTÉRIEURe de la méthode Create action qui sera appelée lorsqu’un utilisateur clique sur le bouton **Enregistrer** . La méthode doit enregistrer le nouvel album dans la base de données.

1. Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio. Ouvrez la classe **StoreManagerController** . Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.
2. Remplacez le code de la méthode d’action **http-après création** par ce qui suit :

    (Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX4 STOREMANAGERCONTROLLER http-après Create action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > L’action Create est assez similaire à la méthode d’action Edit précédente, mais au lieu de définir l’objet comme modifié, elle est ajoutée au contexte.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’application

Dans cette tâche, vous allez tester que la page créer un affichage de **StoreManager** vous permet de créer un nouvel album, puis de rediriger vers la vue index StoreManager.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager/Create**. Remplissez tous les champs de formulaire avec les données d’un nouvel album, comme dans l’illustration suivante :

    ![Création d’un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Création d’un album")

    *Création d’un album*
3. Vérifiez que vous êtes redirigé vers la vue d’index StoreManager qui comprend le nouvel album que vous venez de créer.

    ![Nouvel album créé](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nouvel album créé")

    *Nouvel album créé*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Exercice 5 : gestion de la suppression

La possibilité de supprimer des albums n’est pas encore implémentée. C’est ce que cet exercice va vous présenter. Comme précédemment, vous allez implémenter le scénario de suppression à l’aide de deux méthodes distinctes au sein de la classe **StoreManagerController** :

1. Une méthode d’action affichera un formulaire de confirmation
2. Une deuxième méthode d’action prend en charge l’envoi de formulaire.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Tâche 1 : implémentation de la méthode d’action HTTP-récupérer la suppression

Dans cette tâche, vous allez implémenter la version HTTP-obtenir de la méthode d’action de suppression pour récupérer les informations de l’album.

1. Ouvrez le **début** de la solution situé dans **source/EX5-HandlingDeletion/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la classe **StoreManagerController** . Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.
3. L’action supprimer le contrôleur est exactement la même que l’action précédente du contrôleur des détails du magasin : elle interroge l’objet **album** à partir de la base de données à l’aide de l' **ID** fourni dans l’URL et retourne la **vue**appropriée. Pour ce faire, remplacez le code de la méthode d’action de **suppression** http-do par ce qui suit :

    (Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX5 Handling Delete http-obtenir Delete action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Cliquez avec le bouton droit dans la méthode d’action de **suppression** , puis sélectionnez **Ajouter une vue**. La boîte de dialogue Ajouter une vue s’affiche.
5. Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **supprimer**. Sélectionnez l’option **créer un affichage fortement typé** et sélectionnez **album (MvcMusicStore. Models)** dans la liste déroulante **classe de modèle** . Sélectionnez **supprimer** dans la liste déroulante **modèle de structure** . Laissez les autres champs avec leur valeur par défaut, puis cliquez sur **Ajouter**.

    ![Ajout d’une vue Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Ajout d’une vue Delete")

    *Ajout d’une vue Delete*
6. Le modèle supprimer affiche tous les champs du modèle. Vous n’afficherez que le titre de l’album. Pour ce faire, remplacez le contenu de la vue par le code suivant :

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tâche 2 : exécution de l’application

Au cours de cette tâche, vous allez vérifier que la page **supprimer** la vue **StoreManager** affiche un formulaire de suppression de confirmation.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager**. Sélectionnez un album à supprimer en cliquant sur **supprimer** et vérifiez que la nouvelle vue est chargée.

    ![Suppression d’un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Suppression d’un album")

    *Suppression d’un album*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Tâche 3 : implémentation de la méthode d’action HTTP-après suppression

Dans cette tâche, vous allez implémenter la version HTTP-POSTÉRIEURe de la méthode d’action de suppression qui sera appelée lorsqu’un utilisateur cliquera sur le bouton **supprimer** . La méthode doit supprimer l’album dans la base de données.

1. Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio. Ouvrez la classe **StoreManagerController** . Pour ce faire, développez le dossier **Controllers** , puis double-cliquez sur **StoreManagerController.cs**.
2. Remplacez le code de la méthode d’action **http-après suppression** par ce qui suit :

    (Extrait de code- *ASP.NET MVC 4 helpers and Forms and validation-EX5 Handling Delete http-après suppression*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’application

Dans cette tâche, vous allez tester que la page **StoreManager supprimer** la vue vous permet de supprimer un album, puis de le rediriger vers la vue index StoreManager.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager**. Sélectionnez un album à supprimer en cliquant sur **Supprimer.** Confirmez la suppression en cliquant sur le bouton **supprimer** :

    ![Suppression d’un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Suppression d’un album")

    *Suppression d’un album*
3. Vérifiez que l’album a été supprimé, car il n’apparaît pas dans la page **index** .

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Exercice 6 : ajout de la validation

Actuellement, les formulaires de création et de modification que vous avez en place n’effectuent aucun type de validation. Si l’utilisateur laisse un champ obligatoire vide ou que vous tapez des lettres dans le champ Price, la première erreur que vous obtiendrez provient de la base de données.

Vous pouvez ajouter la validation à l’application en ajoutant des annotations de données à votre classe de modèle. Les annotations de données permettent de décrire les règles que vous souhaitez appliquer à vos propriétés de modèle, et ASP.NET MVC s’occupera de l’application et de l’affichage des messages appropriés aux utilisateurs.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Tâche 1 : ajout d’annotations de données

Dans cette tâche, vous allez ajouter des annotations de données au modèle d’album qui fera en sorte que la page créer et modifier affiche des messages de validation si nécessaire.

Pour une classe de modèle simple, l’ajout d’une annotation de données est simplement gérée en ajoutant une instruction **using** pour **System. ComponentModel. DataAnnotation**, puis en plaçant un attribut **[required]** sur les propriétés appropriées. L’exemple suivant fait de la propriété **Name** un champ obligatoire dans la vue.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

C’est un peu plus complexe dans les cas tels que l’application où la Entity Data Model est générée. Si vous avez ajouté des annotations de données directement aux classes de modèle, elles sont remplacées si vous mettez à jour le modèle à partir de la base de données. Au lieu de cela, vous pouvez utiliser des classes partielles de métadonnées qui existent pour contenir les annotations et qui sont associées aux classes de modèle à l’aide de l’attribut **[méta DataType]** .

1. Ouvrez le **début** de la solution situé dans **source/Ex6-AddingValidation/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez le **album.cs** à partir du dossier **Models** .
3. Remplacez le contenu **album.cs** par le code en surbrillance, afin qu’il ressemble à ce qui suit :

    > [!NOTE]
    > La ligne **[DisplayFormat (ConvertEmptyStringToNull = false)]** indique que les chaînes vides du modèle ne sont pas converties en valeurs NULL lorsque le champ de données est mis à jour dans la source de données. Ce paramètre permet d’éviter une exception lorsque l’Entity Framework assigne des valeurs NULL au modèle avant que l’annotation de données valide les champs.

    (Extrait de code- *application auxiliaires ASP.NET MVC 4 et formulaires et validation-classe partielle des métadonnées de l’album Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Cette classe partielle de l' **album** a un attribut de **type** de données qui pointe vers la classe **AlbumMetaData** pour les annotations de données. Voici quelques-uns des attributs d’annotation de données que vous utilisez pour annoter le modèle d’album :
    > 
    > - Obligatoire-indique que la propriété est un champ obligatoire
    > - DisplayName : définit le texte à utiliser dans les champs de formulaire et les messages de validation
    > - DisplayFormat-spécifie le mode d’affichage et de mise en forme des champs de données.
    > - StringLength : définit une longueur maximale pour un champ de chaîne.
    > - Range : donne une valeur maximale et minimale pour un champ numérique
    > - ScaffoldColumn : permet de masquer les champs des formulaires de l’éditeur

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tâche 2 : exécution de l’application

Dans cette tâche, vous allez vérifier que les pages créer et modifier valident les champs, en utilisant les noms complets choisis dans la dernière tâche.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/StoreManager/Create**. Vérifier que les noms d’affichage correspondent à ceux de la classe partielle (comme l’URL de la pochette de l' **album** au lieu de **AlbumArtUrl**)
3. Cliquez sur **créer**, sans remplir le formulaire. Vérifiez que vous recevez les messages de validation correspondants.

    ![Champs validés dans la page Create](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Champs validés dans la page Create")

    *Champs validés dans la page Create*
4. Vous pouvez vérifier que le même problème se produit avec la page de **modification** . Remplacez l’URL par **/StoreManager/Edit/1** et vérifiez que les noms d’affichage correspondent à ceux de la classe partielle (comme l’URL de la pochette de l' **album** au lieu de **AlbumArtUrl**). Renseignez les champs **titre** et **prix** , puis cliquez sur **Enregistrer**. Vérifiez que vous recevez les messages de validation correspondants.

    ![Champs validés dans la page Modifier](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Champs validés dans la page Modifier*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Exercice 7 : utilisation de jQuery discrète côté client

Dans cet exercice, vous allez apprendre à activer la validation jQuery discrète de MVC 4 côté client.

> [!NOTE]
> Le jQuery discrète utilise le préfixe de données Ajax pour appeler des méthodes d’action sur le serveur plutôt que d’émettre indiscrètement des scripts client Inline.

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Tâche 1 : exécution de l’application avant l’activation de jQuery discrète

Dans cette tâche, vous allez exécuter l’application avant d’inclure jQuery pour pouvoir comparer les deux modèles de validation.

1. Ouvrez le **début** de la solution situé dans **source/EX7-UnobtrusivejQueryValidation/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Appuyez sur **F5** pour exécuter l’application.
3. Le projet démarre sur la page d’hébergement. Parcourez **/StoreManager/Create** , puis cliquez sur **créer** sans remplir le formulaire pour vérifier que vous recevez des messages de validation :

    ![Validation du client désactivée](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Validation du client désactivée")

    *Validation du client désactivée*
4. Dans le navigateur, ouvrez le code source HTML :

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Tâche 2 : activation de la validation de client discrète

Dans cette tâche, vous allez activer la **validation client jQuery discrète** à partir du fichier **Web. config** , qui est défini par défaut sur false dans tous les nouveaux projets ASP.NET MVC 4. En outre, vous allez ajouter les références de scripts nécessaires pour effectuer un travail de validation client jQuery discrète.

1. Ouvrez le fichier **Web. config** à la racine du projet et assurez-vous que les valeurs des clés **ClientValidationEnabled** et **UnobtrusiveJavaScriptEnabled** sont définies sur **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Vous pouvez également activer la validation client par code sur Global.asax.cs pour obtenir les mêmes résultats :
    > 
    > **HtmlHelper. ClientValidationEnabled = true ;**
    > 
    > En outre, vous pouvez affecter un attribut ClientValidationEnabled à n’importe quel contrôleur pour avoir un comportement personnalisé.
2. Ouvrez **Create. cshtml** sur **Views\StoreManager**.
3. Assurez-vous que les fichiers de script suivants, **jQuery. Validate** et **jQuery. Validate. discrètes**, sont référencés dans la vue via le bundle &quot; **~/bundles/jqueryval**&quot;.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Toutes ces bibliothèques jQuery sont incluses dans les nouveaux projets MVC 4. Vous trouverez d’autres bibliothèques dans le dossier **/scripts** de votre projet.
    > 
    > Pour que ces bibliothèques de validation fonctionnent, vous devez ajouter une référence à la bibliothèque jQuery Framework. Étant donné que cette référence est déjà ajoutée au fichier **\_Layout. cshtml** , vous n’avez pas besoin de l’ajouter dans cette vue particulière.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Tâche 3 : exécution de l’application à l’aide de la validation jQuery discrète

Dans cette tâche, vous allez tester que le modèle de vue Create View **StoreManager** effectue la validation côté client à l’aide de bibliothèques jQuery lorsque l’utilisateur crée un nouvel album.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Parcourez **/StoreManager/Create** , puis cliquez sur **créer** sans remplir le formulaire pour vérifier que vous recevez des messages de validation :

    ![Validation du client avec jQuery activée](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Validation du client avec jQuery activée")

    *Validation du client avec jQuery activée*
3. Dans le navigateur, ouvrez le code source de CREATE VIEW :

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Pour chaque règle de validation client, jQuery discrète ajoute un attribut avec les données-Val-*rulename*=&quot;*message*&quot;. Voici une liste de balises que jQuery insère discrètement dans le champ d’entrée HTML pour effectuer la validation du client :
   > 
   > - Données-Val
   > - Données-Val-Number
   > - Données-Val-Range
   > - Données-Val-Range-min/données-Val-Range-Max
   > - Données-Val-requis
   > - Données-Val-length
   > - Données-Val-length-Max/données-Val-length-min
   > 
   > Toutes les valeurs de données sont remplies avec l' **annotation de données**de modèle. Ensuite, toute la logique qui fonctionne côté serveur peut être exécutée côté client. Par exemple, l’attribut Price a l’annotation de données suivante dans le modèle :
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Après l’utilisation de jQuery discrète, le code généré est le suivant :
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En effectuant ce laboratoire pratique, vous avez appris à permettre aux utilisateurs de modifier les données stockées dans la base de données à l’aide des éléments suivants :

- Actions de contrôleur comme index, créer, modifier, supprimer
- Fonctionnalité de génération de modèles automatique de ASP.NET MVC pour l’affichage des propriétés dans un tableau HTML
- Applications auxiliaires HTML personnalisées pour améliorer l’expérience utilisateur
- Méthodes d’action qui réagissent à des appels HTTP-is ou HTTP-POSTCONNEXION
- Modèle d’éditeur partagé pour des modèles de vue similaires comme Create et Edit
- Éléments de formulaire tels que les listes déroulantes
- Annotations de données pour la validation de modèle
- Validation côté client à l’aide de la bibliothèque jQuery discrète

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe A : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *Vignette VS Express pour le Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Annexe B : utilisation d’extraits de code

Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main. Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.

![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")

*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***

1. Placez le curseur à l’endroit où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).
3. Regarder comme IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).
5. Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*

![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")

*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*

***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0. Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.

1. Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.
2. Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.

![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")

*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*

![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")

*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*
