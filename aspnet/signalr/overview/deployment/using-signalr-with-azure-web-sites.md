---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Utilisation de Signalr avec Web Apps dans Azure App Service | Microsoft Docs
author: bradygaster
description: Ce document explique comment configurer une application Signalr qui s’exécute sur Microsoft Azure. Versions logicielles utilisées dans le didacticiel Visual Studio 2013 ou...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558701"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="6aa08-104">Utilisation de SignalR avec Web Apps dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6aa08-104">Using SignalR with Web Apps in Azure App Service</span></span>

<span data-ttu-id="6aa08-105">de [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="6aa08-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="6aa08-106">Ce document explique comment configurer une application Signalr qui s’exécute sur Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="6aa08-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6aa08-107">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="6aa08-107">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="6aa08-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) ou Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6aa08-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) or Visual Studio 2012</span></span>
> - <span data-ttu-id="6aa08-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6aa08-109">.NET 4.5</span></span>
> - <span data-ttu-id="6aa08-110">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="6aa08-110">SignalR version 2</span></span>
> - <span data-ttu-id="6aa08-111">Kit de développement logiciel (SDK) Azure 2,3 pour Visual Studio 2013 ou 2012</span><span class="sxs-lookup"><span data-stu-id="6aa08-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="6aa08-112">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="6aa08-112">Questions and comments</span></span>
>
> <span data-ttu-id="6aa08-113">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="6aa08-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6aa08-114">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), ou sur les [forums Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="6aa08-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="6aa08-115">Sommaire</span><span class="sxs-lookup"><span data-stu-id="6aa08-115">Table of Contents</span></span>

- [<span data-ttu-id="6aa08-116">Introduction</span><span class="sxs-lookup"><span data-stu-id="6aa08-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="6aa08-117">Déploiement d’une application Web Signalr sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6aa08-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="6aa08-118">Activation de WebSockets sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6aa08-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="6aa08-119">Utilisation du fond de panier du cache Redims Azure</span><span class="sxs-lookup"><span data-stu-id="6aa08-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="6aa08-120">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="6aa08-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="6aa08-121">Introduction</span><span class="sxs-lookup"><span data-stu-id="6aa08-121">Introduction</span></span>

<span data-ttu-id="6aa08-122">ASP.NET Signalr peut être utilisé pour mettre en place un nouveau niveau d’interactivité entre les serveurs et les clients Web ou .NET.</span><span class="sxs-lookup"><span data-stu-id="6aa08-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="6aa08-123">Lorsqu’ils sont hébergés dans Azure, les applications Signalr peuvent tirer parti de l’environnement hautement disponible, évolutif et performant qui s’exécute dans le Cloud.</span><span class="sxs-lookup"><span data-stu-id="6aa08-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="6aa08-124">Déploiement d’une application Web Signalr sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6aa08-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="6aa08-125">Signalr n’ajoute pas de complications particulières au déploiement d’une application sur Azure par rapport au déploiement sur un serveur local.</span><span class="sxs-lookup"><span data-stu-id="6aa08-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="6aa08-126">Une application qui utilise Signalr peut être hébergée dans Azure sans aucune modification de configuration ou d’autres paramètres (Toutefois, pour la prise en charge de WebSockets, consultez [activation de WebSockets sur Azure App service](#websocket) ci-dessous). Pour ce didacticiel, vous allez déployer l’application créée dans le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md) sur Azure.</span><span class="sxs-lookup"><span data-stu-id="6aa08-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="6aa08-127">**Conditions préalables**</span><span class="sxs-lookup"><span data-stu-id="6aa08-127">**Prerequisites**</span></span>

- <span data-ttu-id="6aa08-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="6aa08-128">Visual Studio 2013.</span></span> <span data-ttu-id="6aa08-129">Si vous n’avez pas Visual Studio, Visual Studio 2013 Express pour le Web est inclus dans l’installation du kit de développement logiciel (SDK) Azure.</span><span class="sxs-lookup"><span data-stu-id="6aa08-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="6aa08-130">[Azure sdk 2,3 pour Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) ou le [Kit de développement logiciel (sdk) Azure 2,3 pour Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="6aa08-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="6aa08-131">Pour suivre le didacticiel, vous devez disposer d'un abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="6aa08-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="6aa08-132">Vous pouvez [activer les avantages de votre abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)ou vous [inscrire pour bénéficier d’un abonnement d’évaluation](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6aa08-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="6aa08-133">Déploiement d’une application Web Signalr sur Azure</span><span class="sxs-lookup"><span data-stu-id="6aa08-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="6aa08-134">Suivez le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md)ou téléchargez le projet terminé à partir de la [Galerie de code](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="6aa08-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="6aa08-135">Dans Visual Studio, sélectionnez **générer**, **publier signalr chat**.</span><span class="sxs-lookup"><span data-stu-id="6aa08-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="6aa08-136">Dans la boîte de dialogue publier le site Web, sélectionnez « sites Web Windows Azure ».</span><span class="sxs-lookup"><span data-stu-id="6aa08-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Sélectionner les sites Web Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="6aa08-138">Si vous n’êtes pas connecté à votre compte Microsoft, cliquez sur **se connecter...** dans la boîte de dialogue « Sélectionner un site Web existant », puis connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="6aa08-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Sélectionner un site Web existant](using-signalr-with-azure-web-sites/_static/image2.png)    ![Se connecter à Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="6aa08-141">Dans la boîte de dialogue « Sélectionner un site Web existant », cliquez sur **nouveau**.</span><span class="sxs-lookup"><span data-stu-id="6aa08-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nouveau site Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="6aa08-143">Dans la boîte de dialogue « créer un site sur Windows Azure », entrez un nom d’application unique.</span><span class="sxs-lookup"><span data-stu-id="6aa08-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="6aa08-144">Sélectionnez la région la plus proche de vous dans la liste déroulante région.</span><span class="sxs-lookup"><span data-stu-id="6aa08-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="6aa08-145">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6aa08-145">Click **Create**.</span></span>

    ![Créer un site sur Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="6aa08-147">Dans la boîte de dialogue publier le site Web, cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="6aa08-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Publier le site](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="6aa08-149">Une fois que l’application a terminé la publication, l’application Signalr chat hébergée dans Azure App Service Web Apps s’ouvre dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="6aa08-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Ouverture de site dans un navigateur](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="6aa08-151">Activation de WebSockets sur Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="6aa08-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="6aa08-152">Les WebSockets doivent être explicitement activés dans votre application Web pour être utilisés dans une application Signalr. dans le cas contraire, d’autres protocoles seront utilisés (consultez [transports et secours](../getting-started/introduction-to-signalr.md#transports) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="6aa08-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="6aa08-153">Pour utiliser WebSockets sur Azure App Service Web Apps, activez-le dans la section de configuration de l’application Web.</span><span class="sxs-lookup"><span data-stu-id="6aa08-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="6aa08-154">Pour ce faire, ouvrez votre application Web dans le [portail de gestion Azure](https://manage.windowsazure.com/), puis sélectionnez configurer.</span><span class="sxs-lookup"><span data-stu-id="6aa08-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Onglet Configurer](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="6aa08-156">En haut de la page de configuration, assurez-vous que .NET 4,5 est utilisé pour votre application Web.</span><span class="sxs-lookup"><span data-stu-id="6aa08-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Paramètre .NET Framework version 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="6aa08-158">Sur la page Configuration, dans le paramètre **WebSockets** , sélectionnez **activé**.</span><span class="sxs-lookup"><span data-stu-id="6aa08-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Paramètre WebSocket : activé](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="6aa08-160">En bas de la page Configuration, sélectionnez **Enregistrer** pour enregistrer vos modifications.</span><span class="sxs-lookup"><span data-stu-id="6aa08-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Enregistrer les paramètres](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="6aa08-162">Utilisation du fond de panier du cache Redims Azure</span><span class="sxs-lookup"><span data-stu-id="6aa08-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="6aa08-163">Si vous utilisez plusieurs instances pour votre application Web et que les utilisateurs de ces instances doivent interagir les uns avec les autres (par exemple, les messages de conversation créés dans une instance peuvent atteindre les utilisateurs connectés à d’autres instances), le [fond de panier du cache](../performance/scaleout-with-redis.md) d’inversion Azure doit être implémenté dans votre application.</span><span class="sxs-lookup"><span data-stu-id="6aa08-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="6aa08-164">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="6aa08-164">Next Steps</span></span>

<span data-ttu-id="6aa08-165">Pour plus d’informations sur les Web Apps dans Azure App Service, consultez [Web Apps Overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="6aa08-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
