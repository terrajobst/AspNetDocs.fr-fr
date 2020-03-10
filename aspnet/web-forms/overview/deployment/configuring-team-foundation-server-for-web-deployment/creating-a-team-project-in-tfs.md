---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Création d’un projet d’équipe dans TFS | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment créer un nouveau projet d’équipe dans Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639656"
---
# <a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="2e8be-103">Créer un projet d’équipe dans TFS</span><span class="sxs-lookup"><span data-stu-id="2e8be-103">Creating a Team Project in TFS</span></span>

<span data-ttu-id="2e8be-104">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="2e8be-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="2e8be-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="2e8be-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="2e8be-106">Cette rubrique explique comment créer un nouveau projet d’équipe dans Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="2e8be-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>

<span data-ttu-id="2e8be-107">Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="2e8be-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="2e8be-108">Vue d’ensemble des tâches</span><span class="sxs-lookup"><span data-stu-id="2e8be-108">Task Overview</span></span>

<span data-ttu-id="2e8be-109">Pour approvisionner et utiliser un nouveau projet d’équipe dans TFS, vous devez effectuer ces étapes de haut niveau :</span><span class="sxs-lookup"><span data-stu-id="2e8be-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="2e8be-110">Accordez des autorisations à l’utilisateur qui créera le nouveau projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="2e8be-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="2e8be-111">Créez le projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="2e8be-111">Create the team project.</span></span>
- <span data-ttu-id="2e8be-112">Accordez des autorisations aux membres de l’équipe qui vont travailler sur le projet.</span><span class="sxs-lookup"><span data-stu-id="2e8be-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="2e8be-113">Archiver du contenu.</span><span class="sxs-lookup"><span data-stu-id="2e8be-113">Check in some content.</span></span>

<span data-ttu-id="2e8be-114">Cette rubrique vous montre comment effectuer ces procédures et identifie les utilisateurs et les rôles de travail qui sont susceptibles d’être responsables de chaque procédure.</span><span class="sxs-lookup"><span data-stu-id="2e8be-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="2e8be-115">Sachez que, selon la structure de votre organisation, chacune de ces tâches peut être la responsabilité d’une autre personne.</span><span class="sxs-lookup"><span data-stu-id="2e8be-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="2e8be-116">Les tâches et procédures pas à pas de cette rubrique partent du principe que vous avez installé et configuré TFS, et que vous avez créé une collection de projets d’équipe dans le cadre du processus de configuration.</span><span class="sxs-lookup"><span data-stu-id="2e8be-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="2e8be-117">Pour plus d’informations sur ces hypothèses et pour obtenir des informations générales plus générales sur le scénario, consultez [configurer un serveur de builds TFS pour le déploiement Web](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="2e8be-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="2e8be-118">Accorder des autorisations au créateur du projet d’équipe</span><span class="sxs-lookup"><span data-stu-id="2e8be-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="2e8be-119">Pour créer un projet d’équipe, vous devez disposer des autorisations suivantes :</span><span class="sxs-lookup"><span data-stu-id="2e8be-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="2e8be-120">Vous devez disposer de l’autorisation de **création de projets** sur la couche application TFS.</span><span class="sxs-lookup"><span data-stu-id="2e8be-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="2e8be-121">Vous accordez généralement cette autorisation en ajoutant des utilisateurs au groupe TFS administrateurs de la **collection de projets** .</span><span class="sxs-lookup"><span data-stu-id="2e8be-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="2e8be-122">Le groupe global **Team Foundation Administrators** comprend également cette autorisation.</span><span class="sxs-lookup"><span data-stu-id="2e8be-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="2e8be-123">Vous devez avoir l’autorisation de créer des sites d’équipe dans la collection de sites SharePoint qui correspond à la collection de projets d’équipe TFS.</span><span class="sxs-lookup"><span data-stu-id="2e8be-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="2e8be-124">Vous accordez généralement cette autorisation en ajoutant l’utilisateur à un groupe SharePoint avec des droits de **contrôle total** sur la collection de sites SharePoint.</span><span class="sxs-lookup"><span data-stu-id="2e8be-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="2e8be-125">Si vous utilisez SQL Server Reporting Services fonctionnalités, vous devez être membre du rôle gestionnaire de **contenu Team Foundation** dans Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="2e8be-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="2e8be-126">Qui effectue ces procédures ?</span><span class="sxs-lookup"><span data-stu-id="2e8be-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="2e8be-127">En règle générale, la personne ou le groupe qui administre le déploiement TFS effectue également ces procédures.</span><span class="sxs-lookup"><span data-stu-id="2e8be-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="2e8be-128">Étant donné qu’il s’agit d’un jeu d’autorisations à privilèges élevés, les nouveaux projets d’équipe sont généralement créés par un petit sous-ensemble d’utilisateurs responsable de l’administration d’un déploiement TFS.</span><span class="sxs-lookup"><span data-stu-id="2e8be-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="2e8be-129">Les développeurs ne bénéficient généralement pas des autorisations requises pour créer de nouveaux projets d’équipe.</span><span class="sxs-lookup"><span data-stu-id="2e8be-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="2e8be-130">Accorder des autorisations dans TFS</span><span class="sxs-lookup"><span data-stu-id="2e8be-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="2e8be-131">Si vous souhaitez permettre à un utilisateur de créer des projets d’équipe, la première tâche de haut niveau consiste à ajouter l’utilisateur au groupe administrateurs de la **collection de projets** pour la collection de projets d’équipe.</span><span class="sxs-lookup"><span data-stu-id="2e8be-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="2e8be-132">**Pour ajouter un utilisateur au groupe administrateurs de la collection de projets**</span><span class="sxs-lookup"><span data-stu-id="2e8be-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="2e8be-133">Sur le serveur TFS, dans le menu **Démarrer** , pointez sur **tous les programmes**, cliquez sur **Microsoft Team Foundation Server 2010**, puis sur **console Administration Team Foundation**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="2e8be-134">Dans l’arborescence de navigation, développez **couche application**, puis cliquez sur **collections de projets d’équipe**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="2e8be-135">Dans le volet **collections de projets d’équipe** , sélectionnez la collection de projets d’équipe que vous souhaitez gérer.</span><span class="sxs-lookup"><span data-stu-id="2e8be-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="2e8be-136">Sous l’onglet **général** , cliquez sur **appartenance au groupe**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="2e8be-137">Dans la boîte de dialogue **groupes globaux** , sélectionnez le groupe administrateurs de la **collection de projets** , puis cliquez sur **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="2e8be-138">Dans la boîte de dialogue **Propriétés du groupe de Team Foundation Server** , sélectionnez **utilisateur ou groupe Windows**, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="2e8be-139">Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs ou les groupes** , tapez le nom de l’utilisateur pour lequel vous souhaitez pouvoir créer des projets d’équipe, cliquez sur **vérifier les noms**, puis sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="2e8be-140">Dans la boîte de dialogue **Propriétés du groupe de Team Foundation Server** , cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="2e8be-141">Dans la boîte de dialogue **groupes globaux** , cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="2e8be-142">Accorder des autorisations dans SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="2e8be-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="2e8be-143">Ensuite, vous devez accorder à l’utilisateur l’autorisation de créer des sites d’équipe dans la collection de sites SharePoint qui correspond à votre collection de projets d’équipe TFS.</span><span class="sxs-lookup"><span data-stu-id="2e8be-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="2e8be-144">**Pour accorder des autorisations contrôle total sur la collection de sites SharePoint**</span><span class="sxs-lookup"><span data-stu-id="2e8be-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="2e8be-145">Dans la Console Administration Team Foundation Server, sur la page **collections de projets d’équipe** , sélectionnez la collection de projets d’équipe que vous souhaitez gérer.</span><span class="sxs-lookup"><span data-stu-id="2e8be-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="2e8be-146">Dans l’onglet **site SharePoint** , notez la valeur de l’URL d' **emplacement de site par défaut actuelle** .</span><span class="sxs-lookup"><span data-stu-id="2e8be-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="2e8be-147">Ouvrez Internet Explorer, puis accédez à l’URL que vous avez notée à l’étape 2.</span><span class="sxs-lookup"><span data-stu-id="2e8be-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2e8be-148">Si vous n’êtes pas connecté à Windows en tant qu’utilisateur qui a créé la collection de projets d’équipe, vous devez vous connecter à SharePoint en tant qu’utilisateur pour pouvoir continuer.</span><span class="sxs-lookup"><span data-stu-id="2e8be-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="2e8be-149">Dans le menu **Actions de site** , cliquez sur **Paramètres du site**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="2e8be-150">Sur la page **paramètres du site** , sous **utilisateurs et autorisations**, cliquez sur **personnes et groupes**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="2e8be-151">Dans le volet de navigation de gauche, cliquez sur **groupes**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="2e8be-152">Dans la page **personnes et groupes : tous les groupes** , cliquez sur **configurer des groupes pour ce site**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="2e8be-153">Vous pouvez recevoir une erreur <strong>http 404 introuvable</strong> en raison d’un double bogue d’encodage http.</span><span class="sxs-lookup"><span data-stu-id="2e8be-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="2e8be-154">Si cela se produit, remplacez l’URL par le suivant :</span><span class="sxs-lookup"><span data-stu-id="2e8be-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="2e8be-155">`[site_collection_URL]/_layouts/permsetup.aspx` Par exemple :</span><span class="sxs-lookup"><span data-stu-id="2e8be-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="2e8be-156">Dans la page **configurer les groupes pour ce site** , ajoutez l’utilisateur qui créera des projets d’équipe au groupe **propriétaires** , puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="2e8be-157">Pour plus d’informations sur l’activation des utilisateurs pour créer des projets d’équipe dans une collection de projets d’équipe, consultez [définir des autorisations d’administrateur pour les collections de projets d’équipe](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e8be-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="2e8be-158">Créer un projet d’équipe et ajouter des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="2e8be-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="2e8be-159">Une fois que vous disposez des autorisations nécessaires, vous pouvez utiliser la fenêtre **Team Explorer** dans Visual Studio 2010 pour créer un nouveau projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="2e8be-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="2e8be-160">Cette approche fournit un assistant qui collecte toutes les informations requises et effectue les tâches nécessaires dans TFS, SharePoint et SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="2e8be-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="2e8be-161">Vous devez également accorder des autorisations sur le nouveau projet d’équipe aux membres de l’équipe de développement, afin de leur permettre d’ajouter et de modifier du contenu.</span><span class="sxs-lookup"><span data-stu-id="2e8be-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="2e8be-162">Qui effectue ces procédures ?</span><span class="sxs-lookup"><span data-stu-id="2e8be-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="2e8be-163">En général, un administrateur TFS ou un responsable de l’équipe de développement effectue ces procédures.</span><span class="sxs-lookup"><span data-stu-id="2e8be-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="2e8be-164">Créer un nouveau projet d’équipe</span><span class="sxs-lookup"><span data-stu-id="2e8be-164">Create a New Team Project</span></span>

<span data-ttu-id="2e8be-165">La procédure suivante décrit comment créer un nouveau projet d’équipe dans TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="2e8be-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="2e8be-166">**Pour créer un nouveau projet d’équipe**</span><span class="sxs-lookup"><span data-stu-id="2e8be-166">**To create a new team project**</span></span>

1. <span data-ttu-id="2e8be-167">Dans le **menu Démarrer** , pointez **sur tous les programmes**, cliquez sur **Microsoft Visual Studio 2010**, cliquez avec le bouton droit sur **Microsoft Visual Studio 2010**, puis cliquez sur **exécuter en tant qu’administrateur**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2e8be-168">Si vous n’exécutez pas Visual Studio 2010 en tant qu’administrateur, l’Assistant Nouveau projet d’équipe échouera à la dernière étape.</span><span class="sxs-lookup"><span data-stu-id="2e8be-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="2e8be-169">Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="2e8be-170">Dans Visual Studio, dans le menu **équipe** , cliquez sur **se connecter à Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2e8be-171">Si vous avez déjà configuré une connexion à un serveur TFS, vous pouvez omettre les étapes 4-7.</span><span class="sxs-lookup"><span data-stu-id="2e8be-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="2e8be-172">Dans la boîte de dialogue **connexion au projet d’équipe** , cliquez sur **serveurs**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="2e8be-173">Dans la boîte de dialogue **Ajouter/supprimer des Team Foundation Server** , cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="2e8be-174">Dans la boîte de dialogue **Ajouter un Team Foundation Server** , fournissez les détails de votre instance TFS, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="2e8be-175">Dans la boîte de dialogue **Ajouter/supprimer des Team Foundation Server** , cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="2e8be-176">Dans la boîte de dialogue **se connecter au projet d’équipe** , sélectionnez l’instance TFS à laquelle vous souhaitez vous connecter, sélectionnez la collection de projets d’équipe à laquelle vous souhaitez ajouter, puis cliquez sur **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="2e8be-177">Dans la fenêtre **Team Explorer** , cliquez avec le bouton droit sur la collection de projets d’équipe, puis cliquez sur **nouveau projet d’équipe**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="2e8be-178">Dans la boîte de dialogue **nouveau projet d’équipe** , fournissez un nom et une description pour le projet d’équipe, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2e8be-179">Si votre projet d’équipe comprend des espaces, vous risquez de rencontrer des problèmes lorsque vous utilisez l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy) pour déployer des packages à partir du chemin de sortie.</span><span class="sxs-lookup"><span data-stu-id="2e8be-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="2e8be-180">Les espaces dans le chemin d’accès peuvent compliquer considérablement l’exécution des commandes Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="2e8be-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="2e8be-181">Dans la page **Sélectionner un modèle de processus** , sélectionnez le modèle de processus que vous souhaitez utiliser pour gérer le processus de développement, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2e8be-182">Pour plus d’informations sur les modèles de processus pour TFS, consultez [modèles de processus et outils](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="2e8be-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="2e8be-183">Sur la page **paramètres du site d’équipe** , laissez les paramètres par défaut inchangés, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="2e8be-184">Ce paramètre crée, ou identifie, un site d’équipe SharePoint associé au projet d’équipe TFS.</span><span class="sxs-lookup"><span data-stu-id="2e8be-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="2e8be-185">Votre équipe de développement peut utiliser ce site pour gérer la documentation, participer à des thèmes de discussion, créer des pages wiki et effectuer diverses autres tâches qui ne sont pas liées au code.</span><span class="sxs-lookup"><span data-stu-id="2e8be-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="2e8be-186">Pour plus d’informations, consultez [interactions entre les produits SharePoint et Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e8be-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="2e8be-187">Dans la page **spécifier les paramètres du contrôle de code source** , laissez les paramètres par défaut inchangés, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="2e8be-188">Ce paramètre identifie ou crée l’emplacement dans la hiérarchie des dossiers TFS qui fera office de dossier racine pour votre contenu.</span><span class="sxs-lookup"><span data-stu-id="2e8be-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="2e8be-189">Sur la page **confirmer les paramètres du projet d’équipe** , cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="2e8be-190">Lorsque le nouveau projet d’équipe a été créé, dans la page **projet d’équipe créé** , cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="2e8be-191">Ajouter des utilisateurs à un projet d’équipe</span><span class="sxs-lookup"><span data-stu-id="2e8be-191">Add Users to a Team Project</span></span>

<span data-ttu-id="2e8be-192">Maintenant que vous avez créé le nouveau projet d’équipe, vous pouvez accorder des autorisations aux utilisateurs pour leur permettre de commencer à ajouter et à collaborer sur du contenu.</span><span class="sxs-lookup"><span data-stu-id="2e8be-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="2e8be-193">**Pour ajouter des utilisateurs à un projet d’équipe**</span><span class="sxs-lookup"><span data-stu-id="2e8be-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="2e8be-194">Dans Visual Studio 2010, dans la fenêtre **Team Explorer** , cliquez avec le bouton droit sur le projet d’équipe, pointez sur **paramètres du projet d’équipe**, puis cliquez sur appartenance au **groupe**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="2e8be-195">Pour permettre à un utilisateur d’ajouter, de modifier et de supprimer du code sous contrôle de code source, vous devez l’ajouter au groupe **Contributors** .</span><span class="sxs-lookup"><span data-stu-id="2e8be-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="2e8be-196">Dans la boîte de dialogue **groupes de projets** , sélectionnez le groupe **contributeurs** , puis cliquez sur **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="2e8be-197">Dans la boîte de dialogue **Propriétés du groupe de Team Foundation Server** , sélectionnez **utilisateur ou groupe Windows**, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="2e8be-198">Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs ou les groupes** , tapez le nom d’utilisateur de l’utilisateur que vous souhaitez ajouter au projet d’équipe, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="2e8be-199">Dans la boîte de dialogue **Propriétés du groupe de Team Foundation Server** , cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="2e8be-200">Dans la boîte de dialogue **groupes de projets** , cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="2e8be-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="2e8be-201">Conclusion</span><span class="sxs-lookup"><span data-stu-id="2e8be-201">Conclusion</span></span>

<span data-ttu-id="2e8be-202">À ce stade, votre nouveau projet d’équipe est prêt à être utilisé, et votre équipe de développeurs peut commencer à ajouter du contenu et à collaborer sur le processus de développement.</span><span class="sxs-lookup"><span data-stu-id="2e8be-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="2e8be-203">La rubrique suivante, [Ajout de contenu au contrôle de code source](adding-content-to-source-control.md), explique comment ajouter du contenu au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="2e8be-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="2e8be-204">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2e8be-204">Further Reading</span></span>

<span data-ttu-id="2e8be-205">Pour obtenir des conseils plus larges sur la création de projets d’équipe dans TFS, consultez [créer un projet d’équipe](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="2e8be-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="2e8be-206">Pour plus d’informations sur l’activation des utilisateurs pour créer des projets d’équipe dans une collection de projets d’équipe, consultez [définir des autorisations d’administrateur pour les collections de projets d’équipe](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e8be-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="2e8be-207">Pour plus d’informations sur l’ajout d’utilisateurs aux projets d’équipe, consultez [Ajouter des utilisateurs aux projets d’équipe](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e8be-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2e8be-208">[Précédent](configuring-team-foundation-server-for-web-deployment.md)
> [Suivant](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="2e8be-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
