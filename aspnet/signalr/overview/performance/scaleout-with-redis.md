---
uid: signalr/overview/performance/scaleout-with-redis
title: Signalr ScaleOut avec Redims | Microsoft Docs
author: bradygaster
description: Versions logicielles utilisées dans cette rubrique Visual Studio 2013 .NET 4,5 Signalr version 2 versions précédentes de cette rubrique pour plus d’informations sur les versions antérieures de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579225"
---
# <a name="signalr-scaleout-with-redis"></a><span data-ttu-id="95914-103">Scale-out de SignalR avec Redis</span><span class="sxs-lookup"><span data-stu-id="95914-103">SignalR Scaleout with Redis</span></span>

<span data-ttu-id="95914-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="95914-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="95914-105">Versions logicielles utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="95914-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="95914-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="95914-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="95914-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="95914-107">.NET 4.5</span></span>
> - <span data-ttu-id="95914-108">Signalr version 2,4</span><span class="sxs-lookup"><span data-stu-id="95914-108">SignalR version 2.4</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="95914-109">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="95914-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="95914-110">Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="95914-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="95914-111">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="95914-111">Questions and comments</span></span>
>
> <span data-ttu-id="95914-112">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="95914-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="95914-113">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="95914-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="95914-114">Dans ce didacticiel, vous allez utiliser des [redims](http://redis.io/) pour distribuer des messages à travers une application signalr déployée sur deux instances IIS distinctes.</span><span class="sxs-lookup"><span data-stu-id="95914-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="95914-115">Redims est un magasin clé-valeur en mémoire.</span><span class="sxs-lookup"><span data-stu-id="95914-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="95914-116">Il prend également en charge un système de messagerie avec un modèle de publication/abonnement.</span><span class="sxs-lookup"><span data-stu-id="95914-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="95914-117">Le fond de panier ReDim de Signalr utilise la fonctionnalité Pub/Sub pour transférer les messages vers d’autres serveurs.</span><span class="sxs-lookup"><span data-stu-id="95914-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="95914-118">Pour ce didacticiel, vous allez utiliser trois serveurs :</span><span class="sxs-lookup"><span data-stu-id="95914-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="95914-119">Deux serveurs exécutant Windows, que vous utiliserez pour déployer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="95914-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="95914-120">Un serveur exécutant Linux, que vous utiliserez pour exécuter des ReDim.</span><span class="sxs-lookup"><span data-stu-id="95914-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="95914-121">Pour les captures d’écran de ce didacticiel, j’ai utilisé le protocole TLS Ubuntu 12,04.</span><span class="sxs-lookup"><span data-stu-id="95914-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="95914-122">Si vous n’avez pas trois serveurs physiques à utiliser, vous pouvez créer des machines virtuelles sur Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="95914-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="95914-123">Une autre option consiste à créer des machines virtuelles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="95914-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="95914-124">Bien que ce didacticiel utilise l’implémentation de ReDim officielle, il existe également un [port Windows de redims](https://github.com/MSOpenTech/redis) de MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="95914-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="95914-125">L’installation et la configuration sont différentes, mais les étapes sont identiques dans le cas contraire.</span><span class="sxs-lookup"><span data-stu-id="95914-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="95914-126">Signalr ScaleOut avec Redims ne prend pas en charge les clusters ReDim.</span><span class="sxs-lookup"><span data-stu-id="95914-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="95914-127">Présentation</span><span class="sxs-lookup"><span data-stu-id="95914-127">Overview</span></span>

<span data-ttu-id="95914-128">Avant d’accéder au didacticiel détaillé, voici un aperçu rapide de ce que vous allez faire.</span><span class="sxs-lookup"><span data-stu-id="95914-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="95914-129">Installez les ReDim et démarrez le serveur ReDim.</span><span class="sxs-lookup"><span data-stu-id="95914-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="95914-130">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="95914-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="95914-131">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="95914-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="95914-132">Microsoft. AspNet. Signalr. StackExchangeRedis</span><span class="sxs-lookup"><span data-stu-id="95914-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. <span data-ttu-id="95914-133">Créer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="95914-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="95914-134">Ajoutez le code suivant à Startup.cs pour configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="95914-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="95914-135">Ubuntu sur Hyper-V</span><span class="sxs-lookup"><span data-stu-id="95914-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="95914-136">À l’aide de Windows Hyper-V, vous pouvez facilement créer une machine virtuelle Ubuntu sur Windows Server.</span><span class="sxs-lookup"><span data-stu-id="95914-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="95914-137">Téléchargez le fichier ISO Ubuntu à partir de [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="95914-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="95914-138">Dans Hyper-V, ajoutez une nouvelle machine virtuelle.</span><span class="sxs-lookup"><span data-stu-id="95914-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="95914-139">Dans l’étape **connecter un disque dur virtuel** , sélectionnez **créer un disque dur virtuel**.</span><span class="sxs-lookup"><span data-stu-id="95914-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="95914-140">À l’étape **options d’installation** , sélectionnez **fichier image (. ISO)** , cliquez sur **Parcourir**, puis accédez à l’ISO d’installation Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="95914-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="95914-141">Installer les ReDim</span><span class="sxs-lookup"><span data-stu-id="95914-141">Install Redis</span></span>

<span data-ttu-id="95914-142">Suivez les étapes de [http://redis.io/download](http://redis.io/download) pour télécharger et générer des instructions ReDim.</span><span class="sxs-lookup"><span data-stu-id="95914-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="95914-143">Cela génère les fichiers binaires ReDim dans le répertoire `src`.</span><span class="sxs-lookup"><span data-stu-id="95914-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="95914-144">Par défaut, Redims ne requiert pas de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="95914-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="95914-145">Pour définir un mot de passe, modifiez le fichier `redis.conf`, qui se trouve dans le répertoire racine du code source.</span><span class="sxs-lookup"><span data-stu-id="95914-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="95914-146">(Effectuez une copie de sauvegarde du fichier avant de le modifier) Ajoutez la directive suivante à `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="95914-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="95914-147">À présent, démarrez le serveur Redims :</span><span class="sxs-lookup"><span data-stu-id="95914-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="95914-148">Ouvrez le port 6379, qui est le port par défaut sur lequel ReDim écoute.</span><span class="sxs-lookup"><span data-stu-id="95914-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="95914-149">(Vous pouvez modifier le numéro de port dans le fichier de configuration.)</span><span class="sxs-lookup"><span data-stu-id="95914-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="95914-150">Créer l’application Signalr</span><span class="sxs-lookup"><span data-stu-id="95914-150">Create the SignalR Application</span></span>

<span data-ttu-id="95914-151">Créez une application Signalr en suivant l’un des didacticiels suivants :</span><span class="sxs-lookup"><span data-stu-id="95914-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="95914-152">Prise en main avec Signalr 2,0</span><span class="sxs-lookup"><span data-stu-id="95914-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="95914-153">Prise en main avec Signalr 2,0 et MVC 5</span><span class="sxs-lookup"><span data-stu-id="95914-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="95914-154">Ensuite, nous allons modifier l’application de conversation pour prendre en charge ScaleOut avec Redims.</span><span class="sxs-lookup"><span data-stu-id="95914-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="95914-155">Tout d’abord, ajoutez le package NuGet `Microsoft.AspNet.SignalR.StackExchangeRedis` à votre projet.</span><span class="sxs-lookup"><span data-stu-id="95914-155">First, add the `Microsoft.AspNet.SignalR.StackExchangeRedis` NuGet package to your project.</span></span> <span data-ttu-id="95914-156">Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="95914-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="95914-157">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="95914-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="95914-158">Ensuite, ouvrez le fichier Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="95914-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="95914-159">Ajoutez le code suivant à la méthode de **configuration** :</span><span class="sxs-lookup"><span data-stu-id="95914-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="95914-160">« Server » est le nom du serveur qui exécute les insertions.</span><span class="sxs-lookup"><span data-stu-id="95914-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="95914-161">*port* est le numéro de port</span><span class="sxs-lookup"><span data-stu-id="95914-161">*port* is the port number</span></span>
- <span data-ttu-id="95914-162">« Password » est le mot de passe que vous avez défini dans le fichier redims. conf.</span><span class="sxs-lookup"><span data-stu-id="95914-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="95914-163">« AppName » est une chaîne quelconque.</span><span class="sxs-lookup"><span data-stu-id="95914-163">"AppName" is any string.</span></span> <span data-ttu-id="95914-164">Signalr crée un sous-canal de Pub/Sub portant ce nom.</span><span class="sxs-lookup"><span data-stu-id="95914-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="95914-165">Exemple :</span><span class="sxs-lookup"><span data-stu-id="95914-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="95914-166">Déployer et exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="95914-166">Deploy and Run the Application</span></span>

<span data-ttu-id="95914-167">Préparez vos instances de Windows Server pour déployer l’application Signalr.</span><span class="sxs-lookup"><span data-stu-id="95914-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="95914-168">Ajoutez le rôle IIS.</span><span class="sxs-lookup"><span data-stu-id="95914-168">Add the IIS role.</span></span> <span data-ttu-id="95914-169">Inclut des fonctionnalités de développement d’applications, notamment le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="95914-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="95914-170">Incluez également le service de gestion (listé sous outils d’administration).</span><span class="sxs-lookup"><span data-stu-id="95914-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="95914-171">**Installez Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="95914-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="95914-172">Lorsque vous exécutez le gestionnaire des services Internet, vous êtes invité à installer Microsoft Web Platform ou vous pouvez [Télécharger le programme d’installation](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="95914-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="95914-173">Dans le programme d’installation de la plateforme, recherchez Web Deploy et installez Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="95914-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="95914-174">Vérifiez que le service de gestion Web est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="95914-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="95914-175">Si ce n’est pas le cas, démarrez le service.</span><span class="sxs-lookup"><span data-stu-id="95914-175">If not, start the service.</span></span> <span data-ttu-id="95914-176">(Si vous ne voyez pas service de gestion Web dans la liste des services Windows, assurez-vous que vous avez installé le service de gestion lorsque vous avez ajouté le rôle IIS.)</span><span class="sxs-lookup"><span data-stu-id="95914-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="95914-177">Par défaut, le service de gestion Web écoute le port TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="95914-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="95914-178">Dans le pare-feu Windows, créez une nouvelle règle de trafic entrant pour autoriser le trafic TCP sur le port 8172.</span><span class="sxs-lookup"><span data-stu-id="95914-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="95914-179">Pour plus d’informations, consultez [configuration de règles de pare-feu](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="95914-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="95914-180">(Si vous hébergez les machines virtuelles sur Azure, vous pouvez le faire directement dans le Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="95914-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="95914-181">Consultez [Comment configurer des points de terminaison sur une machine virtuelle](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="95914-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="95914-182">Vous êtes maintenant prêt à déployer le projet Visual Studio à partir de votre ordinateur de développement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="95914-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="95914-183">Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="95914-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="95914-184">Pour obtenir une documentation plus détaillée sur le déploiement Web, consultez [mappage de contenu de déploiement Web pour Visual Studio et ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="95914-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="95914-185">Si vous déployez l’application sur deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent tous les messages Signalr de l’autre.</span><span class="sxs-lookup"><span data-stu-id="95914-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="95914-186">(Bien sûr, dans un environnement de production, les deux serveurs se trouvent derrière un équilibreur de charge.)</span><span class="sxs-lookup"><span data-stu-id="95914-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="95914-187">Si vous êtes curieux de voir les messages envoyés aux ReDim, vous pouvez utiliser le client **redims-CLI** , qui est installé avec des inversions.</span><span class="sxs-lookup"><span data-stu-id="95914-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
