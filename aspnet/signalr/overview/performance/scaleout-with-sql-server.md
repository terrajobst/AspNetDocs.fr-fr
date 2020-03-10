---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Signalr ScaleOut avec SQL Server | Microsoft Docs
author: bradygaster
description: Versions logicielles utilisées dans cette rubrique Visual Studio 2013 .NET 4,5 Signalr version 2 versions précédentes de cette rubrique pour plus d’informations sur les versions antérieures de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 709a9ebf8f3396842bee0d87e621c00ae1418ec1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579183"
---
# <a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="c9c81-103">Scale-out de SignalR avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="c9c81-103">SignalR Scaleout with SQL Server</span></span>

<span data-ttu-id="c9c81-104">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c9c81-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="c9c81-105">Versions logicielles utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="c9c81-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="c9c81-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c9c81-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="c9c81-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c9c81-107">.NET 4.5</span></span>
> - <span data-ttu-id="c9c81-108">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="c9c81-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="c9c81-109">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="c9c81-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="c9c81-110">Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="c9c81-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="c9c81-111">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="c9c81-111">Questions and comments</span></span>
>
> <span data-ttu-id="c9c81-112">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="c9c81-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c9c81-113">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c9c81-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="c9c81-114">Dans ce didacticiel, vous allez utiliser SQL Server pour distribuer des messages à travers une application Signalr déployée dans deux instances IIS distinctes.</span><span class="sxs-lookup"><span data-stu-id="c9c81-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="c9c81-115">Vous pouvez également exécuter ce didacticiel sur un seul ordinateur de test, mais pour obtenir un effet complet, vous devez déployer l’application Signalr sur deux serveurs ou plus.</span><span class="sxs-lookup"><span data-stu-id="c9c81-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="c9c81-116">Vous devez également installer SQL Server sur l’un des serveurs, ou sur un serveur dédié distinct.</span><span class="sxs-lookup"><span data-stu-id="c9c81-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="c9c81-117">Une autre option consiste à exécuter le didacticiel à l’aide de machines virtuelles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="c9c81-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="c9c81-118">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="c9c81-118">Prerequisites</span></span>

<span data-ttu-id="c9c81-119">Microsoft SQL Server 2005 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="c9c81-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="c9c81-120">Le fond de panier prend en charge les éditions Desktop et Server de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c9c81-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="c9c81-121">Il ne prend pas en charge l’édition ou la Azure SQL Database SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="c9c81-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="c9c81-122">(Si votre application est hébergée sur Azure, envisagez plutôt le Service Bus backplane.)</span><span class="sxs-lookup"><span data-stu-id="c9c81-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="c9c81-123">Présentation</span><span class="sxs-lookup"><span data-stu-id="c9c81-123">Overview</span></span>

<span data-ttu-id="c9c81-124">Avant d’accéder au didacticiel détaillé, voici un aperçu rapide de ce que vous allez faire.</span><span class="sxs-lookup"><span data-stu-id="c9c81-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="c9c81-125">Créez une nouvelle base de données vide.</span><span class="sxs-lookup"><span data-stu-id="c9c81-125">Create a new empty database.</span></span> <span data-ttu-id="c9c81-126">Le fond de panier crée les tables nécessaires dans cette base de données.</span><span class="sxs-lookup"><span data-stu-id="c9c81-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="c9c81-127">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="c9c81-127">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="c9c81-128">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="c9c81-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="c9c81-129">Microsoft. AspNet. Signalr. SqlServer</span><span class="sxs-lookup"><span data-stu-id="c9c81-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="c9c81-130">Créer une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="c9c81-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="c9c81-131">Ajoutez le code suivant à Startup.cs pour configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="c9c81-131">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="c9c81-132">Ce code configure le fond de panier avec les valeurs par défaut de [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) et [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="c9c81-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="c9c81-133">Pour plus d’informations sur la modification de ces valeurs, consultez [performance de signalr : mesures ScaleOut](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="c9c81-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

## <a name="configure-the-database"></a><span data-ttu-id="c9c81-134">Configurer la base de données</span><span class="sxs-lookup"><span data-stu-id="c9c81-134">Configure the Database</span></span>

<span data-ttu-id="c9c81-135">Déterminez si l’application doit utiliser l’authentification Windows ou l’authentification SQL Server pour accéder à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c9c81-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="c9c81-136">Quelle que soit la valeur, assurez-vous que l’utilisateur de base de données dispose des autorisations nécessaires pour se connecter, créer des schémas et créer des tables.</span><span class="sxs-lookup"><span data-stu-id="c9c81-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="c9c81-137">Créez une nouvelle base de données pour le fond de panier à utiliser.</span><span class="sxs-lookup"><span data-stu-id="c9c81-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="c9c81-138">Vous pouvez attribuer à la base de données n’importe quel nom.</span><span class="sxs-lookup"><span data-stu-id="c9c81-138">You can give the database any name.</span></span> <span data-ttu-id="c9c81-139">Vous n’avez pas besoin de créer de tables dans la base de données ; le fond de panier crée les tables nécessaires.</span><span class="sxs-lookup"><span data-stu-id="c9c81-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="c9c81-140">Activer Service Broker</span><span class="sxs-lookup"><span data-stu-id="c9c81-140">Enable Service Broker</span></span>

<span data-ttu-id="c9c81-141">Il est recommandé d’activer Service Broker pour la base de données de fond de panier.</span><span class="sxs-lookup"><span data-stu-id="c9c81-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="c9c81-142">Service Broker fournit une prise en charge native de la messagerie et de la mise en file d’attente dans SQL Server, ce qui permet au fond de panier de recevoir des mises à jour plus efficacement.</span><span class="sxs-lookup"><span data-stu-id="c9c81-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="c9c81-143">(Toutefois, le fond de panier fonctionne également sans Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="c9c81-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="c9c81-144">Pour vérifier si la Service Broker est activée, interrogez la colonne **is\_Broker\_activée** de l’affichage catalogue **sys. databases** .</span><span class="sxs-lookup"><span data-stu-id="c9c81-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="c9c81-145">Pour activer Service Broker, utilisez la requête SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="c9c81-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="c9c81-146">Si cette requête semble se bloquer, assurez-vous qu’aucune application n’est connectée à la base de la base de la.</span><span class="sxs-lookup"><span data-stu-id="c9c81-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="c9c81-147">Si vous avez activé le suivi, les traces indiquent également si Service Broker est activé.</span><span class="sxs-lookup"><span data-stu-id="c9c81-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="c9c81-148">Créer une application Signalr</span><span class="sxs-lookup"><span data-stu-id="c9c81-148">Create a SignalR Application</span></span>

<span data-ttu-id="c9c81-149">Créez une application Signalr en suivant l’un des didacticiels suivants :</span><span class="sxs-lookup"><span data-stu-id="c9c81-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="c9c81-150">Prise en main avec Signalr 2,0</span><span class="sxs-lookup"><span data-stu-id="c9c81-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="c9c81-151">Prise en main avec Signalr 2,0 et MVC 5</span><span class="sxs-lookup"><span data-stu-id="c9c81-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="c9c81-152">Ensuite, nous allons modifier l’application de conversation pour prendre en charge ScaleOut avec SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c9c81-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="c9c81-153">Tout d’abord, ajoutez le package NuGet Signalr. SqlServer à votre projet.</span><span class="sxs-lookup"><span data-stu-id="c9c81-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="c9c81-154">Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="c9c81-154">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c9c81-155">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c9c81-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="c9c81-156">Ensuite, ouvrez le fichier Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="c9c81-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="c9c81-157">Ajoutez le code suivant à la méthode **configure** :</span><span class="sxs-lookup"><span data-stu-id="c9c81-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="c9c81-158">Déployer et exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="c9c81-158">Deploy and Run the Application</span></span>

<span data-ttu-id="c9c81-159">Préparez vos instances de Windows Server pour déployer l’application Signalr.</span><span class="sxs-lookup"><span data-stu-id="c9c81-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="c9c81-160">Ajoutez le rôle IIS.</span><span class="sxs-lookup"><span data-stu-id="c9c81-160">Add the IIS role.</span></span> <span data-ttu-id="c9c81-161">Inclut des fonctionnalités de développement d’applications, notamment le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="c9c81-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="c9c81-162">Incluez également le service de gestion (listé sous outils d’administration).</span><span class="sxs-lookup"><span data-stu-id="c9c81-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="c9c81-163">**Installez Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="c9c81-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="c9c81-164">Lorsque vous exécutez le gestionnaire des services Internet, vous êtes invité à installer Microsoft Web Platform ou vous pouvez [Télécharger le programme d’installation](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="c9c81-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="c9c81-165">Dans le programme d’installation de la plateforme, recherchez Web Deploy et installez Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="c9c81-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="c9c81-166">Vérifiez que le service de gestion Web est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="c9c81-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="c9c81-167">Si ce n’est pas le cas, démarrez le service.</span><span class="sxs-lookup"><span data-stu-id="c9c81-167">If not, start the service.</span></span> <span data-ttu-id="c9c81-168">(Si vous ne voyez pas service de gestion Web dans la liste des services Windows, assurez-vous que vous avez installé le service de gestion lorsque vous avez ajouté le rôle IIS.)</span><span class="sxs-lookup"><span data-stu-id="c9c81-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="c9c81-169">Enfin, ouvrez le port 8172 pour TCP.</span><span class="sxs-lookup"><span data-stu-id="c9c81-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="c9c81-170">Il s’agit du port utilisé par l’outil Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="c9c81-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="c9c81-171">Vous êtes maintenant prêt à déployer le projet Visual Studio à partir de votre ordinateur de développement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="c9c81-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="c9c81-172">Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="c9c81-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="c9c81-173">Pour obtenir une documentation plus détaillée sur le déploiement Web, consultez [mappage de contenu de déploiement Web pour Visual Studio et ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="c9c81-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="c9c81-174">Si vous déployez l’application sur deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent tous les messages Signalr de l’autre.</span><span class="sxs-lookup"><span data-stu-id="c9c81-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="c9c81-175">(Bien sûr, dans un environnement de production, les deux serveurs se trouvent derrière un équilibreur de charge.)</span><span class="sxs-lookup"><span data-stu-id="c9c81-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="c9c81-176">Après avoir exécuté l’application, vous pouvez voir que Signalr a créé automatiquement des tables dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="c9c81-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="c9c81-177">Signalr gère les tables.</span><span class="sxs-lookup"><span data-stu-id="c9c81-177">SignalR manages the tables.</span></span> <span data-ttu-id="c9c81-178">Tant que votre application est déployée, ne supprimez pas les lignes, modifiez la table, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="c9c81-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
