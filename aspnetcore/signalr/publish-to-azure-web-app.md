---
title: Publier une application ASP.NET Core SignalR sur Azure Web App
author: bradygaster
description: Publier une application ASP.NET Core SignalR sur Azure Web App
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 66fa855590c49c4284e4b42cae57f3d4d81dd0fc
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57027426"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="22cff-103">Publier une application ASP.NET Core SignalR sur Azure Web App</span><span class="sxs-lookup"><span data-stu-id="22cff-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="22cff-104">[Azure Web App](/azure/app-service/app-service-web-overview) est une plateforme de services de [Cloud computing de Microsoft](https://azure.microsoft.com/) pour l’hébergement d’applications web, y compris ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="22cff-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="22cff-105">Cet article fait référence à la publication d’une application ASP.NET Core SignalR à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="22cff-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="22cff-106">Visitez [service SignalR pour Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) pour plus d’informations sur l’utilisation de SignalR sur Azure.</span><span class="sxs-lookup"><span data-stu-id="22cff-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="22cff-107">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="22cff-107">Publish the app</span></span>

<span data-ttu-id="22cff-108">Visual Studio fournit des outils intégrés pour la publication sur Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="22cff-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="22cff-109">Un utilisateur de Visual Studio Code peut utiliser les commandes [Azure CLI](/cli/azure) pour publier des applications sur Azure.</span><span class="sxs-lookup"><span data-stu-id="22cff-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="22cff-110">Cet article couvre la publication avec les outils dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="22cff-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="22cff-111">Pour publier une application à l’aide d’Azure CLI, consultez [publier une application ASP.NET Core sur Azure avec les outils de ligne de commande](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="22cff-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="22cff-112">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="22cff-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="22cff-113">Vérifiez que **créer** est archivé le **choisir une cible de publication** boîte de dialogue, puis sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="22cff-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Choix de publication cible](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="22cff-115">Entrez les informations suivantes dans la boîte de dialogue **Créer un App Service** et sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="22cff-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="22cff-116">Élément</span><span class="sxs-lookup"><span data-stu-id="22cff-116">Item</span></span> | <span data-ttu-id="22cff-117">Description</span><span class="sxs-lookup"><span data-stu-id="22cff-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="22cff-118">**Nom de l’application**</span><span class="sxs-lookup"><span data-stu-id="22cff-118">**App name**</span></span> | <span data-ttu-id="22cff-119">Un nom unique de l’application.</span><span class="sxs-lookup"><span data-stu-id="22cff-119">A unique name of the app.</span></span> |
| <span data-ttu-id="22cff-120">**Abonnement**</span><span class="sxs-lookup"><span data-stu-id="22cff-120">**Subscription**</span></span> | <span data-ttu-id="22cff-121">L’abonnement Azure qui utilise l’application.</span><span class="sxs-lookup"><span data-stu-id="22cff-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="22cff-122">**Groupe de ressources**</span><span class="sxs-lookup"><span data-stu-id="22cff-122">**Resource Group**</span></span> | <span data-ttu-id="22cff-123">Le groupe de ressources liées à laquelle appartient l’application.</span><span class="sxs-lookup"><span data-stu-id="22cff-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="22cff-124">**Plan d’hébergement**</span><span class="sxs-lookup"><span data-stu-id="22cff-124">**Hosting Plan**</span></span> | <span data-ttu-id="22cff-125">Le plan de tarification pour l’application web.</span><span class="sxs-lookup"><span data-stu-id="22cff-125">The pricing plan for the web app.</span></span> |

![Créer App Service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="22cff-127">Visual Studio effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="22cff-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="22cff-128">Crée un profil de publication contenant les paramètres de publication.</span><span class="sxs-lookup"><span data-stu-id="22cff-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="22cff-129">Crée ou utilise une *Azure Web App* existante avec les détails fournis.</span><span class="sxs-lookup"><span data-stu-id="22cff-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="22cff-130">Publie l’application.</span><span class="sxs-lookup"><span data-stu-id="22cff-130">Publishes the app.</span></span>
* <span data-ttu-id="22cff-131">Lance un navigateur, avec l’application web publiée chargée.</span><span class="sxs-lookup"><span data-stu-id="22cff-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="22cff-132">Notez que le format de l’URL pour l’application est *{nom de l’application}.azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="22cff-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="22cff-133">Par exemple, une application nommée `SignalRChattR` possède une URL qui ressemble à `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="22cff-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="22cff-134">Si une erreur HTTP 502.2 se produit, consultez [préversion déployer ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/index) pour résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="22cff-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="22cff-135">Configurer l’application web de SignalR</span><span class="sxs-lookup"><span data-stu-id="22cff-135">Configure SignalR web app</span></span>

<span data-ttu-id="22cff-136">Les applications ASP.NET Core SignalR qui sont publiées comme une application Web Azure doit avoir [affinité ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) activé.</span><span class="sxs-lookup"><span data-stu-id="22cff-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="22cff-137">[WebSockets](xref:fundamentals/websockets) doit être activée pour permettre le transport WebSocket de la fonction.</span><span class="sxs-lookup"><span data-stu-id="22cff-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="22cff-138">Dans le portail Azure, accédez à **paramètres de l’application** pour votre application web.</span><span class="sxs-lookup"><span data-stu-id="22cff-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="22cff-139">Définissez **WebSockets** à **sur**et vérifiez **affinité ARR** est **sur**.</span><span class="sxs-lookup"><span data-stu-id="22cff-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Paramètres de l’application Web Azure dans le portail Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="22cff-141">Les WebSockets et autres transports [sont limités selon le Plan App Service](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="22cff-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="22cff-142">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="22cff-142">Related resources</span></span>

* [<span data-ttu-id="22cff-143">Publier une application ASP.NET Core sur Azure avec les outils de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="22cff-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="22cff-144">Publier une application ASP.NET Core sur Azure avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22cff-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="22cff-145">Héberger et déployer des applications ASP.NET Core Preview sur Azure</span><span class="sxs-lookup"><span data-stu-id="22cff-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
