---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hôte API Web ASP.NET 2 dans un rôle de travail Azure-ASP.NET 4. x
author: MikeWasson
description: 'Didacticiel : héberger API Web ASP.NET dans un rôle de travail Azure, à l’aide de OWIN pour auto-héberger l’infrastructure de l’API Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556629"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="8e420-103">Hôte API Web ASP.NET 2 dans un rôle de travail Azure</span><span class="sxs-lookup"><span data-stu-id="8e420-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="8e420-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8e420-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8e420-105">Ce didacticiel montre comment héberger des API Web ASP.NET dans un rôle de travail Azure, à l’aide de OWIN pour auto-héberger l’infrastructure de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="8e420-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="8e420-106">[Open Web interface for .net](http://owin.org/) (OWIN) définit une abstraction entre les serveurs Web .net et les applications Web.</span><span class="sxs-lookup"><span data-stu-id="8e420-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="8e420-107">OWIN découple l’application Web du serveur, ce qui fait de OWIN idéal pour l’auto-hébergement d’une application Web dans votre propre processus, en dehors d’IIS, par exemple, à l’intérieur d’un rôle de travail Azure.</span><span class="sxs-lookup"><span data-stu-id="8e420-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="8e420-108">Dans ce didacticiel, vous allez utiliser le package Microsoft. Owin. Host. HttpListener, qui fournit un serveur HTTP utilisé pour héberger des applications OWIN.</span><span class="sxs-lookup"><span data-stu-id="8e420-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8e420-109">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="8e420-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="8e420-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8e420-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="8e420-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="8e420-111">Web API 2</span></span>
> - [<span data-ttu-id="8e420-112">Kit de développement logiciel (SDK) Azure pour .NET 2,3</span><span class="sxs-lookup"><span data-stu-id="8e420-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="8e420-113">Créer un projet Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="8e420-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="8e420-114">Démarrez Visual Studio avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="8e420-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="8e420-115">Des privilèges d’administrateur sont nécessaires pour déboguer l’application localement, à l’aide de l’émulateur de calcul Azure.</span><span class="sxs-lookup"><span data-stu-id="8e420-115">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="8e420-116">Dans le menu **fichier** , cliquez sur **nouveau**, puis sur **projet**.</span><span class="sxs-lookup"><span data-stu-id="8e420-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="8e420-117">Dans **modèles installés**, sous Visual C#, cliquez sur **Cloud** , puis sur **service Cloud Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="8e420-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="8e420-118">Nommez le projet « AzureApp », puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e420-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="8e420-119">Dans la boîte de dialogue **nouveau service Cloud Windows Azure** , double-cliquez sur **rôle de travail**.</span><span class="sxs-lookup"><span data-stu-id="8e420-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="8e420-120">Laissez le nom par défaut (« WorkerRole1 »).</span><span class="sxs-lookup"><span data-stu-id="8e420-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="8e420-121">Cette étape ajoute un rôle de travail à la solution.</span><span class="sxs-lookup"><span data-stu-id="8e420-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="8e420-122">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e420-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="8e420-123">La solution Visual Studio créée contient deux projets :</span><span class="sxs-lookup"><span data-stu-id="8e420-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="8e420-124">&quot;AzureApp&quot; définit les rôles et la configuration de l’application Azure.</span><span class="sxs-lookup"><span data-stu-id="8e420-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="8e420-125">&quot;WorkerRole1&quot; contient le code du rôle de travail.</span><span class="sxs-lookup"><span data-stu-id="8e420-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="8e420-126">En général, une application Azure peut contenir plusieurs rôles, bien que ce didacticiel utilise un rôle unique.</span><span class="sxs-lookup"><span data-stu-id="8e420-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="8e420-127">Ajouter l’API Web et les packages OWIN</span><span class="sxs-lookup"><span data-stu-id="8e420-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="8e420-128">Dans le menu **Outils** , cliquez sur **Gestionnaire de package NuGet**, puis sur **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="8e420-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="8e420-129">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="8e420-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="8e420-130">Ajouter un point de terminaison HTTP</span><span class="sxs-lookup"><span data-stu-id="8e420-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="8e420-131">Dans Explorateur de solutions, développez le projet AzureApp.</span><span class="sxs-lookup"><span data-stu-id="8e420-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="8e420-132">Développez le nœud rôles, cliquez avec le bouton droit sur WorkerRole1, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="8e420-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="8e420-133">Cliquez sur **Points de terminaison**, puis sur **Ajouter un point de terminaison**.</span><span class="sxs-lookup"><span data-stu-id="8e420-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="8e420-134">Dans la liste déroulante **protocole** , sélectionnez « http ».</span><span class="sxs-lookup"><span data-stu-id="8e420-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="8e420-135">Dans **port public** et **port privé**, tapez 80.</span><span class="sxs-lookup"><span data-stu-id="8e420-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="8e420-136">Ces derniers peuvent être différents.</span><span class="sxs-lookup"><span data-stu-id="8e420-136">These port numbers can be different.</span></span> <span data-ttu-id="8e420-137">Le port public est utilisé par les clients lorsqu’ils envoient une demande au rôle.</span><span class="sxs-lookup"><span data-stu-id="8e420-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="8e420-138">Configurer l’API Web pour l’auto-hébergement</span><span class="sxs-lookup"><span data-stu-id="8e420-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="8e420-139">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet WorkerRole1 et sélectionnez **Ajouter** une / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="8e420-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="8e420-140">Nommez la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8e420-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="8e420-141">Remplacez tout le code réutilisable dans ce fichier par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="8e420-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="8e420-142">Ajouter un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="8e420-142">Add a Web API Controller</span></span>

<span data-ttu-id="8e420-143">Ensuite, ajoutez une classe de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="8e420-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="8e420-144">Cliquez avec le bouton droit sur le projet WorkerRole1 et sélectionnez **ajouter** / **classe**.</span><span class="sxs-lookup"><span data-stu-id="8e420-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="8e420-145">Nommez la classe TestController.</span><span class="sxs-lookup"><span data-stu-id="8e420-145">Name the class TestController.</span></span> <span data-ttu-id="8e420-146">Remplacez tout le code réutilisable dans ce fichier par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="8e420-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="8e420-147">Par souci de simplicité, ce contrôleur définit simplement deux méthodes d’extraction qui retournent du texte brut.</span><span class="sxs-lookup"><span data-stu-id="8e420-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="8e420-148">Démarrer l’hôte OWIN</span><span class="sxs-lookup"><span data-stu-id="8e420-148">Start the OWIN Host</span></span>

<span data-ttu-id="8e420-149">Ouvrez le fichier WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="8e420-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="8e420-150">Cette classe définit le code qui s’exécute lorsque le rôle de travail est démarré et arrêté.</span><span class="sxs-lookup"><span data-stu-id="8e420-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="8e420-151">Ajoutez les instructions using suivantes :</span><span class="sxs-lookup"><span data-stu-id="8e420-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="8e420-152">Ajoutez un membre **IDisposable** à la classe `WorkerRole` :</span><span class="sxs-lookup"><span data-stu-id="8e420-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="8e420-153">Dans la méthode `OnStart`, ajoutez le code suivant pour démarrer l’hôte :</span><span class="sxs-lookup"><span data-stu-id="8e420-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="8e420-154">La méthode **WebApp. Start** démarre l’hôte OWIN.</span><span class="sxs-lookup"><span data-stu-id="8e420-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="8e420-155">Le nom de la classe `Startup` est un paramètre de type de la méthode.</span><span class="sxs-lookup"><span data-stu-id="8e420-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="8e420-156">Par Convention, l’hôte appellera la méthode `Configure` de cette classe.</span><span class="sxs-lookup"><span data-stu-id="8e420-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="8e420-157">Remplacez le `OnStop` pour supprimer l’instance d' *application\_* :</span><span class="sxs-lookup"><span data-stu-id="8e420-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="8e420-158">Voici le code complet pour WorkerRole.cs :</span><span class="sxs-lookup"><span data-stu-id="8e420-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="8e420-159">Générez la solution, puis appuyez sur F5 pour exécuter l’application localement dans l’émulateur de calcul Azure.</span><span class="sxs-lookup"><span data-stu-id="8e420-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="8e420-160">Selon les paramètres de votre pare-feu, vous devrez peut-être autoriser l’émulateur via votre pare-feu.</span><span class="sxs-lookup"><span data-stu-id="8e420-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="8e420-161">Si vous obtenez une exception semblable à la suivante, veuillez consulter ce billet de [blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) pour obtenir une solution de contournement.</span><span class="sxs-lookup"><span data-stu-id="8e420-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="8e420-162">«Impossible de charger le fichier ou l’assembly’Microsoft. Owin, version = 2.0.2.0, culture = neutral, PublicKeyToken = 31bf3856ad364e35 'ou l’une de ses dépendances.</span><span class="sxs-lookup"><span data-stu-id="8e420-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="8e420-163">La définition du manifeste de l’assembly trouvé ne correspond pas à la référence de l’assembly.</span><span class="sxs-lookup"><span data-stu-id="8e420-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="8e420-164">(Exception de HRESULT : 0x80131040)»</span><span class="sxs-lookup"><span data-stu-id="8e420-164">(Exception from HRESULT: 0x80131040)"</span></span>

<span data-ttu-id="8e420-165">L’émulateur de calcul affecte une adresse IP locale au point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="8e420-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="8e420-166">Vous pouvez trouver l’adresse IP en affichant l’interface utilisateur de l’émulateur de calcul.</span><span class="sxs-lookup"><span data-stu-id="8e420-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="8e420-167">Cliquez avec le bouton droit sur l’icône de l’émulateur dans la zone de notification de la barre des tâches, puis sélectionnez **afficher l’interface utilisateur de l’émulateur de calcul**.</span><span class="sxs-lookup"><span data-stu-id="8e420-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="8e420-168">Recherchez l’adresse IP sous déploiements du service, déploiement [id], Détails du service.</span><span class="sxs-lookup"><span data-stu-id="8e420-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="8e420-169">Ouvrez un navigateur Web et accédez à l'<em>adresse</em>http:///test/1, où <em>adresse</em> est l’adresse IP assignée par l’émulateur de calcul. par exemple, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="8e420-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="8e420-170">Vous devez voir la réponse du contrôleur d’API Web :</span><span class="sxs-lookup"><span data-stu-id="8e420-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="8e420-171">Déployer sur Azure</span><span class="sxs-lookup"><span data-stu-id="8e420-171">Deploy to Azure</span></span>

<span data-ttu-id="8e420-172">Pour cette étape, vous devez disposer d’un compte Azure.</span><span class="sxs-lookup"><span data-stu-id="8e420-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="8e420-173">Si vous n’en avez pas déjà un, vous pouvez créer un compte d’évaluation gratuit en quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="8e420-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="8e420-174">Pour plus d’informations, consultez [Microsoft Azure version d’évaluation gratuite](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="8e420-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="8e420-175">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet AzureApp.</span><span class="sxs-lookup"><span data-stu-id="8e420-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="8e420-176">Sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="8e420-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="8e420-177">Si vous n’êtes pas connecté à votre compte Azure, cliquez sur **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="8e420-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="8e420-178">Une fois que vous êtes connecté, choisissez un abonnement et cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="8e420-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="8e420-179">Entrez un nom pour le service Cloud et choisissez une région.</span><span class="sxs-lookup"><span data-stu-id="8e420-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="8e420-180">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="8e420-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="8e420-181">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="8e420-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="8e420-182">La fenêtre Journal d’activité Azure affiche la progression du déploiement.</span><span class="sxs-lookup"><span data-stu-id="8e420-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="8e420-183">Lorsque l’application est déployée, accédez à http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="8e420-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="8e420-184">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8e420-184">Additional Resources</span></span>

- [<span data-ttu-id="8e420-185">Vue d’ensemble du projet Katana</span><span class="sxs-lookup"><span data-stu-id="8e420-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="8e420-186">Projet Katana sur GitHub</span><span class="sxs-lookup"><span data-stu-id="8e420-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
