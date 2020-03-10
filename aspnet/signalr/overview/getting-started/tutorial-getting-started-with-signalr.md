---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Didacticiel : conversation en temps réel avec Signalr 2 | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel. Vous ajoutez Signalr à une application Web ASP.NET vide.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536980"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="4b4a8-104">Didacticiel : conversation en temps réel avec Signalr 2</span><span class="sxs-lookup"><span data-stu-id="4b4a8-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="4b4a8-105">Ce didacticiel vous montre comment utiliser Signalr pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="4b4a8-106">Vous ajoutez Signalr à une application Web ASP.NET vide et créez une page HTML pour envoyer et afficher des messages.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="4b4a8-107">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="4b4a8-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4b4a8-108">Configuration du projet</span><span class="sxs-lookup"><span data-stu-id="4b4a8-108">Set up the project</span></span>
> * <span data-ttu-id="4b4a8-109">Exécution de l'exemple</span><span class="sxs-lookup"><span data-stu-id="4b4a8-109">Run the sample</span></span>
> * <span data-ttu-id="4b4a8-110">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="4b4a8-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="4b4a8-111">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="4b4a8-111">Prerequisites</span></span>

* <span data-ttu-id="4b4a8-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail **Développement ASP.NET et web**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="4b4a8-113">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="4b4a8-113">Set up the Project</span></span>

<span data-ttu-id="4b4a8-114">Cette section montre comment utiliser Visual Studio 2017 et Signalr 2 pour créer une application Web ASP.NET vide, ajouter Signalr et créer l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="4b4a8-115">Dans Visual Studio, créez une application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Créer un site Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="4b4a8-117">Dans la fenêtre **nouveau projet ASP.net-SignalRChat** , laissez **vide** sélectionné et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="4b4a8-118">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="4b4a8-119">Dans **Ajouter un nouvel élément-SignalRChat**, **Sélectionnez installé** > **Visual C#**  > **Web** > **signalr** , puis sélectionnez **classe de concentrateur signalr (v2)** .</span><span class="sxs-lookup"><span data-stu-id="4b4a8-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="4b4a8-120">Nommez la classe *ChatHub* et ajoutez-la au projet.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="4b4a8-121">Cette étape crée le fichier de classe *ChatHub.cs* et ajoute un ensemble de fichiers de script et de références d’assembly qui prennent en charge signalr au projet.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="4b4a8-122">Remplacez le code dans le nouveau fichier de classe *ChatHub.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4b4a8-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="4b4a8-123">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="4b4a8-124">Dans **Ajouter un nouvel élément-SignalRChat** , sélectionnez **installé** > **Visual C#**  > **Web** , puis sélectionnez **classe de démarrage OWIN**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="4b4a8-125">Nommez le *démarrage* de la classe et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="4b4a8-126">Remplacez le code par défaut de la classe *Startup* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4b4a8-126">Replace the default code in *Startup* class with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="4b4a8-127">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **page HTML**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-127">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="4b4a8-128">Nommez le nouvel *index* de page et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-128">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="4b4a8-129">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page HTML que vous avez créée, puis sélectionnez **définir comme page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-129">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="4b4a8-130">Remplacez le code par défaut de la page HTML par ce code :</span><span class="sxs-lookup"><span data-stu-id="4b4a8-130">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="4b4a8-131">Dans **Explorateur de solutions**, développez **scripts**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-131">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="4b4a8-132">Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-132">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4b4a8-133">Le gestionnaire de package peut avoir installé une version plus récente des scripts Signalr.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-133">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="4b4a8-134">Vérifiez que les références de script dans le bloc de code correspondent aux versions des fichiers de script dans le projet.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-134">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="4b4a8-135">Références de script à partir du bloc de code d’origine :</span><span class="sxs-lookup"><span data-stu-id="4b4a8-135">Script references from the original code block:</span></span>

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="4b4a8-136">Si elles ne correspondent pas, mettez à jour le fichier *. html* .</span><span class="sxs-lookup"><span data-stu-id="4b4a8-136">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="4b4a8-137">Dans la barre de menus, sélectionnez **fichier** > **enregistrer tout**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-137">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="4b4a8-138">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="4b4a8-138">Run the Sample</span></span>

1. <span data-ttu-id="4b4a8-139">Dans la barre d’outils, activez le **débogage de script** , puis sélectionnez le bouton de lecture pour exécuter l’exemple en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-139">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="4b4a8-141">Lorsque le navigateur s’ouvre, entrez un nom pour votre identité de conversation.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-141">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="4b4a8-142">Copiez l’URL à partir du navigateur, ouvrez deux autres navigateurs, puis collez les URL dans les barres d’adresses.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-142">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="4b4a8-143">Dans chaque navigateur, entrez un nom unique.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-143">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="4b4a8-144">Maintenant, ajoutez un commentaire et sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-144">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="4b4a8-145">Répétez cette opération dans les autres navigateurs.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-145">Repeat that in the other browsers.</span></span> <span data-ttu-id="4b4a8-146">Les commentaires s’affichent en temps réel.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-146">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4b4a8-147">Cette simple application de conversation ne gère pas le contexte de discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-147">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="4b4a8-148">Le concentrateur diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-148">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="4b4a8-149">Les utilisateurs qui rejoignent la conversation voient les messages ajoutés à partir du moment où ils se joignent.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-149">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="4b4a8-150">Découvrez comment l’application de conversation s’exécute dans trois navigateurs différents.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-150">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="4b4a8-151">Lorsque Tom, Anand et Suzanne envoient des messages, tous les navigateurs sont mis à jour en temps réel :</span><span class="sxs-lookup"><span data-stu-id="4b4a8-151">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Les trois navigateurs affichent le même historique de conversation](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="4b4a8-153">Dans **Explorateur de solutions**, examinez le nœud **documents de script** pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-153">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="4b4a8-154">Un fichier de script nommé *hubs* est généré par la bibliothèque signalr au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-154">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="4b4a8-155">Ce fichier gère la communication entre le script jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-155">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script hubs généré automatiquement dans le nœud documents de script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="4b4a8-157">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="4b4a8-157">Examine the Code</span></span>

<span data-ttu-id="4b4a8-158">L’application SignalRChat illustre deux tâches de développement Signalr de base.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-158">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="4b4a8-159">Il vous montre comment créer un Hub.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-159">It shows you how to create a hub.</span></span> <span data-ttu-id="4b4a8-160">Le serveur utilise ce concentrateur comme objet de coordination principal.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-160">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="4b4a8-161">Le Hub utilise la bibliothèque jQuery Signalr pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-161">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="4b4a8-162">Concentrateurs signalr dans le ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="4b4a8-162">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="4b4a8-163">Dans l’exemple de code ci-dessus, la classe `ChatHub` dérive de la classe `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-163">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="4b4a8-164">La dérivation de la classe `Hub` est un moyen utile de générer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-164">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="4b4a8-165">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur, puis utiliser ces méthodes en les appelant à partir de scripts dans une page Web.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-165">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="4b4a8-166">Dans le code de conversation, les clients appellent la méthode `ChatHub.Send` pour envoyer un nouveau message.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-166">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="4b4a8-167">Le Hub envoie ensuite le message à tous les clients en appelant `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-167">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="4b4a8-168">La méthode `Send` illustre plusieurs concepts de concentrateur :</span><span class="sxs-lookup"><span data-stu-id="4b4a8-168">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="4b4a8-169">Déclarez les méthodes publiques sur un concentrateur afin que les clients puissent les appeler.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-169">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="4b4a8-170">Utilisez la propriété dynamique `Microsoft.AspNet.SignalR.Hub.Clients` pour communiquer avec tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-170">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="4b4a8-171">Appelez une fonction sur le client (comme la fonction `broadcastMessage`) pour mettre à jour les clients.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-171">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="4b4a8-172">Signalr et jQuery dans index. html</span><span class="sxs-lookup"><span data-stu-id="4b4a8-172">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="4b4a8-173">La page *index. html* de l’exemple de code montre comment utiliser la bibliothèque jQuery de signalr pour communiquer avec un concentrateur signalr.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-173">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="4b4a8-174">Le code effectue de nombreuses tâches importantes.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-174">The code carries out many important tasks.</span></span> <span data-ttu-id="4b4a8-175">Elle déclare un proxy pour référencer le concentrateur, déclare une fonction que le serveur peut appeler pour envoyer le contenu aux clients et démarre une connexion pour envoyer des messages au Hub.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-175">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="4b4a8-176">En JavaScript, la référence à la classe de serveur et à ses membres doit être la casse mixte.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-176">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="4b4a8-177">L’exemple de code fait C# référence à la classe *ChatHub* dans JavaScript en tant que `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-177">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="4b4a8-178">Dans ce bloc de code, vous créez une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-178">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="4b4a8-179">La classe de concentrateur sur le serveur appelle cette fonction pour transmettre les mises à jour de contenu à chaque client.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-179">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="4b4a8-180">Les deux lignes qui encodent le contenu au format HTML avant de l’afficher sont facultatives et présentent un bon moyen d’empêcher l’injection de script.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-180">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="4b4a8-181">Ce code ouvre une connexion avec le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-181">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="4b4a8-182">Cette approche garantit que le code établit une connexion avant l’exécution du gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-182">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="4b4a8-183">Le code démarre la connexion, puis lui passe une fonction pour gérer l’événement de clic sur le bouton **Envoyer** dans la page html.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-183">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="4b4a8-184">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="4b4a8-184">Get the code</span></span>

[<span data-ttu-id="4b4a8-185">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="4b4a8-185">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="4b4a8-186">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4b4a8-186">Additional resources</span></span>

<span data-ttu-id="4b4a8-187">Pour plus d’informations sur Signalr, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="4b4a8-187">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="4b4a8-188">Projet signalr</span><span class="sxs-lookup"><span data-stu-id="4b4a8-188">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="4b4a8-189">Signalr GitHub et exemples</span><span class="sxs-lookup"><span data-stu-id="4b4a8-189">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="4b4a8-190">Wiki signalr</span><span class="sxs-lookup"><span data-stu-id="4b4a8-190">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="4b4a8-191">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="4b4a8-191">Next steps</span></span>

<span data-ttu-id="4b4a8-192">Dans ce didacticiel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="4b4a8-192">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4b4a8-193">Configuration du projet</span><span class="sxs-lookup"><span data-stu-id="4b4a8-193">Set up the project</span></span>
> * <span data-ttu-id="4b4a8-194">Exécution de l’exemple</span><span class="sxs-lookup"><span data-stu-id="4b4a8-194">Ran the sample</span></span>
> * <span data-ttu-id="4b4a8-195">Examen du code</span><span class="sxs-lookup"><span data-stu-id="4b4a8-195">Examined the code</span></span>

<span data-ttu-id="4b4a8-196">Passez à l’article suivant pour apprendre à utiliser Signalr et MVC 5.</span><span class="sxs-lookup"><span data-stu-id="4b4a8-196">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4b4a8-197">Signalr 2 et MVC 5</span><span class="sxs-lookup"><span data-stu-id="4b4a8-197">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)