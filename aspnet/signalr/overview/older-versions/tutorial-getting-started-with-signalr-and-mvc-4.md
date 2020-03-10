---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Didacticiel : Prise en main avec Signalr 1. x et MVC 4 | Microsoft Docs'
author: bradygaster
description: Utilisez ASP.NET Signalr et ASP.NET MVC 4 pour créer une application de conversation en temps réel.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579568"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="41b0a-103">Didacticiel : Prise en main avec Signalr 1. x et MVC 4</span><span class="sxs-lookup"><span data-stu-id="41b0a-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>

<span data-ttu-id="41b0a-104">de [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="41b0a-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="41b0a-105">Ce didacticiel montre comment utiliser ASP.NET Signalr pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="41b0a-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="41b0a-106">Vous allez ajouter Signalr à une application MVC 4 et créer une vue conversation pour envoyer et afficher des messages.</span><span class="sxs-lookup"><span data-stu-id="41b0a-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="41b0a-107">Présentation</span><span class="sxs-lookup"><span data-stu-id="41b0a-107">Overview</span></span>

<span data-ttu-id="41b0a-108">Ce didacticiel vous présente le développement d’applications Web en temps réel avec ASP.NET Signalr et ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="41b0a-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="41b0a-109">Ce didacticiel utilise le même code d’application de conversation que le [didacticiel prise en main signalr](tutorial-getting-started-with-signalr.md), mais il montre comment l’ajouter à une application MVC 4 basée sur le modèle Internet.</span><span class="sxs-lookup"><span data-stu-id="41b0a-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="41b0a-110">Dans cette rubrique, vous allez apprendre les tâches de développement Signalr suivantes :</span><span class="sxs-lookup"><span data-stu-id="41b0a-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="41b0a-111">Ajout de la bibliothèque Signalr à une application MVC 4.</span><span class="sxs-lookup"><span data-stu-id="41b0a-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="41b0a-112">Création d’une classe de concentrateur pour transmettre le contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="41b0a-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="41b0a-113">Utilisation de la bibliothèque jQuery Signalr dans une page Web pour envoyer des messages et afficher des mises à jour à partir du Hub.</span><span class="sxs-lookup"><span data-stu-id="41b0a-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="41b0a-114">La capture d’écran suivante montre l’application chat terminée s’exécutant dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Instances de conversation](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="41b0a-116">Sections</span><span class="sxs-lookup"><span data-stu-id="41b0a-116">Sections:</span></span>

- [<span data-ttu-id="41b0a-117">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="41b0a-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="41b0a-118">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="41b0a-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="41b0a-119">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="41b0a-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="41b0a-120">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="41b0a-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="41b0a-121">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="41b0a-121">Set up the Project</span></span>

<span data-ttu-id="41b0a-122">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="41b0a-122">Prerequisites:</span></span>

- <span data-ttu-id="41b0a-123">Visual Studio 2010 SP1, Visual Studio 2012 ou Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="41b0a-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="41b0a-124">Si vous ne disposez pas de Visual Studio, consultez [ASP.net downloads](https://www.asp.net/downloads) pour obtenir l’outil gratuit visual studio 2012 Express Development.</span><span class="sxs-lookup"><span data-stu-id="41b0a-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="41b0a-125">Pour Visual Studio 2010, installez [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="41b0a-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="41b0a-126">Cette section montre comment créer une application ASP.NET MVC 4, ajouter la bibliothèque Signalr et créer l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="41b0a-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="41b0a-127">Dans Visual Studio, créez une application ASP.NET MVC 4, nommez-la SignalRChat, puis cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="41b0a-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="41b0a-128">Dans VS 2010, sélectionnez **.NET Framework 4** dans le contrôle de liste déroulante version du Framework.</span><span class="sxs-lookup"><span data-stu-id="41b0a-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="41b0a-129">Le code signalr s’exécute sur .NET Framework versions 4 et 4,5.</span><span class="sxs-lookup"><span data-stu-id="41b0a-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Créer un site Web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="41b0a-131">Sélectionnez le modèle application Internet, désactivez l’option de **création d’un projet de test unitaire**, puis cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="41b0a-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Créer un site Internet MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="41b0a-133">Ouvrez **outils > gestionnaire de package NuGet > console du gestionnaire de package** et exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="41b0a-133">Open the **Tools > NuGet Package Manager > Package Manager Console** and run the following command.</span></span> <span data-ttu-id="41b0a-134">Cette étape ajoute au projet un ensemble de fichiers de script et de références d’assembly qui activent la fonctionnalité Signalr.</span><span class="sxs-lookup"><span data-stu-id="41b0a-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="41b0a-135">Dans **Explorateur de solutions** développez le dossier scripts.</span><span class="sxs-lookup"><span data-stu-id="41b0a-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="41b0a-136">Notez que les bibliothèques de scripts pour Signalr ont été ajoutées au projet.</span><span class="sxs-lookup"><span data-stu-id="41b0a-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Références de bibliothèque](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="41b0a-138">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter | Nouveau dossier**, puis ajoutez un nouveau dossier nommé **hubs**.</span><span class="sxs-lookup"><span data-stu-id="41b0a-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="41b0a-139">Cliquez avec le bouton droit sur le dossier **hubs** , puis cliquez sur **Ajouter |** Et créez une nouvelle C# classe nommée **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="41b0a-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="41b0a-140">Vous allez utiliser cette classe comme concentrateur de serveur Signalr qui envoie des messages à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="41b0a-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="41b0a-141">Si vous utilisez Visual Studio 2012 et que vous avez installé la [mise à jour ASP.NET et Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), vous pouvez utiliser le nouveau modèle d’élément signalr pour créer la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="41b0a-142">Pour ce faire, cliquez avec le bouton droit sur le dossier **hubs** , puis cliquez sur **Ajouter | Nouvel élément**, sélectionnez **classe de concentrateur signalr (v1)** et nommez la classe **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="41b0a-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>

1. <span data-ttu-id="41b0a-143">Remplacez le code de la classe **ChatHub** par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="41b0a-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="41b0a-144">Ouvrez le fichier **global. asax** pour le projet, puis ajoutez un appel à la méthode `RouteTable.Routes.MapHubs();` en tant que première ligne de code dans la méthode `Application_Start`.</span><span class="sxs-lookup"><span data-stu-id="41b0a-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="41b0a-145">Ce code inscrit l’itinéraire par défaut pour les concentrateurs Signalr et doit être appelé avant d’inscrire d’autres itinéraires.</span><span class="sxs-lookup"><span data-stu-id="41b0a-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="41b0a-146">La méthode `Application_Start` terminée ressemble à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="41b0a-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="41b0a-147">Modifiez la classe `HomeController` trouvée dans **Controllers/HomeController. cs** et ajoutez la méthode suivante à la classe.</span><span class="sxs-lookup"><span data-stu-id="41b0a-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="41b0a-148">Cette méthode retourne l’affichage **conversation** que vous allez créer dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="41b0a-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="41b0a-149">Cliquez avec le bouton droit dans la méthode de `Chat` que vous venez de créer, puis cliquez sur **Ajouter une vue** pour créer un nouveau fichier de vue.</span><span class="sxs-lookup"><span data-stu-id="41b0a-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="41b0a-150">Dans la boîte de dialogue **Ajouter une vue** , assurez-vous que la case à cocher est activée pour **utiliser une disposition ou une page maître** (désactivez les autres cases à cocher), puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="41b0a-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Ajouter une vue](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="41b0a-152">Modifiez le nouveau fichier de vue nommé **chat. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="41b0a-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="41b0a-153">Après la balise &lt;H2&gt;, collez la section de&gt; &lt;div suivante et `@section scripts` bloc de code dans la page.</span><span class="sxs-lookup"><span data-stu-id="41b0a-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="41b0a-154">Ce script permet à la page d’envoyer des messages de conversation et d’afficher des messages à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="41b0a-155">Le code complet de l’affichage conversation s’affiche dans le bloc de code suivant.</span><span class="sxs-lookup"><span data-stu-id="41b0a-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="41b0a-156">Quand vous ajoutez Signalr et d’autres bibliothèques de scripts à votre projet Visual Studio, le gestionnaire de package peut installer des versions des scripts qui sont plus récents que les versions présentées dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="41b0a-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="41b0a-157">Assurez-vous que les références de script dans votre code correspondent aux versions des bibliothèques de scripts installées dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="41b0a-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="41b0a-158">**Enregistrer tout** pour le projet.</span><span class="sxs-lookup"><span data-stu-id="41b0a-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="41b0a-159">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="41b0a-159">Run the Sample</span></span>

1. <span data-ttu-id="41b0a-160">Appuyez sur F5 pour exécuter le projet en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="41b0a-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="41b0a-161">Dans la ligne d’adresse du navigateur, ajoutez **/Home/chat** à l’URL de la page par défaut du projet.</span><span class="sxs-lookup"><span data-stu-id="41b0a-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="41b0a-162">La page de conversation est chargée dans une instance de navigateur et vous invite à entrer un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="41b0a-164">Entrez un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-164">Enter a user name.</span></span>
4. <span data-ttu-id="41b0a-165">Copiez l’URL à partir de la ligne d’adresse du navigateur et utilisez-la pour ouvrir deux autres instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="41b0a-166">Dans chaque instance de navigateur, entrez un nom d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="41b0a-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="41b0a-167">Dans chaque instance de navigateur, ajoutez un commentaire et cliquez sur **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="41b0a-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="41b0a-168">Les commentaires doivent s’afficher dans toutes les instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="41b0a-169">Cette simple application de conversation ne gère pas le contexte de discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="41b0a-170">Le concentrateur diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="41b0a-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="41b0a-171">Les utilisateurs qui rejoignent la conversation voient les messages ajoutés à partir du moment où ils se joignent.</span><span class="sxs-lookup"><span data-stu-id="41b0a-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="41b0a-172">La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="41b0a-174">Dans **Explorateur de solutions**, examinez le nœud **documents de script** pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="41b0a-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="41b0a-175">Ce nœud est visible en mode débogage si vous utilisez Internet Explorer comme navigateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="41b0a-176">Un fichier de script nommé **hubs** est généré dynamiquement au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="41b0a-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="41b0a-177">Ce fichier gère la communication entre le script jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="41b0a-178">Si vous utilisez un navigateur autre qu’Internet Explorer, vous pouvez également accéder au fichier **hubs** dynamiques en y accédant directement, par exemple http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="41b0a-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Script de concentrateur généré](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="41b0a-180">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="41b0a-180">Examine the Code</span></span>

<span data-ttu-id="41b0a-181">L’application de conversation vocale Signalr montre deux tâches de développement Signalr de base : la création d’un hub en tant qu’objet de coordination principal sur le serveur et l’utilisation de la bibliothèque jQuery de Signalr pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="41b0a-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="41b0a-182">Hubs SignalR</span><span class="sxs-lookup"><span data-stu-id="41b0a-182">SignalR Hubs</span></span>

<span data-ttu-id="41b0a-183">Dans l’exemple de code, la classe **ChatHub** dérive de la classe **Microsoft. Aspnet. signalr. Hub** .</span><span class="sxs-lookup"><span data-stu-id="41b0a-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="41b0a-184">La dérivation à partir de la classe de **concentrateur** est un moyen utile de générer une application signalr.</span><span class="sxs-lookup"><span data-stu-id="41b0a-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="41b0a-185">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur, puis accéder à ces méthodes en les appelant à partir de scripts jQuery dans une page Web.</span><span class="sxs-lookup"><span data-stu-id="41b0a-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="41b0a-186">Dans le code de conversation, les clients appellent la méthode **ChatHub. Send** pour envoyer un nouveau message.</span><span class="sxs-lookup"><span data-stu-id="41b0a-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="41b0a-187">Le Hub envoie ensuite le message à tous les clients en appelant **clients. All. addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="41b0a-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="41b0a-188">La méthode **Send** illustre plusieurs concepts de concentrateur :</span><span class="sxs-lookup"><span data-stu-id="41b0a-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="41b0a-189">Déclarez les méthodes publiques sur un concentrateur afin que les clients puissent les appeler.</span><span class="sxs-lookup"><span data-stu-id="41b0a-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="41b0a-190">Utilisez la propriété **Microsoft. Aspnet. signalr. Hub. clients** pour accéder à tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="41b0a-191">Appelez une fonction jQuery sur le client (par exemple, la fonction `addNewMessageToPage`) pour mettre à jour les clients.</span><span class="sxs-lookup"><span data-stu-id="41b0a-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="41b0a-192">Signalr et jQuery</span><span class="sxs-lookup"><span data-stu-id="41b0a-192">SignalR and jQuery</span></span>

<span data-ttu-id="41b0a-193">Le fichier de vue de **conversation. cshtml** dans l’exemple de code montre comment utiliser la bibliothèque jQuery signalr pour communiquer avec un concentrateur signalr.</span><span class="sxs-lookup"><span data-stu-id="41b0a-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="41b0a-194">Les tâches essentielles du code sont la création d’une référence au proxy généré automatiquement pour le concentrateur, la déclaration d’une fonction que le serveur peut appeler pour transmettre du contenu aux clients et le démarrage d’une connexion pour envoyer des messages au Hub.</span><span class="sxs-lookup"><span data-stu-id="41b0a-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="41b0a-195">Le code suivant déclare un proxy pour un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="41b0a-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="41b0a-196">Dans jQuery, la référence à la classe de serveur et à ses membres est en casse mixte.</span><span class="sxs-lookup"><span data-stu-id="41b0a-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="41b0a-197">L’exemple de code fait C# référence à la classe **ChatHub** dans jQuery en tant que **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="41b0a-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="41b0a-198">Si vous souhaitez référencer la classe `ChatHub` dans jQuery avec la casse conventionnelle conventionnel comme vous C#le feriez dans, modifiez le fichier de classe ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="41b0a-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="41b0a-199">Ajoutez une instruction `using` pour référencer l’espace de noms `Microsoft.AspNet.SignalR.Hubs`.</span><span class="sxs-lookup"><span data-stu-id="41b0a-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="41b0a-200">Ajoutez ensuite l’attribut `HubName` à la classe `ChatHub`, par exemple `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="41b0a-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="41b0a-201">Enfin, mettez à jour votre référence jQuery à la classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="41b0a-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>

<span data-ttu-id="41b0a-202">Le code suivant montre comment créer une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="41b0a-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="41b0a-203">La classe de concentrateur sur le serveur appelle cette fonction pour transmettre les mises à jour de contenu à chaque client.</span><span class="sxs-lookup"><span data-stu-id="41b0a-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="41b0a-204">L’appel facultatif à la fonction `htmlEncode` montre comment encoder le contenu du message au format HTML avant de l’afficher dans la page pour empêcher l’injection de script.</span><span class="sxs-lookup"><span data-stu-id="41b0a-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="41b0a-205">Le code suivant montre comment ouvrir une connexion avec le Hub.</span><span class="sxs-lookup"><span data-stu-id="41b0a-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="41b0a-206">Le code démarre la connexion, puis lui passe une fonction pour gérer l’événement de clic sur le bouton **Envoyer** dans la page de conversation.</span><span class="sxs-lookup"><span data-stu-id="41b0a-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="41b0a-207">Cette approche garantit que la connexion est établie avant l’exécution du gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="41b0a-207">This approach ensures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="41b0a-208">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="41b0a-208">Next Steps</span></span>

<span data-ttu-id="41b0a-209">Vous avez appris que Signalr est une infrastructure permettant de créer des applications Web en temps réel.</span><span class="sxs-lookup"><span data-stu-id="41b0a-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="41b0a-210">Vous avez également appris plusieurs tâches de développement Signalr : comment ajouter Signalr à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du Hub.</span><span class="sxs-lookup"><span data-stu-id="41b0a-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="41b0a-211">Pour en savoir plus sur les concepts d’évolution de Signalr plus avancés, visitez les sites suivants pour le code source et les ressources Signalr :</span><span class="sxs-lookup"><span data-stu-id="41b0a-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="41b0a-212">Projet signalr</span><span class="sxs-lookup"><span data-stu-id="41b0a-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="41b0a-213">Signalr GitHub et exemples</span><span class="sxs-lookup"><span data-stu-id="41b0a-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="41b0a-214">Wiki signalr</span><span class="sxs-lookup"><span data-stu-id="41b0a-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
