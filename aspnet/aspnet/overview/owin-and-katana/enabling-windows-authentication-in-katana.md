---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Activation de l’authentification Windows dans Katana | Microsoft Docs
author: MikeWasson
description: 'Cet article montre comment activer l’authentification Windows dans Katana. Il aborde deux scénarios : l’utilisation d’IIS pour héberger Katana et l’utilisation de HttpListener pour auto-héberger des Kat...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617179"
---
# <a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="e4117-104">Activation de l’authentification Windows dans Katana</span><span class="sxs-lookup"><span data-stu-id="e4117-104">Enabling Windows Authentication in Katana</span></span>

<span data-ttu-id="e4117-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e4117-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e4117-106">Cet article montre comment activer l’authentification Windows dans Katana.</span><span class="sxs-lookup"><span data-stu-id="e4117-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="e4117-107">Il aborde deux scénarios : l’utilisation d’IIS pour héberger Katana et l’utilisation de HttpListener pour auto-héberger des Katana dans un processus personnalisé.</span><span class="sxs-lookup"><span data-stu-id="e4117-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="e4117-108">Grâce à Barry Dorrans, David Matson et Chris Ross pour l’examen de cet article.</span><span class="sxs-lookup"><span data-stu-id="e4117-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>

<span data-ttu-id="e4117-109">Katana est l’implémentation Microsoft de [OWIN](http://owin.org/), l’interface Web ouverte pour .net.</span><span class="sxs-lookup"><span data-stu-id="e4117-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="e4117-110">Vous pouvez lire une présentation de OWIN et Katana [ici](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="e4117-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="e4117-111">L’architecture OWIN a plusieurs couches :</span><span class="sxs-lookup"><span data-stu-id="e4117-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="e4117-112">Hôte : gère le processus dans lequel le pipeline OWIN s’exécute.</span><span class="sxs-lookup"><span data-stu-id="e4117-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="e4117-113">Serveur : ouvre un socket réseau et écoute les demandes.</span><span class="sxs-lookup"><span data-stu-id="e4117-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="e4117-114">Intergiciel (middleware) : traite la requête et la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="e4117-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="e4117-115">Katana fournit actuellement deux serveurs, qui prennent tous deux en charge l’authentification intégrée de Windows :</span><span class="sxs-lookup"><span data-stu-id="e4117-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="e4117-116">**Microsoft. Owin. Host. SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="e4117-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="e4117-117">Utilise IIS avec le pipeline ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e4117-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="e4117-118">**Microsoft. Owin. Host. HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="e4117-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="e4117-119">Utilise [System .net. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4117-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="e4117-120">Ce serveur est actuellement l’option par défaut lors de l’auto-hébergement de Katana.</span><span class="sxs-lookup"><span data-stu-id="e4117-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="e4117-121">Katana ne fournit pas actuellement de OWIN middleware pour l’authentification Windows, car cette fonctionnalité est déjà disponible sur les serveurs.</span><span class="sxs-lookup"><span data-stu-id="e4117-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>

## <a name="windows-authentication-in-iis"></a><span data-ttu-id="e4117-122">Authentification Windows dans IIS</span><span class="sxs-lookup"><span data-stu-id="e4117-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="e4117-123">À l’aide de Microsoft. Owin. Host. SystemWeb, vous pouvez simplement activer l’authentification Windows dans IIS.</span><span class="sxs-lookup"><span data-stu-id="e4117-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="e4117-124">Commençons par créer une nouvelle application ASP.NET à l’aide du modèle de projet « application Web vide ASP.NET ».</span><span class="sxs-lookup"><span data-stu-id="e4117-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="e4117-125">Ensuite, ajoutez des packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="e4117-125">Next, add NuGet packages.</span></span> <span data-ttu-id="e4117-126">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="e4117-126">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e4117-127">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e4117-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="e4117-128">À présent, ajoutez une classe nommée `Startup` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4117-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="e4117-129">C’est tout ce dont vous avez besoin pour créer une application « Hello World » pour OWIN, en cours d’exécution sur IIS.</span><span class="sxs-lookup"><span data-stu-id="e4117-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="e4117-130">Appuyez sur F5 pour déboguer l'application.</span><span class="sxs-lookup"><span data-stu-id="e4117-130">Press F5 to debug the application.</span></span> <span data-ttu-id="e4117-131">« Hello World! » doit</span><span class="sxs-lookup"><span data-stu-id="e4117-131">You should see "Hello World!"</span></span> <span data-ttu-id="e4117-132">dans la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e4117-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="e4117-133">Nous allons ensuite activer l’authentification Windows dans IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e4117-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="e4117-134">Dans le menu **affichage** , sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="e4117-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="e4117-135">Cliquez sur le nom du projet dans Explorateur de solutions pour afficher les propriétés du projet.</span><span class="sxs-lookup"><span data-stu-id="e4117-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="e4117-136">Dans la fenêtre **Propriétés** , affectez à **authentification anonyme** la valeur **désactivé** et à **authentification Windows** la valeur **activé**.</span><span class="sxs-lookup"><span data-stu-id="e4117-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="e4117-137">Quand vous exécutez l’application à partir de Visual Studio, IIS Express nécessitera les informations d’identification Windows de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e4117-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="e4117-138">Vous pouvez le voir à l’aide de [Fiddler](http://fiddler2.com/home) ou d’un autre outil de débogage http.</span><span class="sxs-lookup"><span data-stu-id="e4117-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="e4117-139">Voici un exemple de réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="e4117-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="e4117-140">Les en-têtes WWW-Authenticate dans cette réponse indiquent que le serveur prend en charge le protocole [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , qui utilise Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="e4117-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="e4117-141">Plus tard, lorsque vous déploierez l’application sur un serveur, procédez comme [suit](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) pour activer l’authentification Windows dans IIS sur ce serveur.</span><span class="sxs-lookup"><span data-stu-id="e4117-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="e4117-142">Authentification Windows dans HttpListener</span><span class="sxs-lookup"><span data-stu-id="e4117-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="e4117-143">Si vous utilisez Microsoft. Owin. Host. HttpListener pour l’auto-hébergement des Katana, vous pouvez activer l’authentification Windows directement sur l’instance **HttpListener** .</span><span class="sxs-lookup"><span data-stu-id="e4117-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="e4117-144">Tout d’abord, créez une application console.</span><span class="sxs-lookup"><span data-stu-id="e4117-144">First, create a new console application.</span></span> <span data-ttu-id="e4117-145">Ensuite, ajoutez des packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="e4117-145">Next, add NuGet packages.</span></span> <span data-ttu-id="e4117-146">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="e4117-146">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e4117-147">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e4117-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="e4117-148">À présent, ajoutez une classe nommée `Startup` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4117-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="e4117-149">Cette classe implémente le même exemple « Hello World » que précédemment, mais définit également l’authentification Windows comme schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="e4117-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="e4117-150">À l’intérieur de la fonction `Main`, démarrez le pipeline OWIN :</span><span class="sxs-lookup"><span data-stu-id="e4117-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="e4117-151">Vous pouvez envoyer une demande dans Fiddler pour confirmer que l’application utilise l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="e4117-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="e4117-152">Rubriques connexes</span><span class="sxs-lookup"><span data-stu-id="e4117-152">Related Topics</span></span>

[<span data-ttu-id="e4117-153">Vue d’ensemble du projet Katana</span><span class="sxs-lookup"><span data-stu-id="e4117-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="e4117-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="e4117-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="e4117-155">Fonctionnement de l’authentification par formulaire OWIN dans MVC 5</span><span class="sxs-lookup"><span data-stu-id="e4117-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
