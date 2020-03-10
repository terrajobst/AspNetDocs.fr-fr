---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Didacticiel : Prise en main avec Signalr 1. x | Microsoft Docs'
author: bradygaster
description: Utilisez ASP.NET Signalr pour créer une application de conversation en temps réel dans une page HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623542"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="cf2a5-103">Didacticiel : Prise en main avec Signalr 1. x</span><span class="sxs-lookup"><span data-stu-id="cf2a5-103">Tutorial: Getting Started with SignalR 1.x</span></span>

<span data-ttu-id="cf2a5-104">de [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="cf2a5-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="cf2a5-105">Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="cf2a5-106">Vous allez ajouter Signalr à une application Web ASP.NET vide et créer une page HTML pour envoyer et afficher des messages.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="cf2a5-107">Présentation</span><span class="sxs-lookup"><span data-stu-id="cf2a5-107">Overview</span></span>

<span data-ttu-id="cf2a5-108">Ce didacticiel présente le développement Signalr en expliquant comment créer une application de conversation simple basée sur un navigateur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="cf2a5-109">Vous allez ajouter la bibliothèque Signalr à une application Web ASP.NET vide, créer une classe de concentrateur pour envoyer des messages aux clients et créer une page HTML qui permet aux utilisateurs d’envoyer et de recevoir des messages de conversation.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="cf2a5-110">Pour obtenir un didacticiel similaire qui montre comment créer une application de conversation dans MVC 4 à l’aide d’une vue MVC, consultez [prise en main avec signalr et MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="cf2a5-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="cf2a5-111">Ce didacticiel utilise la version Release (1. x) de Signalr.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="cf2a5-112">Pour plus d’informations sur les modifications apportées entre Signalr 1. x et 2,0, consultez [mise à niveau des projets signalr 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="cf2a5-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="cf2a5-113">Signalr est une bibliothèque .NET Open source permettant de créer des applications Web nécessitant des mises à jour en temps réel des données ou des interactions avec l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="cf2a5-114">Exemples : applications sociales, jeux multi-utilisateurs, collaboration professionnelle, Actualités, météo ou mises à jour financières.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="cf2a5-115">Il s’agit souvent d’applications en temps réel.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-115">These are often called real-time applications.</span></span>

<span data-ttu-id="cf2a5-116">Signalr simplifie le processus de création d’applications en temps réel.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="cf2a5-117">Il comprend une bibliothèque de serveur ASP.NET et une bibliothèque cliente JavaScript pour faciliter la gestion des connexions client-serveur et l’envoi de mises à jour de contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="cf2a5-118">Vous pouvez ajouter la bibliothèque Signalr à une application ASP.NET existante pour obtenir des fonctionnalités en temps réel.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="cf2a5-119">Ce didacticiel présente les tâches de développement Signalr suivantes :</span><span class="sxs-lookup"><span data-stu-id="cf2a5-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="cf2a5-120">Ajout de la bibliothèque Signalr à une application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="cf2a5-121">Création d’une classe de concentrateur pour transmettre le contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="cf2a5-122">Utilisation de la bibliothèque jQuery Signalr dans une page Web pour envoyer des messages et afficher des mises à jour à partir du Hub.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="cf2a5-123">La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="cf2a5-124">Chaque nouvel utilisateur peut poster des commentaires et voir les commentaires ajoutés après que l’utilisateur s’est joint à la conversation.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instances de conversation](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="cf2a5-126">Sections</span><span class="sxs-lookup"><span data-stu-id="cf2a5-126">Sections:</span></span>

- [<span data-ttu-id="cf2a5-127">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="cf2a5-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="cf2a5-128">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="cf2a5-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="cf2a5-129">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="cf2a5-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="cf2a5-130">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="cf2a5-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="cf2a5-131">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="cf2a5-131">Set up the Project</span></span>

<span data-ttu-id="cf2a5-132">Cette section montre comment créer une application Web ASP.NET vide, ajouter Signalr et créer l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="cf2a5-133">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="cf2a5-133">Prerequisites:</span></span>

- <span data-ttu-id="cf2a5-134">Visual Studio 2010 SP1 ou 2012.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="cf2a5-135">Si vous ne disposez pas de Visual Studio, consultez [ASP.net downloads](https://www.asp.net/downloads) pour obtenir l’outil gratuit visual studio 2012 Express Development.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="cf2a5-136">[Microsoft ASP.NET et Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="cf2a5-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="cf2a5-137">Pour Visual Studio 2012, ce programme d’installation ajoute de nouvelles fonctionnalités ASP.NET, notamment des modèles Signalr à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="cf2a5-138">Pour Visual Studio 2010 SP1, un programme d’installation n’est pas disponible, mais vous pouvez suivre le didacticiel en installant le package NuGet Signalr, comme décrit dans la procédure d’installation.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="cf2a5-139">Les étapes suivantes utilisent Visual Studio 2012 pour créer une application Web vide ASP.NET et ajouter la bibliothèque Signalr :</span><span class="sxs-lookup"><span data-stu-id="cf2a5-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="cf2a5-140">Dans Visual Studio, créez une application Web vide ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Créer un site Web vide](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="cf2a5-142">Ouvrez la **console du gestionnaire de package** en sélectionnant **Outils | Gestionnaire de package NuGet | Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-142">Open the **Package Manager Console** by selecting **Tools | NuGet Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="cf2a5-143">Entrez la commande suivante dans la fenêtre de console :</span><span class="sxs-lookup"><span data-stu-id="cf2a5-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="cf2a5-144">Cette commande installe la dernière version de Signalr 1. x.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="cf2a5-145">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter | Classe**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="cf2a5-146">Nommez la nouvelle classe **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="cf2a5-147">Dans **Explorateur de solutions** développez le nœud scripts.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="cf2a5-148">Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Références de bibliothèque](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="cf2a5-150">Remplacez le code de la classe **ChatHub** par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="cf2a5-151">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis cliquez sur **Ajouter | Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="cf2a5-152">Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **classe d’application globale** , puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Ajouter global](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="cf2a5-154">Ajoutez les instructions `using` suivantes après les instructions `using` fournies dans la classe Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="cf2a5-155">Ajoutez la ligne de code suivante à la méthode `Application_Start` de la classe globale pour enregistrer l’itinéraire par défaut pour les concentrateurs Signalr.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="cf2a5-156">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis cliquez sur **Ajouter | Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="cf2a5-157">Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez page HTML, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="cf2a5-158">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page HTML que vous venez de créer, puis cliquez sur **définir comme page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="cf2a5-159">Remplacez le code par défaut dans la page HTML par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="cf2a5-160">**Enregistrer tout** pour le projet.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="cf2a5-161">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="cf2a5-161">Run the Sample</span></span>

1. <span data-ttu-id="cf2a5-162">Appuyez sur F5 pour exécuter le projet en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="cf2a5-163">La page HTML est chargée dans une instance de navigateur et demande un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="cf2a5-165">Entrez un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-165">Enter a user name.</span></span>
3. <span data-ttu-id="cf2a5-166">Copiez l’URL à partir de la ligne d’adresse du navigateur et utilisez-la pour ouvrir deux autres instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="cf2a5-167">Dans chaque instance de navigateur, entrez un nom d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="cf2a5-168">Dans chaque instance de navigateur, ajoutez un commentaire et cliquez sur **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="cf2a5-169">Les commentaires doivent s’afficher dans toutes les instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cf2a5-170">Cette simple application de conversation ne gère pas le contexte de discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="cf2a5-171">Le concentrateur diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="cf2a5-172">Les utilisateurs qui rejoignent la conversation voient les messages ajoutés à partir du moment où ils se joignent.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="cf2a5-173">La capture d’écran suivante montre l’application de conversation en cours d’exécution dans trois instances de navigateur, toutes mises à jour lorsqu’une instance envoie un message :</span><span class="sxs-lookup"><span data-stu-id="cf2a5-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="cf2a5-175">Dans **Explorateur de solutions**, examinez le nœud **documents de script** pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="cf2a5-176">Un fichier de script nommé **hubs** est généré dynamiquement au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="cf2a5-177">Ce fichier gère la communication entre le script jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Script de concentrateur généré](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="cf2a5-179">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="cf2a5-179">Examine the Code</span></span>

<span data-ttu-id="cf2a5-180">L’application de conversation vocale Signalr montre deux tâches de développement Signalr de base : la création d’un hub en tant qu’objet de coordination principal sur le serveur et l’utilisation de la bibliothèque jQuery de Signalr pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="cf2a5-181">Hubs SignalR</span><span class="sxs-lookup"><span data-stu-id="cf2a5-181">SignalR Hubs</span></span>

<span data-ttu-id="cf2a5-182">Dans l’exemple de code, la classe **ChatHub** dérive de la classe **Microsoft. Aspnet. signalr. Hub** .</span><span class="sxs-lookup"><span data-stu-id="cf2a5-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="cf2a5-183">La dérivation à partir de la classe de **concentrateur** est un moyen utile de générer une application signalr.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="cf2a5-184">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur, puis accéder à ces méthodes en les appelant à partir de scripts jQuery dans une page Web.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="cf2a5-185">Dans le code de conversation, les clients appellent la méthode **ChatHub. Send** pour envoyer un nouveau message.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="cf2a5-186">Le Hub envoie ensuite le message à tous les clients en appelant **clients. All. broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="cf2a5-187">La méthode **Send** illustre plusieurs concepts de concentrateur :</span><span class="sxs-lookup"><span data-stu-id="cf2a5-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="cf2a5-188">Déclarez les méthodes publiques sur un concentrateur afin que les clients puissent les appeler.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="cf2a5-189">Utilisez la propriété dynamique **Microsoft. Aspnet. signalr. Hub. clients** pour accéder à tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="cf2a5-190">Appelez une fonction jQuery sur le client (par exemple, la fonction `broadcastMessage`) pour mettre à jour les clients.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="cf2a5-191">Signalr et jQuery</span><span class="sxs-lookup"><span data-stu-id="cf2a5-191">SignalR and jQuery</span></span>

<span data-ttu-id="cf2a5-192">La page HTML de l’exemple de code montre comment utiliser la bibliothèque jQuery Signalr pour communiquer avec un concentrateur Signalr.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="cf2a5-193">Les tâches essentielles du code déclarent un proxy pour faire référence au Hub, en déclarant une fonction que le serveur peut appeler pour envoyer du contenu aux clients et en commençant une connexion pour envoyer des messages au Hub.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="cf2a5-194">Le code suivant déclare un proxy pour un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="cf2a5-195">Dans jQuery, la référence à la classe de serveur et à ses membres est en casse mixte.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="cf2a5-196">L’exemple de code fait C# référence à la classe **ChatHub** dans jQuery en tant que **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>

<span data-ttu-id="cf2a5-197">Le code suivant montre comment créer une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="cf2a5-198">La classe de concentrateur sur le serveur appelle cette fonction pour transmettre les mises à jour de contenu à chaque client.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="cf2a5-199">Les deux lignes qui encodent le contenu au format HTML avant de l’afficher sont facultatives et montrent un moyen simple d’empêcher l’injection de script.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="cf2a5-200">Le code suivant montre comment ouvrir une connexion avec le Hub.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="cf2a5-201">Le code démarre la connexion, puis lui passe une fonction pour gérer l’événement de clic sur le bouton **Envoyer** dans la page html.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="cf2a5-202">Cette approche garantit que la connexion est établie avant l’exécution du gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-202">This approach insures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="cf2a5-203">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="cf2a5-203">Next Steps</span></span>

<span data-ttu-id="cf2a5-204">Vous avez appris que Signalr est une infrastructure permettant de créer des applications Web en temps réel.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="cf2a5-205">Vous avez également appris plusieurs tâches de développement Signalr : comment ajouter Signalr à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du Hub.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="cf2a5-206">Vous pouvez rendre l’exemple d’application dans ce didacticiel ou d’autres applications signaler disponibles sur Internet en les déployant sur un fournisseur d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="cf2a5-207">Microsoft offre un hébergement Web gratuit pour un maximum de 10 sites Web dans un [compte d’essai gratuit de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="cf2a5-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="cf2a5-208">Pour obtenir une procédure pas à pas sur le déploiement de l’exemple d’application Signalr, consultez [publier l’exemple signalr prise en main en tant que site Web Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="cf2a5-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="cf2a5-209">Pour plus d’informations sur le déploiement d’un projet Web Visual Studio sur un site Web Windows Azure, consultez [déploiement d’une Application ASP.net sur un site Web Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="cf2a5-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="cf2a5-210">(Remarque : le transport WebSocket n’est actuellement pas pris en charge pour les sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="cf2a5-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="cf2a5-211">Lorsque le transport WebSocket n’est pas disponible, Signalr utilise les autres transports disponibles, comme décrit dans la section Transports de la [rubrique Présentation de signalr](index.md).)</span><span class="sxs-lookup"><span data-stu-id="cf2a5-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="cf2a5-212">Pour en savoir plus sur les concepts d’évolution de Signalr plus avancés, visitez les sites suivants pour le code source et les ressources Signalr :</span><span class="sxs-lookup"><span data-stu-id="cf2a5-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="cf2a5-213">Projet signalr</span><span class="sxs-lookup"><span data-stu-id="cf2a5-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="cf2a5-214">Signalr GitHub et exemples</span><span class="sxs-lookup"><span data-stu-id="cf2a5-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="cf2a5-215">Wiki signalr</span><span class="sxs-lookup"><span data-stu-id="cf2a5-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
