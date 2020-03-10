---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Signalr ScaleOut avec Azure Service Bus | Microsoft Docs
author: bradygaster
description: Versions logicielles utilisées dans cette rubrique Visual Studio 2013 .NET 4,5 Signalr version 2 versions précédentes de cette rubrique pour la version Signalr 1. x de cette rubrique,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579176"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="17226-103">Scale-out de SignalR avec Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="17226-103">SignalR Scaleout with Azure Service Bus</span></span>

<span data-ttu-id="17226-104">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="17226-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="17226-105">Dans ce didacticiel, vous allez déployer une application Signalr sur un rôle Web Windows Azure, à l’aide de la Service Bus fond de panier pour distribuer des messages à chaque instance de rôle.</span><span class="sxs-lookup"><span data-stu-id="17226-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="17226-106">(Vous pouvez également utiliser la Service Bus fond de panier avec les [applications Web dans Azure App service](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="17226-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="17226-107">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="17226-107">Prerequisites:</span></span>

- <span data-ttu-id="17226-108">Un compte Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="17226-108">A Windows Azure account.</span></span>
- <span data-ttu-id="17226-109">Le [Kit de développement logiciel (SDK) Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="17226-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="17226-110">Visual Studio 2012 ou 2013.</span><span class="sxs-lookup"><span data-stu-id="17226-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="17226-111">Le fond de panier service bus est également compatible avec [service bus pour Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1,1.</span><span class="sxs-lookup"><span data-stu-id="17226-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="17226-112">Toutefois, il n’est pas compatible avec la version 1,0 de Service Bus pour Windows Server.</span><span class="sxs-lookup"><span data-stu-id="17226-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="17226-113">Prix</span><span class="sxs-lookup"><span data-stu-id="17226-113">Pricing</span></span>

<span data-ttu-id="17226-114">Le fond de panier Service Bus utilise les rubriques pour envoyer des messages.</span><span class="sxs-lookup"><span data-stu-id="17226-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="17226-115">Pour obtenir les dernières informations sur la tarification, consultez [service bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="17226-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="17226-116">Au moment de la rédaction de cet article, vous pouvez envoyer 1 million messages par mois pour moins de $1.</span><span class="sxs-lookup"><span data-stu-id="17226-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="17226-117">Le fond de panier envoie un message service bus pour chaque appel d’une méthode de concentrateur Signalr.</span><span class="sxs-lookup"><span data-stu-id="17226-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="17226-118">Il existe également des messages de contrôle pour les connexions, les déconnexions, la jonction ou la sortie de groupes, etc.</span><span class="sxs-lookup"><span data-stu-id="17226-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="17226-119">Dans la plupart des applications, la majorité du trafic de messages est l’appel de méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="17226-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="17226-120">Présentation</span><span class="sxs-lookup"><span data-stu-id="17226-120">Overview</span></span>

<span data-ttu-id="17226-121">Avant d’accéder au didacticiel détaillé, voici un aperçu rapide de ce que vous allez faire.</span><span class="sxs-lookup"><span data-stu-id="17226-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="17226-122">Utilisez le Portail Azure Windows pour créer un espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="17226-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="17226-123">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="17226-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="17226-124">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="17226-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="17226-125">[Microsoft. Aspnet. signalr. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) ou [Microsoft. Aspnet. Signalr. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="17226-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="17226-126">Créer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="17226-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="17226-127">Ajoutez le code suivant à Startup.cs pour configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="17226-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="17226-128">Ce code configure le fond de panier avec les valeurs par défaut pour [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) et [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="17226-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="17226-129">Pour plus d’informations sur la modification de ces valeurs, consultez [performance de signalr : mesures ScaleOut](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="17226-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="17226-130">Pour chaque application, choisissez une autre valeur pour « nomapp ».</span><span class="sxs-lookup"><span data-stu-id="17226-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="17226-131">N’utilisez pas la même valeur pour plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="17226-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="17226-132">Créer les services Azure</span><span class="sxs-lookup"><span data-stu-id="17226-132">Create the Azure Services</span></span>

<span data-ttu-id="17226-133">Créez un service Cloud, comme décrit dans [comment créer et déployer un service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="17226-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="17226-134">Suivez les étapes de la section « Procédure : créer un service Cloud à l’aide de la création rapide ».</span><span class="sxs-lookup"><span data-stu-id="17226-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="17226-135">Pour ce didacticiel, vous n’avez pas besoin de télécharger un certificat.</span><span class="sxs-lookup"><span data-stu-id="17226-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="17226-136">Créez un espace de noms Service Bus, comme décrit dans [How to Use service bus topics/subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="17226-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="17226-137">Suivez les étapes de la section « créer un espace de noms de service ».</span><span class="sxs-lookup"><span data-stu-id="17226-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="17226-138">Veillez à sélectionner la même région pour le service Cloud et l’espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="17226-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="17226-139">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="17226-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="17226-140">Démarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17226-140">Start Visual Studio.</span></span> <span data-ttu-id="17226-141">Dans le menu **Fichier**, cliquez sur **Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="17226-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="17226-142">Dans la boîte de dialogue **nouveau projet** , développez **visuel C#** .</span><span class="sxs-lookup"><span data-stu-id="17226-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="17226-143">Sous **modèles installés**, sélectionnez **Cloud** , puis sélectionnez **service Cloud Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="17226-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="17226-144">Conservez la valeur par défaut .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="17226-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="17226-145">Nommez l’application ChatService, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="17226-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="17226-146">Dans la boîte de dialogue **nouveau service Cloud Windows Azure** , sélectionnez rôle Web ASP.net.</span><span class="sxs-lookup"><span data-stu-id="17226-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="17226-147">Cliquez sur le bouton de flèche droite ( **&gt;** ) pour ajouter le rôle à votre solution.</span><span class="sxs-lookup"><span data-stu-id="17226-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="17226-148">Placez le curseur de la souris sur le nouveau rôle pour afficher l’icône de crayon.</span><span class="sxs-lookup"><span data-stu-id="17226-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="17226-149">Cliquez sur cette icône pour renommer le rôle.</span><span class="sxs-lookup"><span data-stu-id="17226-149">Click this icon to rename the role.</span></span> <span data-ttu-id="17226-150">Nommez le rôle « SignalRChat », puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="17226-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="17226-151">Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez **MVC**, puis cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="17226-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="17226-152">L’Assistant projet crée deux projets :</span><span class="sxs-lookup"><span data-stu-id="17226-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="17226-153">ChatService : ce projet est l’application Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="17226-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="17226-154">Il définit les rôles Azure et d’autres options de configuration.</span><span class="sxs-lookup"><span data-stu-id="17226-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="17226-155">SignalRChat : ce projet est votre projet ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="17226-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="17226-156">Créer l’application Signalr chat</span><span class="sxs-lookup"><span data-stu-id="17226-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="17226-157">Pour créer l’application de conversation, suivez les étapes décrites dans le didacticiel [prise en main avec signalr et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="17226-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="17226-158">Utilisez NuGet pour installer les bibliothèques requises.</span><span class="sxs-lookup"><span data-stu-id="17226-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="17226-159">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="17226-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="17226-160">Dans la fenêtre **console du gestionnaire de package** , entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="17226-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="17226-161">Utilisez l’option `-ProjectName` pour installer les packages dans le projet MVC ASP.NET, plutôt que dans le projet Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="17226-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="17226-162">Configurer le fond de panier</span><span class="sxs-lookup"><span data-stu-id="17226-162">Configure the Backplane</span></span>

<span data-ttu-id="17226-163">Dans le fichier Startup.cs de votre application, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="17226-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="17226-164">Vous devez maintenant accéder à votre chaîne de connexion service bus.</span><span class="sxs-lookup"><span data-stu-id="17226-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="17226-165">Dans le Portail Azure, sélectionnez l’espace de noms service bus que vous avez créé, puis cliquez sur l’icône de clé d’accès.</span><span class="sxs-lookup"><span data-stu-id="17226-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="17226-166">Copiez la chaîne de connexion dans le presse-papiers, puis collez-la dans la variable *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="17226-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="17226-167">Déployer sur Azure</span><span class="sxs-lookup"><span data-stu-id="17226-167">Deploy to Azure</span></span>

<span data-ttu-id="17226-168">Dans Explorateur de solutions, développez le dossier **rôles** à l’intérieur du projet ChatService.</span><span class="sxs-lookup"><span data-stu-id="17226-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="17226-169">Cliquez avec le bouton droit sur le rôle SignalRChat et sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="17226-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="17226-170">Sélectionnez l’onglet **configuration** . Sous **instances** , sélectionnez 2.</span><span class="sxs-lookup"><span data-stu-id="17226-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="17226-171">Vous pouvez également définir la taille de la machine virtuelle sur **très petite**.</span><span class="sxs-lookup"><span data-stu-id="17226-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="17226-172">Enregistrez les modifications.</span><span class="sxs-lookup"><span data-stu-id="17226-172">Save the changes.</span></span>

<span data-ttu-id="17226-173">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet ChatService.</span><span class="sxs-lookup"><span data-stu-id="17226-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="17226-174">Sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="17226-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="17226-175">S’il s’agit de votre première publication sur Windows Azure, vous devez télécharger vos informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="17226-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="17226-176">Dans l’Assistant **publication** , cliquez sur « se connecter pour télécharger les informations d’identification ».</span><span class="sxs-lookup"><span data-stu-id="17226-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="17226-177">Vous êtes alors invité à vous connecter au Portail Azure Windows et à télécharger un fichier de paramètres de publication.</span><span class="sxs-lookup"><span data-stu-id="17226-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="17226-178">Cliquez sur **Importer** , puis sélectionnez le fichier de paramètres de publication que vous avez téléchargé.</span><span class="sxs-lookup"><span data-stu-id="17226-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="17226-179">Cliquez sur **Next**.</span><span class="sxs-lookup"><span data-stu-id="17226-179">Click **Next**.</span></span> <span data-ttu-id="17226-180">Dans la boîte de dialogue **paramètres de publication** , sous **service Cloud**, sélectionnez le service Cloud que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="17226-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="17226-181">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="17226-181">Click **Publish**.</span></span> <span data-ttu-id="17226-182">Le déploiement de l’application et le démarrage des machines virtuelles peuvent prendre quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="17226-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="17226-183">Désormais, lorsque vous exécutez l’application de conversation, les instances de rôle communiquent via Azure Service Bus, à l’aide d’une rubrique Service Bus.</span><span class="sxs-lookup"><span data-stu-id="17226-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="17226-184">Une rubrique est une file d’attente de messages qui autorise plusieurs abonnés.</span><span class="sxs-lookup"><span data-stu-id="17226-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="17226-185">Le fond de panier crée automatiquement la rubrique et les abonnements.</span><span class="sxs-lookup"><span data-stu-id="17226-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="17226-186">Pour afficher les abonnements et l’activité des messages, ouvrez le Portail Azure, sélectionnez l’espace de noms Service Bus, puis cliquez sur « rubriques ».</span><span class="sxs-lookup"><span data-stu-id="17226-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="17226-187">Quelques minutes sont nécessaires pour que l’activité des messages apparaisse dans le tableau de bord.</span><span class="sxs-lookup"><span data-stu-id="17226-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="17226-188">Signalr gère la durée de vie de la rubrique.</span><span class="sxs-lookup"><span data-stu-id="17226-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="17226-189">Tant que votre application est déployée, n’essayez pas de supprimer manuellement des rubriques ou de modifier les paramètres de la rubrique.</span><span class="sxs-lookup"><span data-stu-id="17226-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="17226-190">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="17226-190">Troubleshooting</span></span>

<span data-ttu-id="17226-191">**System. InvalidOperationException "la seule IsolationLevel prise en charge est’IsolationLevel. Serializable'."**</span><span class="sxs-lookup"><span data-stu-id="17226-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="17226-192">Cette erreur peut se produire si le niveau de transaction d’une opération est défini sur une valeur autre que `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="17226-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="17226-193">Vérifiez qu’aucune opération n’est effectuée avec d’autres niveaux de transaction.</span><span class="sxs-lookup"><span data-stu-id="17226-193">Verify that no operations are being performed with other transaction levels.</span></span>
