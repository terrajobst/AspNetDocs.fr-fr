---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Didacticiel : conversation en temps réel avec Signalr 2 et MVC 5 | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment utiliser ASP.NET Signalr 2 pour créer une application de conversation en temps réel. Vous ajoutez Signalr à une application MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600482"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="e4c5f-104">Didacticiel : conversation en temps réel avec Signalr 2 et MVC 5</span><span class="sxs-lookup"><span data-stu-id="e4c5f-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="e4c5f-105">Ce didacticiel montre comment utiliser ASP.NET Signalr 2 pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="e4c5f-106">Vous ajoutez Signalr à une application MVC 5 et créez une vue conversation pour envoyer et afficher des messages.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="e4c5f-107">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e4c5f-108">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="e4c5f-108">Set up the project</span></span>
> * <span data-ttu-id="e4c5f-109">Exécuter l'exemple</span><span class="sxs-lookup"><span data-stu-id="e4c5f-109">Run the sample</span></span>
> * <span data-ttu-id="e4c5f-110">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="e4c5f-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="e4c5f-111">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="e4c5f-111">Prerequisites</span></span>

* <span data-ttu-id="e4c5f-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail de **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="e4c5f-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="e4c5f-113">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="e4c5f-113">Set up the Project</span></span>

<span data-ttu-id="e4c5f-114">Cette section montre comment utiliser Visual Studio 2017 et Signalr 2 pour créer une application ASP.NET MVC 5 vide, ajouter la bibliothèque Signalr et créer l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="e4c5f-115">Dans Visual Studio, créez une C# application ASP.net qui cible .NET Framework 4,5, nommez-la SignalRChat, puis cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Créer un site Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="e4c5f-117">Dans **nouvelle application Web ASP.net-SignalRMvcChat**, sélectionnez **MVC** , puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="e4c5f-118">Dans **modifier l’authentification**, sélectionnez **aucune authentification** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Sélectionner aucune authentification](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="e4c5f-120">Dans **nouvelle application Web ASP.net-SignalRMvcChat**, sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="e4c5f-121">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="e4c5f-122">Dans **Ajouter un nouvel élément-SignalRChat**, **Sélectionnez installé** > **Visual C#**  > **Web** > **signalr** , puis sélectionnez **classe de concentrateur signalr (v2)** .</span><span class="sxs-lookup"><span data-stu-id="e4c5f-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="e4c5f-123">Nommez la classe *ChatHub* et ajoutez-la au projet.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="e4c5f-124">Cette étape crée le fichier de classe *ChatHub.cs* et ajoute un ensemble de fichiers de script et de références d’assembly qui prennent en charge signalr au projet.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="e4c5f-125">Remplacez le code dans le nouveau fichier de classe *ChatHub.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="e4c5f-126">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="e4c5f-127">Nommez le nouveau *démarrage* de la classe et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="e4c5f-128">Remplacez le code dans le fichier de classe *Startup.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="e4c5f-129">Dans **Explorateur de solutions**, sélectionnez **contrôleurs** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="e4c5f-130">Ajoutez cette méthode à *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="e4c5f-131">Cette méthode retourne l’affichage **conversation** que vous avez créé lors d’une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="e4c5f-132">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **vues** > **page**d’affichage, puis sélectionnez **Ajouter** une **vue**de >  .</span><span class="sxs-lookup"><span data-stu-id="e4c5f-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="e4c5f-133">Dans **Ajouter une vue**, nommez la nouvelle vue **conversation** , puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="e4c5f-134">Remplacez le contenu de **conversation. cshtml** par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="e4c5f-135">Dans **Explorateur de solutions**, développez **scripts**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="e4c5f-136">Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e4c5f-137">Le gestionnaire de package peut avoir installé une version plus récente des scripts Signalr.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="e4c5f-138">Vérifiez que les références de script dans le bloc de code correspondent aux versions des fichiers de script dans le projet.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="e4c5f-139">Références de script à partir du bloc de code d’origine :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="e4c5f-140">Si elles ne correspondent pas, mettez à jour le fichier *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e4c5f-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="e4c5f-141">Dans la barre de menus, sélectionnez **fichier** > **enregistrer tout**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="e4c5f-142">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="e4c5f-142">Run the Sample</span></span>

1. <span data-ttu-id="e4c5f-143">Dans la barre d’outils, activez le **débogage de script** , puis sélectionnez le bouton de lecture pour exécuter l’exemple en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="e4c5f-145">Lorsque le navigateur s’ouvre, entrez un nom pour votre identité de conversation.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="e4c5f-146">Copiez l’URL à partir du navigateur, ouvrez deux autres navigateurs, puis collez les URL dans les barres d’adresses.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="e4c5f-147">Dans chaque navigateur, entrez un nom unique.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="e4c5f-148">Maintenant, ajoutez un commentaire et sélectionnez **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="e4c5f-149">Répétez cette opération dans les autres navigateurs.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="e4c5f-150">Les commentaires s’affichent en temps réel.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4c5f-151">Cette simple application de conversation ne gère pas le contexte de discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="e4c5f-152">Le concentrateur diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="e4c5f-153">Les utilisateurs qui rejoignent la conversation voient les messages ajoutés à partir du moment où ils se joignent.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="e4c5f-154">Découvrez comment l’application de conversation s’exécute dans trois navigateurs différents.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="e4c5f-155">Lorsque Tom, Anand et Suzanne envoient des messages, tous les navigateurs sont mis à jour en temps réel :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Les trois navigateurs affichent le même historique de conversation](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="e4c5f-157">Dans **Explorateur de solutions**, examinez le nœud **documents de script** pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="e4c5f-158">Un fichier de script nommé *hubs* est généré par la bibliothèque signalr au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="e4c5f-159">Ce fichier gère la communication entre le script jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script hubs généré automatiquement dans le nœud documents de script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="e4c5f-161">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="e4c5f-161">Examine the Code</span></span>

<span data-ttu-id="e4c5f-162">L’application de conversation vocale Signalr illustre deux tâches de développement Signalr de base.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="e4c5f-163">Il vous montre comment créer un Hub.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-163">It shows you how to create a hub.</span></span> <span data-ttu-id="e4c5f-164">Le serveur utilise ce concentrateur comme objet de coordination principal.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="e4c5f-165">Le Hub utilise la bibliothèque jQuery Signalr pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="e4c5f-166">Concentrateurs signalr dans le ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="e4c5f-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="e4c5f-167">Dans l’exemple de code, la classe `ChatHub` dérive de la classe `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="e4c5f-168">La dérivation de la classe `Hub` est un moyen utile de générer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="e4c5f-169">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur, puis accéder à ces méthodes en les appelant à partir de scripts dans une page Web.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="e4c5f-170">Dans le code de conversation, les clients appellent la méthode `ChatHub.Send` pour envoyer un nouveau message.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="e4c5f-171">Le Hub envoie ensuite le message à tous les clients en appelant `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="e4c5f-172">La méthode `Send` illustre plusieurs concepts de concentrateur :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="e4c5f-173">Déclarez les méthodes publiques sur un concentrateur afin que les clients puissent les appeler.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="e4c5f-174">Utilisez la propriété dynamique `Microsoft.AspNet.SignalR.Hub.Clients` pour communiquer avec tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="e4c5f-175">Appelez une fonction sur le client (comme la fonction `addNewMessageToPage`) pour mettre à jour les clients.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="e4c5f-176">Signalr et jQuery chat. cshtml</span><span class="sxs-lookup"><span data-stu-id="e4c5f-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="e4c5f-177">Le fichier de vue de *conversation. cshtml* dans l’exemple de code montre comment utiliser la bibliothèque jQuery signalr pour communiquer avec un concentrateur signalr.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="e4c5f-178">Le code effectue de nombreuses tâches importantes.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-178">The code carries out many important tasks.</span></span> <span data-ttu-id="e4c5f-179">Elle crée une référence au proxy généré automatiquement pour le concentrateur, déclare une fonction que le serveur peut appeler pour envoyer le contenu aux clients et démarre une connexion pour envoyer des messages au Hub.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="e4c5f-180">En JavaScript, la référence à la classe de serveur et à ses membres se trouve dans la casse mixte.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="e4c5f-181">L’exemple de code fait C# référence à la classe `ChatHub` dans JavaScript comme `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="e4c5f-182">Dans ce bloc de code, vous créez une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="e4c5f-183">La classe de concentrateur sur le serveur appelle cette fonction pour transmettre les mises à jour de contenu à chaque client.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="e4c5f-184">L’appel facultatif à la fonction `htmlEncode` montre comment encoder le contenu du message au format HTML avant de l’afficher dans la page.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="e4c5f-185">C’est un moyen d’empêcher l’injection de scripts.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="e4c5f-186">Ce code ouvre une connexion avec le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="e4c5f-187">Cette approche garantit que vous établissez une connexion avant que le gestionnaire d’événements s’exécute.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="e4c5f-188">Le code démarre la connexion, puis lui passe une fonction pour gérer l’événement de clic sur le bouton **Envoyer** dans la page de conversation.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="e4c5f-189">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="e4c5f-189">Get the code</span></span>

[<span data-ttu-id="e4c5f-190">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="e4c5f-190">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a><span data-ttu-id="e4c5f-191">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e4c5f-191">Additional resources</span></span>

<span data-ttu-id="e4c5f-192">Pour plus d’informations sur Signalr, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-192">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="e4c5f-193">Projet signalr</span><span class="sxs-lookup"><span data-stu-id="e4c5f-193">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="e4c5f-194">Signalr GitHub et exemples</span><span class="sxs-lookup"><span data-stu-id="e4c5f-194">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="e4c5f-195">Wiki signalr</span><span class="sxs-lookup"><span data-stu-id="e4c5f-195">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="e4c5f-196">Étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-196">Next steps</span></span>

<span data-ttu-id="e4c5f-197">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4c5f-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e4c5f-198">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="e4c5f-198">Set up the project</span></span>
> * <span data-ttu-id="e4c5f-199">Exécution de l’exemple</span><span class="sxs-lookup"><span data-stu-id="e4c5f-199">Ran the sample</span></span>
> * <span data-ttu-id="e4c5f-200">Examen du code</span><span class="sxs-lookup"><span data-stu-id="e4c5f-200">Examined the code</span></span>

<span data-ttu-id="e4c5f-201">Passez à l’article suivant pour apprendre à créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de messagerie à fréquence élevée.</span><span class="sxs-lookup"><span data-stu-id="e4c5f-201">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="e4c5f-202">Application Web avec messagerie à fréquence élevée</span><span class="sxs-lookup"><span data-stu-id="e4c5f-202">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)