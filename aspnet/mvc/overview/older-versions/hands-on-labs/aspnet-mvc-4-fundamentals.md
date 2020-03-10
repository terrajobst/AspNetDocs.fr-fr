---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Notions de base de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ce laboratoire pratique est basé sur le magasin de musique MVC (Model View Controller), une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598818"
---
# <a name="aspnet-mvc-4-fundamentals"></a>Concepts de base d’ASP.NET MVC 4

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

Ce laboratoire pratique est basé sur le magasin de musique MVC (Model View Controller), une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio. Tout au long du laboratoire, vous apprendrez la simplicité, mais la puissance de l’utilisation conjointe de ces technologies. Vous allez commencer par une application simple et la générer jusqu’à ce que vous disposiez d’une application Web ASP.NET MVC 4 entièrement fonctionnelle.

Ce laboratoire fonctionne avec ASP.NET MVC 4.

Si vous souhaitez explorer la version ASP.NET MVC 3 de l’application du didacticiel, vous pouvez la trouver dans [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Ce laboratoire pratique part du principe que le développeur a une expérience dans les technologies de développement Web, telles que HTML et JavaScript.

> [!NOTE]
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet propre à ce laboratoire est disponible à l’adresse [notions de base de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Application du Store Music

L’application Web du Store musical qui sera créée tout au long de ce laboratoire comprend trois parties principales : achat, retrait et administration. Les visiteurs pourront parcourir les albums par genre, ajouter des albums à leur panier, passer en revue leur sélection, puis passer à l’extraction pour se connecter et terminer la commande. En outre, les administrateurs de magasins pourront gérer les albums disponibles ainsi que leurs principales propriétés.

![Écrans du Store musique](aspnet-mvc-4-fundamentals/_static/image1.png "Écrans du Store musique")

*Écrans du Store musique*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

L’application du Store musical est créée à l’aide du **contrôleur MVC (Model View Controller)** , un modèle architectural qui sépare une application en trois composants principaux :

- **Modèles**: les objets de modèle sont les parties de l’application qui implémentent la logique de domaine. Souvent, les objets de modèle récupèrent et stockent également l’état du modèle dans une base de données.
- **Vues :** Les vues sont les composants qui affichent l’interface utilisateur de l’application. En général, cette interface utilisateur est créée à partir des données du modèle. Par exemple, la vue Edit des albums affiche des zones de texte et une liste déroulante en fonction de l’état actuel d’un objet album.
- **Contrôleurs :** Les contrôleurs sont les composants qui gèrent l’interaction avec l’utilisateur, manipulent le modèle et finalement sélectionnent une vue pour afficher l’interface utilisateur. Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.

Le modèle MVC vous aide à créer des applications qui séparent les différents aspects de l’application (logique d’entrée, logique métier et logique d’interface utilisateur), tout en fournissant un couplage faible entre ces éléments. Cette séparation vous aide à gérer la complexité lorsque vous générez une application, car elle vous permet de vous concentrer sur un aspect de l’implémentation à la fois. En outre, le modèle MVC facilite le test des applications, en encourageant également l’utilisation du développement piloté par les tests (TDD) pour la création d’applications.

L’infrastructure **MVC ASP.net** fournit une alternative au modèle de Web Forms ASP.net pour la création d’applications Web basées sur ASP.NET MVC. L’infrastructure **MVC ASP.net** est un Framework de présentation léger et hautement testable qui (comme avec les applications basées sur les formulaires Web) est intégré aux fonctionnalités ASP.NET existantes, telles que les pages maîtres et l’authentification basée sur l’appartenance. vous bénéficiez ainsi de toute la puissance du .NET Framework principal. Cela est utile si vous êtes déjà familiarisé avec ASP.NET Web Forms, car toutes les bibliothèques que vous utilisez déjà sont également disponibles dans ASP.NET MVC 4.

En outre, le couplage faible entre les trois principaux composants d’une application MVC favorise également le développement parallèle. Par exemple, un développeur peut travailler sur la vue, un deuxième développeur peut travailler sur la logique du contrôleur, et un troisième développeur peut se concentrer sur la logique métier dans le modèle.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Créer une application ASP.NET MVC à partir de zéro en suivant le didacticiel sur l’application du Store Music
- Ajoutez des contrôleurs pour gérer les URL vers la page d’accueil du site et pour parcourir ses principales fonctionnalités.
- Ajouter une vue pour personnaliser le contenu affiché avec son style
- Ajouter des classes de modèle pour contenir et gérer la logique de données et de domaine
- Utiliser le modèle afficher le modèle pour passer des informations des actions du contrôleur aux modèles de vue
- Explorez le nouveau modèle ASP.NET MVC 4 pour les applications Internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :

- [Visual Studio 2012 Express pour le Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Installation

**Installation d’extraits de code**

Pour plus de commodité, la majeure partie du code que vous allez gérer dans le cadre de ce laboratoire est disponible sous forme d’extraits de code Visual Studio. Pour installer les extraits de code, exécutez le fichier **.\Source\Setup\CodeSnippets.vsi** .

Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;l' [annexe C : utilisation d’extraits de Code](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique est constitué des exercices suivants :

1. [Exercice 1 : création d’un projet d’application Web MusicStore ASP.NET MVC](#Exercise1)
2. [Exercice 2 : création d’un contrôleur](#Exercise2)
3. [Exercice 3 : passage de paramètres à un contrôleur](#Exercise3)
4. [Exercice 4 : création d’une vue](#Exercise4)
5. [Exercice 5 : création d’un modèle de vue](#Exercise5)
6. [Exercice 6 : utilisation des paramètres dans la vue](#Exercise6)
7. [Exercice 7 : un nouveau modèle ASP.NET MVC 4](#Exercise7)

> [!NOTE]
> Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices. Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.

Durée estimée pour effectuer ce laboratoire : **60 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Exercice 1 : création d’un projet d’application Web MusicStore ASP.NET MVC

Dans cet exercice, vous allez apprendre à créer une application ASP.NET MVC dans Visual Studio 2012 Express pour le Web, ainsi que son organisation principale de dossiers. En outre, vous apprendrez à ajouter un nouveau contrôleur et à le faire afficher une simple chaîne dans la page d’affichage de l’application.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Tâche 1 : création du projet d’application Web MVC ASP.NET

1. Dans cette tâche, vous allez créer un projet d’application ASP.NET MVC vide à l’aide du modèle MVC Visual Studio. Démarrez **vs Express pour le Web**.
2. Dans le menu **Fichier**, cliquez sur **Nouveau projet**.
3. Dans la boîte de dialogue **nouveau projet** , sélectionnez le type de projet d' **application Web ASP.NET MVC 4** , situé sous **Visual C#, liste de** modèles **Web** .
4. Remplacez le **nom** par *MvcMusicStore*.
5. Définissez l’emplacement de la solution à l’intérieur d’un nouveau dossier **Begin** dans le dossier source de cet exercice, par exemple **[Your-Hol-path] \Source\Ex01-CreatingMusicStoreProject\Begin**. Cliquez sur **OK**.

    ![Boîte de dialogue créer un nouveau projet](aspnet-mvc-4-fundamentals/_static/image2.png "Boîte de dialogue créer un nouveau projet")

    *Boîte de dialogue créer un nouveau projet*
6. Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle de **base** et assurez-vous que le **moteur d’affichage** sélectionné est **Razor**. Cliquez sur **OK**.

    ![Boîte de dialogue Nouveau projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "Boîte de dialogue Nouveau projet ASP.NET MVC 4")

    *Boîte de dialogue Nouveau projet ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Tâche 2 : exploration de la structure de la solution

L’infrastructure MVC ASP.NET comprend un modèle de projet Visual Studio qui vous aide à créer des applications Web prenant en charge le modèle MVC. Ce modèle crée une application Web ASP.NET MVC avec les dossiers requis, les modèles d’élément et les entrées du fichier de configuration.

Dans cette tâche, vous allez examiner la structure de la solution pour comprendre les éléments impliqués et leurs relations. Les dossiers suivants sont inclus dans toutes les applications ASP.NET MVC, car l’infrastructure ASP.NET MVC utilise par défaut une convention &quot;sur la configuration&quot; l’approche et effectue certaines hypothèses par défaut en fonction des conventions d’affectation des noms de dossiers.

1. Une fois le projet créé, passez en revue la structure de dossiers qui a été créée dans le Explorateur de solutions sur le côté droit :

    ![Structure de dossiers ASP.NET MVC dans Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image4.png "Structure de dossiers ASP.NET MVC dans Explorateur de solutions")

    *Structure de dossiers ASP.NET MVC dans Explorateur de solutions*

   1. Les **contrôleurs**. Ce dossier contiendra les classes de contrôleur. Dans une application basée sur MVC, les contrôleurs sont responsables du traitement de l’interaction de l’utilisateur final, de la manipulation du modèle et de la sélection d’une vue pour le rendu de l’interface utilisateur.

       > [!NOTE]
       > L’infrastructure MVC requiert que les noms de tous les contrôleurs se terminent par &quot;contrôleur&quot;, par exemple HomeController, LoginController ou ProductController.
   2. **Modèles**. Ce dossier est fourni pour les classes qui représentent le modèle d’application pour l’application Web MVC. Cela comprend généralement le code qui définit les objets et la logique d’interaction avec le magasin de données. En général, les objets de modèle réels sont placés dans des bibliothèques de classes séparées. Toutefois, lorsque vous créez une application, vous pouvez inclure des classes, puis les déplacer dans des bibliothèques de classes distinctes à un stade ultérieur du cycle de développement.
   3. **Vues**. Ce dossier est l’emplacement recommandé pour les vues, les composants responsables de l’affichage de l’interface utilisateur de l’application. Les vues utilisent des fichiers. aspx,. ascx,. cshtml et. Master, en plus de tous les autres fichiers liés aux affichages de rendu. Le dossier views contient un dossier pour chaque contrôleur ; le dossier est nommé avec le préfixe de nom de contrôleur. Par exemple, si vous avez un contrôleur nommé **HomeController**, le dossier views contient un dossier nommé page d’hébergement. Par défaut, lorsque l’infrastructure MVC ASP.NET charge une vue, il recherche un fichier. aspx avec le nom de la vue demandée dans le dossier Views\controllerName (**views [ControllerName] [action]. aspx**) ou (**views [ControllerName] [action]. cshtml**) pour les vues Razor.

      > [!NOTE]
      > En plus des dossiers indiqués précédemment, une application Web MVC utilise le fichier **global. asax** pour définir les valeurs par défaut de routage d’URL globales et utilise le fichier **Web. config** pour configurer l’application.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Tâche 3 : ajout d’un HomeController

Dans les applications ASP.NET qui n’utilisent pas l’infrastructure MVC, l’interaction avec l’utilisateur est organisée autour des pages, et autour du déclenchement et de la gestion d’événements à partir de ces pages. En revanche, l’interaction de l’utilisateur avec les applications ASP.NET MVC est organisée autour des contrôleurs et de leurs méthodes d’action.

En revanche, l’infrastructure MVC ASP.NET mappe les URL aux classes appelées contrôleurs. Les contrôleurs traitent les demandes entrantes, gèrent les entrées et les interactions de l’utilisateur, exécutent la logique d’application appropriée et déterminent la réponse à renvoyer au client (affichage HTML, téléchargement d’un fichier, redirection vers une autre URL, etc.). Dans le cas de l’affichage HTML, une classe de contrôleur appelle généralement un composant de vue séparé pour générer le balisage HTML de la requête. Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.

Au cours de cette tâche, vous allez ajouter une classe de contrôleur qui traitera les URL vers la page d’hébergement du site du Store musical.

1. Cliquez avec le bouton droit sur le dossier **Controllers** dans le Explorateur de solutions, sélectionnez **Ajouter** , puis commande du **contrôleur** :

    ![Ajouter une commande de contrôleur](aspnet-mvc-4-fundamentals/_static/image5.png "Ajouter une commande de contrôleur")

    *Commande Add Controller*
2. La boîte de dialogue **Ajouter un contrôleur** s’affiche. Nommez le contrôle *HomeController* et appuyez sur **Ajouter**.

    ![Boîte de dialogue Ajouter un contrôleur](aspnet-mvc-4-fundamentals/_static/image6.png "Boîte de dialogue Ajouter un contrôleur")

    *Boîte de dialogue Ajouter un contrôleur*
3. Le fichier **HomeController.cs** est créé dans le dossier **Controllers** . Pour que le **HomeController** retourne une chaîne sur son action d’index, remplacez la méthode d' **index** par le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-index du HomeController de EX1*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’application

Dans cette tâche, vous allez essayer l’application dans un navigateur Web.

1. Appuyez sur **F5** pour exécuter l’application. Le projet est compilé et le serveur Web IIS local démarre. Le serveur Web IIS local ouvre automatiquement un navigateur Web qui pointe vers l’URL du serveur Web.

    ![Application s’exécutant dans un navigateur Web](aspnet-mvc-4-fundamentals/_static/image7.png "Application s’exécutant dans un navigateur Web")

    *Application s’exécutant dans un navigateur Web*

    > [!NOTE]
    > Le serveur Web IIS local exécute le site Web sur un numéro de port libre aléatoire. Dans la figure ci-dessus, le site s’exécute sur `http://localhost:50103/`, donc il utilise le port 50103. Votre numéro de port peut varier.
2. Fermez le navigateur.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Exercice 2 : création d’un contrôleur

Dans cet exercice, vous allez apprendre à mettre à jour le contrôleur pour implémenter une fonctionnalité simple de l’application du Store Music. Ce contrôleur définit des méthodes d’action pour gérer chacune des requêtes spécifiques suivantes :

- Page de liste des genres musicaux dans le magasin musical
- Page parcourir qui répertorie tous les albums musicaux pour un genre particulier
- Page de détails qui affiche des informations sur un album de musique spécifique

Dans le cadre de cet exercice, ces actions retournent simplement une chaîne par la suite.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Tâche 1 : ajout d’une nouvelle classe StoreController

Dans cette tâche, vous allez ajouter un nouveau contrôleur.

1. S’il n’est pas déjà ouvert, démarrez **VS Express pour le Web 2012**.
2. Dans le menu **fichier** , choisissez **ouvrir un projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex02-CreatingAController\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**. Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Ajoutez le nouveau contrôleur. Pour ce faire, cliquez avec le bouton droit sur le dossier **Controllers** dans le Explorateur de solutions, sélectionnez **Ajouter** , puis la commande **contrôleur** . Remplacez le **nom du contrôleur** par *StoreController*, puis cliquez sur **Ajouter**.

    ![Boîte de dialogue Ajouter un contrôleur](aspnet-mvc-4-fundamentals/_static/image8.png "Boîte de dialogue Ajouter un contrôleur")

    *Boîte de dialogue Ajouter un contrôleur*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Tâche 2 : modification des actions de StoreController

Dans cette tâche, vous allez modifier les méthodes de contrôleur appelées **actions**. Les actions sont responsables de la gestion des demandes d’URL et de la détermination du contenu qui doit être renvoyé au navigateur ou à l’utilisateur qui a appelé l’URL.

1. La classe **StoreController** a déjà une méthode d' **index** . Vous l’utiliserez ultérieurement dans ce laboratoire pour implémenter la page qui répertorie tous les genres du magasin musical. Pour le moment, il vous suffit de remplacer la méthode **index** par le code suivant, qui retourne une chaîne &quot;Hello de Store. index ()&quot;:

    (Extrait de code- *notions de base de ASP.NET MVC 4-EX2 StoreController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Ajoutez les méthodes **Browse** et **Details** . Pour ce faire, ajoutez le code suivant au **StoreController**:

    (Extrait de code- *notions de base de ASP.NET MVC 4-EX2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’application

Dans cette tâche, vous allez essayer l’application dans un navigateur Web.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d' **hébergement** . Modifiez l’URL pour vérifier l’implémentation de chaque action.

    1. **/Store**. Vous verrez **&quot;Hello de Store. index ()&quot;** .
    2. **/Store/Browse**. Vous verrez **&quot;Hello de Store. Browse ()&quot;** .
    3. **/Store/Details**. Vous verrez **&quot;Hello de Store. Details ()&quot;** .

        ![Navigation dans StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Navigation dans StoreBrowse")

        *Navigation dans/Store/Browse*
3. Fermez le navigateur.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Exercice 3 : passage de paramètres à un contrôleur

Jusqu’à présent, vous renvoyiez des chaînes constantes à partir des contrôleurs. Dans cet exercice, vous allez apprendre à transmettre des paramètres à un contrôleur à l’aide de l’URL et de QueryString, puis à faire en sorte que les actions de méthode répondent avec du texte au navigateur.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Tâche 1 : ajout du paramètre genre à StoreController

Dans cette tâche, vous allez utiliser **QueryString** pour envoyer des paramètres à la méthode d’action **Browse** dans le **StoreController**.

1. S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**.
2. Dans le menu **fichier** , choisissez **ouvrir un projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex03-PassingParametersToAController\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**. Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Ouvrez la classe **StoreController** . Pour ce faire, dans le **Explorateur de solutions**, développez le dossier **Controllers** , puis double-cliquez sur **StoreController.cs**.
4. Modifiez la méthode **Browse** , en ajoutant un paramètre de chaîne pour demander un genre spécifique. ASP.NET MVC passe automatiquement tout paramètre de publication QueryString ou de formulaire nommé **genre** à cette méthode d’action lorsqu’il est appelé. Pour ce faire, remplacez la méthode **Browse** par le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-EX3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Vous utilisez la méthode de l’utilitaire **HttpUtility. HtmlEncode** pour empêcher les utilisateurs d’injecter du code JavaScript dans la vue à l’aide d’un lien tel que **/Store/Browse ? Genre =&lt;script&gt;Window. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** .
> 
> Pour plus d’informations, consultez [cet article MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tâche 2 : exécution de l’application

Dans cette tâche, vous allez essayer l’application dans un navigateur Web et utiliser le paramètre **genre** .

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d' **hébergement** . Remplacer l’URL par */Store/Browse ? Genre = Disco* pour vérifier que l’action reçoit le paramètre genre.

    ![Navigation StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Navigation StoreBrowseGenre = Disco")

    *Vous parcourez/Store/Browse ? Genre = Disco*
3. Fermez le navigateur.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Tâche 3 : ajout d’un paramètre d’ID incorporé dans l’URL

Dans cette tâche, vous allez utiliser l' **URL** pour passer un paramètre **ID** à la méthode d’action **Details** de **StoreController**. La Convention de routage par défaut d’ASP.NET MVC consiste à traiter le segment d’une URL après le nom de la méthode d’action en tant que paramètre nommé **ID**. Si votre méthode d’action a un paramètre nommé ID, ASP.NET MVC passe automatiquement le segment d’URL à vous en tant que paramètre. Dans le magasin d’URL **/Détails/5**, l' **ID** sera interprété comme **5**.

1. Modifiez la méthode **Details** de **StoreController**, en ajoutant un paramètre **int** appelé **ID**. Pour ce faire, remplacez la méthode **Details** par le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-EX3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’application

Dans cette tâche, vous allez essayer l’application dans un navigateur Web et utiliser le paramètre **ID** .

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d' **hébergement** . Remplacez l’URL par */Store/Details/5* pour vérifier que l’action reçoit le paramètre ID.

    ![Navigation dans StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Navigation dans StoreDetails5")

    *Navigation dans/Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Exercice 4 : création d’une vue

Jusqu’à présent, vous retournez des chaînes à partir d’actions de contrôleur. Bien qu’il s’agisse d’un moyen utile pour comprendre le fonctionnement des contrôleurs, il ne s’agit pas de la façon dont vos applications Web réelles sont générées. Les vues sont des composants qui offrent une meilleure approche pour la génération de code HTML dans le navigateur avec l’utilisation de fichiers de modèle.

Dans cet exercice, vous allez apprendre à ajouter une page maître de disposition pour configurer un modèle pour le contenu HTML courant, une feuille de style pour améliorer l’apparence du site et enfin un modèle de vue pour permettre au HomeController de retourner du code HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a>Tâche 1 : modification du fichier \_Layout. cshtml

Le fichier **~/Views/Shared/\_Layout. cshtml** vous permet de configurer un modèle de code HTML commun à utiliser sur l’ensemble du site Web. Au cours de cette tâche, vous allez ajouter une page maître de disposition avec un en-tête commun avec des liens vers la page d’hébergement et la zone de stockage.

1. S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**.
2. Dans le menu **fichier** , choisissez **ouvrir un projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex04-CreatingAView\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**. Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Le fichier <strong>\_Layout. cshtml</strong> contient la disposition de conteneur html pour toutes les pages du site. Il comprend l’élément <strong>&lt;html&gt;</strong> pour la réponse HTML, ainsi que les éléments de la <strong>&lt;head&gt;</strong> et <strong>&lt;Body&gt;</strong> . <strong>@RenderBody()</strong> dans le corps HTML Identifiez les régions dans lesquelles les modèles de vue peuvent être renseignés avec du contenu dynamique.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Ajoutez un en-tête commun avec des liens vers la page d’hébergement et la zone de stockage sur toutes les pages du site. Pour ce faire, ajoutez le code suivant sous &lt;instruction&gt; corps.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Incluez un DIV pour restituer la section de corps de chaque page. Remplacez <strong>@RenderBody()</strong> par le code en surbrillanceC#suivant : ()

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Saviez-vous ? Visual Studio 2012 contient des extraits de code qui facilitent l’ajout de code couramment utilisé en HTML, les fichiers de code et bien plus encore ! Essayez-le en tapant **&lt;div&gt;** et en appuyant deux fois sur **Tab** pour insérer une balise **div** complète.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Tâche 2 : ajout d’une feuille de style CSS

Le modèle de projet vide comprend un fichier CSS très rationalisé qui comprend simplement les styles utilisés pour afficher les formulaires de base et les messages de validation. Vous allez utiliser des CSS et des images supplémentaires (éventuellement fournies par un concepteur) afin d’améliorer l’apparence du site.

Dans cette tâche, vous allez ajouter une feuille de style CSS pour définir les styles du site.

1. Le fichier CSS et les images sont inclus dans le dossier **Source\Assets\Content** de cet atelier. Pour les ajouter à l’application, faites glisser leur contenu à partir d’une fenêtre de l' **Explorateur Windows** vers le **Explorateur de solutions** dans Visual Studio Express pour le Web, comme indiqué ci-dessous :

    ![Glissement du contenu de style](aspnet-mvc-4-fundamentals/_static/image12.png "Glissement du contenu de style")

    *Glissement du contenu de style*
2. Une boîte de dialogue d’avertissement s’affiche, vous demandant de confirmer le remplacement du fichier **site. CSS** et de certaines images existantes. Cochez la case **appliquer à tous les éléments** , puis cliquez sur **Oui**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Tâche 3 : ajout d’un modèle de vue

Dans cette tâche, vous allez ajouter un modèle de vue pour générer la réponse HTML qui utilisera la page maître de disposition et le CSS ajouté dans cet exercice.

1. Pour utiliser un modèle de vue lorsque vous parcourez la page d’hébergement du site, vous devez d’abord indiquer qu’au lieu de retourner une chaîne, la méthode d' **index HomeController** renverra une **vue**. Ouvrez la classe **HomeController** et modifiez sa méthode d' **index** pour retourner un **ActionResult**et Return **View ()** .

    (Extrait de code- *notions de base de ASP.NET MVC 4-index HomeController EX4*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. À présent, vous devez ajouter un modèle de vue approprié. Pour ce faire, **cliquez avec le bouton droit** dans la méthode d’action **index** , puis sélectionnez **Ajouter une vue**. La boîte de dialogue **Ajouter une vue s’affiche** .

    ![Ajout d’une vue à partir de la méthode index](aspnet-mvc-4-fundamentals/_static/image13.png "Ajout d’une vue à partir de la méthode index")

    *Ajout d’une vue à partir de la méthode index*
3. La boîte de dialogue **Ajouter une vue** s’affiche pour générer un fichier de modèle de vue. Par défaut, cette boîte de dialogue prérenseigne le nom du modèle de vue afin qu’il corresponde à la méthode d’action qui va l’utiliser. Étant donné que vous avez utilisé le menu contextuel **Ajouter une vue** dans la méthode d’action **index** au sein du HomeController, la boîte de dialogue **Ajouter une vue** contient index comme nom de vue par défaut. Cliquez sur **Ajouter**.

    ![Boîte de dialogue Ajouter une vue](aspnet-mvc-4-fundamentals/_static/image14.png "Boîte de dialogue Ajouter une vue")

    *Boîte de dialogue Ajouter une vue*
4. Visual Studio génère un modèle d’affichage **index. cshtml** dans le dossier **Views\Home** , puis l’ouvre.

    ![Vue d’index de démarrage créée](aspnet-mvc-4-fundamentals/_static/image15.png "Vue d’index de démarrage créée")

    *Vue d’index de démarrage créée*

    > [!NOTE]
    > le nom et l’emplacement du fichier **index. cshtml** sont pertinents et respectent les conventions d’affectation de noms ASP.NET MVC par défaut.
    > 
    > Le dossier \Views\*la *page d’hébergement** correspond au nom du contrôleur (contrôleur d'**hébergement** ). Le nom du modèle de vue (**index**) correspond à la méthode d’action du contrôleur qui affiche la vue.
    > 
    > De cette façon, ASP.NET MVC évite d’avoir à spécifier explicitement le nom ou l’emplacement d’un modèle de vue lors de l’utilisation de cette Convention d’affectation de noms pour retourner une vue.
5. Le modèle de vue généré est basé sur le modèle **\_Layout. cshtml** précédemment défini. Mettez **à jour**la propriété ViewBag. title et remplacez le contenu principal par celui de **la page d’accueil**, comme indiqué dans le code ci-dessous :

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Sélectionnez projet **MvcMusicStore** dans le Explorateur de solutions, puis appuyez sur **F5** pour exécuter l’application.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Tâche 4 : vérification

Pour vérifier que vous avez correctement effectué toutes les étapes de l’exercice précédent, procédez comme suit :

Une fois l’application ouverte dans un navigateur, vous devez noter que :

1. La méthode d’action d’index du HomeController a trouvé et affiché le modèle de vue **\Views\Home\Index.cshtml** , bien que le code appelé **Return View ()** , parce que le modèle de vue a suivi la Convention d’affectation de noms standard.
2. La page d’accueil affiche le message de bienvenue défini dans le modèle de vue **\Views\Home\Index.cshtml** .
3. La page d’accueil utilise le modèle **\_Layout. cshtml** et le message d’accueil est donc contenu dans la disposition HTML du site standard.

    ![Affichage de l’index de démarrage à l’aide du LayoutPage et du style définis](aspnet-mvc-4-fundamentals/_static/image16.png "Affichage de l’index de démarrage à l’aide du LayoutPage et du style définis")

    *Affichage de l’index de démarrage à l’aide du LayoutPage et du style définis*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Exercice 5 : création d’un modèle de vue

Jusqu’à présent, vous avez fait en sorte que vos vues affichent du code HTML codé en dur, mais, afin de créer des applications Web dynamiques, le modèle de vue doit recevoir des informations du contrôleur. Une technique courante à utiliser à cet effet est le modèle **ViewModel** , qui permet à un contrôleur de regrouper toutes les informations nécessaires à la génération de la réponse HTML appropriée.

Dans cet exercice, vous allez apprendre à créer une classe ViewModel et à ajouter les propriétés requises : le nombre de genres dans le magasin et une liste de ces genres. Vous allez également mettre à jour le StoreController pour utiliser le ViewModel créé et, enfin, vous allez créer un nouveau modèle de vue qui affichera les propriétés mentionnées dans la page.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Tâche 1 : création d’une classe ViewModel

Dans cette tâche, vous allez créer une classe ViewModel qui implémentera le scénario de liste de genre de magasin.

1. S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**.
2. Dans le menu **fichier** , choisissez **ouvrir un projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex05-CreatingAViewModel\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**. Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Créez un dossier **ViewModels** pour contenir le ViewModel. Pour ce faire, cliquez avec le bouton droit sur le projet **MvcMusicStore** de niveau supérieur, sélectionnez **Ajouter** , puis **nouveau dossier**.

    ![Ajout d’un nouveau dossier](aspnet-mvc-4-fundamentals/_static/image17.png "Ajout d’un nouveau dossier")

    *Ajout d’un nouveau dossier*
4. Nommez le dossier *ViewModels*.

    ![Dossier ViewModels dans Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image18.png "Dossier ViewModels dans Explorateur de solutions")

    *Dossier ViewModels dans Explorateur de solutions*
5. Créez une classe **ViewModel** . Pour ce faire, cliquez avec le bouton droit sur le dossier **ViewModels** créé récemment, sélectionnez **Ajouter** , puis **nouvel élément**. Sous **code**, choisissez l’élément de **classe** et nommez le fichier *StoreIndexViewModel.cs*, puis cliquez sur **Ajouter**.

    ![Ajout d’une nouvelle classe](aspnet-mvc-4-fundamentals/_static/image19.png "Ajout d’une nouvelle classe")

    *Ajout d’une nouvelle classe*

    ![Création de la classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Création de la classe StoreIndexViewModel")

    *Création de la classe StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Tâche 2 : ajout de propriétés à la classe ViewModel

Deux paramètres doivent être passés du StoreController au modèle de vue pour générer la réponse HTML attendue : le nombre de genres dans le magasin et une liste de ces genres.

Dans cette tâche, vous allez ajouter ces 2 propriétés à la classe **StoreIndexViewModel** : **NumberOfGenres** (un entier) et **genres** (une liste de chaînes).

1. Ajoutez les propriétés **NumberOfGenres** et **genres** à la classe **StoreIndexViewModel** . Pour ce faire, ajoutez les 2 lignes suivantes à la définition de classe :

    (Extrait de code- *notions de base de ASP.NET MVC 4-propriétés de EX5 StoreIndexViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> La notation **{obtient ; Set ;}** utilise la fonctionnalité C#de propriétés implémentées automatiquement de. Elle offre les avantages d’une propriété sans que nous puissions déclarer un champ de stockage.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Tâche 3 : mise à jour de StoreController pour utiliser StoreIndexViewModel

La classe **StoreIndexViewModel** encapsule les informations nécessaires pour passer de la méthode d' **index** de **StoreController**à un modèle de vue afin de générer une réponse.

Dans cette tâche, vous allez mettre à jour **StoreController** pour utiliser **StoreIndexViewModel**.

1. Ouvrez la classe **StoreController** .

    ![Ouverture de la classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Ouverture de la classe StoreController")

    *Ouverture de la classe StoreController*
2. Pour pouvoir utiliser la classe **StoreIndexViewModel** à partir de **StoreController**, ajoutez l’espace de noms suivant en haut du code **StoreController** :

    (Extrait de code- *notions de base de ASP.NET MVC 4-EX5 StoreIndexViewModel à l’aide de ViewModels*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Modifiez la méthode d’action d' **index** de **StoreController**afin qu’elle crée et remplisse un objet **StoreIndexViewModel** , puis la passe à un modèle de vue pour générer une réponse html.

    > [!NOTE]
    > Dans Lab &quot;les modèles MVC ASP.NET et l’accès aux données&quot; vous allez écrire du code qui récupère la liste des genres du magasin à partir d’une base de données. Dans le code suivant, vous allez créer une **liste** de genres de données factices qui rempliront le **StoreIndexViewModel**.
    > 
    > Une fois l’objet **StoreIndexViewModel** créé et configuré, il est passé comme argument à la méthode **View** . Cela indique que le modèle de vue utilisera cet objet pour générer une réponse HTML.
4. Remplacez la méthode d' **index** par le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-méthode d’index EX5 StoreController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Si vous n’êtes pas familiarisé C#avec, vous pouvez supposer que l’utilisation de **var** signifie que la variable **ViewModel** est à liaison tardive. Ce n’est pas correct : C# le compilateur utilise l’inférence de type en fonction de ce que vous attribuez à la variable pour déterminer que le **ViewModel** est de type **StoreIndexViewModel**. En outre, en compilant la variable **ViewModel** locale comme type **StoreIndexViewModel** , vous bénéficiez de la vérification au moment de la compilation et de la prise en charge de l’éditeur de code Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Tâche 4 : création d’un modèle de vue qui utilise StoreIndexViewModel

Dans cette tâche, vous allez créer un modèle de vue qui utilisera un objet StoreIndexViewModel transmis à partir du contrôleur pour afficher une liste de genres.

1. Avant de créer le modèle de vue, nous allons générer le projet afin que la **boîte de dialogue Ajouter une vue** sache la classe **StoreIndexViewModel** . Générez le projet en sélectionnant l’élément de menu **générer** , puis **créez MvcMusicStore**.

    ![Génération du projet](aspnet-mvc-4-fundamentals/_static/image22.png "Génération du projet")

    *Génération du projet*
2. Créez un modèle de vue. Pour ce faire, cliquez avec le bouton droit à l’intérieur de la méthode **index** et sélectionnez **Ajouter une vue**.

    ![Ajout d’une vue](aspnet-mvc-4-fundamentals/_static/image23.png "Ajout d’une vue")

    *Ajout d’une vue*
3. Étant donné que la **boîte de dialogue Ajouter une vue** a été appelée à partir du **StoreController**, le modèle d’affichage est ajouté par défaut dans un fichier **\Views\Store\Index.cshtml** . Cochez la case **créer une vue fortement typée** , puis sélectionnez **StoreIndexViewModel** comme classe de **modèle**. En outre, assurez-vous que le moteur d’affichage sélectionné est **Razor**. Cliquez sur **Ajouter**.

    ![Boîte de dialogue Ajouter une vue](aspnet-mvc-4-fundamentals/_static/image24.png "Boîte de dialogue Ajouter une vue")

    *Boîte de dialogue Ajouter une vue*

    Le fichier de modèle de vue **\Views\Store\Index.cshtml** est créé et ouvert. En fonction des informations fournies dans la boîte de dialogue **Ajouter une vue** à la dernière étape, le modèle de vue attend une instance **StoreIndexViewModel** comme données à utiliser pour générer une réponse html. Vous remarquerez que le modèle hérite d’un C#`ViewPage<musicstore.viewmodels.storeindexviewmodel>` dans.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Tâche 5 : mise à jour du modèle de vue

Dans cette tâche, vous allez mettre à jour le modèle de vue créé dans la dernière tâche pour récupérer le nombre de genres et leurs noms dans la page.

> [!NOTE]
> Vous allez utiliser la syntaxe @ (souvent appelée &quot;code pépites&quot;) pour exécuter du code dans le modèle de vue.

1. Dans le fichier **index. cshtml** , dans le dossier **Store** , remplacez son code par ce qui suit :

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Parcourez la liste de genres dans **StoreIndexViewModel** et créez une liste HTML **&lt;UL&gt;** à l’aide d’une boucle **foreach** .
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Appuyez sur **F5** pour exécuter l’application et parcourir **/Store**. Vous verrez la liste des genres transmis à l’objet **StoreIndexViewModel** à partir du **StoreController** vers le modèle de vue.

    ![Affichage affichant une liste de genres](aspnet-mvc-4-fundamentals/_static/image26.png "Affichage affichant une liste de genres")

    *Affichage affichant une liste de genres*
4. Fermez le navigateur.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Exercice 6 : utilisation des paramètres dans la vue

Dans l’exercice 3, vous avez appris à passer des paramètres au contrôleur. Dans cet exercice, vous allez apprendre à utiliser ces paramètres dans le modèle de vue. À cet effet, vous allez tout d’abord créer des classes de modèle qui vous aideront à gérer vos données et la logique de domaine. En outre, vous apprendrez à créer des liens vers des pages à l’intérieur de l’application ASP.NET MVC sans vous soucier de l’encodage des chemins d’URL.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Tâche 1 : ajout de classes de modèle

Contrairement à ViewModels, qui sont créés juste pour transmettre des informations du contrôleur à la vue, les classes de modèle sont conçues pour contenir et gérer la logique des données et du domaine. Dans cette tâche, vous allez ajouter deux classes de modèle pour représenter ces concepts : **genre** et **album**.

1. S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**
2. Dans le menu **fichier** , choisissez **ouvrir un projet**. Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex06-UsingParametersInView\Begin**, sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**. Vous pouvez également continuer avec la solution obtenue à l’issue de l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
3. Ajoutez une classe de modèle de **genre** . Pour ce faire, cliquez avec le bouton droit sur le dossier **Models** dans le **Explorateur de solutions**, sélectionnez **Ajouter** , puis l’option **nouvel élément** . Sous **code**, choisissez l’élément de **classe** et nommez le fichier *genre.cs*, puis cliquez sur **Ajouter**.

    ![Ajout d’une classe](aspnet-mvc-4-fundamentals/_static/image27.png "Ajout d’une classe")

    *Ajout d’un nouvel élément*

    ![Ajouter une classe de modèle de genre](aspnet-mvc-4-fundamentals/_static/image28.png "Ajouter une classe de modèle de genre")

    *Ajouter une classe de modèle de genre*
4. Ajoutez une propriété **Name** à la classe genre. Pour ce faire, ajoutez le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 genre*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. En suivant la même procédure qu’avant, ajoutez une classe **album** . Pour ce faire, cliquez avec le bouton droit sur le dossier **Models** dans le **Explorateur de solutions**, sélectionnez **Ajouter** , puis l’option **nouvel élément** . Sous **code**, choisissez l’élément de **classe** et nommez le fichier *album.cs*, puis cliquez sur **Ajouter**.
6. Ajoutez deux propriétés à la classe album : **genre** et **titre**. Pour ce faire, ajoutez le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 album*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Tâche 2 : ajout d’un StoreBrowseViewModel

Un **StoreBrowseViewModel** sera utilisé dans cette tâche pour afficher les albums qui correspondent à un genre sélectionné. Dans cette tâche, vous allez créer cette classe, puis ajouter deux propriétés pour gérer le **genre** et la liste de son **album**.

1. Ajoutez une classe **StoreBrowseViewModel** . Pour ce faire, cliquez avec le bouton droit sur le dossier **ViewModels** dans le **Explorateur de solutions**, sélectionnez **Ajouter** , puis l’option **nouvel élément** . Sous **code**, choisissez l’élément de **classe** et nommez le fichier *StoreBrowseViewModel.cs*, puis cliquez sur **Ajouter**.
2. Ajoutez une référence aux modèles dans la classe **StoreBrowseViewModel** . Pour ce faire, ajoutez les éléments suivants à l’aide de l’espace de noms :

    (Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Ajoutez deux propriétés à la classe **StoreBrowseViewModel** : **genre** et **albums**. Pour ce faire, ajoutez le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> Qu’est-ce que la **liste&lt;Album&gt;** ?: cette définition utilise le type de **liste&lt;t&gt;** , où **t** limite le type auquel les éléments de cette **liste** appartiennent, dans ce cas **album** (ou l’un de ses descendants).
> 
> Cette capacité à concevoir des classes et des méthodes qui diffèrent la spécification d’un ou plusieurs types jusqu’à ce que la classe ou la méthode soit déclarée et instanciée C# par le code client est une fonctionnalité du langage appelé **génériques**.
> 
> **List&lt;t&gt;** est l’équivalent générique du type **ArrayList** et est disponible dans l’espace de noms **System. Collections. Generic** . L’un des avantages de l’utilisation des **génériques** est que puisque le type est spécifié, vous n’avez pas besoin de prendre en charge les opérations de vérification de type telles que le casting des éléments dans l' **album** comme vous le feriez avec une **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Tâche 3 : utilisation du nouveau ViewModel dans StoreController

Dans cette tâche, vous allez modifier les méthodes d’action **Browse** et **Details** de **StoreController**pour utiliser le nouveau **StoreBrowseViewModel**.

1. Ajoutez une référence au dossier Models dans la classe **StoreController** . Pour ce faire, développez le dossier **Controllers** dans le **Explorateur de solutions** et ouvrez la classe **StoreController** . Ajoutez ensuite le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Remplacez la méthode d’action **Browse** pour utiliser la classe **StoreViewBrowseController** . Vous allez créer un genre et deux nouveaux objets d’albums avec des données factices (dans le prochain atelier pratique, vous utiliserez les données réelles d’une base de données). Pour ce faire, remplacez la méthode **Browse** par le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Remplacez la méthode d’action **Details** pour utiliser la classe **StoreViewBrowseController** . Vous allez créer un nouvel objet **album** à retourner à la **vue**. Pour ce faire, remplacez la méthode **Details** par le code suivant :

    (Extrait de code- *notions de base de ASP.NET MVC 4-Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Tâche 4 : ajout d’un modèle de vue Parcourir

Dans cette tâche, vous allez ajouter une vue **Parcourir** pour afficher les albums trouvés pour un genre spécifique.

1. Avant de créer le modèle de vue, vous devez générer le projet afin que la boîte de dialogue **Ajouter une vue** sache la classe **ViewModel** à utiliser. Générez le projet en sélectionnant l’élément de menu **générer** , puis **créez MvcMusicStore**.
2. Ajoutez une vue **Parcourir** . Pour ce faire, cliquez avec le bouton droit dans la méthode d’action **Browse** du **StoreController** , puis cliquez sur **Ajouter une vue**.
3. Dans la boîte de dialogue **Ajouter une vue** , vérifiez que le nom de la vue est **Parcourir**. Cochez la case **créer une vue fortement typée** et sélectionnez **StoreBrowseViewModel** dans la liste déroulante **classe de modèle** . Laissez les autres champs avec leur valeur par défaut. Cliquez ensuite sur **Ajouter**.

    ![Ajout d’une vue Parcourir](aspnet-mvc-4-fundamentals/_static/image29.png "Ajout d’une vue Parcourir")

    *Ajout d’une vue Parcourir*
4. Modifiez **Browse. cshtml** pour afficher les informations du genre, en accédant à l’objet **StoreBrowseViewModel** qui est passé au modèle de vue. Pour ce faire, remplacez le contenu par le code suivant :C#()

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tâche 5 : exécution de l’application

Dans cette tâche, vous allez vérifier que la méthode **Browse** récupère des albums à partir de l’action de méthode **Browse** .

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacer l’URL par **/Store/Browse ? Genre = Disco** pour vérifier que l’action retourne deux albums.

    ![Exploration des albums de magasin Disco](aspnet-mvc-4-fundamentals/_static/image30.png "Exploration des albums de magasin Disco")

    *Exploration des albums de magasin Disco*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Tâche 6 : affichage d’informations sur un album spécifique

Dans cette tâche, vous allez implémenter l’affichage **magasin/détails** pour afficher des informations sur un album spécifique. Dans ce laboratoire pratique, tout ce que vous allez voir sur l’album est déjà contenu dans le modèle de **vue** . Ainsi, au lieu de créer une classe **StoreDetailsViewModel** , vous allez utiliser le modèle **StoreBrowseViewModel** actuel en lui transmettant l’album.

1. Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio. Ajoutez un nouvel affichage **Détails** pour la méthode d’action **Details** de **StoreController**. Pour ce faire, cliquez avec le bouton droit sur la méthode **Details** dans la classe **StoreController** , puis cliquez sur **Ajouter une vue**.
2. Dans la boîte de dialogue **Ajouter une vue** , vérifiez que le nom de la **vue** est **Détails**. Cochez la case **créer une vue fortement typée** et sélectionnez **album** dans la liste déroulante **classe de modèle** . Laissez les autres champs avec leur valeur par défaut. Cliquez ensuite sur **Ajouter**. Cela permet de créer et d’ouvrir un fichier **\Views\Store\Details.cshtml** .

    ![Ajout d’un mode Détails](aspnet-mvc-4-fundamentals/_static/image31.png "Ajout d’un mode Détails")

    *Ajout d’un mode Détails*
3. Modifiez le fichier **Details. cshtml** pour afficher les informations de l’album, en accédant à l’objet **album** qui est transmis au modèle de vue. Pour ce faire, remplacez le contenu par le code suivant :C#()

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tâche 7 : exécution de l’application

Dans cette tâche, vous allez vérifier que la vue **Détails** récupère les informations de l’album à partir de la méthode d' **action Details** .

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d' **hébergement** . Remplacez l’URL par **/Store/Details/5** pour vérifier les informations de l’album.

    ![Exploration des détails des albums](aspnet-mvc-4-fundamentals/_static/image32.png "Exploration des détails des albums")

    *Exploration des détails de l’album*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Tâche 8 : ajout de liens entre les pages

Au cours de cette tâche, vous allez ajouter un lien dans la vue Store pour avoir un lien dans chaque nom de genre vers l’URL **/Store/Browse** appropriée. Ainsi, lorsque vous cliquez sur un genre, par exemple **Disco**, vous accédez à **/Store/Browse ? genre = URL Disco** .

1. Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio. Mettez à jour la page d' **index** pour ajouter un lien vers la page de **navigation** . Pour ce faire, dans le **Explorateur de solutions** développez le dossier **views** , puis le dossier **Store** et double-cliquez sur la page **index. cshtml** .
2. Ajoutez un lien vers l’affichage de navigation indiquant le genre sélectionné. Pour ce faire, remplacez le code en surbrillance suivant dans les balises **&lt;li&gt;** : (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > une autre approche consisterait à établir une liaison directe avec la page, avec un code semblable au suivant :
   > 
   > &lt;a href =&quot;/Store/Browse ? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Bien que cette approche fonctionne, elle dépend d’une chaîne codée en dur. Si, par la suite, vous renommez le contrôleur, vous devrez modifier cette instruction manuellement. Une meilleure solution consiste à utiliser une méthode **d’assistance HTML** . ASP.NET MVC comprend une méthode d’assistance HTML qui est disponible pour de telles tâches. La méthode d’assistance **html. ActionLink ()** facilite la génération de code HTML **&lt;un&gt;** des liens, en veillant à ce que les chemins d’URL soient correctement encodés dans une URL.
   > 
   > Html. ActionLink a plusieurs surcharges. Dans cet exercice, vous allez utiliser un qui accepte trois paramètres :
   > 
   > 1. Texte du lien qui affichera le nom du genre
   > 2. Nom de l’action du contrôleur (**Parcourir**)
   > 3. Router les valeurs des paramètres, en spécifiant à la fois le nom (**genre**) et la valeur (**genre Name**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Tâche 9 : exécution de l’application

Dans cette tâche, vous allez vérifier que chaque genre s’affiche avec un lien vers l’URL **/Store/Browse** appropriée.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d’hébergement. Remplacez l’URL par **/Store** pour vérifier que chaque genre est lié à l’URL **/Store/Browse** appropriée.

    ![Exploration des genres avec des liens vers la page parcourir](aspnet-mvc-4-fundamentals/_static/image33.png "Exploration des genres avec des liens vers la page parcourir")

    *Exploration des genres avec des liens vers la page parcourir*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Tâche 10-utilisation de la collection ViewModel dynamique pour passer des valeurs

Dans cette tâche, vous allez apprendre une méthode simple et puissante pour passer des valeurs entre le contrôleur et la vue sans apporter de modifications au modèle. ASP.NET MVC 4 fournit la collection &quot;&quot;ViewModel, qui peut être assignée à toute valeur dynamique et accessible dans les contrôleurs et les vues.

Vous allez maintenant utiliser la collection dynamique ViewBag pour transmettre une liste de &quot;**genres une étoile**&quot; du contrôleur à la vue. La vue store index peut accéder à **ViewModel** et afficher les informations.

1. Fermez le navigateur si nécessaire pour revenir à la fenêtre Visual Studio. Ouvrez **StoreController.cs** et modifiez la méthode **index** pour créer une liste de genres une étoile dans la collection ViewModel :

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Vous pouvez également utiliser la syntaxe **ViewBag [&quot;une étoile&quot;]** pour accéder aux propriétés.
2. L’icône d’étoile **&quot;une étoile. png&quot;** est incluse dans le dossier **Source\Assets\Images** de ce Lab. Pour l’ajouter à l’application, faites glisser son contenu à partir d’une fenêtre de l' **Explorateur Windows** vers le **Explorateur de solutions** dans Visual Web Developer Express, comme indiqué ci-dessous :

    ![Ajout d’une image étoile à la solution](aspnet-mvc-4-fundamentals/_static/image34.png "Ajout d’une image étoile à la solution")

    *Ajout d’une image étoile à la solution*
3. Ouvrez la vue **Store/index. cshtml** et modifiez le contenu. Vous allez lire la propriété &quot;une étoile&quot; dans la collection **ViewBag** et demander si le nom actuel du genre figure dans la liste. Dans ce cas, vous affichez une icône d’étoile directement sur le lien genre.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Tâche 11 : exécution de l’application

Dans cette tâche, vous allez vérifier que les genres une étoile affichent une icône en étoile.

1. Appuyez sur **F5** pour exécuter l’application.
2. Le projet démarre sur la page d' **hébergement** . Remplacez l’URL par **/Store** pour vérifier que chaque genre proposé a l’étiquette de respect :

    ![Exploration de genres avec des éléments une étoile](aspnet-mvc-4-fundamentals/_static/image35.png "Exploration de genres avec des éléments une étoile")

    *Exploration de genres avec des éléments une étoile*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Exercice 7 : un nouveau modèle ASP.NET MVC 4

Dans cet exercice, vous allez découvrir les améliorations apportées aux modèles de projet ASP.NET MVC 4, en examinant les fonctionnalités les plus pertinentes du nouveau modèle.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Tâche 1 : exploration du modèle d’application Internet ASP.NET MVC 4

1. S’il n’est pas déjà ouvert, démarrez **vs Express pour le Web**
2. Sélectionnez le **fichier | Nouveau |** Commande de menu projet. Dans la boîte de dialogue **nouveau projet** , sélectionnez le **visuel C#| Modèle Web** dans l’arborescence du volet gauche, puis choisissez l' **Application Web ASP.NET MVC 4**. **Nommez** le projet *MusicStore* et mettez à jour le nom de la **solution** pour *commencer*, puis sélectionnez un emplacement (ou laissez la valeur par défaut) et cliquez sur **OK**.

    ![Création d’un nouveau projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Création d’un nouveau projet ASP.NET MVC 4")

    *Création d’un nouveau projet ASP.NET MVC 4*
3. Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle de projet **application Internet** , puis cliquez sur **OK**. Notez que vous pouvez sélectionner Razor ou ASPX comme moteur de vue.

    ![Création d’une nouvelle application Internet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "Création d’une nouvelle application Internet ASP.NET MVC 4")

    *Création d’une nouvelle application Internet ASP.NET MVC 4*

    > [!NOTE]
    > Syntaxe Razor a été introduite dans ASP.NET MVC 3. Son objectif est de réduire le nombre de caractères et les séquences de touches nécessaires dans un fichier, ce qui permet un flux de travail de codage rapide et fluide. Razor exploite les C#compétences du langage/vb (ou d’autres) existantes et fournit une syntaxe de balisage de modèle qui permet un flux de travail de construction html impressionnant.
4. Appuyez sur **F5** pour exécuter la solution et voir le modèle renouvelé. Vous pouvez consulter les fonctionnalités suivantes :

    1. **Modèles modernes**

        Les modèles ont été renouvelés, ce qui fournit des styles plus modernes.

        ![Modèles ASP.NET MVC 4 restylisés](aspnet-mvc-4-fundamentals/_static/image38.png "Modèles ASP.NET MVC 4 restylisés")

        *Modèles ASP.NET MVC 4 restylisés*
    2. **Rendu adaptatif**

        Examinez le redimensionnement de la fenêtre du navigateur et notez la façon dont la mise en page s’adapte dynamiquement à la nouvelle taille de fenêtre. Ces modèles utilisent la technique de rendu adaptatif pour un rendu correct sur les plateformes de bureau et mobiles sans aucune personnalisation.

        ![Modèle de projet ASP.NET MVC 4 dans différentes tailles de navigateur](aspnet-mvc-4-fundamentals/_static/image39.png "Modèle de projet ASP.NET MVC 4 dans différentes tailles de navigateur")

        *Modèle de projet ASP.NET MVC 4 dans différentes tailles de navigateur*
5. Fermez le navigateur pour arrêter le débogueur et revenir à Visual Studio.
6. Vous pouvez à présent explorer la solution et découvrir les nouvelles fonctionnalités introduites par ASP.NET MVC 4 dans le modèle de projet.

    ![ASP.NET MVC4-Internet-application-modèle de projet](aspnet-mvc-4-fundamentals/_static/image40.png "Modèle de projet d’application Internet ASP.NET MVC 4")

    *Modèle de projet d’application Internet ASP.NET MVC 4*

   1. **Balisage HTML5**

       Parcourez les vues de modèle pour connaître le nouveau balisage du thème, par exemple ouvrir la vue **about. cshtml** dans le dossier de **démarrage** .

       ![Nouveau modèle, à l’aide du balisage Razor et HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Nouveau modèle, à l’aide du balisage Razor et HTML5")

       *Nouveau modèle, à l’aide du balisage Razor et HTML5*
   2. **Bibliothèques JavaScript incluses**

      1. **jQuery**: jQuery simplifie le parcours des documents html, la gestion des événements, l’animation et les interactions Ajax.
      2. **interface utilisateur jQuery**: cette bibliothèque fournit des abstractions pour l’interaction et l’animation de bas niveau, les effets avancés et les widgets thématiques, basés sur la bibliothèque JavaScript jQuery.

         > [!NOTE]
         > Vous pouvez en savoir plus sur jQuery et l’interface utilisateur jQuery dans [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
      3. **KnockoutJS**: le modèle par défaut ASP.NET MVC 4 comprend désormais **KnockoutJS**, une infrastructure MVVM JavaScript qui vous permet de créer des applications Web riches et très réactives à l’aide de JavaScript et html. Comme dans ASP.NET MVC 3, les bibliothèques d’interface utilisateur jQuery et jQuery sont également incluses dans ASP.NET MVC 4.

          > [!NOTE]
          > Vous pouvez obtenir plus d’informations sur la bibliothèque KnockOutJS dans ce lien : [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
      4. **Modernizr**: cette bibliothèque s’exécute automatiquement, ce qui rend votre site compatible avec les anciens navigateurs lors de l’utilisation des technologies HTML5 et CSS3.

          > [!NOTE]
          > Vous pouvez obtenir plus d’informations sur la bibliothèque Modernizr dans ce lien : [http://www.modernizr.com/](http://www.modernizr.com/).
   3. **SimpleMembership inclus dans la solution**

       SimpleMembership a été conçu pour remplacer le rôle ASP.NET et le système de fournisseur d’appartenances précédents. Il offre de nombreuses nouvelles fonctionnalités qui facilitent l’utilisation du développeur pour sécuriser les pages Web de manière plus flexible.

       Le modèle Internet a déjà configuré quelques éléments pour intégrer SimpleMembership, par exemple, AccountController est prêt à utiliser OAuthWebSecurity (pour l’inscription, la connexion, la gestion, etc. du compte OAuth) et la sécurité Web.

       ![SimpleMembership inclus dans la solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership inclus dans la solution")

       *SimpleMembership inclus dans la solution*

       > [!NOTE]
       > Pour plus d’informations sur [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) , consultez MSDN.

> [!NOTE]
> En outre, vous pouvez déployer cette application sur les sites Web Windows Azure à l' [annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En suivant ce laboratoire pratique, vous avez appris les notions de base de ASP.NET MVC :

- Les éléments de base d’une application MVC et la façon dont ils interagissent
- Comment créer une application ASP.NET MVC
- Comment ajouter et configurer des contrôleurs pour gérer les paramètres transmis par le biais de l’URL et de QueryString
- Comment ajouter une page maître de disposition pour configurer un modèle pour du contenu HTML commun, une feuille de style pour améliorer l’apparence et un modèle de vue pour afficher du contenu HTML
- Comment utiliser le modèle ViewModel pour passer des propriétés au modèle de vue pour afficher des informations dynamiques
- Comment utiliser les paramètres passés aux contrôleurs dans le modèle de vue
- Comment ajouter des liens vers des pages à l’intérieur de l’application ASP.NET MVC
- Comment ajouter et utiliser des propriétés dynamiques dans une vue
- Les améliorations apportées aux modèles de projet ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe A : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](aspnet-mvc-4-fundamentals/_static/image47.png)

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

    ![Ouvrir une session sur Windows Portail Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Ouvrir une session sur Windows Portail Azure")

    *Connectez-vous à Windows Azure Portail de gestion*
2. Cliquez sur **nouveau** dans la barre de commandes.

    ![Création d’un nouveau site Web](aspnet-mvc-4-fundamentals/_static/image49.png "Création d’un nouveau site Web")

    *Création d’un nouveau site Web*
3. Cliquez sur **compute** | **site Web**. Sélectionnez ensuite l’option **création rapide** . Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.

    > [!NOTE]
    > Un site Web Windows Azure est l’hôte d’une application Web s’exécutant dans le Cloud que vous pouvez contrôler et gérer. L’option création rapide vous permet de déployer une application Web terminée sur le site Web Windows Azure depuis l’extérieur du portail. Elle n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un nouveau site Web à l’aide de la création rapide](aspnet-mvc-4-fundamentals/_static/image50.png "Création d’un nouveau site Web à l’aide de la création rapide")

    *Création d’un nouveau site Web à l’aide de la création rapide*
4. Patientez jusqu’à la création du nouveau **site Web** .
5. Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** . Vérifiez que le nouveau site Web fonctionne.

    ![Navigation vers le nouveau site Web](aspnet-mvc-4-fundamentals/_static/image51.png "Navigation vers le nouveau site Web")

    *Navigation vers le nouveau site Web*

    ![Site Web en cours d’exécution](aspnet-mvc-4-fundamentals/_static/image52.png "Site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.

    ![Ouverture des pages de gestion de site Web](aspnet-mvc-4-fundamentals/_static/image53.png "Ouverture des pages de gestion de site Web")

    *Ouverture des pages de gestion de site Web*
7. Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .

    > [!NOTE]
    > Le *profil de publication* contient toutes les informations requises pour publier une application Web sur un site Web Windows Azure pour chaque méthode de publication activée. Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur les sites Web Windows Azure.

    ![Téléchargement du profil de publication du site Web](aspnet-mvc-4-fundamentals/_static/image54.png "Téléchargement du profil de publication du site Web")

    *Téléchargement du profil de publication du site Web*
8. Téléchargez le fichier de profil de publication dans un emplacement connu. Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur un site Web Windows Azure à partir de Visual Studio.

    ![Enregistrement du fichier de profil de publication](aspnet-mvc-4-fundamentals/_static/image55.png "Enregistrement du profil de publication")

    *Enregistrement du fichier de profil de publication*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database. Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.

1. Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application. Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Windows Azure, à partir des **bases de données SQL** | **serveurs** | **tableau de bord du serveur**. Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes. Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.

    ![Tableau de bord du serveur SQL Database](aspnet-mvc-4-fundamentals/_static/image56.png "Tableau de bord du serveur SQL Database")

    *Tableau de bord du serveur SQL Database*
2. Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](aspnet-mvc-4-fundamentals/_static/image57.png).

    ![Ajout de l’adresse IP du client](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Ajout de l’adresse IP du client*
3. Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Confirmer les modifications*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.

    ![Publication de l’application](aspnet-mvc-4-fundamentals/_static/image60.png "Publication de l'application")

    *Publication du site Web*
2. Importez le profil de publication que vous avez enregistré dans la première tâche.

    ![Importation du profil de publication](aspnet-mvc-4-fundamentals/_static/image61.png "Importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la validation terminée, cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.

    ![Validation de la connexion](aspnet-mvc-4-fundamentals/_static/image62.png "Validation de la connexion")

    *Validation de la connexion*
4. Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).

    ![Configuration de Web Deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Configuration de Web Deploy")

    *Configuration de Web Deploy*
5. Configurez la connexion de base de données comme suit :

   - Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .
   - Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.
   - Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.
   - Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.

     ![Configuration de la chaîne de connexion de destination](aspnet-mvc-4-fundamentals/_static/image64.png "Configuration de la chaîne de connexion de destination")

     *Configuration de la chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](aspnet-mvc-4-fundamentals/_static/image65.png "Création de la chaîne de base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Chaîne de connexion pointant vers SQL Database")

    *Chaîne de connexion pointant vers SQL Database*
8. Dans la page **Aperçu** , cliquez sur **publier**.

    ![Publication de l’application Web](aspnet-mvc-4-fundamentals/_static/image67.png "Publication de l’application Web")

    *Publication de l’application Web*
9. Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.

    ![Application publiée sur Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application publiée sur Windows Azure")

    *Application publiée sur Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Annexe C : utilisation d’extraits de code

Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main. Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.

![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-fundamentals/_static/image69.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")

*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***

1. Placez le curseur à l’endroit où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).
3. Regarder comme IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).
5. Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-fundamentals/_static/image70.png "Commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-fundamentals/_static/image71.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*

![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-fundamentals/_static/image72.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")

*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*

***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0. Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.

1. Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.
2. Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.

![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-fundamentals/_static/image73.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")

*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*

![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-fundamentals/_static/image74.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")

*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*
