---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Didacticiel : auto-hôte Signalr | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment créer un serveur auto-hébergé Signalr 2 et comment s’y connecter avec un client JavaScript. Versions logicielles utilisées dans le didacticiel V...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74578575"
---
# <a name="tutorial-signalr-self-host"></a><span data-ttu-id="41eb7-104">Didacticiel : auto-hôte Signalr</span><span class="sxs-lookup"><span data-stu-id="41eb7-104">Tutorial: SignalR Self-Host</span></span>

<span data-ttu-id="41eb7-105">de [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="41eb7-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[<span data-ttu-id="41eb7-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="41eb7-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="41eb7-107">Ce didacticiel montre comment créer un serveur auto-hébergé Signalr 2 et comment s’y connecter avec un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="41eb7-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="41eb7-108">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="41eb7-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="41eb7-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="41eb7-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="41eb7-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="41eb7-110">.NET 4.5</span></span>
> - <span data-ttu-id="41eb7-111">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="41eb7-111">SignalR version 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="41eb7-112">Utilisation de Visual Studio 2012 avec ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="41eb7-112">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="41eb7-113">Pour utiliser Visual Studio 2012 dans ce didacticiel, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="41eb7-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="41eb7-114">Mettez à jour votre [Gestionnaire de package](http://docs.nuget.org/docs/start-here/installing-nuget) avec la dernière version.</span><span class="sxs-lookup"><span data-stu-id="41eb7-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="41eb7-115">Installez le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="41eb7-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="41eb7-116">Dans le Web Platform Installer, recherchez et installez **ASP.NET et Web Tools 2013,1 pour Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="41eb7-117">Cela permet d’installer les modèles Visual Studio pour les classes Signalr telles que **Hub**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="41eb7-118">Certains modèles (par exemple, la **classe de démarrage OWIN**) ne sont pas disponibles. pour ceux-ci, utilisez plutôt un fichier de classe.</span><span class="sxs-lookup"><span data-stu-id="41eb7-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="41eb7-119">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="41eb7-119">Questions and comments</span></span>
>
> <span data-ttu-id="41eb7-120">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="41eb7-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="41eb7-121">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="41eb7-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="41eb7-122">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="41eb7-122">Overview</span></span>

<span data-ttu-id="41eb7-123">Un serveur Signalr est généralement hébergé dans une application ASP.NET dans IIS, mais il peut également être auto-hébergé (comme dans une application console ou un service Windows) à l’aide de la bibliothèque auto-hôte.</span><span class="sxs-lookup"><span data-stu-id="41eb7-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="41eb7-124">Cette bibliothèque, comme la totalité de Signalr 2, repose sur OWIN ([Open Web interface pour .net](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="41eb7-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="41eb7-125">OWIN définit une abstraction entre les serveurs Web .NET et les applications Web.</span><span class="sxs-lookup"><span data-stu-id="41eb7-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="41eb7-126">OWIN découple l’application Web du serveur, ce qui fait de OWIN idéal pour l’auto-hébergement d’une application Web dans votre propre processus, en dehors d’IIS.</span><span class="sxs-lookup"><span data-stu-id="41eb7-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="41eb7-127">Les raisons pour lesquelles l’hébergement n’est pas dans IIS sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="41eb7-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="41eb7-128">Environnements où IIS n’est pas disponible ou souhaitable, par exemple une batterie de serveurs existante sans IIS.</span><span class="sxs-lookup"><span data-stu-id="41eb7-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="41eb7-129">La surcharge de performances d’IIS doit être évitée.</span><span class="sxs-lookup"><span data-stu-id="41eb7-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="41eb7-130">La fonctionnalité signalr doit être ajoutée à une application existante qui s’exécute dans un service Windows, un rôle de travail Azure ou tout autre processus.</span><span class="sxs-lookup"><span data-stu-id="41eb7-130">SignalR functionality is to be added to an existing application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="41eb7-131">Si une solution est développée comme auto-hébergée pour des raisons de performances, il est recommandé de tester également l’application hébergée dans IIS pour déterminer l’avantage en matière de performances.</span><span class="sxs-lookup"><span data-stu-id="41eb7-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="41eb7-132">Ce didacticiel contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="41eb7-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="41eb7-133">Création du serveur</span><span class="sxs-lookup"><span data-stu-id="41eb7-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="41eb7-134">Accès au serveur avec un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="41eb7-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="41eb7-135">Création du serveur</span><span class="sxs-lookup"><span data-stu-id="41eb7-135">Creating the server</span></span>

<span data-ttu-id="41eb7-136">Dans ce didacticiel, vous allez créer un serveur hébergé dans une application console, mais le serveur peut être hébergé dans n’importe quel processus, tel qu’un service Windows ou un rôle de travail Azure.</span><span class="sxs-lookup"><span data-stu-id="41eb7-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="41eb7-137">Pour obtenir un exemple de code pour l’hébergement d’un serveur Signalr dans un service Windows, consultez [auto-hébergement signalr dans un service Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="41eb7-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="41eb7-138">Ouvrez Visual Studio 2013 avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="41eb7-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="41eb7-139">Sélectionnez **fichier**, **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="41eb7-140">Sélectionnez **fenêtres** sous le **nœud C# visuel** dans le volet **modèles** , puis sélectionnez le modèle **application console** .</span><span class="sxs-lookup"><span data-stu-id="41eb7-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="41eb7-141">Nommez le nouveau projet « SignalRSelfHost », puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="41eb7-142">Ouvrez la console du gestionnaire de package NuGet en sélectionnant **outils** > **Gestionnaire de package NuGet** > **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-142">Open the NuGet package manager console by selecting **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="41eb7-143">Dans la console du gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="41eb7-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="41eb7-144">Cette commande ajoute les bibliothèques d’auto-hébergement Signalr 2 au projet.</span><span class="sxs-lookup"><span data-stu-id="41eb7-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="41eb7-145">Dans la console du gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="41eb7-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="41eb7-146">Cette commande ajoute la bibliothèque Microsoft. Owin. cors au projet.</span><span class="sxs-lookup"><span data-stu-id="41eb7-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="41eb7-147">Cette bibliothèque sera utilisée pour la prise en charge inter-domaines, qui est requise pour les applications qui hébergent Signalr et un client de page Web dans des domaines différents.</span><span class="sxs-lookup"><span data-stu-id="41eb7-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="41eb7-148">Étant donné que vous hébergerez le serveur Signalr et le client Web sur des ports différents, cela signifie que l’activation de la communication entre domaines doit être activée entre les composants.</span><span class="sxs-lookup"><span data-stu-id="41eb7-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="41eb7-149">Remplacez le contenu de Program.cs par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="41eb7-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="41eb7-150">Le code ci-dessus comprend trois classes :</span><span class="sxs-lookup"><span data-stu-id="41eb7-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="41eb7-151">**, Y**compris la méthode **main** définissant le chemin d’accès principal d’exécution.</span><span class="sxs-lookup"><span data-stu-id="41eb7-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="41eb7-152">Dans cette méthode, une application Web de type **Startup** est démarrée à l’URL spécifiée (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="41eb7-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="41eb7-153">Si la sécurité est requise sur le point de terminaison, vous pouvez implémenter le protocole SSL.</span><span class="sxs-lookup"><span data-stu-id="41eb7-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="41eb7-154">Pour plus d’informations, consultez [procédure : configurer un port avec un certificat SSL](https://msdn.microsoft.com/library/ms733791.aspx) .</span><span class="sxs-lookup"><span data-stu-id="41eb7-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="41eb7-155">**Startup**, la classe contenant la configuration du serveur signalr (la seule configuration utilisée par ce didacticiel est l’appel à `UseCors`) et l’appel à `MapSignalR`, qui crée des itinéraires pour tous les objets Hub du projet.</span><span class="sxs-lookup"><span data-stu-id="41eb7-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="41eb7-156">**Monconcentrateur**, la classe de concentrateur signalr que l’application fournira aux clients.</span><span class="sxs-lookup"><span data-stu-id="41eb7-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="41eb7-157">Cette classe possède une méthode unique, **Envoyer**, que les clients appellera pour diffuser un message à tous les autres clients connectés.</span><span class="sxs-lookup"><span data-stu-id="41eb7-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="41eb7-158">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="41eb7-158">Compile and run the application.</span></span> <span data-ttu-id="41eb7-159">L’adresse que le serveur exécute doit s’afficher dans une fenêtre de console.</span><span class="sxs-lookup"><span data-stu-id="41eb7-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="41eb7-160">Si l’exécution échoue avec l’exception `System.Reflection.TargetInvocationException was unhandled`, vous devrez redémarrer Visual Studio avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="41eb7-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="41eb7-161">Arrêtez l’application avant de passer à la section suivante.</span><span class="sxs-lookup"><span data-stu-id="41eb7-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="41eb7-162">Accès au serveur avec un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="41eb7-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="41eb7-163">Dans cette section, vous allez utiliser le même client JavaScript que le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="41eb7-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="41eb7-164">Nous allons uniquement apporter une seule modification au client, ce qui permet de définir explicitement l’URL du Hub.</span><span class="sxs-lookup"><span data-stu-id="41eb7-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="41eb7-165">Avec une application auto-hébergée, le serveur ne doit pas nécessairement se trouver à la même adresse que l’URL de connexion (en raison des proxies inverses et des équilibreurs de charge), de sorte que l’URL doit être définie explicitement.</span><span class="sxs-lookup"><span data-stu-id="41eb7-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="41eb7-166">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution et sélectionnez **Ajouter**, **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="41eb7-167">Sélectionnez le nœud **Web** , puis sélectionnez le modèle **application Web ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="41eb7-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="41eb7-168">Nommez le projet « JavascriptClient », puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="41eb7-169">Sélectionnez le modèle **vide** et laissez les autres options désélectionnées.</span><span class="sxs-lookup"><span data-stu-id="41eb7-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="41eb7-170">Sélectionnez **créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="41eb7-171">Dans la console du gestionnaire de package, sélectionnez le projet « JavascriptClient » dans la liste déroulante **projet par défaut** , puis exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="41eb7-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="41eb7-172">Cette commande installe les bibliothèques Signalr et JQuery dont vous aurez besoin dans le client.</span><span class="sxs-lookup"><span data-stu-id="41eb7-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="41eb7-173">Cliquez avec le bouton droit sur votre projet et sélectionnez **Ajouter**, **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="41eb7-174">Sélectionnez le nœud **Web** , puis page html.</span><span class="sxs-lookup"><span data-stu-id="41eb7-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="41eb7-175">Nommez la page **default. html**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="41eb7-176">Remplacez le contenu de la nouvelle page HTML par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="41eb7-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="41eb7-177">Vérifiez que les références de script correspondent aux scripts dans le dossier scripts du projet.</span><span class="sxs-lookup"><span data-stu-id="41eb7-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="41eb7-178">Le code suivant (mis en surbrillance dans l’exemple de code ci-dessus) est l’ajout que vous avez apporté au client utilisé dans le didacticiel obtenir des étoiles (en plus de la mise à niveau du code vers Signalr version 2 bêta).</span><span class="sxs-lookup"><span data-stu-id="41eb7-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="41eb7-179">Cette ligne de code définit explicitement l’URL de connexion de base pour Signalr sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="41eb7-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="41eb7-180">Cliquez avec le bouton droit sur la solution, puis sélectionnez **définir les projets de démarrage...** . Sélectionnez la case d’option **plusieurs projets de démarrage** et définissez l' **action** les deux projets sur **Démarrer**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="41eb7-181">Cliquez avec le bouton droit sur « default. html » et sélectionnez **définir comme page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="41eb7-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="41eb7-182">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="41eb7-182">Run the application.</span></span> <span data-ttu-id="41eb7-183">Le serveur et la page seront lancés.</span><span class="sxs-lookup"><span data-stu-id="41eb7-183">The server and page will launch.</span></span> <span data-ttu-id="41eb7-184">Vous devrez peut-être recharger la page Web (ou sélectionner **Continuer** dans le débogueur) si la page se charge avant le démarrage du serveur.</span><span class="sxs-lookup"><span data-stu-id="41eb7-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="41eb7-185">Dans le navigateur, indiquez un nom d’utilisateur lorsque vous y êtes invité.</span><span class="sxs-lookup"><span data-stu-id="41eb7-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="41eb7-186">Copiez l’URL de la page dans un autre onglet ou une autre fenêtre de navigateur et fournissez un nom d’utilisateur différent.</span><span class="sxs-lookup"><span data-stu-id="41eb7-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="41eb7-187">Vous serez en mesure d’envoyer des messages d’un volet de navigateur à l’autre, comme dans le didacticiel Prise en main.</span><span class="sxs-lookup"><span data-stu-id="41eb7-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
