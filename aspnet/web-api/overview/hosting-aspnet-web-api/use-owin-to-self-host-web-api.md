---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Utiliser OWIN pour auto-héberger API Web ASP.NET - ASP.NET 4.x
author: rick-anderson
description: Didacticiel de code montrant comment héberger des API Web ASP.NET dans une application console.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: a67db0bd061846af2db3599e0843ed7c6a22db1e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59386513"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="556b2-103">Utiliser OWIN pour auto-héberger l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="556b2-103">Use OWIN to Self-Host ASP.NET Web API</span></span> 


> <span data-ttu-id="556b2-104">Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide d’OWIN pour auto-héberger l’infrastructure API Web.</span><span class="sxs-lookup"><span data-stu-id="556b2-104">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="556b2-105">[Open Web Interface pour .NET](http://owin.org) (OWIN) définit une abstraction entre les serveurs web de .NET et des applications web.</span><span class="sxs-lookup"><span data-stu-id="556b2-105">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="556b2-106">OWIN dissocie de l’application web à partir du serveur, ce qui rend OWIN idéale pour l’hébergement automatique d’une application web dans votre propre processus, en dehors d’IIS.</span><span class="sxs-lookup"><span data-stu-id="556b2-106">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="556b2-107">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="556b2-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="556b2-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="556b2-108">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="556b2-109">API 5.2.7 Web</span><span class="sxs-lookup"><span data-stu-id="556b2-109">Web API 5.2.7</span></span>


> [!NOTE]
> <span data-ttu-id="556b2-110">Vous trouverez le code source complet pour ce didacticiel à [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span><span class="sxs-lookup"><span data-stu-id="556b2-110">You can find the complete source code for this tutorial at [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="556b2-111">Créer une application console</span><span class="sxs-lookup"><span data-stu-id="556b2-111">Create a console application</span></span>

<span data-ttu-id="556b2-112">Sur le **fichier** menu, **New**, puis sélectionnez **projet**.</span><span class="sxs-lookup"><span data-stu-id="556b2-112">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="556b2-113">À partir de **installé**, sous **Visual C#** , sélectionnez **Windows Desktop** , puis sélectionnez **application Console (.Net Framework)**.</span><span class="sxs-lookup"><span data-stu-id="556b2-113">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="556b2-114">Nommez le projet « OwinSelfhostSample » et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="556b2-114">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="556b2-115">Ajoutez les packages de l’API Web et la bibliothèque OWIN</span><span class="sxs-lookup"><span data-stu-id="556b2-115">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="556b2-116">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="556b2-116">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="556b2-117">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="556b2-117">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="556b2-118">Cela installera le package de selfhost WebAPI OWIN et tous les packages OWIN requis.</span><span class="sxs-lookup"><span data-stu-id="556b2-118">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="556b2-119">Configurer les API Web pour Self-host</span><span class="sxs-lookup"><span data-stu-id="556b2-119">Configure Web API for self-host</span></span>

<span data-ttu-id="556b2-120">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="556b2-120">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="556b2-121">Nommez la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="556b2-121">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="556b2-122">Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="556b2-122">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="556b2-123">Ajouter un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="556b2-123">Add a Web API controller</span></span>

<span data-ttu-id="556b2-124">Ensuite, ajoutez une classe de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="556b2-124">Next, add a Web API controller class.</span></span> <span data-ttu-id="556b2-125">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="556b2-125">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="556b2-126">Nommez la classe `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="556b2-126">Name the class `ValuesController`.</span></span>

<span data-ttu-id="556b2-127">Remplacez tout le code réutilisable dans ce fichier avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="556b2-127">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="556b2-128">Démarrer l’hôte OWIN et faire une demande avec HttpClient</span><span class="sxs-lookup"><span data-stu-id="556b2-128">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="556b2-129">Remplacez tout le code réutilisable dans le fichier Program.cs par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="556b2-129">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="556b2-130">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="556b2-130">Run the application</span></span>

<span data-ttu-id="556b2-131">Pour exécuter l’application, appuyez sur F5 dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="556b2-131">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="556b2-132">La sortie doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="556b2-132">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="556b2-133">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="556b2-133">Additional resources</span></span>

[<span data-ttu-id="556b2-134">Vue d’ensemble du projet Katana</span><span class="sxs-lookup"><span data-stu-id="556b2-134">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="556b2-135">Héberger des API Web ASP.NET dans un rôle Worker Azure</span><span class="sxs-lookup"><span data-stu-id="556b2-135">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
