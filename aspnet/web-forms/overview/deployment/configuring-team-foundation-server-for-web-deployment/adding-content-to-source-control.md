---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Ajout de contenu au contrôle de code source | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment ajouter du contenu au contrôle de code source dans Team Foundation Server (TFS) 2010. Il décrit comment ajouter des solutions et des projets à une équipe...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634462"
---
# <a name="adding-content-to-source-control"></a><span data-ttu-id="804c2-104">Ajout de contenu au contrôle de code source</span><span class="sxs-lookup"><span data-stu-id="804c2-104">Adding Content to Source Control</span></span>

<span data-ttu-id="804c2-105">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="804c2-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="804c2-106">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="804c2-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="804c2-107">Cette rubrique explique comment ajouter du contenu au contrôle de code source dans Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="804c2-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="804c2-108">Il décrit comment ajouter des solutions et des projets à un projet d’équipe dans TFS, et explique comment ajouter des dépendances externes, telles que des frameworks ou des assemblys, au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="804c2-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>

<span data-ttu-id="804c2-109">Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="804c2-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="804c2-110">Vue d’ensemble des tâches</span><span class="sxs-lookup"><span data-stu-id="804c2-110">Task Overview</span></span>

<span data-ttu-id="804c2-111">Dans la plupart des cas, chaque membre de l’équipe de développement doit être en mesure d’ajouter du contenu au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="804c2-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="804c2-112">Pour ajouter une solution au contrôle de code source dans TFS, vous devez effectuer ces étapes de haut niveau :</span><span class="sxs-lookup"><span data-stu-id="804c2-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="804c2-113">Connectez-vous à un projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="804c2-113">Connect to a team project.</span></span>
- <span data-ttu-id="804c2-114">Mappez la structure de dossiers du projet d’équipe sur le serveur à une structure de dossiers sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="804c2-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="804c2-115">Ajoutez la solution et son contenu au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="804c2-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="804c2-116">Ajoutez toutes les dépendances externes au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="804c2-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="804c2-117">Cette rubrique vous montre comment effectuer ces procédures.</span><span class="sxs-lookup"><span data-stu-id="804c2-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="804c2-118">Les tâches et procédures pas à pas de cette rubrique partent du principe que vous avez déjà créé un nouveau projet d’équipe TFS pour gérer votre contenu.</span><span class="sxs-lookup"><span data-stu-id="804c2-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="804c2-119">Pour plus d’informations sur la création d’un projet d’équipe, consultez [création d’un projet d’équipe dans TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="804c2-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="804c2-120">Qui effectue ces procédures ?</span><span class="sxs-lookup"><span data-stu-id="804c2-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="804c2-121">Dans la plupart des cas, chaque membre de l’équipe de développement doit être en mesure d’ajouter et de modifier du contenu dans des projets d’équipe spécifiques.</span><span class="sxs-lookup"><span data-stu-id="804c2-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="804c2-122">Se connecter à un projet d’équipe et créer un mappage de dossier</span><span class="sxs-lookup"><span data-stu-id="804c2-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="804c2-123">Avant d’ajouter du contenu au contrôle de code source, vous devez vous connecter à un projet d’équipe et créer un mappage entre la structure de dossiers sur le serveur et le système de fichiers sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="804c2-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="804c2-124">**Pour se connecter à un projet d’équipe et mapper un chemin d’accès local**</span><span class="sxs-lookup"><span data-stu-id="804c2-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="804c2-125">Sur votre station de travail de développement, ouvrez Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="804c2-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="804c2-126">Dans Visual Studio, dans le menu **équipe** , cliquez sur **se connecter à Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="804c2-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="804c2-127">Si vous avez déjà configuré une connexion à un serveur TFS, vous pouvez omettre les étapes 3-6.</span><span class="sxs-lookup"><span data-stu-id="804c2-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="804c2-128">Dans la boîte de dialogue **connexion au projet d’équipe** , cliquez sur **serveurs**.</span><span class="sxs-lookup"><span data-stu-id="804c2-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="804c2-129">Dans la boîte de dialogue **Ajouter/supprimer des Team Foundation Server** , cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="804c2-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="804c2-130">Dans la boîte de dialogue **Ajouter un Team Foundation Server** , fournissez les détails de votre instance TFS, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="804c2-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="804c2-131">Dans la boîte de dialogue **Ajouter/supprimer des Team Foundation Server** , cliquez sur **Fermer**.</span><span class="sxs-lookup"><span data-stu-id="804c2-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="804c2-132">Dans la boîte de dialogue **se connecter au projet d’équipe** , sélectionnez l’instance TFS à laquelle vous souhaitez vous connecter, sélectionnez la collection de projets d’équipe, sélectionnez le projet d’équipe auquel vous souhaitez ajouter, puis cliquez sur **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="804c2-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="804c2-133">Dans la fenêtre **Team Explorer** , développez votre projet d’équipe, puis double-cliquez sur **contrôle de code source**.</span><span class="sxs-lookup"><span data-stu-id="804c2-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="804c2-134">Sous l’onglet **Explorateur du contrôle de code source** , cliquez sur **non mappé**.</span><span class="sxs-lookup"><span data-stu-id="804c2-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="804c2-135">Dans la boîte de dialogue **mapper** , dans la zone **dossier local** , accédez à (ou créez) un dossier local pour agir en tant que dossier racine du projet d’équipe, puis cliquez sur **mapper**.</span><span class="sxs-lookup"><span data-stu-id="804c2-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="804c2-136">Lorsque vous êtes invité à télécharger les fichiers sources, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="804c2-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="804c2-137">À ce stade, vous avez mappé le dossier côté serveur du projet d’équipe vers un dossier local sur votre station de travail de développeur.</span><span class="sxs-lookup"><span data-stu-id="804c2-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="804c2-138">Vous avez également téléchargé tout le contenu existant à partir du projet d’équipe dans la structure de vos dossiers locaux.</span><span class="sxs-lookup"><span data-stu-id="804c2-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="804c2-139">Vous pouvez maintenant commencer à ajouter votre propre contenu au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="804c2-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="804c2-140">Ajouter des projets et des solutions au contrôle de code source</span><span class="sxs-lookup"><span data-stu-id="804c2-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="804c2-141">Pour ajouter des projets et des solutions au contrôle de code source, vous devez d’abord les déplacer vers le dossier mappé du projet d’équipe sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="804c2-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="804c2-142">Vous pouvez ensuite archiver le contenu pour synchroniser vos ajouts avec le serveur.</span><span class="sxs-lookup"><span data-stu-id="804c2-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="804c2-143">**Pour ajouter des projets au contrôle de code source**</span><span class="sxs-lookup"><span data-stu-id="804c2-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="804c2-144">Sur votre station de travail de développement, déplacez vos projets et solutions vers un emplacement approprié dans la structure de dossiers mappés pour le projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="804c2-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="804c2-145">De nombreuses organisations auront une approche privilégiée de l’Organisation des projets et des solutions dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="804c2-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="804c2-146">Pour obtenir des conseils sur la structure des dossiers, consultez [Comment : structurer vos dossiers de contrôle de code source dans Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="804c2-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="804c2-147">Ouvrez la solution dans Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="804c2-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="804c2-148">Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur la solution, puis cliquez sur **Ajouter la solution au contrôle de code source**.</span><span class="sxs-lookup"><span data-stu-id="804c2-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="804c2-149">Dans certains cas, selon la façon dont votre organisation aime le contenu de la structure dans TFS, vous devrez peut-être ajouter des projets au contrôle de code source individuellement pour mieux contrôler la façon dont votre code source est organisé.</span><span class="sxs-lookup"><span data-stu-id="804c2-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="804c2-150">Vérifiez que l’onglet **Explorateur du contrôle de code source** affiche le contenu que vous avez ajouté dans la structure de dossiers du serveur pour le projet d’équipe.</span><span class="sxs-lookup"><span data-stu-id="804c2-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="804c2-151">L’onglet **Explorateur du contrôle de code source** affiche votre contenu sans invite supplémentaire, car vous avez ajouté votre solution à un dossier mappé sur le système de fichiers local.</span><span class="sxs-lookup"><span data-stu-id="804c2-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="804c2-152">Si votre solution se trouvait dans un emplacement non mappé, vous serez invité à spécifier des emplacements de dossiers dans TFS et dans votre système de fichiers local.</span><span class="sxs-lookup"><span data-stu-id="804c2-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="804c2-153">Sous l’onglet **Explorateur du contrôle de code source** , dans le volet **dossiers** , cliquez avec le bouton droit sur le projet d’équipe (par exemple, **ContactManager**), puis cliquez sur **archiver les modifications en attente**.</span><span class="sxs-lookup"><span data-stu-id="804c2-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="804c2-154">Dans la boîte de dialogue **Archiver – fichiers sources** , tapez un commentaire, puis cliquez sur **Archiver**.</span><span class="sxs-lookup"><span data-stu-id="804c2-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="804c2-155">À ce stade, vous avez ajouté votre solution au contrôle de code source dans TFS.</span><span class="sxs-lookup"><span data-stu-id="804c2-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="804c2-156">Ajouter des dépendances externes au contrôle de code source</span><span class="sxs-lookup"><span data-stu-id="804c2-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="804c2-157">Lorsque vous ajoutez un projet ou une solution au contrôle de code source, tous les fichiers et dossiers de votre projet ou de votre solution sont également ajoutés.</span><span class="sxs-lookup"><span data-stu-id="804c2-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="804c2-158">Toutefois, dans de nombreux cas, les projets et les solutions s’appuient également sur des dépendances externes, comme les assemblys locaux, pour fonctionner correctement.</span><span class="sxs-lookup"><span data-stu-id="804c2-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="804c2-159">Vous devez ajouter des ressources de ce type au contrôle de code source pour permettre à la fois à Team Build et à d’autres membres de l’équipe de développement de générer votre code avec succès.</span><span class="sxs-lookup"><span data-stu-id="804c2-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="804c2-160">Par exemple, la structure de dossiers de l’exemple de solution du gestionnaire de contacts comprend un dossier nommé packages.</span><span class="sxs-lookup"><span data-stu-id="804c2-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="804c2-161">Contient l’assembly et diverses ressources de prise en charge pour le ADO.NET Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="804c2-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="804c2-162">Le dossier Packages ne fait pas partie de la solution gestionnaire de contacts, mais la solution ne sera pas correctement générée sans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="804c2-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="804c2-163">Pour permettre à Team Build de générer la solution, vous devez ajouter le dossier Packages au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="804c2-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="804c2-164">L’inclusion d’un dossier Packages est typique de ce qui se passe lorsque vous ajoutez le Entity Framework, ou des ressources similaires, à votre solution à l’aide de l’extension NuGet pour Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="804c2-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>

<span data-ttu-id="804c2-165">**Pour ajouter du contenu non-projet au contrôle de code source**</span><span class="sxs-lookup"><span data-stu-id="804c2-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="804c2-166">Assurez-vous que les éléments que vous souhaitez ajouter (par exemple, le dossier Packages) se trouvent à un emplacement approprié au sein d’un dossier mappé sur votre système de fichiers local.</span><span class="sxs-lookup"><span data-stu-id="804c2-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="804c2-167">Dans Visual Studio 2010, dans la fenêtre **Team Explorer** , développez votre projet d’équipe, puis double-cliquez sur **contrôle de code source**.</span><span class="sxs-lookup"><span data-stu-id="804c2-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="804c2-168">Sous l’onglet **Explorateur du contrôle de code source** , dans le volet **dossiers** , sélectionnez le dossier qui contient l’élément ou les éléments que vous souhaitez ajouter.</span><span class="sxs-lookup"><span data-stu-id="804c2-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="804c2-169">Cliquez sur le bouton **Ajouter des éléments au dossier** .</span><span class="sxs-lookup"><span data-stu-id="804c2-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="804c2-170">Dans la boîte de dialogue **Ajouter au contrôle de code source** , sélectionnez le ou les éléments que vous souhaitez ajouter, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="804c2-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="804c2-171">Dans l’onglet **éléments exclus** , sélectionnez tous les éléments requis qui ont été automatiquement exclus (par exemple, les assemblys), puis cliquez sur **inclure**le ou les éléments.</span><span class="sxs-lookup"><span data-stu-id="804c2-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="804c2-172">Dans l’onglet **éléments à ajouter** , vérifiez que tous les fichiers que vous souhaitez inclure sont répertoriés, puis cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="804c2-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="804c2-173">Dans la fenêtre **Explorateur du contrôle de code source** , cliquez sur le bouton **Archiver** .</span><span class="sxs-lookup"><span data-stu-id="804c2-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="804c2-174">Dans la boîte de dialogue **Archiver – fichiers sources** , tapez un commentaire, puis cliquez sur **Archiver**.</span><span class="sxs-lookup"><span data-stu-id="804c2-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="804c2-175">À ce stade, vous avez ajouté les dépendances externes de votre solution au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="804c2-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="804c2-176">Conclusion</span><span class="sxs-lookup"><span data-stu-id="804c2-176">Conclusion</span></span>

<span data-ttu-id="804c2-177">Cette rubrique décrit comment se connecter à un projet d’équipe, mapper une structure de dossiers et ajouter du contenu au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="804c2-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="804c2-178">Pour plus d’informations sur l’utilisation des éléments sous contrôle de code source, consultez [utilisation du contrôle de version](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="804c2-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="804c2-179">La rubrique suivante, [configuration d’un serveur de builds TFS pour le déploiement Web](configuring-a-tfs-build-server-for-web-deployment.md), explique comment préparer un serveur TFS Team Build pour la génération et le déploiement de votre solution.</span><span class="sxs-lookup"><span data-stu-id="804c2-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="804c2-180">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="804c2-180">Further Reading</span></span>

<span data-ttu-id="804c2-181">Pour obtenir des informations plus complètes sur l’utilisation du contrôle de code source dans TFS, consultez [utilisation du contrôle de version](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="804c2-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="804c2-182">[Précédent](creating-a-team-project-in-tfs.md)
> [Suivant](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="804c2-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
