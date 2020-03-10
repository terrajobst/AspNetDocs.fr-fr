---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Didacticiel : créer une application en temps réel haute fréquence avec Signalr 2 | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr pour fournir une fonctionnalité de messagerie à fréquence élevée.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558624"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="d96d7-103">Didacticiel : créer une application en temps réel haute fréquence avec Signalr 2</span><span class="sxs-lookup"><span data-stu-id="d96d7-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="d96d7-104">Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de messagerie à fréquence élevée.</span><span class="sxs-lookup"><span data-stu-id="d96d7-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="d96d7-105">Dans ce cas, « messagerie à fréquence élevée » signifie que le serveur envoie des mises à jour à une fréquence fixe.</span><span class="sxs-lookup"><span data-stu-id="d96d7-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="d96d7-106">Vous envoyez jusqu’à 10 messages par seconde.</span><span class="sxs-lookup"><span data-stu-id="d96d7-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="d96d7-107">L’application que vous créez affiche une forme que les utilisateurs peuvent faire glisser.</span><span class="sxs-lookup"><span data-stu-id="d96d7-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="d96d7-108">Le serveur met à jour la position de la forme dans tous les navigateurs connectés pour qu’elle corresponde à la position de la forme glissée à l’aide des mises à jour chronométrées.</span><span class="sxs-lookup"><span data-stu-id="d96d7-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="d96d7-109">Les concepts présentés dans ce didacticiel ont des applications en temps réel et d’autres applications de simulation.</span><span class="sxs-lookup"><span data-stu-id="d96d7-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="d96d7-110">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="d96d7-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d96d7-111">Configuration du projet</span><span class="sxs-lookup"><span data-stu-id="d96d7-111">Set up the project</span></span>
> * <span data-ttu-id="d96d7-112">Créer l’application de base</span><span class="sxs-lookup"><span data-stu-id="d96d7-112">Create the base application</span></span>
> * <span data-ttu-id="d96d7-113">Mapper au concentrateur au démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="d96d7-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="d96d7-114">Ajouter le client</span><span class="sxs-lookup"><span data-stu-id="d96d7-114">Add the client</span></span>
> * <span data-ttu-id="d96d7-115">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="d96d7-115">Run the app</span></span>
> * <span data-ttu-id="d96d7-116">Ajouter la boucle cliente</span><span class="sxs-lookup"><span data-stu-id="d96d7-116">Add the client loop</span></span>
> * <span data-ttu-id="d96d7-117">Ajouter la boucle serveur</span><span class="sxs-lookup"><span data-stu-id="d96d7-117">Add the server loop</span></span>
> * <span data-ttu-id="d96d7-118">Ajouter une animation lisse</span><span class="sxs-lookup"><span data-stu-id="d96d7-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="d96d7-119">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="d96d7-119">Prerequisites</span></span>

* <span data-ttu-id="d96d7-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail **Développement ASP.NET et web**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="d96d7-121">Configuration du projet</span><span class="sxs-lookup"><span data-stu-id="d96d7-121">Set up the project</span></span>

<span data-ttu-id="d96d7-122">Dans cette section, vous allez créer le projet dans Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d96d7-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="d96d7-123">Cette section montre comment utiliser Visual Studio 2017 pour créer une application Web ASP.NET vide et ajouter les bibliothèques Signalr et jQuery. UI.</span><span class="sxs-lookup"><span data-stu-id="d96d7-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="d96d7-124">Dans Visual Studio, créez une application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d96d7-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Créer un site Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="d96d7-126">Dans la fenêtre **nouvelle application Web ASP.net-MoveShapeDemo** , laissez **vide** sélectionné, puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="d96d7-127">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="d96d7-128">Dans **Ajouter un nouvel élément-MoveShapeDemo**, **Sélectionnez installé** > **Visual C#**  > **Web** > **signalr** , puis sélectionnez **classe de concentrateur signalr (v2)** .</span><span class="sxs-lookup"><span data-stu-id="d96d7-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="d96d7-129">Nommez la classe *MoveShapeHub* et ajoutez-la au projet.</span><span class="sxs-lookup"><span data-stu-id="d96d7-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="d96d7-130">Cette étape crée le fichier de classe *MoveShapeHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="d96d7-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="d96d7-131">Simultanément, elle ajoute un ensemble de fichiers de script et de références d’assembly qui prennent en charge Signalr au projet.</span><span class="sxs-lookup"><span data-stu-id="d96d7-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="d96d7-132">Cliquez sur **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="d96d7-133">Dans la **console du gestionnaire de package**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d96d7-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="d96d7-134">La commande installe la bibliothèque de l’interface utilisateur jQuery.</span><span class="sxs-lookup"><span data-stu-id="d96d7-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="d96d7-135">Vous l’utilisez pour animer la forme.</span><span class="sxs-lookup"><span data-stu-id="d96d7-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="d96d7-136">Dans **Explorateur de solutions**, développez le nœud scripts.</span><span class="sxs-lookup"><span data-stu-id="d96d7-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Références de la bibliothèque de scripts](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="d96d7-138">Les bibliothèques de scripts pour jQuery, jQueryUI et Signalr sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="d96d7-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="d96d7-139">Créer l’application de base</span><span class="sxs-lookup"><span data-stu-id="d96d7-139">Create the base application</span></span>

<span data-ttu-id="d96d7-140">Dans cette section, vous allez créer une application de navigateur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-140">In this section, you create a browser application.</span></span> <span data-ttu-id="d96d7-141">L’application envoie l’emplacement de la forme au serveur lors de chaque événement de déplacement de la souris.</span><span class="sxs-lookup"><span data-stu-id="d96d7-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="d96d7-142">Le serveur diffuse ces informations à tous les autres clients connectés en temps réel.</span><span class="sxs-lookup"><span data-stu-id="d96d7-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="d96d7-143">Vous en apprendrez davantage sur cette application dans les sections ultérieures.</span><span class="sxs-lookup"><span data-stu-id="d96d7-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="d96d7-144">Ouvrez le fichier *MoveShapeHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="d96d7-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="d96d7-145">Remplacez le code du fichier *MoveShapeHub.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d96d7-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="d96d7-146">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="d96d7-146">Save the file.</span></span>

<span data-ttu-id="d96d7-147">La classe `MoveShapeHub` est une implémentation d’un concentrateur Signalr.</span><span class="sxs-lookup"><span data-stu-id="d96d7-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="d96d7-148">Comme dans le didacticiel [prise en main avec signalr](tutorial-getting-started-with-signalr.md) , le Hub a une méthode que les clients appellent directement.</span><span class="sxs-lookup"><span data-stu-id="d96d7-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="d96d7-149">Dans ce cas, le client envoie un objet avec les nouvelles coordonnées X et Y de la forme au serveur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="d96d7-150">Ces coordonnées sont diffusées à tous les autres clients connectés.</span><span class="sxs-lookup"><span data-stu-id="d96d7-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="d96d7-151">Signalr sérialise automatiquement cet objet à l’aide de JSON.</span><span class="sxs-lookup"><span data-stu-id="d96d7-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="d96d7-152">L’application envoie l’objet `ShapeModel` au client.</span><span class="sxs-lookup"><span data-stu-id="d96d7-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="d96d7-153">Il a des membres pour stocker la position de la forme.</span><span class="sxs-lookup"><span data-stu-id="d96d7-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="d96d7-154">La version de l’objet sur le serveur a également un membre pour suivre les données du client qui sont stockées.</span><span class="sxs-lookup"><span data-stu-id="d96d7-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="d96d7-155">Cet objet empêche le serveur de renvoyer les données d’un client à lui-même.</span><span class="sxs-lookup"><span data-stu-id="d96d7-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="d96d7-156">Ce membre utilise l’attribut `JsonIgnore` pour empêcher l’application de sérialiser les données et de les renvoyer au client.</span><span class="sxs-lookup"><span data-stu-id="d96d7-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="d96d7-157">Mapper au concentrateur au démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="d96d7-157">Map to the hub when app starts</span></span>

<span data-ttu-id="d96d7-158">Ensuite, vous configurez le mappage au hub lorsque l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="d96d7-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="d96d7-159">Dans Signalr 2, l’ajout d’une classe de démarrage OWIN crée le mappage.</span><span class="sxs-lookup"><span data-stu-id="d96d7-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="d96d7-160">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="d96d7-161">Dans **Ajouter un nouvel élément-MoveShapeDemo** , sélectionnez **installé** > **Visual C#**  > **Web** , puis sélectionnez **classe de démarrage OWIN**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="d96d7-162">Nommez le *démarrage* de la classe, puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="d96d7-163">Remplacez le code par défaut dans le fichier *Startup.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d96d7-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="d96d7-164">La classe de démarrage OWIN appelle `MapSignalR` lorsque l’application exécute la méthode `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="d96d7-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="d96d7-165">L’application ajoute la classe au processus de démarrage de OWIN à l’aide de l’attribut d’assembly `OwinStartup`.</span><span class="sxs-lookup"><span data-stu-id="d96d7-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="d96d7-166">Ajouter le client</span><span class="sxs-lookup"><span data-stu-id="d96d7-166">Add the client</span></span>

<span data-ttu-id="d96d7-167">Ajoutez la page HTML pour le client.</span><span class="sxs-lookup"><span data-stu-id="d96d7-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="d96d7-168">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **page HTML**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="d96d7-169">Nommez la page **par défaut** , puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="d96d7-170">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *default. html* , puis sélectionnez **définir comme page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="d96d7-171">Remplacez le code par défaut dans le fichier *default. html* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d96d7-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="d96d7-172">Dans **Explorateur de solutions**, développez **scripts**.</span><span class="sxs-lookup"><span data-stu-id="d96d7-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="d96d7-173">Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="d96d7-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d96d7-174">Le gestionnaire de package installe une version plus récente des scripts Signalr.</span><span class="sxs-lookup"><span data-stu-id="d96d7-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="d96d7-175">Mettez à jour les références de script dans le bloc de code afin qu’elles correspondent aux versions des fichiers de script dans le projet.</span><span class="sxs-lookup"><span data-stu-id="d96d7-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="d96d7-176">Ce code HTML et JavaScript crée une `div` rouge appelée `shape`.</span><span class="sxs-lookup"><span data-stu-id="d96d7-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="d96d7-177">Il active le comportement de glissement de la forme à l’aide de la bibliothèque jQuery et utilise l’événement `drag` pour envoyer la position de la forme au serveur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="d96d7-178">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="d96d7-178">Run the app</span></span>

<span data-ttu-id="d96d7-179">Vous pouvez exécuter l’application pour se’e.</span><span class="sxs-lookup"><span data-stu-id="d96d7-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="d96d7-180">Lorsque vous faites glisser la forme autour d’une fenêtre de navigateur, la forme se déplace également dans les autres navigateurs.</span><span class="sxs-lookup"><span data-stu-id="d96d7-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="d96d7-181">Dans la barre d’outils, activez le **débogage de script** , puis sélectionnez le bouton de lecture pour exécuter l’application en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="d96d7-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Capture d’écran de l’activation du mode de débogage par l’utilisateur et de la sélection de lecture.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="d96d7-183">Une fenêtre de navigateur s’ouvre avec la forme rouge dans le coin supérieur droit.</span><span class="sxs-lookup"><span data-stu-id="d96d7-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="d96d7-184">Copiez l’URL de la page.</span><span class="sxs-lookup"><span data-stu-id="d96d7-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="d96d7-185">Ouvrez un autre navigateur et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="d96d7-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="d96d7-186">Faites glisser la forme dans l’une des fenêtres du navigateur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="d96d7-187">La forme dans l’autre fenêtre de navigateur suit.</span><span class="sxs-lookup"><span data-stu-id="d96d7-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="d96d7-188">Bien que l’application fonctionne avec cette méthode, il ne s’agit pas d’un modèle de programmation recommandé.</span><span class="sxs-lookup"><span data-stu-id="d96d7-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="d96d7-189">Il n’existe aucune limite supérieure pour le nombre de messages envoyés.</span><span class="sxs-lookup"><span data-stu-id="d96d7-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="d96d7-190">Par conséquent, les clients et le serveur sont submergés par les messages et les dégradations de performances.</span><span class="sxs-lookup"><span data-stu-id="d96d7-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="d96d7-191">En outre, l’application affiche une animation disjointe sur le client.</span><span class="sxs-lookup"><span data-stu-id="d96d7-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="d96d7-192">Cette animation saccadée se produit parce que la forme se déplace instantanément par chaque méthode.</span><span class="sxs-lookup"><span data-stu-id="d96d7-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="d96d7-193">Il est préférable que la forme se déplace correctement vers chaque nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="d96d7-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="d96d7-194">Ensuite, vous allez apprendre à résoudre ces problèmes.</span><span class="sxs-lookup"><span data-stu-id="d96d7-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="d96d7-195">Ajouter la boucle cliente</span><span class="sxs-lookup"><span data-stu-id="d96d7-195">Add the client loop</span></span>

<span data-ttu-id="d96d7-196">L’envoi de l’emplacement de la forme sur chaque événement de déplacement de la souris crée un volume de trafic réseau inutile.</span><span class="sxs-lookup"><span data-stu-id="d96d7-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="d96d7-197">L’application doit limiter les messages du client.</span><span class="sxs-lookup"><span data-stu-id="d96d7-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="d96d7-198">Utilisez la fonction JavaScript `setInterval` pour configurer une boucle qui envoie de nouvelles informations de position au serveur à un taux fixe.</span><span class="sxs-lookup"><span data-stu-id="d96d7-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="d96d7-199">Cette boucle est une représentation de base d’une « boucle de jeu ».</span><span class="sxs-lookup"><span data-stu-id="d96d7-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="d96d7-200">Il s’agit d’une fonction appelée à plusieurs reprises qui pilote toutes les fonctionnalités d’un jeu.</span><span class="sxs-lookup"><span data-stu-id="d96d7-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="d96d7-201">Remplacez le code client dans le fichier *default. html* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="d96d7-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="d96d7-202">Vous devez reremplacer les références de script.</span><span class="sxs-lookup"><span data-stu-id="d96d7-202">You have to replace the script references again.</span></span> <span data-ttu-id="d96d7-203">Ils doivent correspondre aux versions des scripts dans le projet.</span><span class="sxs-lookup"><span data-stu-id="d96d7-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="d96d7-204">Ce nouveau code ajoute la fonction `updateServerModel`.</span><span class="sxs-lookup"><span data-stu-id="d96d7-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="d96d7-205">Elle est appelée sur une fréquence fixe.</span><span class="sxs-lookup"><span data-stu-id="d96d7-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="d96d7-206">La fonction envoie les données de position au serveur chaque fois que l’indicateur `moved` indique qu’il y a de nouvelles données de position à envoyer.</span><span class="sxs-lookup"><span data-stu-id="d96d7-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="d96d7-207">Sélectionner le bouton de lecture pour démarrer l’application</span><span class="sxs-lookup"><span data-stu-id="d96d7-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="d96d7-208">Copiez l’URL de la page.</span><span class="sxs-lookup"><span data-stu-id="d96d7-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="d96d7-209">Ouvrez un autre navigateur et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="d96d7-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="d96d7-210">Faites glisser la forme dans l’une des fenêtres du navigateur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="d96d7-211">La forme dans l’autre fenêtre de navigateur suit.</span><span class="sxs-lookup"><span data-stu-id="d96d7-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="d96d7-212">Étant donné que l’application limite le nombre de messages envoyés au serveur, l’animation ne s’affiche pas comme étant lisse au préalable.</span><span class="sxs-lookup"><span data-stu-id="d96d7-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="d96d7-213">Ajouter la boucle serveur</span><span class="sxs-lookup"><span data-stu-id="d96d7-213">Add the server loop</span></span>

<span data-ttu-id="d96d7-214">Dans l’application actuelle, les messages envoyés depuis le serveur vers le client se déplacent aussi souvent qu’ils sont reçus.</span><span class="sxs-lookup"><span data-stu-id="d96d7-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="d96d7-215">Ce trafic réseau présente un problème similaire, comme nous le voyons sur le client.</span><span class="sxs-lookup"><span data-stu-id="d96d7-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="d96d7-216">L’application peut envoyer des messages plus souvent que nécessaire.</span><span class="sxs-lookup"><span data-stu-id="d96d7-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="d96d7-217">La connexion peut être saturée par conséquent.</span><span class="sxs-lookup"><span data-stu-id="d96d7-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="d96d7-218">Cette section décrit comment mettre à jour le serveur pour ajouter un minuteur qui limite le taux des messages sortants.</span><span class="sxs-lookup"><span data-stu-id="d96d7-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="d96d7-219">Remplacez le contenu de `MoveShapeHub.cs` par ce code :</span><span class="sxs-lookup"><span data-stu-id="d96d7-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="d96d7-220">Sélectionnez le bouton de lecture pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="d96d7-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="d96d7-221">Copiez l’URL de la page.</span><span class="sxs-lookup"><span data-stu-id="d96d7-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="d96d7-222">Ouvrez un autre navigateur et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="d96d7-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="d96d7-223">Faites glisser la forme dans l’une des fenêtres du navigateur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="d96d7-224">Ce code développe le client pour ajouter la classe `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="d96d7-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="d96d7-225">La nouvelle classe limite les messages sortants à l’aide de la classe `Timer` à partir du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d96d7-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="d96d7-226">Il est bon de savoir que le Hub lui-même est passager.</span><span class="sxs-lookup"><span data-stu-id="d96d7-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="d96d7-227">Il est créé chaque fois qu’il est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="d96d7-227">It's created every time it's needed.</span></span> <span data-ttu-id="d96d7-228">L’application crée donc le `Broadcaster` en tant que singleton.</span><span class="sxs-lookup"><span data-stu-id="d96d7-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="d96d7-229">Elle utilise l’initialisation tardive pour différer la création de `Broadcaster`jusqu’à ce qu’elle soit nécessaire.</span><span class="sxs-lookup"><span data-stu-id="d96d7-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="d96d7-230">Cela garantit que l’application crée la première instance de Hub complètement avant de démarrer la minuterie.</span><span class="sxs-lookup"><span data-stu-id="d96d7-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="d96d7-231">L’appel à la fonction `UpdateShape` des clients est ensuite déplacé hors de la méthode de `UpdateModel` du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="d96d7-232">Elle n’est plus appelée immédiatement chaque fois que l’application reçoit des messages entrants.</span><span class="sxs-lookup"><span data-stu-id="d96d7-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="d96d7-233">Au lieu de cela, l’application envoie les messages aux clients à un débit de 25 appels par seconde.</span><span class="sxs-lookup"><span data-stu-id="d96d7-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="d96d7-234">Le processus est géré par le minuteur `_broadcastLoop` à partir de la classe `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="d96d7-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="d96d7-235">Enfin, au lieu d’appeler directement la méthode du client à partir du concentrateur, la classe `Broadcaster` doit obtenir une référence au concentrateur `_hubContext` en cours d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="d96d7-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="d96d7-236">Elle obtient la référence avec l' `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="d96d7-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="d96d7-237">Ajouter une animation lisse</span><span class="sxs-lookup"><span data-stu-id="d96d7-237">Add smooth animation</span></span>

<span data-ttu-id="d96d7-238">L’application est presque terminée, mais nous pourrions améliorer l’application.</span><span class="sxs-lookup"><span data-stu-id="d96d7-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="d96d7-239">L’application déplace la forme sur le client en réponse aux messages du serveur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="d96d7-240">Au lieu de définir la position de la forme sur le nouvel emplacement donné par le serveur, utilisez la fonction `animate` de la bibliothèque de l’interface utilisateur JQuery.</span><span class="sxs-lookup"><span data-stu-id="d96d7-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="d96d7-241">Il peut déplacer la forme en douceur entre sa position actuelle et la nouvelle position.</span><span class="sxs-lookup"><span data-stu-id="d96d7-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="d96d7-242">Mettez à jour la méthode de `updateShape` du client dans le fichier *default. html* pour qu’elle ressemble au code mis en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="d96d7-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="d96d7-243">Sélectionnez le bouton de lecture pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="d96d7-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="d96d7-244">Copiez l’URL de la page.</span><span class="sxs-lookup"><span data-stu-id="d96d7-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="d96d7-245">Ouvrez un autre navigateur et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="d96d7-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="d96d7-246">Faites glisser la forme dans l’une des fenêtres du navigateur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="d96d7-247">Le mouvement de la forme dans l’autre fenêtre apparaît moins saccadé.</span><span class="sxs-lookup"><span data-stu-id="d96d7-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="d96d7-248">L’application interpole ses mouvements dans le temps plutôt qu’une seule fois par message entrant.</span><span class="sxs-lookup"><span data-stu-id="d96d7-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="d96d7-249">Ce code déplace la forme de l’ancien emplacement vers le nouveau.</span><span class="sxs-lookup"><span data-stu-id="d96d7-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="d96d7-250">Le serveur donne la position de la forme au cours de l’intervalle d’animation.</span><span class="sxs-lookup"><span data-stu-id="d96d7-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="d96d7-251">Dans ce cas, il s’agit de 100 millisecondes.</span><span class="sxs-lookup"><span data-stu-id="d96d7-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="d96d7-252">L’application efface toute animation précédente s’exécutant sur la forme avant le démarrage de la nouvelle animation.</span><span class="sxs-lookup"><span data-stu-id="d96d7-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="d96d7-253">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="d96d7-253">Get the code</span></span>

[<span data-ttu-id="d96d7-254">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="d96d7-254">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="d96d7-255">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d96d7-255">Additional resources</span></span>

<span data-ttu-id="d96d7-256">Le paradigme de communication que vous venez de découvrir est utile pour le développement de jeux en ligne et d’autres simulations, comme [le jeu de pousseurs créé avec signalr](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="d96d7-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="d96d7-257">Pour plus d’informations sur Signalr, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="d96d7-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="d96d7-258">Projet signalr</span><span class="sxs-lookup"><span data-stu-id="d96d7-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="d96d7-259">Signalr GitHub et exemples</span><span class="sxs-lookup"><span data-stu-id="d96d7-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="d96d7-260">Wiki signalr</span><span class="sxs-lookup"><span data-stu-id="d96d7-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="d96d7-261">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="d96d7-261">Next steps</span></span>

<span data-ttu-id="d96d7-262">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="d96d7-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d96d7-263">Configuration du projet</span><span class="sxs-lookup"><span data-stu-id="d96d7-263">Set up the project</span></span>
> * <span data-ttu-id="d96d7-264">Création de l’application de base</span><span class="sxs-lookup"><span data-stu-id="d96d7-264">Created the base application</span></span>
> * <span data-ttu-id="d96d7-265">Mappé au concentrateur au démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="d96d7-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="d96d7-266">Ajout du client</span><span class="sxs-lookup"><span data-stu-id="d96d7-266">Added the client</span></span>
> * <span data-ttu-id="d96d7-267">L’application a été exécutée</span><span class="sxs-lookup"><span data-stu-id="d96d7-267">Ran the app</span></span>
> * <span data-ttu-id="d96d7-268">Ajout de la boucle client</span><span class="sxs-lookup"><span data-stu-id="d96d7-268">Added the client loop</span></span>
> * <span data-ttu-id="d96d7-269">Ajout de la boucle serveur</span><span class="sxs-lookup"><span data-stu-id="d96d7-269">Added the server loop</span></span>
> * <span data-ttu-id="d96d7-270">Animation lisse ajoutée</span><span class="sxs-lookup"><span data-stu-id="d96d7-270">Added smooth animation</span></span>

<span data-ttu-id="d96d7-271">Passez à l’article suivant pour apprendre à créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de diffusion serveur.</span><span class="sxs-lookup"><span data-stu-id="d96d7-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d96d7-272">Signalr 2 et diffusion du serveur</span><span class="sxs-lookup"><span data-stu-id="d96d7-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)