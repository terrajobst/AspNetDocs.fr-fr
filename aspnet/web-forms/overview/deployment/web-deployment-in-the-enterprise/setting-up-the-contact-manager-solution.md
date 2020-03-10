---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configuration de la solution du gestionnaire de contacts | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment télécharger et configurer la solution gestionnaire de contacts pour une exécution locale sur une station de travail de développeur.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629856"
---
# <a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="3fcc8-103">Configuration de la solution Gestionnaire de contacts</span><span class="sxs-lookup"><span data-stu-id="3fcc8-103">Setting Up the Contact Manager Solution</span></span>

<span data-ttu-id="3fcc8-104">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="3fcc8-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="3fcc8-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="3fcc8-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="3fcc8-106">Cette rubrique explique comment télécharger et configurer la solution gestionnaire de contacts pour une exécution locale sur une station de travail de développeur.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>

## <a name="system-requirements"></a><span data-ttu-id="3fcc8-107">Configuration système requise</span><span class="sxs-lookup"><span data-stu-id="3fcc8-107">System Requirements</span></span>

<span data-ttu-id="3fcc8-108">Pour exécuter la solution du gestionnaire de contacts localement et effectuer les autres tâches décrites dans ce didacticiel, vous devez installer ce logiciel sur votre station de travail de développeur :</span><span class="sxs-lookup"><span data-stu-id="3fcc8-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="3fcc8-109">Visual Studio 2010 Service Pack 1, édition Premium ou édition intégrale</span><span class="sxs-lookup"><span data-stu-id="3fcc8-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="3fcc8-110">Services Internet (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="3fcc8-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="3fcc8-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="3fcc8-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="3fcc8-112">Outil de déploiement Web IIS (Web Deploy) 2,1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="3fcc8-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="3fcc8-113">ASP.NET 4,0</span><span class="sxs-lookup"><span data-stu-id="3fcc8-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="3fcc8-114">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3fcc8-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="3fcc8-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="3fcc8-115">.NET Framework 4</span></span>
- <span data-ttu-id="3fcc8-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="3fcc8-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="3fcc8-117">À l’exception de Visual Studio 2010, vous pouvez télécharger et installer les dernières versions de tous ces produits et composants par le biais du [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="3fcc8-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="3fcc8-118">Télécharger et extraire la solution</span><span class="sxs-lookup"><span data-stu-id="3fcc8-118">Download and Extract the Solution</span></span>

<span data-ttu-id="3fcc8-119">Vous pouvez télécharger l’exemple d’application du gestionnaire de contacts à partir de la Galerie de code MSDN [ici](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="3fcc8-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="3fcc8-120">Configuration et exécution de la solution</span><span class="sxs-lookup"><span data-stu-id="3fcc8-120">Configure and Run the Solution</span></span>

<span data-ttu-id="3fcc8-121">Pour configurer et exécuter la solution gestionnaire de contacts sur votre ordinateur local, vous devez effectuer les étapes principales suivantes :</span><span class="sxs-lookup"><span data-stu-id="3fcc8-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="3fcc8-122">Si vous n’en avez pas déjà un, créez une base de données des services d’application ASP.NET locale avec les fonctionnalités d’appartenance et de gestion des rôles activées.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="3fcc8-123">Modifiez les chaînes de connexion dans les fichiers *Web. config* pour pointer vers votre instance de SQL Server Express locale.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="3fcc8-124">Exécutez la solution à partir de Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="3fcc8-125">Le reste de cette section fournit des conseils supplémentaires sur la façon d’effectuer chacune de ces tâches.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="3fcc8-126">**Pour créer la base de données des services d’application**</span><span class="sxs-lookup"><span data-stu-id="3fcc8-126">**To create the application services database**</span></span>

1. <span data-ttu-id="3fcc8-127">Ouvrez une invite de commandes de Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="3fcc8-128">Pour ce faire, dans le menu **Démarrer** , pointez **sur tous les programmes**, cliquez sur **Microsoft Visual Studio 2010**, sur **Visual Studio Tools**, puis sur invite de **commandes de Visual Studio (2010)** .</span><span class="sxs-lookup"><span data-stu-id="3fcc8-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="3fcc8-129">À l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée :</span><span class="sxs-lookup"><span data-stu-id="3fcc8-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="3fcc8-130">Utilisez le commutateur **– C** pour spécifier la chaîne de connexion de votre serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="3fcc8-131">Utilisez le commutateur **– A** pour spécifier les fonctionnalités des services d’application que vous souhaitez ajouter à la base de données.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="3fcc8-132">Dans ce cas, **m** indique que vous souhaitez ajouter la prise en charge pour le fournisseur d’appartenances et **r** indique que vous souhaitez ajouter la prise en charge du gestionnaire de rôles.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="3fcc8-133">Utilisez le commutateur **– d** pour spécifier un nom pour votre base de données de services d’application.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="3fcc8-134">Si vous omettez ce commutateur, l’utilitaire crée une base de données avec le nom par défaut **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="3fcc8-135">Lorsque la base de données a été créée avec succès, l’invite de commandes affiche une confirmation.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="3fcc8-136">Pour plus d’informations sur l’utilitaire ASPNET\_RegSql, consultez [ASP.NET SQL Server Registration Tool (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="3fcc8-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>

<span data-ttu-id="3fcc8-137">L’étape suivante consiste à s’assurer que les chaînes de connexion dans la solution du gestionnaire de contacts pointent vers votre instance locale de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="3fcc8-138">**Pour mettre à jour les chaînes de connexion**</span><span class="sxs-lookup"><span data-stu-id="3fcc8-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="3fcc8-139">Ouvrez la solution gestionnaire de contacts dans Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="3fcc8-140">Dans la fenêtre **Explorateur de solutions** , développez le projet **ContactManager. Mvc** , puis double-cliquez sur le nœud **Web. config** .</span><span class="sxs-lookup"><span data-stu-id="3fcc8-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3fcc8-141">Le projet ContactManager. Mvc comprend deux fichiers *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="3fcc8-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="3fcc8-142">Vous devez modifier le fichier au niveau du projet.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="3fcc8-143">Dans l’élément **connectionStrings** , vérifiez que la chaîne de connexion nommée **ApplicationServices** pointe vers votre base de données ASP.net application services locale.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="3fcc8-144">Dans la fenêtre **Explorateur de solutions** , développez le projet **ContactManager. service** , puis double-cliquez sur le nœud **Web. config** .</span><span class="sxs-lookup"><span data-stu-id="3fcc8-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="3fcc8-145">Dans l’élément **connectionStrings** , dans la chaîne de connexion nommée **ContactManagerContext**, vérifiez que la propriété de la **source de données** est définie sur votre instance locale de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="3fcc8-146">Vous n’avez pas besoin de modifier quoi que ce soit d’autre dans la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="3fcc8-147">Enregistrez tous les fichiers ouverts.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-147">Save all open files.</span></span>

<span data-ttu-id="3fcc8-148">Vous devez maintenant être prêt à exécuter la solution du gestionnaire de contacts sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="3fcc8-149">Si vous suivez ces étapes sans créer au préalable une base de données de services d’application, ASP.NET crée la base de données la première fois que vous tentez de créer un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="3fcc8-150">Toutefois, la création manuelle de la base de données vous donne beaucoup plus de contrôle sur l’ensemble de fonctionnalités des services d’application que vous souhaitez prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>

<span data-ttu-id="3fcc8-151">**Pour exécuter la solution du gestionnaire de contacts**</span><span class="sxs-lookup"><span data-stu-id="3fcc8-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="3fcc8-152">Dans Visual Studio 2010, appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="3fcc8-153">Internet Explorer démarre et demande l’URL de l’application ASP.NET MVC 3 du gestionnaire de contacts.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="3fcc8-154">Par défaut, l’application affiche la page **tous les contacts** .</span><span class="sxs-lookup"><span data-stu-id="3fcc8-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="3fcc8-155">Ajoutez quelques contacts, puis vérifiez que l’application fonctionne comme prévu.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="3fcc8-156">Accédez à `http://localhost:50114/Account/Register` (ajustez l’URL si vous hébergez l’application sur un autre port).</span><span class="sxs-lookup"><span data-stu-id="3fcc8-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="3fcc8-157">Ajoutez un nom d’utilisateur, une adresse de messagerie et un mot de passe, puis vérifiez que vous êtes en mesure d’inscrire correctement un compte.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="3fcc8-158">Accédez à `http://localhost:50114/Account/LogOn` (ajustez l’URL si vous hébergez l’application sur un autre port).</span><span class="sxs-lookup"><span data-stu-id="3fcc8-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="3fcc8-159">Vérifiez que vous êtes en mesure d’ouvrir une session à l’aide du compte que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="3fcc8-160">Fermez Internet Explorer pour arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="3fcc8-161">Conclusion</span><span class="sxs-lookup"><span data-stu-id="3fcc8-161">Conclusion</span></span>

<span data-ttu-id="3fcc8-162">À ce stade, la solution du gestionnaire de contacts doit être entièrement configurée pour s’exécuter sur votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="3fcc8-163">Vous pouvez utiliser la solution comme référence lorsque vous travaillez dans les autres rubriques de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="3fcc8-164">La rubrique suivante, qui décrit [le fichier projet](understanding-the-project-file.md), explique comment vous pouvez utiliser les fichiers de projet de Microsoft Build Engine personnalisé (MSBuild) dans la solution du gestionnaire de contacts pour contrôler le processus de déploiement.</span><span class="sxs-lookup"><span data-stu-id="3fcc8-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3fcc8-165">[Précédent](the-contact-manager-solution.md)
> [Suivant](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="3fcc8-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
