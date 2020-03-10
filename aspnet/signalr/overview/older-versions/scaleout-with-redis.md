---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Signalr ScaleOut avec des ReDim (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 5079cf3fa773faeac911eddc75dc66e94ad98bb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536560"
---
# <a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="3f586-102">Scale-out de SignalR avec Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="3f586-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>

<span data-ttu-id="3f586-103">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="3f586-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="3f586-104">Dans ce didacticiel, vous allez utiliser des [redims](http://redis.io/) pour distribuer des messages à travers une application signalr déployée sur deux instances IIS distinctes.</span><span class="sxs-lookup"><span data-stu-id="3f586-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="3f586-105">Redims est un magasin clé-valeur en mémoire.</span><span class="sxs-lookup"><span data-stu-id="3f586-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="3f586-106">Il prend également en charge un système de messagerie avec un modèle de publication/abonnement.</span><span class="sxs-lookup"><span data-stu-id="3f586-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="3f586-107">Le fond de panier ReDim de Signalr utilise la fonctionnalité Pub/Sub pour transférer les messages vers d’autres serveurs.</span><span class="sxs-lookup"><span data-stu-id="3f586-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="3f586-108">Pour ce didacticiel, vous allez utiliser trois serveurs :</span><span class="sxs-lookup"><span data-stu-id="3f586-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="3f586-109">Deux serveurs exécutant Windows, que vous utiliserez pour déployer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="3f586-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="3f586-110">Un serveur exécutant Linux, que vous utiliserez pour exécuter des ReDim.</span><span class="sxs-lookup"><span data-stu-id="3f586-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="3f586-111">Pour les captures d’écran de ce didacticiel, j’ai utilisé le protocole TLS Ubuntu 12,04.</span><span class="sxs-lookup"><span data-stu-id="3f586-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="3f586-112">Si vous n’avez pas trois serveurs physiques à utiliser, vous pouvez créer des machines virtuelles sur Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="3f586-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="3f586-113">Une autre option consiste à créer des machines virtuelles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="3f586-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="3f586-114">Bien que ce didacticiel utilise l’implémentation de ReDim officielle, il existe également un [port Windows de redims](https://github.com/MSOpenTech/redis) de MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="3f586-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="3f586-115">L’installation et la configuration sont différentes, mais les étapes sont identiques dans le cas contraire.</span><span class="sxs-lookup"><span data-stu-id="3f586-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="3f586-116">Signalr ScaleOut avec Redims ne prend pas en charge les clusters ReDim.</span><span class="sxs-lookup"><span data-stu-id="3f586-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="3f586-117">Présentation</span><span class="sxs-lookup"><span data-stu-id="3f586-117">Overview</span></span>

<span data-ttu-id="3f586-118">Avant d’accéder au didacticiel détaillé, voici un aperçu rapide de ce que vous allez faire.</span><span class="sxs-lookup"><span data-stu-id="3f586-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="3f586-119">Installez les ReDim et démarrez le serveur ReDim.</span><span class="sxs-lookup"><span data-stu-id="3f586-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="3f586-120">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="3f586-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="3f586-121">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="3f586-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="3f586-122">Microsoft. AspNet. Signalr. Redims</span><span class="sxs-lookup"><span data-stu-id="3f586-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="3f586-123">Créer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="3f586-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="3f586-124">Ajoutez le code suivant à global. asax pour configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="3f586-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="3f586-125">Ubuntu sur Hyper-V</span><span class="sxs-lookup"><span data-stu-id="3f586-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="3f586-126">À l’aide de Windows Hyper-V, vous pouvez facilement créer une machine virtuelle Ubuntu sur Windows Server.</span><span class="sxs-lookup"><span data-stu-id="3f586-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="3f586-127">Téléchargez le fichier ISO Ubuntu à partir de [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="3f586-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="3f586-128">Dans Hyper-V, ajoutez une nouvelle machine virtuelle.</span><span class="sxs-lookup"><span data-stu-id="3f586-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="3f586-129">Dans l’étape **connecter un disque dur virtuel** , sélectionnez **créer un disque dur virtuel**.</span><span class="sxs-lookup"><span data-stu-id="3f586-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="3f586-130">À l’étape **options d’installation** , sélectionnez **fichier image (. ISO)** , cliquez sur **Parcourir**, puis accédez à l’ISO d’installation Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="3f586-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="3f586-131">Installer les ReDim</span><span class="sxs-lookup"><span data-stu-id="3f586-131">Install Redis</span></span>

<span data-ttu-id="3f586-132">Suivez les étapes de [http://redis.io/download](http://redis.io/download) pour télécharger et générer des instructions ReDim.</span><span class="sxs-lookup"><span data-stu-id="3f586-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="3f586-133">Cela génère les fichiers binaires ReDim dans le répertoire `src`.</span><span class="sxs-lookup"><span data-stu-id="3f586-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="3f586-134">Par défaut, Redims ne requiert pas de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="3f586-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="3f586-135">Pour définir un mot de passe, modifiez le fichier `redis.conf`, qui se trouve dans le répertoire racine du code source.</span><span class="sxs-lookup"><span data-stu-id="3f586-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="3f586-136">(Effectuez une copie de sauvegarde du fichier avant de le modifier) Ajoutez la directive suivante à `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="3f586-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="3f586-137">À présent, démarrez le serveur Redims :</span><span class="sxs-lookup"><span data-stu-id="3f586-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="3f586-138">Ouvrez le port 6379, qui est le port par défaut sur lequel ReDim écoute.</span><span class="sxs-lookup"><span data-stu-id="3f586-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="3f586-139">(Vous pouvez modifier le numéro de port dans le fichier de configuration.)</span><span class="sxs-lookup"><span data-stu-id="3f586-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="3f586-140">Créer l’application Signalr</span><span class="sxs-lookup"><span data-stu-id="3f586-140">Create the SignalR Application</span></span>

<span data-ttu-id="3f586-141">Créez une application Signalr en suivant l’un des didacticiels suivants :</span><span class="sxs-lookup"><span data-stu-id="3f586-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="3f586-142">Prise en main avec Signalr</span><span class="sxs-lookup"><span data-stu-id="3f586-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="3f586-143">Prise en main avec Signalr et MVC 4</span><span class="sxs-lookup"><span data-stu-id="3f586-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="3f586-144">Ensuite, nous allons modifier l’application de conversation pour prendre en charge ScaleOut avec Redims.</span><span class="sxs-lookup"><span data-stu-id="3f586-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="3f586-145">Tout d’abord, ajoutez le package NuGet Signalr. Redims à votre projet.</span><span class="sxs-lookup"><span data-stu-id="3f586-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="3f586-146">Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="3f586-146">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3f586-147">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="3f586-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="3f586-148">Ensuite, ouvrez le fichier global. asax.</span><span class="sxs-lookup"><span data-stu-id="3f586-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="3f586-149">Ajoutez le code suivant à l' **Application\_méthode Start** :</span><span class="sxs-lookup"><span data-stu-id="3f586-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="3f586-150">« Server » est le nom du serveur qui exécute les insertions.</span><span class="sxs-lookup"><span data-stu-id="3f586-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="3f586-151">*port* est le numéro de port</span><span class="sxs-lookup"><span data-stu-id="3f586-151">*port* is the port number</span></span>
- <span data-ttu-id="3f586-152">« Password » est le mot de passe que vous avez défini dans le fichier redims. conf.</span><span class="sxs-lookup"><span data-stu-id="3f586-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="3f586-153">« AppName » est une chaîne quelconque.</span><span class="sxs-lookup"><span data-stu-id="3f586-153">"AppName" is any string.</span></span> <span data-ttu-id="3f586-154">Signalr crée un sous-canal de Pub/Sub portant ce nom.</span><span class="sxs-lookup"><span data-stu-id="3f586-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="3f586-155">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3f586-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="3f586-156">Déployer et exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="3f586-156">Deploy and Run the Application</span></span>

<span data-ttu-id="3f586-157">Préparez vos instances de Windows Server pour déployer l’application Signalr.</span><span class="sxs-lookup"><span data-stu-id="3f586-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="3f586-158">Ajoutez le rôle IIS.</span><span class="sxs-lookup"><span data-stu-id="3f586-158">Add the IIS role.</span></span> <span data-ttu-id="3f586-159">Inclut des fonctionnalités de développement d’applications, notamment le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3f586-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="3f586-160">Incluez également le service de gestion (listé sous outils d’administration).</span><span class="sxs-lookup"><span data-stu-id="3f586-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="3f586-161">**Installez Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="3f586-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="3f586-162">Lorsque vous exécutez le gestionnaire des services Internet, vous êtes invité à installer Microsoft Web Platform ou vous pouvez [Télécharger le programme d’installation](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="3f586-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="3f586-163">Dans le programme d’installation de la plateforme, recherchez Web Deploy et installez Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="3f586-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="3f586-164">Vérifiez que le service de gestion Web est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="3f586-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="3f586-165">Si ce n’est pas le cas, démarrez le service.</span><span class="sxs-lookup"><span data-stu-id="3f586-165">If not, start the service.</span></span> <span data-ttu-id="3f586-166">(Si vous ne voyez pas service de gestion Web dans la liste des services Windows, assurez-vous que vous avez installé le service de gestion lorsque vous avez ajouté le rôle IIS.)</span><span class="sxs-lookup"><span data-stu-id="3f586-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="3f586-167">Par défaut, le service de gestion Web écoute le port TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="3f586-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="3f586-168">Dans le pare-feu Windows, créez une nouvelle règle de trafic entrant pour autoriser le trafic TCP sur le port 8172.</span><span class="sxs-lookup"><span data-stu-id="3f586-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="3f586-169">Pour plus d’informations, consultez [configuration de règles de pare-feu](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="3f586-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="3f586-170">(Si vous hébergez les machines virtuelles sur Azure, vous pouvez le faire directement dans le Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="3f586-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="3f586-171">Consultez [Comment configurer des points de terminaison sur une machine virtuelle](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="3f586-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="3f586-172">Vous êtes maintenant prêt à déployer le projet Visual Studio à partir de votre ordinateur de développement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="3f586-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="3f586-173">Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="3f586-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="3f586-174">Pour obtenir une documentation plus détaillée sur le déploiement Web, consultez [mappage de contenu de déploiement Web pour Visual Studio et ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="3f586-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="3f586-175">Si vous déployez l’application sur deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent tous les messages Signalr de l’autre.</span><span class="sxs-lookup"><span data-stu-id="3f586-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="3f586-176">(Bien sûr, dans un environnement de production, les deux serveurs se trouvent derrière un équilibreur de charge.)</span><span class="sxs-lookup"><span data-stu-id="3f586-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="3f586-177">Si vous êtes curieux de voir les messages envoyés aux ReDim, vous pouvez utiliser le client **redims-CLI** , qui est installé avec des inversions.</span><span class="sxs-lookup"><span data-stu-id="3f586-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
