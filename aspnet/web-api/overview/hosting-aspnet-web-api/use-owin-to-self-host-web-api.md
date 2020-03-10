---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Utiliser OWIN pour l’auto-hébergement API Web ASP.NET-ASP.NET 4. x
author: rick-anderson
description: Didacticiel contenant du code illustrant comment héberger des API Web ASP.NET dans une application console.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556538"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="dd223-103">Utiliser OWIN pour l’auto-hébergement API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dd223-103">Use OWIN to Self-Host ASP.NET Web API</span></span> 

> <span data-ttu-id="dd223-104">Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide de OWIN pour auto-héberger l’infrastructure de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="dd223-104">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="dd223-105">[Open Web interface for .net](http://owin.org) (OWIN) définit une abstraction entre les serveurs Web .net et les applications Web.</span><span class="sxs-lookup"><span data-stu-id="dd223-105">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="dd223-106">OWIN découple l’application Web du serveur, ce qui fait de OWIN idéal pour l’auto-hébergement d’une application Web dans votre propre processus, en dehors d’IIS.</span><span class="sxs-lookup"><span data-stu-id="dd223-106">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="dd223-107">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="dd223-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="dd223-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="dd223-108">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="dd223-109">API Web 5.2.7</span><span class="sxs-lookup"><span data-stu-id="dd223-109">Web API 5.2.7</span></span>

> [!NOTE]
> <span data-ttu-id="dd223-110">Vous trouverez le code source complet de ce didacticiel sur [github.com/ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span><span class="sxs-lookup"><span data-stu-id="dd223-110">You can find the complete source code for this tutorial at [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="dd223-111">Créer une application console</span><span class="sxs-lookup"><span data-stu-id="dd223-111">Create a console application</span></span>

<span data-ttu-id="dd223-112">Dans le menu **fichier** , cliquez sur **nouveau**, puis sélectionnez **projet**.</span><span class="sxs-lookup"><span data-stu-id="dd223-112">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="dd223-113">Dans **installé**, sous **visuel C#** , sélectionnez **Bureau Windows** , puis sélectionnez **application console (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="dd223-113">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="dd223-114">Nommez le projet « OwinSelfhostSample » et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="dd223-114">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="dd223-115">Ajouter l’API Web et les packages OWIN</span><span class="sxs-lookup"><span data-stu-id="dd223-115">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="dd223-116">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="dd223-116">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="dd223-117">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="dd223-117">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="dd223-118">Cette opération installe le package WebAPI OWIN selfHost et tous les packages OWIN requis.</span><span class="sxs-lookup"><span data-stu-id="dd223-118">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="dd223-119">Configurer l’API Web pour l’auto-hébergement</span><span class="sxs-lookup"><span data-stu-id="dd223-119">Configure Web API for self-host</span></span>

<span data-ttu-id="dd223-120">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="dd223-120">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="dd223-121">Nommez la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="dd223-121">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="dd223-122">Remplacez tout le code réutilisable dans ce fichier par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="dd223-122">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="dd223-123">Ajouter un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="dd223-123">Add a Web API controller</span></span>

<span data-ttu-id="dd223-124">Ensuite, ajoutez une classe de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="dd223-124">Next, add a Web API controller class.</span></span> <span data-ttu-id="dd223-125">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="dd223-125">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="dd223-126">Nommez la classe `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="dd223-126">Name the class `ValuesController`.</span></span>

<span data-ttu-id="dd223-127">Remplacez tout le code réutilisable dans ce fichier par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="dd223-127">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="dd223-128">Démarrer l’hôte OWIN et effectuer une demande avec HttpClient</span><span class="sxs-lookup"><span data-stu-id="dd223-128">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="dd223-129">Remplacez tout le code réutilisable dans le fichier Program.cs par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dd223-129">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="dd223-130">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="dd223-130">Run the application</span></span>

<span data-ttu-id="dd223-131">Pour exécuter l’application, appuyez sur F5 dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd223-131">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="dd223-132">La sortie doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="dd223-132">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="dd223-133">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dd223-133">Additional resources</span></span>

[<span data-ttu-id="dd223-134">Vue d’ensemble du projet Katana</span><span class="sxs-lookup"><span data-stu-id="dd223-134">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="dd223-135">API Web ASP.NET d’hôte dans un rôle de travail Azure</span><span class="sxs-lookup"><span data-stu-id="dd223-135">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
