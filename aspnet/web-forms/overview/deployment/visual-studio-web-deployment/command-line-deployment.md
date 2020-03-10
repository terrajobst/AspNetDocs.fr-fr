---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement à partir de la ligne de commande | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630920"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="f0e00-103">Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement à partir de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="f0e00-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>

<span data-ttu-id="f0e00-104">par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f0e00-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f0e00-105">Télécharger le projet de démarrage</span><span class="sxs-lookup"><span data-stu-id="f0e00-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="f0e00-106">Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="f0e00-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="f0e00-107">Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="f0e00-108">Présentation</span><span class="sxs-lookup"><span data-stu-id="f0e00-108">Overview</span></span>

<span data-ttu-id="f0e00-109">Ce didacticiel vous montre comment appeler le pipeline de publication Web de Visual Studio à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="f0e00-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="f0e00-110">Cela est utile pour les scénarios où vous souhaitez [automatiser le processus de déploiement](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) au lieu de le faire manuellement dans Visual Studio, généralement à l’aide d’un [système de contrôle de version de code source](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="f0e00-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="f0e00-111">Apporter une modification au déploiement</span><span class="sxs-lookup"><span data-stu-id="f0e00-111">Make a change to deploy</span></span>

<span data-ttu-id="f0e00-112">Actuellement, la page about affiche le code du modèle.</span><span class="sxs-lookup"><span data-stu-id="f0e00-112">Currently the About page displays the template code.</span></span>

![Page about avec code de modèle](command-line-deployment/_static/image1.png)

<span data-ttu-id="f0e00-114">Vous allez remplacer cela par du code qui affiche un résumé de l’inscription d’un étudiant.</span><span class="sxs-lookup"><span data-stu-id="f0e00-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="f0e00-115">Ouvrez la page *about. aspx* , supprimez tout le balisage à l’intérieur de l’élément `MainContent` `Content` et insérez le balisage suivant à la place :</span><span class="sxs-lookup"><span data-stu-id="f0e00-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="f0e00-116">Exécutez le projet et sélectionnez la page **à propos** de.</span><span class="sxs-lookup"><span data-stu-id="f0e00-116">Run the project and select the **About** page.</span></span>

![Page About](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="f0e00-118">Déployer pour effectuer des tests à l’aide de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="f0e00-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="f0e00-119">Si vous ne déployez pas une autre modification de base de données, désactivez le déploiement de base de données dbDacFx pour la base de données ASPNET-ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="f0e00-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="f0e00-120">Ouvrez l’Assistant **publier le site Web** et, dans chacun des trois profils de publication, désactivez la case à cocher **mettre à jour la base de données** sous l’onglet **paramètres** .</span><span class="sxs-lookup"><span data-stu-id="f0e00-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="f0e00-121">Dans la page de démarrage de Windows 8, recherchez **invite de commandes développeur pour VS2012**.</span><span class="sxs-lookup"><span data-stu-id="f0e00-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="f0e00-122">Cliquez avec le bouton droit sur l’icône de **invite de commandes développeur pour VS2012** , puis cliquez sur **exécuter en tant qu’administrateur**.</span><span class="sxs-lookup"><span data-stu-id="f0e00-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="f0e00-123">Entrez la commande suivante à l’invite de commandes, en remplaçant le chemin d’accès au fichier solution par le chemin d’accès à votre fichier solution :</span><span class="sxs-lookup"><span data-stu-id="f0e00-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="f0e00-124">MSBuild génère la solution et la déploie dans l’environnement de test.</span><span class="sxs-lookup"><span data-stu-id="f0e00-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Sortie de la ligne de commande](command-line-deployment/_static/image3.png)

<span data-ttu-id="f0e00-126">Ouvrez un navigateur et accédez à `http://localhost/ContosoUniversity`, puis cliquez sur la page à **propos** de pour vérifier que le déploiement a réussi.</span><span class="sxs-lookup"><span data-stu-id="f0e00-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="f0e00-127">Si vous n’avez créé aucun étudiant dans le cadre d’un test, vous verrez une page vide sous le titre **statistiques du corps** de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="f0e00-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="f0e00-128">Accédez à la page des **étudiants** , cliquez sur **Ajouter un élève**, ajoutez des élèves, puis revenez à la page à **propos** de pour afficher les statistiques des étudiants.</span><span class="sxs-lookup"><span data-stu-id="f0e00-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Page à propos de dans l’environnement de test](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="f0e00-130">Options de ligne de commande de clé</span><span class="sxs-lookup"><span data-stu-id="f0e00-130">Key command line options</span></span>

<span data-ttu-id="f0e00-131">La commande que vous avez entrée a passé le chemin d’accès au fichier solution et deux propriétés à MSBuild :</span><span class="sxs-lookup"><span data-stu-id="f0e00-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="f0e00-132">Déploiement de la solution et déploiement de projets individuels</span><span class="sxs-lookup"><span data-stu-id="f0e00-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="f0e00-133">Si vous spécifiez le fichier solution, tous les projets de la solution sont générés.</span><span class="sxs-lookup"><span data-stu-id="f0e00-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="f0e00-134">Si vous avez plusieurs projets Web dans la solution, le comportement MSBuild suivant s’applique :</span><span class="sxs-lookup"><span data-stu-id="f0e00-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="f0e00-135">Les propriétés que vous spécifiez sur la ligne de commande sont passées à chaque projet.</span><span class="sxs-lookup"><span data-stu-id="f0e00-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="f0e00-136">Par conséquent, chaque projet Web doit avoir un profil de publication portant le nom que vous spécifiez.</span><span class="sxs-lookup"><span data-stu-id="f0e00-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="f0e00-137">Si vous spécifiez `/p:PublishProfile=Test`, chaque projet Web doit avoir un profil de publication nommé *test*.</span><span class="sxs-lookup"><span data-stu-id="f0e00-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="f0e00-138">Vous pouvez publier un projet avec succès alors qu’un autre n’est pas encore généré.</span><span class="sxs-lookup"><span data-stu-id="f0e00-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="f0e00-139">Pour plus d’informations, consultez le thread StackOverflow [MSBuild échoue avec deux packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="f0e00-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="f0e00-140">Si vous spécifiez un projet individuel plutôt qu’une solution, vous devez ajouter un paramètre qui spécifie la version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0e00-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="f0e00-141">Si vous utilisez Visual Studio 2012, la ligne de commande est similaire à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f0e00-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="f0e00-142">Le numéro de version de Visual Studio 2010 est 10,0.</span><span class="sxs-lookup"><span data-stu-id="f0e00-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="f0e00-143">Pour plus d’informations, consultez [compatibilité des projets Visual Studio et VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) sur le blog de Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="f0e00-143">For more information, see [Visual Studio project compatibility and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="f0e00-144">Spécification du profil de publication</span><span class="sxs-lookup"><span data-stu-id="f0e00-144">Specifying the publish profile</span></span>

<span data-ttu-id="f0e00-145">Vous pouvez spécifier le profil de publication par nom ou par le chemin d’accès complet au fichier *. pubxml* , comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f0e00-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="f0e00-146">Méthodes de publication Web prises en charge pour la publication à partir de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="f0e00-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="f0e00-147">Trois méthodes de publication sont prises en charge pour la publication à partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="f0e00-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="f0e00-148">`MSDeploy`-publiez à l’aide de Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="f0e00-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="f0e00-149">`Package`-publiez en créant un package Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="f0e00-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="f0e00-150">Vous devez installer le package séparément de la commande MSBuild qui le crée.</span><span class="sxs-lookup"><span data-stu-id="f0e00-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="f0e00-151">`FileSystem`-publier en copiant les fichiers dans un dossier spécifié.</span><span class="sxs-lookup"><span data-stu-id="f0e00-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="f0e00-152">Spécification de la configuration et de la plateforme de build</span><span class="sxs-lookup"><span data-stu-id="f0e00-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="f0e00-153">La configuration et la plateforme de build doivent être définies dans Visual Studio ou sur la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="f0e00-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="f0e00-154">Les profils de publication incluent des propriétés nommées `LastUsedBuildConfiguration` et `LastUsedPlatform`, mais vous ne pouvez pas définir ces propriétés afin de déterminer la façon dont le projet est généré.</span><span class="sxs-lookup"><span data-stu-id="f0e00-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="f0e00-155">Pour plus d’informations, consultez [MSBuild : comment définir la propriété de configuration sur le](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) blog de Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="f0e00-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="f0e00-156">Déployer vers un environnement intermédiaire</span><span class="sxs-lookup"><span data-stu-id="f0e00-156">Deploy to staging</span></span>

<span data-ttu-id="f0e00-157">Pour effectuer un déploiement sur Azure, vous devez ajouter le mot de passe à la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="f0e00-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="f0e00-158">Si vous avez enregistré le mot de passe dans le profil de publication dans Visual Studio, il est stocké sous forme chiffrée dans le fichier *. pubxml. User* .</span><span class="sxs-lookup"><span data-stu-id="f0e00-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="f0e00-159">MSBuild n’accède pas à ce fichier quand vous effectuez un déploiement de ligne de commande. vous devez donc passer le mot de passe dans un paramètre de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="f0e00-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="f0e00-160">Copiez le mot de passe dont vous avez besoin à partir du fichier *. publishsettings* que vous avez téléchargé précédemment pour le site Web intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="f0e00-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="f0e00-161">Le mot de passe est la valeur de l’attribut `userPWD` pour l’élément `publishProfile` Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="f0e00-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Mot de passe Web Deploy](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="f0e00-163">Dans la page de démarrage de Windows 8, recherchez **invite de commandes développeur pour VS2012**, puis cliquez sur l’icône pour ouvrir l’invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="f0e00-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="f0e00-164">(Vous n’avez pas besoin de l’ouvrir en tant qu’administrateur cette fois, car vous n’êtes pas déployé sur IIS sur l’ordinateur local.)</span><span class="sxs-lookup"><span data-stu-id="f0e00-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="f0e00-165">Entrez la commande suivante à l’invite de commandes, en remplaçant le chemin d’accès au fichier solution par le chemin d’accès à votre fichier de solution et par le mot de passe avec votre mot de passe :</span><span class="sxs-lookup"><span data-stu-id="f0e00-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="f0e00-166">Notez que cette ligne de commande comprend un paramètre supplémentaire : `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="f0e00-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="f0e00-167">Lors de l’écriture de ce didacticiel, la propriété `AllowUntrustedCertificate` doit être définie lors de la publication sur Azure à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="f0e00-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="f0e00-168">Lorsque le correctif pour ce bogue est publié, vous n’avez pas besoin de ce paramètre.</span><span class="sxs-lookup"><span data-stu-id="f0e00-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="f0e00-169">Ouvrez un navigateur et accédez à l’URL de votre site intermédiaire, puis cliquez sur la page à **propos** de pour vérifier que le déploiement a réussi.</span><span class="sxs-lookup"><span data-stu-id="f0e00-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="f0e00-170">Comme vous l’avez vu précédemment pour l’environnement de test, vous devrez peut-être créer des élèves pour afficher des statistiques sur la page **à propos** de.</span><span class="sxs-lookup"><span data-stu-id="f0e00-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="f0e00-171">Déployer en production</span><span class="sxs-lookup"><span data-stu-id="f0e00-171">Deploy to production</span></span>

<span data-ttu-id="f0e00-172">Le processus de déploiement en production est similaire au processus de mise en lots.</span><span class="sxs-lookup"><span data-stu-id="f0e00-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="f0e00-173">Copiez le mot de passe dont vous avez besoin à partir du fichier *. publishsettings* que vous avez téléchargé précédemment pour le site Web de production.</span><span class="sxs-lookup"><span data-stu-id="f0e00-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="f0e00-174">Ouvrez **invite de commandes développeur pour VS2012**.</span><span class="sxs-lookup"><span data-stu-id="f0e00-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="f0e00-175">Entrez la commande suivante à l’invite de commandes, en remplaçant le chemin d’accès au fichier solution par le chemin d’accès à votre fichier de solution et par le mot de passe avec votre mot de passe :</span><span class="sxs-lookup"><span data-stu-id="f0e00-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="f0e00-176">Pour un site de production réel, en cas de modification d’une base de données, vous devez généralement copier l' *application\_fichier offline. htm* sur le site avant le déploiement et la supprimer après un déploiement réussi.</span><span class="sxs-lookup"><span data-stu-id="f0e00-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="f0e00-177">Ouvrez un navigateur et accédez à l’URL de votre site intermédiaire, puis cliquez sur la page à **propos** de pour vérifier que le déploiement a réussi.</span><span class="sxs-lookup"><span data-stu-id="f0e00-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="f0e00-178">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="f0e00-178">Summary</span></span>

<span data-ttu-id="f0e00-179">Vous avez maintenant déployé une mise à jour d’application à l’aide de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="f0e00-179">You have now deployed an application update by using the command line.</span></span>

![Page à propos de dans l’environnement de test](command-line-deployment/_static/image6.png)

<span data-ttu-id="f0e00-181">Dans le didacticiel suivant, vous verrez un exemple d’extension du pipeline de publication Web.</span><span class="sxs-lookup"><span data-stu-id="f0e00-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="f0e00-182">L’exemple vous montre comment déployer des fichiers qui ne sont pas inclus dans le projet.</span><span class="sxs-lookup"><span data-stu-id="f0e00-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f0e00-183">[Précédent](deploying-a-database-update.md)
> [Suivant](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="f0e00-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
