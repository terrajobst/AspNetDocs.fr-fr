---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Signalr ScaleOut avec Azure Service Bus (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e64f84db00b571c01ea52f48d1ac1af46698d391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558414"
---
# <a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="8cba7-102">Scale-out de SignalR avec Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="8cba7-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>

<span data-ttu-id="8cba7-103">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="8cba7-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="8cba7-104">Dans ce didacticiel, vous allez déployer une application Signalr sur un rôle Web Windows Azure, à l’aide de la Service Bus fond de panier pour distribuer des messages à chaque instance de rôle.</span><span class="sxs-lookup"><span data-stu-id="8cba7-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="8cba7-105">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="8cba7-105">Prerequisites:</span></span>

- <span data-ttu-id="8cba7-106">Un compte Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8cba7-106">A Windows Azure account.</span></span>
- <span data-ttu-id="8cba7-107">Le [Kit de développement logiciel (SDK) Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="8cba7-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="8cba7-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8cba7-108">Visual Studio 2012.</span></span>

<span data-ttu-id="8cba7-109">Le fond de panier service bus est également compatible avec [service bus pour Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1,1.</span><span class="sxs-lookup"><span data-stu-id="8cba7-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="8cba7-110">Toutefois, il n’est pas compatible avec la version 1,0 de Service Bus pour Windows Server.</span><span class="sxs-lookup"><span data-stu-id="8cba7-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="8cba7-111">Prix</span><span class="sxs-lookup"><span data-stu-id="8cba7-111">Pricing</span></span>

<span data-ttu-id="8cba7-112">Le fond de panier Service Bus utilise les rubriques pour envoyer des messages.</span><span class="sxs-lookup"><span data-stu-id="8cba7-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="8cba7-113">Pour obtenir les dernières informations sur la tarification, consultez [service bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="8cba7-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="8cba7-114">Au moment de la rédaction de cet article, vous pouvez envoyer 1 million messages par mois pour moins de $1.</span><span class="sxs-lookup"><span data-stu-id="8cba7-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="8cba7-115">Le fond de panier envoie un message service bus pour chaque appel d’une méthode de concentrateur Signalr.</span><span class="sxs-lookup"><span data-stu-id="8cba7-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="8cba7-116">Il existe également des messages de contrôle pour les connexions, les déconnexions, la jonction ou la sortie de groupes, etc.</span><span class="sxs-lookup"><span data-stu-id="8cba7-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="8cba7-117">Dans la plupart des applications, la majorité du trafic de messages est l’appel de méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="8cba7-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="8cba7-118">Présentation</span><span class="sxs-lookup"><span data-stu-id="8cba7-118">Overview</span></span>

<span data-ttu-id="8cba7-119">Avant d’accéder au didacticiel détaillé, voici un aperçu rapide de ce que vous allez faire.</span><span class="sxs-lookup"><span data-stu-id="8cba7-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="8cba7-120">Utilisez le Portail Azure Windows pour créer un espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="8cba7-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="8cba7-121">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="8cba7-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="8cba7-122">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="8cba7-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="8cba7-123">Microsoft. AspNet. Signalr. ServiceBus</span><span class="sxs-lookup"><span data-stu-id="8cba7-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="8cba7-124">Créer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="8cba7-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="8cba7-125">Ajoutez le code suivant à global. asax pour configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="8cba7-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="8cba7-126">Pour chaque application, choisissez une autre valeur pour « nomapp ».</span><span class="sxs-lookup"><span data-stu-id="8cba7-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="8cba7-127">N’utilisez pas la même valeur pour plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="8cba7-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="8cba7-128">Créer les services Azure</span><span class="sxs-lookup"><span data-stu-id="8cba7-128">Create the Azure Services</span></span>

<span data-ttu-id="8cba7-129">Créez un service Cloud, comme décrit dans [comment créer et déployer un service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="8cba7-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="8cba7-130">Suivez les étapes de la section « Procédure : créer un service Cloud à l’aide de la création rapide ».</span><span class="sxs-lookup"><span data-stu-id="8cba7-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="8cba7-131">Pour ce didacticiel, vous n’avez pas besoin de télécharger un certificat.</span><span class="sxs-lookup"><span data-stu-id="8cba7-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="8cba7-132">Créez un espace de noms Service Bus, comme décrit dans [How to Use service bus topics/subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="8cba7-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="8cba7-133">Suivez les étapes de la section « créer un espace de noms de service ».</span><span class="sxs-lookup"><span data-stu-id="8cba7-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="8cba7-134">Veillez à sélectionner la même région pour le service Cloud et l’espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="8cba7-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="8cba7-135">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8cba7-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="8cba7-136">Démarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8cba7-136">Start Visual Studio.</span></span> <span data-ttu-id="8cba7-137">Dans le menu **Fichier**, cliquez sur **Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="8cba7-138">Dans la boîte de dialogue **nouveau projet** , développez **visuel C#** .</span><span class="sxs-lookup"><span data-stu-id="8cba7-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="8cba7-139">Sous **modèles installés**, sélectionnez **Cloud** , puis sélectionnez **service Cloud Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="8cba7-140">Conservez la valeur par défaut .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="8cba7-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="8cba7-141">Nommez l’application ChatService, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="8cba7-142">Dans la boîte de dialogue **nouveau service Cloud Windows Azure** , sélectionnez rôle Web ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8cba7-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="8cba7-143">Cliquez sur le bouton de flèche droite ( **&gt;** ) pour ajouter le rôle à votre solution.</span><span class="sxs-lookup"><span data-stu-id="8cba7-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="8cba7-144">Placez le curseur de la souris sur le nouveau rôle pour afficher l’icône de crayon.</span><span class="sxs-lookup"><span data-stu-id="8cba7-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="8cba7-145">Cliquez sur cette icône pour renommer le rôle.</span><span class="sxs-lookup"><span data-stu-id="8cba7-145">Click this icon to rename the role.</span></span> <span data-ttu-id="8cba7-146">Nommez le rôle « SignalRChat », puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="8cba7-147">Dans l’Assistant **nouveau projet ASP.NET MVC 4** , sélectionnez **application Internet**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="8cba7-148">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-148">Click **OK**.</span></span> <span data-ttu-id="8cba7-149">L’Assistant projet crée deux projets :</span><span class="sxs-lookup"><span data-stu-id="8cba7-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="8cba7-150">ChatService : ce projet est l’application Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8cba7-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="8cba7-151">Il définit les rôles Azure et d’autres options de configuration.</span><span class="sxs-lookup"><span data-stu-id="8cba7-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="8cba7-152">SignalRChat : ce projet est votre projet ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8cba7-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="8cba7-153">Créer l’application Signalr chat</span><span class="sxs-lookup"><span data-stu-id="8cba7-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="8cba7-154">Pour créer l’application de conversation, suivez les étapes décrites dans le didacticiel [prise en main avec signalr et MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="8cba7-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="8cba7-155">Utilisez NuGet pour installer les bibliothèques requises.</span><span class="sxs-lookup"><span data-stu-id="8cba7-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="8cba7-156">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8cba7-157">Dans la fenêtre **console du gestionnaire de package** , entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="8cba7-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="8cba7-158">Utilisez l’option `-ProjectName` pour installer les packages dans le projet MVC ASP.NET, plutôt que dans le projet Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8cba7-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="8cba7-159">Configurer le fond de panier</span><span class="sxs-lookup"><span data-stu-id="8cba7-159">Configure the Backplane</span></span>

<span data-ttu-id="8cba7-160">Dans le fichier global. asax de votre application, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8cba7-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="8cba7-161">Vous devez maintenant accéder à votre chaîne de connexion service bus.</span><span class="sxs-lookup"><span data-stu-id="8cba7-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="8cba7-162">Dans le Portail Azure, sélectionnez l’espace de noms service bus que vous avez créé, puis cliquez sur l’icône de clé d’accès.</span><span class="sxs-lookup"><span data-stu-id="8cba7-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="8cba7-163">Copiez la chaîne de connexion dans le presse-papiers, puis collez-la dans la variable *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="8cba7-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="8cba7-164">Déployer sur Azure</span><span class="sxs-lookup"><span data-stu-id="8cba7-164">Deploy to Azure</span></span>

<span data-ttu-id="8cba7-165">Dans Explorateur de solutions, développez le dossier **rôles** à l’intérieur du projet ChatService.</span><span class="sxs-lookup"><span data-stu-id="8cba7-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="8cba7-166">Cliquez avec le bouton droit sur le rôle SignalRChat et sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="8cba7-167">Sélectionnez l’onglet **configuration** . Sous **instances** , sélectionnez 2.</span><span class="sxs-lookup"><span data-stu-id="8cba7-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="8cba7-168">Vous pouvez également définir la taille de la machine virtuelle sur **très petite**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="8cba7-169">Enregistrez les modifications.</span><span class="sxs-lookup"><span data-stu-id="8cba7-169">Save the changes.</span></span>

<span data-ttu-id="8cba7-170">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet ChatService.</span><span class="sxs-lookup"><span data-stu-id="8cba7-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="8cba7-171">Sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="8cba7-172">S’il s’agit de votre première publication sur Windows Azure, vous devez télécharger vos informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="8cba7-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="8cba7-173">Dans l’Assistant **publication** , cliquez sur « se connecter pour télécharger les informations d’identification ».</span><span class="sxs-lookup"><span data-stu-id="8cba7-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="8cba7-174">Vous êtes alors invité à vous connecter au Portail Azure Windows et à télécharger un fichier de paramètres de publication.</span><span class="sxs-lookup"><span data-stu-id="8cba7-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="8cba7-175">Cliquez sur **Importer** , puis sélectionnez le fichier de paramètres de publication que vous avez téléchargé.</span><span class="sxs-lookup"><span data-stu-id="8cba7-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="8cba7-176">Cliquez sur **Next**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-176">Click **Next**.</span></span> <span data-ttu-id="8cba7-177">Dans la boîte de dialogue **paramètres de publication** , sous **service Cloud**, sélectionnez le service Cloud que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="8cba7-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="8cba7-178">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="8cba7-178">Click **Publish**.</span></span> <span data-ttu-id="8cba7-179">Le déploiement de l’application et le démarrage des machines virtuelles peuvent prendre quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="8cba7-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="8cba7-180">Désormais, lorsque vous exécutez l’application de conversation, les instances de rôle communiquent via Azure Service Bus, à l’aide d’une rubrique Service Bus.</span><span class="sxs-lookup"><span data-stu-id="8cba7-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="8cba7-181">Une rubrique est une file d’attente de messages qui autorise plusieurs abonnés.</span><span class="sxs-lookup"><span data-stu-id="8cba7-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="8cba7-182">Le fond de panier crée automatiquement la rubrique et les abonnements.</span><span class="sxs-lookup"><span data-stu-id="8cba7-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="8cba7-183">Pour afficher les abonnements et l’activité des messages, ouvrez le Portail Azure, sélectionnez l’espace de noms Service Bus, puis cliquez sur « rubriques ».</span><span class="sxs-lookup"><span data-stu-id="8cba7-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="8cba7-184">Quelques minutes sont nécessaires pour que l’activité des messages apparaisse dans le tableau de bord.</span><span class="sxs-lookup"><span data-stu-id="8cba7-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="8cba7-185">Signalr gère la durée de vie de la rubrique.</span><span class="sxs-lookup"><span data-stu-id="8cba7-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="8cba7-186">Tant que votre application est déployée, n’essayez pas de supprimer manuellement des rubriques ou de modifier les paramètres de la rubrique.</span><span class="sxs-lookup"><span data-stu-id="8cba7-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
