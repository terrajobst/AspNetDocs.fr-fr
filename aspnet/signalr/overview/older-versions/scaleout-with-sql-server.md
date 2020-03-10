---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Signalr ScaleOut avec SQL Server (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 8674b06a2a751c9a7b1c6c9b19782974d0602a62
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536469"
---
# <a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="1baed-102">Scale-out de SignalR avec SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="1baed-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>

<span data-ttu-id="1baed-103">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1baed-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="1baed-104">Dans ce didacticiel, vous allez utiliser SQL Server pour distribuer des messages à travers une application Signalr déployée dans deux instances IIS distinctes.</span><span class="sxs-lookup"><span data-stu-id="1baed-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="1baed-105">Vous pouvez également exécuter ce didacticiel sur un seul ordinateur de test, mais pour obtenir un effet complet, vous devez déployer l’application Signalr sur deux serveurs ou plus.</span><span class="sxs-lookup"><span data-stu-id="1baed-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="1baed-106">Vous devez également installer SQL Server sur l’un des serveurs, ou sur un serveur dédié distinct.</span><span class="sxs-lookup"><span data-stu-id="1baed-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="1baed-107">Une autre option consiste à exécuter le didacticiel à l’aide de machines virtuelles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="1baed-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="1baed-108">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="1baed-108">Prerequisites</span></span>

<span data-ttu-id="1baed-109">Microsoft SQL Server 2005 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="1baed-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="1baed-110">Le fond de panier prend en charge les éditions Desktop et Server de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1baed-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="1baed-111">Il ne prend pas en charge l’édition ou la Azure SQL Database SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="1baed-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="1baed-112">(Si votre application est hébergée sur Azure, envisagez plutôt le Service Bus backplane.)</span><span class="sxs-lookup"><span data-stu-id="1baed-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="1baed-113">Présentation</span><span class="sxs-lookup"><span data-stu-id="1baed-113">Overview</span></span>

<span data-ttu-id="1baed-114">Avant d’accéder au didacticiel détaillé, voici un aperçu rapide de ce que vous allez faire.</span><span class="sxs-lookup"><span data-stu-id="1baed-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="1baed-115">Créez une nouvelle base de données vide.</span><span class="sxs-lookup"><span data-stu-id="1baed-115">Create a new empty database.</span></span> <span data-ttu-id="1baed-116">Le fond de panier crée les tables nécessaires dans cette base de données.</span><span class="sxs-lookup"><span data-stu-id="1baed-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="1baed-117">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="1baed-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="1baed-118">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="1baed-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="1baed-119">Microsoft. AspNet. Signalr. SqlServer</span><span class="sxs-lookup"><span data-stu-id="1baed-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="1baed-120">Créer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="1baed-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="1baed-121">Ajoutez le code suivant à global. asax pour configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="1baed-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="1baed-122">Configurer la base de données</span><span class="sxs-lookup"><span data-stu-id="1baed-122">Configure the Database</span></span>

<span data-ttu-id="1baed-123">Déterminez si l’application doit utiliser l’authentification Windows ou l’authentification SQL Server pour accéder à la base de données.</span><span class="sxs-lookup"><span data-stu-id="1baed-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="1baed-124">Quelle que soit la valeur, assurez-vous que l’utilisateur de base de données dispose des autorisations nécessaires pour se connecter, créer des schémas et créer des tables.</span><span class="sxs-lookup"><span data-stu-id="1baed-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="1baed-125">Créez une nouvelle base de données pour le fond de panier à utiliser.</span><span class="sxs-lookup"><span data-stu-id="1baed-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="1baed-126">Vous pouvez attribuer à la base de données n’importe quel nom.</span><span class="sxs-lookup"><span data-stu-id="1baed-126">You can give the database any name.</span></span> <span data-ttu-id="1baed-127">Vous n’avez pas besoin de créer de tables dans la base de données ; le fond de panier crée les tables nécessaires.</span><span class="sxs-lookup"><span data-stu-id="1baed-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="1baed-128">Activer Service Broker</span><span class="sxs-lookup"><span data-stu-id="1baed-128">Enable Service Broker</span></span>

<span data-ttu-id="1baed-129">Il est recommandé d’activer Service Broker pour la base de données de fond de panier.</span><span class="sxs-lookup"><span data-stu-id="1baed-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="1baed-130">Service Broker fournit une prise en charge native de la messagerie et de la mise en file d’attente dans SQL Server, ce qui permet au fond de panier de recevoir des mises à jour plus efficacement.</span><span class="sxs-lookup"><span data-stu-id="1baed-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="1baed-131">(Toutefois, le fond de panier fonctionne également sans Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="1baed-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="1baed-132">Pour vérifier si la Service Broker est activée, interrogez la colonne **is\_Broker\_activée** de l’affichage catalogue **sys. databases** .</span><span class="sxs-lookup"><span data-stu-id="1baed-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="1baed-133">Pour activer Service Broker, utilisez la requête SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="1baed-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="1baed-134">Si cette requête semble se bloquer, assurez-vous qu’aucune application n’est connectée à la base de la base de la.</span><span class="sxs-lookup"><span data-stu-id="1baed-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="1baed-135">Si vous avez activé le suivi, les traces indiquent également si Service Broker est activé.</span><span class="sxs-lookup"><span data-stu-id="1baed-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="1baed-136">Créer une application Signalr</span><span class="sxs-lookup"><span data-stu-id="1baed-136">Create a SignalR Application</span></span>

<span data-ttu-id="1baed-137">Créez une application Signalr en suivant l’un des didacticiels suivants :</span><span class="sxs-lookup"><span data-stu-id="1baed-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="1baed-138">Prise en main avec Signalr</span><span class="sxs-lookup"><span data-stu-id="1baed-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="1baed-139">Prise en main avec Signalr et MVC 4</span><span class="sxs-lookup"><span data-stu-id="1baed-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="1baed-140">Ensuite, nous allons modifier l’application de conversation pour prendre en charge ScaleOut avec SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1baed-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="1baed-141">Tout d’abord, ajoutez le package NuGet Signalr. SqlServer à votre projet.</span><span class="sxs-lookup"><span data-stu-id="1baed-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="1baed-142">Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="1baed-142">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1baed-143">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1baed-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="1baed-144">Ensuite, ouvrez le fichier global. asax.</span><span class="sxs-lookup"><span data-stu-id="1baed-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="1baed-145">Ajoutez le code suivant à l' **Application\_méthode Start** :</span><span class="sxs-lookup"><span data-stu-id="1baed-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="1baed-146">Déployer et exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="1baed-146">Deploy and Run the Application</span></span>

<span data-ttu-id="1baed-147">Préparez vos instances de Windows Server pour déployer l’application Signalr.</span><span class="sxs-lookup"><span data-stu-id="1baed-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="1baed-148">Ajoutez le rôle IIS.</span><span class="sxs-lookup"><span data-stu-id="1baed-148">Add the IIS role.</span></span> <span data-ttu-id="1baed-149">Inclut des fonctionnalités de développement d’applications, notamment le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1baed-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="1baed-150">Incluez également le service de gestion (listé sous outils d’administration).</span><span class="sxs-lookup"><span data-stu-id="1baed-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="1baed-151">**Installez Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="1baed-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="1baed-152">Lorsque vous exécutez le gestionnaire des services Internet, vous êtes invité à installer Microsoft Web Platform ou vous pouvez [Télécharger le programme d’installation](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="1baed-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="1baed-153">Dans le programme d’installation de la plateforme, recherchez Web Deploy et installez Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="1baed-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="1baed-154">Vérifiez que le service de gestion Web est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="1baed-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="1baed-155">Si ce n’est pas le cas, démarrez le service.</span><span class="sxs-lookup"><span data-stu-id="1baed-155">If not, start the service.</span></span> <span data-ttu-id="1baed-156">(Si vous ne voyez pas service de gestion Web dans la liste des services Windows, assurez-vous que vous avez installé le service de gestion lorsque vous avez ajouté le rôle IIS.)</span><span class="sxs-lookup"><span data-stu-id="1baed-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="1baed-157">Enfin, ouvrez le port 8172 pour TCP.</span><span class="sxs-lookup"><span data-stu-id="1baed-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="1baed-158">Il s’agit du port utilisé par l’outil Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="1baed-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="1baed-159">Vous êtes maintenant prêt à déployer le projet Visual Studio à partir de votre ordinateur de développement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="1baed-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="1baed-160">Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="1baed-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="1baed-161">Pour obtenir une documentation plus détaillée sur le déploiement Web, consultez [mappage de contenu de déploiement Web pour Visual Studio et ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="1baed-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="1baed-162">Si vous déployez l’application sur deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent tous les messages Signalr de l’autre.</span><span class="sxs-lookup"><span data-stu-id="1baed-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="1baed-163">(Bien sûr, dans un environnement de production, les deux serveurs se trouvent derrière un équilibreur de charge.)</span><span class="sxs-lookup"><span data-stu-id="1baed-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="1baed-164">Après avoir exécuté l’application, vous pouvez voir que Signalr a créé automatiquement des tables dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="1baed-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="1baed-165">Signalr gère les tables.</span><span class="sxs-lookup"><span data-stu-id="1baed-165">SignalR manages the tables.</span></span> <span data-ttu-id="1baed-166">Tant que votre application est déployée, ne supprimez pas les lignes, modifiez la table, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="1baed-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
