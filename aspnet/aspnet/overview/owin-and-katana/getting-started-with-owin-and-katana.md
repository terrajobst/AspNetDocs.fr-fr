---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Prise en main avec OWIN et Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584671"
---
# <a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="f3064-102">Bien démarrer avec OWIN et Katana</span><span class="sxs-lookup"><span data-stu-id="f3064-102">Getting Started with OWIN and Katana</span></span>

<span data-ttu-id="f3064-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f3064-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f3064-104">[Open Web interface for .net (OWIN)](http://owin.org/) définit une abstraction entre les serveurs Web .net et les applications Web.</span><span class="sxs-lookup"><span data-stu-id="f3064-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="f3064-105">En découplant le serveur Web de l’application, OWIN facilite la création d’un intergiciel (middleware) pour le développement Web .NET.</span><span class="sxs-lookup"><span data-stu-id="f3064-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="f3064-106">De plus, OWIN facilite le portage des applications Web vers d’autres&#8212;hôtes, par exemple l’auto-hébergement dans un service Windows ou tout autre processus.</span><span class="sxs-lookup"><span data-stu-id="f3064-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="f3064-107">OWIN est une spécification appartenant à la Communauté, et non une implémentation.</span><span class="sxs-lookup"><span data-stu-id="f3064-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="f3064-108">Le projet Katana est un ensemble de composants OWIN Open source développés par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f3064-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="f3064-109">Pour obtenir une vue d’ensemble générale des OWIN et Katana, consultez [vue d’ensemble du projet Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="f3064-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="f3064-110">Dans cet article, je vais passer directement au code pour commencer.</span><span class="sxs-lookup"><span data-stu-id="f3064-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="f3064-111">Ce didacticiel utilise [Visual Studio 2013 version Release candidate](https://go.microsoft.com/fwlink/?LinkId=306566), mais vous pouvez également utiliser Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f3064-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="f3064-112">Certaines étapes sont différentes dans Visual Studio 2012, comme je l’ai indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f3064-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="f3064-113">Héberger OWIN dans IIS</span><span class="sxs-lookup"><span data-stu-id="f3064-113">Host OWIN in IIS</span></span>

<span data-ttu-id="f3064-114">Dans cette section, nous allons héberger OWIN dans IIS.</span><span class="sxs-lookup"><span data-stu-id="f3064-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="f3064-115">Cette option vous offre la flexibilité et la composition d’un pipeline OWIN avec l’ensemble de fonctionnalités matures d’IIS.</span><span class="sxs-lookup"><span data-stu-id="f3064-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="f3064-116">À l’aide de cette option, l’application OWIN s’exécute dans le pipeline de demande ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f3064-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="f3064-117">Tout d’abord, créez un projet d’application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f3064-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="f3064-118">(Dans Visual Studio 2012, utilisez le type de projet d’application Web vide ASP.NET.)</span><span class="sxs-lookup"><span data-stu-id="f3064-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="f3064-119">Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **vide** .</span><span class="sxs-lookup"><span data-stu-id="f3064-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="f3064-120">Ajouter des paquets NuGet</span><span class="sxs-lookup"><span data-stu-id="f3064-120">Add NuGet Packages</span></span>

<span data-ttu-id="f3064-121">Ensuite, ajoutez les packages NuGet requis.</span><span class="sxs-lookup"><span data-stu-id="f3064-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="f3064-122">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="f3064-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f3064-123">Dans la fenêtre de la console du gestionnaire de package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f3064-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="f3064-124">Ajouter une classe de démarrage</span><span class="sxs-lookup"><span data-stu-id="f3064-124">Add a Startup Class</span></span>

<span data-ttu-id="f3064-125">Ensuite, ajoutez une classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="f3064-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="f3064-126">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter**, puis sélectionnez **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="f3064-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="f3064-127">Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **Owin Startup Class**.</span><span class="sxs-lookup"><span data-stu-id="f3064-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="f3064-128">Pour plus d’informations sur la configuration de la classe de démarrage, consultez détection de la [classe de démarrage OWIN](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="f3064-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="f3064-129">Ajoutez le code suivant à la méthode `Startup1.Configuration` :</span><span class="sxs-lookup"><span data-stu-id="f3064-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="f3064-130">Ce code ajoute un simple élément d’intergiciel au pipeline OWIN, implémenté comme une fonction qui reçoit une instance **Microsoft. OWIN. IOwinContext** .</span><span class="sxs-lookup"><span data-stu-id="f3064-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="f3064-131">Lorsque le serveur reçoit une requête HTTP, le pipeline OWIN appelle l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="f3064-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="f3064-132">L’intergiciel définit le type de contenu de la réponse et écrit le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="f3064-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="f3064-133">Le modèle de classe de démarrage OWIN est disponible dans Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="f3064-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="f3064-134">Si vous utilisez Visual Studio 2012, ajoutez simplement une nouvelle classe vide nommée `Startup1`, puis collez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f3064-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="f3064-135">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="f3064-135">Run the Application</span></span>

<span data-ttu-id="f3064-136">Appuyez sur F5 pour commencer le débogage.</span><span class="sxs-lookup"><span data-stu-id="f3064-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="f3064-137">Visual Studio ouvre une fenêtre de navigateur pour `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="f3064-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="f3064-138">La page doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f3064-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="f3064-139">OWIN de l’auto-hébergement dans une application console</span><span class="sxs-lookup"><span data-stu-id="f3064-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="f3064-140">Il est facile de convertir cette application de l’hébergement IIS vers l’auto-hébergement dans un processus personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f3064-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="f3064-141">Avec l’hébergement IIS, IIS agit à la fois comme serveur HTTP et comme processus qui héberge le service.</span><span class="sxs-lookup"><span data-stu-id="f3064-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="f3064-142">Avec l’auto-hébergement, votre application crée le processus et utilise la classe **HttpListener** comme serveur http.</span><span class="sxs-lookup"><span data-stu-id="f3064-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="f3064-143">Dans Visual Studio, créez une application console.</span><span class="sxs-lookup"><span data-stu-id="f3064-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="f3064-144">Dans la fenêtre de la console du gestionnaire de package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f3064-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="f3064-145">Ajoutez une classe `Startup1` de la partie 1 de ce didacticiel au projet.</span><span class="sxs-lookup"><span data-stu-id="f3064-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="f3064-146">Vous n’avez pas besoin de modifier cette classe.</span><span class="sxs-lookup"><span data-stu-id="f3064-146">You don't need to modify this class.</span></span>

<span data-ttu-id="f3064-147">Implémentez la méthode `Main` de l’application comme suit.</span><span class="sxs-lookup"><span data-stu-id="f3064-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="f3064-148">Lorsque vous exécutez l’application console, le serveur commence à écouter `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="f3064-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="f3064-149">Si vous accédez à cette adresse dans un navigateur Web, la page « Hello World » s’affiche.</span><span class="sxs-lookup"><span data-stu-id="f3064-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="f3064-150">Ajouter OWIN Diagnostics</span><span class="sxs-lookup"><span data-stu-id="f3064-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="f3064-151">Le package Microsoft. Owin. Diagnostics contient un intergiciel (middleware) qui intercepte les exceptions non gérées et affiche une page HTML avec les détails de l’erreur.</span><span class="sxs-lookup"><span data-stu-id="f3064-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="f3064-152">Cette page fonctionne de façon très similaire à la page d’erreur ASP.NET, parfois appelée «[écran jaune de décès](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)» (YSOD).</span><span class="sxs-lookup"><span data-stu-id="f3064-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="f3064-153">Comme YSOD, la page d’erreur Katana est utile lors du développement, mais il est conseillé de la désactiver en mode production.</span><span class="sxs-lookup"><span data-stu-id="f3064-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="f3064-154">Pour installer le package de diagnostics dans votre projet, tapez la commande suivante dans la fenêtre console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="f3064-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="f3064-155">Modifiez le code de votre méthode `Startup1.Configuration` comme suit :</span><span class="sxs-lookup"><span data-stu-id="f3064-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="f3064-156">Utilisez maintenant CTRL + F5 pour exécuter l’application sans débogage, de sorte que Visual Studio ne s’arrête pas sur l’exception.</span><span class="sxs-lookup"><span data-stu-id="f3064-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="f3064-157">L’application se comporte de la même manière qu’auparavant, jusqu’à ce que vous naviguiez vers `http://localhost/fail`, à ce stade, l’application lève l’exception.</span><span class="sxs-lookup"><span data-stu-id="f3064-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="f3064-158">L’intergiciel (middleware) de la page d’erreurs intercepte l’exception et affiche une page HTML contenant des informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="f3064-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="f3064-159">Vous pouvez cliquer sur les onglets pour afficher la pile, la chaîne de requête, les cookies, l’en-tête de demande et les variables d’environnement OWIN.</span><span class="sxs-lookup"><span data-stu-id="f3064-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="f3064-160">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="f3064-160">Next Steps</span></span>

- [<span data-ttu-id="f3064-161">Détection de classe de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="f3064-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="f3064-162">Utiliser OWIN pour l’auto-hébergement API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f3064-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="f3064-163">Utiliser OWIN pour l’auto-hébergement Signalr</span><span class="sxs-lookup"><span data-stu-id="f3064-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
