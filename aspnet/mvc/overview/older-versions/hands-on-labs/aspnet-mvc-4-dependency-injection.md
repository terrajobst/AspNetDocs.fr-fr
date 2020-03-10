---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Injection de dépendance ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Remarque : ce laboratoire pratique suppose que vous avez une connaissance de base des filtres ASP.NET MVC et ASP.NET MVC 4. Si vous n’avez pas utilisé de filtres ASP.NET MVC 4 avant, nous avons reporté...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560612"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Injection de dépendances d’ASP.NET MVC 4

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

Ce laboratoire pratique suppose que vous avez une connaissance de base des filtres **ASP.NET MVC** et **ASP.NET MVC 4**. Si vous n’avez pas encore utilisé les **filtres ASP.NET MVC 4** , nous vous recommandons de passer au-dessus de l’atelier pratique des **filtres d’action personnalisée ASP.NET MVC** .

> [!NOTE]
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet propre à ce laboratoire est disponible sur l' [injection de dépendance ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

Dans le paradigme de **programmation orientée objet** , les objets fonctionnent ensemble dans un modèle de collaboration où il existe des contributeurs et des consommateurs. Naturellement, ce modèle de communication génère des dépendances entre les objets et les composants, ce qui devient difficile à gérer lorsque la complexité augmente.

![Dépendances de classes et complexité du modèle](aspnet-mvc-4-dependency-injection/_static/image1.png "Dépendances de classes et complexité du modèle")

*Dépendances de classes et complexité du modèle*

Vous avez probablement entendu parler du **modèle de fabrique** et de la séparation entre l’interface et l’implémentation à l’aide de services, où les objets clients sont souvent responsables de l’emplacement du service.

Le modèle d’injection de dépendances est une implémentation particulière d’inversion de contrôle. **Inversion de contrôle (IOC, inversion of Control)** signifie que les objets ne créent pas d’autres objets sur lesquels ils s’appuient pour effectuer leur travail. Au lieu de cela, ils obtiennent les objets dont ils ont besoin à partir d’une source externe (par exemple, un fichier de configuration XML).

L' **injection de dépendance (di)** signifie que cette opération est effectuée sans l’intervention de l’objet, généralement par un composant de l’infrastructure qui passe les paramètres du constructeur et définit les propriétés.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Le modèle de conception d’injection de dépendances (DI)

À un niveau élevé, l’objectif de l’injection de dépendances est qu’une classe de client (par exemple, *le golfeur*) a besoin d’une interface qui satisfait une interface (par exemple, *iClub*). Il ne se soucie pas du type concret (par exemple *, WoodClub, IronClub, WedgeClub* ou *PutterClub*), il veut que quelqu’un d’autre le gère (par exemple, un bon *chariot*). Le résolveur de dépendance dans ASP.NET MVC peut vous permettre d’inscrire votre logique de dépendance ailleurs (par exemple, un conteneur ou un *sac de trèfle*).

![Diagramme d’injection de dépendances](aspnet-mvc-4-dependency-injection/_static/image2.png "Illustration de l’injection de dépendances")

*Injection de dépendances-analogie de golf*

Les avantages de l’utilisation du modèle d’injection de dépendances et de l’inversion de contrôle sont les suivants :

- Réduit le couplage de classe
- Augmente la réutilisation du code
- Améliore la maintenabilité du code
- Améliore les tests d’application

> [!NOTE]
> L’injection de dépendances est parfois comparée au modèle de conception de fabrique abstraite, mais il existe une légère différence entre les deux approches. L’injection de dépendance a une infrastructure qui fonctionne en arrière-plan pour résoudre les dépendances en appelant les fabriques et les services inscrits.

Maintenant que vous comprenez le modèle d’injection de dépendances, vous allez apprendre dans ce laboratoire comment l’appliquer dans ASP.NET MVC 4. Vous allez commencer à utiliser l’injection de dépendances dans les **contrôleurs** pour inclure un service d’accès à la base de données. Ensuite, vous allez appliquer l’injection de dépendances aux **vues** pour utiliser un service et afficher des informations. Enfin, vous allez étendre les filtres DI à ASP.NET MVC 4, en injectant un filtre d’action personnalisé dans la solution.

Dans ce laboratoire pratique, vous allez apprendre à :

- Intégrer ASP.NET MVC 4 avec Unity pour l’injection de dépendances à l’aide de packages NuGet
- Utiliser l’injection de dépendances dans un contrôleur MVC ASP.NET
- Utiliser l’injection de dépendances dans une vue MVC ASP.NET
- Utiliser l’injection de dépendances à l’intérieur d’un filtre d’action MVC ASP.NET

> [!NOTE]
> Ce laboratoire utilise le package NuGet Unity. Mvc3 pour la résolution des dépendances, mais il est possible d’adapter n’importe quel Framework d’injection de dépendances pour qu’il fonctionne avec ASP.NET MVC 4.

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

Ce laboratoire pratique est constitué des exercices suivants :

1. [Exercice 1 : injection d’un contrôleur](#Exercise1)
2. [Exercice 2 : injection d’une vue](#Exercise2)
3. [Exercice 3 : injection de filtres](#Exercise3)

> [!NOTE]
> Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices. Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.

Durée estimée pour effectuer ce laboratoire : **30 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Exercice 1 : injection d’un contrôleur

Dans cet exercice, vous allez apprendre à utiliser l’injection de dépendances dans les contrôleurs MVC ASP.NET en intégrant Unity à l’aide d’un package NuGet. Pour cette raison, vous allez inclure des services dans vos contrôleurs MvcMusicStore pour séparer la logique de l’accès aux données. Les services créeront une nouvelle dépendance dans le constructeur du contrôleur, qui sera résolu à l’aide de l’injection de dépendances à l’aide d' **Unity**.

Cette approche vous montrera comment générer des applications moins couplées, qui sont plus flexibles et plus faciles à gérer et à tester. Vous allez également apprendre à intégrer ASP.NET MVC avec Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>À propos du service StoreManager

Le magasin de musique MVC fourni dans la solution Begin comprend maintenant un service qui gère les données du contrôleur de magasin nommé **StoreService**. Vous trouverez ci-dessous l’implémentation du service Store. Notez que toutes les méthodes retournent des entités de modèle.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** à partir de la solution Begin consomme désormais **StoreService**. Toutes les références de données ont été supprimées de **StoreController**, et il est maintenant possible de modifier le fournisseur d’accès aux données actuel sans modifier une méthode qui consomme **StoreService**.

Vous trouverez ci-dessous que l’implémentation de **StoreController** a une dépendance avec **StoreService** à l’intérieur du constructeur de classe.

> [!NOTE]
> La dépendance introduite dans cet exercice est liée à l' **inversion de contrôle** (IOC).
> 
> Le constructeur de classe **StoreController** reçoit un paramètre de type **IStoreService** , qui est essentiel pour effectuer des appels de service à l’intérieur de la classe. Toutefois, **StoreController** n’implémente pas le constructeur par défaut (sans paramètres) que n’importe quel contrôleur doit avoir pour fonctionner avec ASP.NET MVC.
> 
> Pour résoudre la dépendance, le contrôleur doit être créé par une fabrique abstraite (une classe qui retourne un objet du type spécifié).

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Vous obtiendrez une erreur lorsque la classe tentera de créer le StoreController sans envoyer l’objet de service, car aucun constructeur sans paramètre n’est déclaré.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Tâche 1 : exécution de l’application

Dans cette tâche, vous allez exécuter l’application Begin, qui comprend le service dans le contrôleur de magasin qui sépare l’accès aux données de la logique d’application.

Lors de l’exécution de l’application, vous recevez une exception, car le service de contrôleur n’est pas transmis en tant que paramètre par défaut :

1. Ouvrez la solution **Begin** située dans **Source\Ex01-injecting Controller\Begin**.

   1. Vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Appuyez sur **CTRL + F5** pour exécuter l’application sans débogage. Vous obtiendrez le message d’erreur &quot;**aucun constructeur sans paramètre défini pour cet objet**&quot;:

    ![Erreur lors de l’exécution de ASP.NET MVC Begin application](aspnet-mvc-4-dependency-injection/_static/image3.png "Erreur lors de l’exécution de ASP.NET MVC Begin application")

    *Erreur lors de l’exécution de ASP.NET MVC Begin application*
3. Fermez le navigateur.

Dans les étapes suivantes, vous allez travailler sur la solution Store Music pour injecter la dépendance dont ce contrôleur a besoin.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Tâche 2 : inclure Unity dans la solution MvcMusicStore

Dans cette tâche, vous allez inclure **Unity. Mvc3** NuGet package à la solution.

> [!NOTE]
> Le package Unity. Mvc3 a été conçu pour ASP.NET MVC 3, mais il est entièrement compatible avec ASP.NET MVC 4.
> 
> Unity est un conteneur d’injection de dépendances léger et extensible avec prise en charge facultative de l’interception d’instances et de types. Il s’agit d’un conteneur à usage général à utiliser dans n’importe quel type d’application .NET. Il fournit toutes les fonctionnalités communes disponibles dans les mécanismes d’injection de dépendances, notamment la création d’objets, l’abstraction des exigences en spécifiant des dépendances au moment de l’exécution et la flexibilité, en reportant la configuration du composant dans le conteneur.

1. Installez le package NuGet **Unity. Mvc3** dans le projet **MvcMusicStore** . Pour ce faire, ouvrez la **console du gestionnaire de package** à partir de l' **affichage** | **autres fenêtres**.
2. Exécutez la commande suivante.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Installation du package NuGet Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Installation du package NuGet Unity. Mvc3")

    *Installation du package NuGet Unity. Mvc3*
3. Une fois le package **Unity. Mvc3** installé, explorez les fichiers et les dossiers qu’il ajoute automatiquement afin de simplifier la configuration Unity.

    ![Package Unity. Mvc3 installé](aspnet-mvc-4-dependency-injection/_static/image5.png "Package Unity. Mvc3 installé")

    *Package Unity. Mvc3 installé*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a>Tâche 3 : enregistrement d’Unity dans l’application Global.asax.cs\_démarrer

Dans cette tâche, vous allez mettre à jour l' **Application\_méthode Start** située dans **global.asax.cs** pour appeler l’initialiseur de programme d’amorçage Unity, puis mettre à jour le fichier du programme d’amorçage en inscrivant le service et le contrôleur que vous utiliserez pour l’injection de dépendances.

1. À présent, vous allez raccorder le programme d’amorçage qui est le fichier qui initialise le conteneur Unity et le résolveur de dépendance. Pour ce faire, ouvrez **global.asax.cs** et ajoutez le code en surbrillance suivant au sein de l' **application\_méthode Start** .

    (Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex01-Initialize Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Ouvrez le fichier **Bootstrapper.cs** .
3. Incluez les espaces de noms suivants : **MvcMusicStore. services** et **MusicStore. Controllers**.

    (Extrait de code- *ASP.net injection de dépendances Lab-Ex01-programme d’amorçage ajoutant des espaces de noms*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Remplacez le contenu de la méthode **BuildUnityContainer** par le code suivant qui enregistre le contrôleur de magasin et le service de banque d’informations.

    (Extrait de code- *ASP.net injection de dépendances Lab-Ex01-Register Store Controller and service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’application

Dans cette tâche, vous allez exécuter l’application pour vérifier qu’elle peut maintenant être chargée après l’intégration d’Unity.

1. Appuyez sur **F5** pour exécuter l’application, l’application doit maintenant être chargée sans qu’aucun message d’erreur ne s’affiche.

    ![Exécution de l’application avec injection de dépendances](aspnet-mvc-4-dependency-injection/_static/image6.png "Exécution de l’application avec injection de dépendances")

    *Exécution de l’application avec injection de dépendances*
2. Accédez à **/Store**. Cela appellera **StoreController**, qui est maintenant créé à l’aide d' **Unity**.

    ![Magasin de musique MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Magasin de musique MVC")

    *Magasin de musique MVC*
3. Fermez le navigateur.

Dans les exercices suivants, vous allez apprendre à étendre l’étendue de l’injection de dépendances pour l’utiliser dans les vues ASP.NET MVC et les filtres d’action.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Exercice 2 : injection d’une vue

Dans cet exercice, vous allez apprendre à utiliser l’injection de dépendances dans une vue avec les nouvelles fonctionnalités de ASP.NET MVC 4 pour l’intégration Unity. Pour ce faire, vous allez appeler un service personnalisé à l’intérieur de la vue de navigation du magasin, qui affiche un message et une image ci-dessous.

Ensuite, vous allez intégrer le projet à Unity et créer un programme de résolution de dépendance personnalisé pour injecter les dépendances.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Tâche 1 : création d’une vue qui consomme un service

Dans cette tâche, vous allez créer une vue qui effectue un appel de service pour générer une nouvelle dépendance. Le service est constitué d’un simple service de messagerie inclus dans cette solution.

1. Ouvrez la solution **Begin** située dans le dossier **Source\Ex02-injecting View\Begin** . Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
      > 
      > Pour plus d’informations, consultez cet article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluez les classes **MessageService.cs** et **IMessageService.cs** situées dans le dossier **\Assets source** dans **/services**. Pour ce faire, cliquez avec le bouton droit sur le dossier **services** et sélectionnez **Ajouter un élément existant**. Accédez à l’emplacement des fichiers et incluez-les.

    ![Ajout du service de message et de l’interface de service](aspnet-mvc-4-dependency-injection/_static/image8.png "Ajout du service de message et de l’interface de service")

    *Ajout du service de message et de l’interface de service*

    > [!NOTE]
    > L’interface **IMessageService** définit deux propriétés implémentées par la classe **MessageService** . Ces propriétés-**message** et **ImageUrl**-stockent le message et l’URL de l’image à afficher.
3. Créez le dossier **/pages** dans le dossier racine du projet, puis ajoutez la classe **MyBasePage.cs** existante à partir de **Source\Assets**. La page de base à partir de laquelle héritera a la structure suivante.

    ![Dossier pages](aspnet-mvc-4-dependency-injection/_static/image9.png "Dossier Pages")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Ouvrez la vue **Browse. cshtml** à partir du dossier **/views/Store** et faites en sorte qu’elle hérite de **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. Dans l’affichage **Parcourir** , ajoutez un appel à **MessageService** pour afficher une image et un message récupéré par le service.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Tâche 2 : inclure un résolveur de dépendance personnalisé et un activateur de page de vue personnalisé

Dans la tâche précédente, vous avez injecté une nouvelle dépendance à l’intérieur d’une vue pour effectuer un appel de service à l’intérieur de celle-ci. À présent, vous allez résoudre cette dépendance en implémentant les interfaces d’injection de dépendances MVC ASP.NET **IViewPageActivator** et **IDependencyResolver**. Vous allez inclure dans la solution une implémentation de **IDependencyResolver** qui traitera la récupération du service à l’aide d’Unity. Ensuite, vous allez inclure une autre implémentation personnalisée de l’interface **IViewPageActivator** qui résoudra la création des vues.

> [!NOTE]
> Depuis ASP.NET MVC 3, l’implémentation pour l’injection de dépendances avait simplifié les interfaces pour inscrire les services. **IDependencyResolver** et **IViewPageActivator** font partie des fonctionnalités ASP.NET MVC 3 pour l’injection de dépendances.
> 
> **-** L’interface IDependencyResolver remplace le IMvcServiceLocator précédent. Les implémenteurs de IDependencyResolver doivent retourner une instance du service ou une collection de services.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-** L’interface IViewPageActivator fournit un contrôle plus précis sur la façon dont les pages de vue sont instanciées via l’injection de dépendances. Les classes qui implémentent l’interface **IViewPageActivator** peuvent créer des instances de vue à l’aide des informations de contexte.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. Créez le dossier/**usines** dans le dossier racine du projet.
2. Incluez **CustomViewPageActivator.cs** à votre solution à partir de **/sources/Assets/** vers le dossier **usines** . Pour ce faire, cliquez avec le bouton droit sur le dossier **/Factories** , puis sélectionnez **Ajouter | Élément existant** , puis sélectionnez **CustomViewPageActivator.cs**. Cette classe implémente l’interface **IViewPageActivator** pour contenir le conteneur Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** est responsable de la gestion de la création d’une vue à l’aide d’un conteneur Unity.
3. Incluez le fichier **UnityDependencyResolver.cs** de **/sources/Assets** dans le dossier **/Factories** . Pour ce faire, cliquez avec le bouton droit sur le dossier **/Factories** , puis sélectionnez **Ajouter | Élément existant** , puis sélectionnez le fichier **UnityDependencyResolver.cs** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > La classe **UnityDependencyResolver** est un DependencyResolver personnalisé pour Unity. Lorsqu’un service est introuvable dans le conteneur Unity, le programme de résolution de base est invocated.

Dans la tâche suivante, les deux implémentations sont inscrites pour permettre au modèle de connaître l’emplacement des services et les vues.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Tâche 3 : inscription pour l’injection de dépendances dans un conteneur Unity

Dans cette tâche, vous allez regrouper toutes les opérations précédentes pour rendre le travail d’injection de dépendances.

Jusqu’à présent, votre solution comporte les éléments suivants :

- Vue de **navigation** qui hérite de **MyBaseClass** et consomme **MessageService**.
- Classe intermédiaire-**MyBaseClass**-qui a une injection de dépendances déclarée pour l’interface de service.
- Service- **MessageService** -et son interface **IMessageService**.
- Un programme de résolution des dépendances personnalisé pour Unity- **UnityDependencyResolver** , qui concerne la récupération du service.
- Une page de vue Activator- **CustomViewPageActivator** -qui crée la page.

Pour injecter la vue **Parcourir** , vous allez maintenant inscrire le résolveur de dépendance personnalisé dans le conteneur Unity.

1. Ouvrez le fichier **Bootstrapper.cs** .
2. Inscrivez une instance de **MessageService** dans le conteneur Unity pour initialiser le service :

    (Extrait de code- *ASP.net d’injection de dépendances Lab-Ex02-Register message service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Ajoutez une référence à l’espace de noms **MvcMusicStore. Factory** .

    (Extrait de code- *ASP.net injection de dépendances Lab-Ex02-Factory*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Inscrivez **CustomViewPageActivator** comme activateur de page de vue dans le conteneur Unity :

    (Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex02-Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Remplacez le résolveur de dépendance par défaut ASP.NET MVC 4 par une instance de **UnityDependencyResolver**. Pour ce faire, remplacez **initialiser** le contenu de la méthode par le code suivant :

    (Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex02-mettre à jour le*programme de résolution de dépendance)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC fournit une classe de programme de résolution de dépendance par défaut. Pour utiliser les programmes de résolution de dépendance personnalisés comme celui que nous avons créé pour Unity, ce programme de résolution doit être remplacé.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tâche 4 : exécution de l’application

Au cours de cette tâche, vous allez exécuter l’application pour vérifier que l’Explorateur Windows Store consomme le service et affiche l’image et le message récupérés :

1. Appuyez sur **F5** pour exécuter l’application.
2. Cliquez sur **Rock** dans le menu genres pour voir comment le **MessageService** a été injecté dans la vue et chargé le message d’accueil et l’image. Dans cet exemple, nous allons entrer dans &quot;&quot;**Rock** :

    ![Magasin de musique MVC-afficher l’injection](aspnet-mvc-4-dependency-injection/_static/image10.png "Magasin de musique MVC-afficher l’injection")

    *Magasin de musique MVC-afficher l’injection*
3. Fermez le navigateur.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Exercice 3 : injection de filtres d’action

Dans les **filtres d’action personnalisée** de laboratoire pratiques précédents, vous avez travaillé avec des filtres de personnalisation et d’injection. Dans cet exercice, vous allez apprendre à injecter des filtres avec l’injection de dépendances à l’aide du conteneur Unity. Pour ce faire, vous allez ajouter à la solution Store Music un filtre d’action personnalisé qui effectuera le suivi de l’activité du site.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Tâche 1 : inclure le filtre de suivi dans la solution

Dans cette tâche, vous allez inclure dans le magasin musical un filtre d’action personnalisé pour suivre les événements. Comme les concepts de filtre d’action personnalisée sont déjà traités dans le laboratoire précédent &quot;les filtres d’action personnalisés&quot;, vous incluez simplement la classe de filtre dans le dossier composants de ce Lab, puis vous créez un fournisseur de filtres pour Unity :

1. Ouvrez la solution **Begin** située dans le dossier **Source\Ex03-injecting action Filter\Begin** . Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
      > 
      > Pour plus d’informations, consultez cet article : [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluez le fichier **TraceActionFilter.cs** de **/sources/Assets** dans le dossier **/Filters** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Ce filtre d’action personnalisé effectue le suivi ASP.NET. Pour plus d’informations, consultez &quot;filtres d’action locaux et dynamiques ASP.NET MVC 4&quot; Lab.
3. Ajoutez la classe vide **FilterProvider.cs** au projet dans le dossier **/Filters.**
4. Ajoutez les espaces de noms **System. Web. Mvc** et **Microsoft. practices. unity** dans **FilterProvider.cs**.

    (Extrait de code- *ASP.net d’injection de dépendances Lab-Ex03-fournisseur de filtre ajoutant des espaces de noms*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Faites en sorte que la classe hérite de l’interface **IFilterProvider** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Ajoutez une propriété **IUnityContainer** dans la classe **FilterProvider** , puis créez un constructeur de classe pour assigner le conteneur.

    (Extrait de code- *test d’injection de dépendance ASP.net-Lab-Ex03-constructeur du fournisseur de filtre*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Le constructeur de classe du fournisseur de filtres ne crée pas un **nouvel** objet à l’intérieur de. Le conteneur est passé en tant que paramètre et la dépendance est résolue par Unity.
7. Dans la classe **FilterProvider** , implémentez la méthode **GetFilters** à partir de l’interface **IFilterProvider** .

    (Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex03-Provider GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Tâche 2 : inscription et activation du filtre

Dans cette tâche, vous allez activer le suivi de site. Pour ce faire, vous allez inscrire le filtre dans la méthode **Bootstrapper.cs BuildUnityContainer** pour démarrer le suivi :

1. Ouvrez le **fichier Web. config** situé dans la racine du projet et activez le suivi des traces sur le groupe System. Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Ouvrez **Bootstrapper.cs** à la racine du projet.
3. Ajoutez une référence à l’espace de noms **MvcMusicStore. filters** .

    (Extrait de code- *ASP.net injection de dépendances Lab-Ex03-programme d’amorçage ajoutant des espaces de noms*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Sélectionnez la méthode **BuildUnityContainer** et inscrivez le filtre dans le conteneur Unity. Vous devez inscrire le fournisseur de filtres et le filtre d’action.

    (Extrait de code- *laboratoire d’injection de dépendances ASP.net-Ex03-Register FilterProvider et ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tâche 3 : exécution de l’application

Dans cette tâche, vous allez exécuter l’application et vérifier que le filtre d’action personnalisé effectue le suivi de l’activité :

1. Appuyez sur **F5** pour exécuter l’application.
2. Cliquez sur **Rock** dans le menu genres. Vous pouvez accéder à plus de genres si vous le souhaitez.

    ![Magasin de musique](aspnet-mvc-4-dependency-injection/_static/image11.png "Magasin de musique")

    *Magasin de musique*
3. Accédez à **/trace.axd** pour afficher la page de trace de l’application, puis cliquez sur **afficher les détails**.

    ![Journal de suivi d’application](aspnet-mvc-4-dependency-injection/_static/image12.png "Journal de suivi d’application")

    *Journal de suivi d’application*

    ![Suivi de l’application-détails de la demande](aspnet-mvc-4-dependency-injection/_static/image13.png "Suivi de l’application-détails de la demande")

    *Suivi de l’application-détails de la demande*
4. Fermez le navigateur.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En effectuant ce laboratoire pratique, vous avez appris à utiliser l’injection de dépendances dans ASP.NET MVC 4 en intégrant Unity à l’aide d’un package NuGet. Pour ce faire, vous avez utilisé l’injection de dépendances dans les contrôleurs, les vues et les filtres d’action.

Les concepts suivants ont été abordés :

- Fonctionnalités d’injection de dépendances ASP.NET MVC 4
- Intégration Unity utilisant le package NuGet Unity. Mvc3
- Injection de dépendances dans les contrôleurs
- Injection de dépendances dans les vues
- Injection de dépendances de filtres d’action

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe A : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Vignette VS Express pour le Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Annexe B : utilisation d’extraits de code

Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main. Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.

![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-dependency-injection/_static/image19.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")

*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***

1. Placez le curseur à l’endroit où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).
3. Regarder comme IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).
5. Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-dependency-injection/_static/image20.png "Commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](aspnet-mvc-4-dependency-injection/_static/image21.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*

![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](aspnet-mvc-4-dependency-injection/_static/image22.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")

*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*

***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)*** 1,0. Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.

1. Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.
2. Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.

![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](aspnet-mvc-4-dependency-injection/_static/image23.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")

*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*

![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](aspnet-mvc-4-dependency-injection/_static/image24.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")

*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*
