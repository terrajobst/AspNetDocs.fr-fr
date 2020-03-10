---
uid: whitepapers/aspnet-and-iis6
title: Exécution de ASP.NET 1,1 avec IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Bien que Windows Server 2003 inclue à la fois IIS 6,0 et ASP.NET 1,1, ces composants sont désactivés par défaut. Ce livre blanc explique comment activer IIS 6,0...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523309"
---
# <a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="5c7ba-104">Exécution d’ASP.NET 1.1 avec IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="5c7ba-104">Running ASP.NET 1.1 with IIS 6.0</span></span>

> <span data-ttu-id="5c7ba-105">Bien que Windows Server 2003 inclue à la fois IIS 6,0 et ASP.NET 1,1, ces composants sont désactivés par défaut.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="5c7ba-106">Ce livre blanc explique comment activer IIS 6,0 et ASP.NET 1,1, et recommande plusieurs paramètres de configuration pour optimiser les performances d’IIS et de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="5c7ba-107">S’applique à ASP.NET 1,1 et IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>

<span data-ttu-id="5c7ba-108">ASP.NET 1,1 est fourni avec Windows Server 2003, qui inclut également la dernière version d’Internet Information Server (IIS) version 6,0.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="5c7ba-109">IIS 6,0 et ASP.NET 1,1 sont conçus pour s’intégrer en toute transparence et ASP.NET désormais par défaut au nouveau modèle de processus de travail IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="5c7ba-110">ASP.NET 1,1 n’est pas installé par défaut</span><span class="sxs-lookup"><span data-stu-id="5c7ba-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="5c7ba-111">Contrairement aux versions précédentes des systèmes d’exploitation serveur de Microsoft, Internet Information Server (IIS) n’est pas activé par défaut. il ne s’agit pas non plus de ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="5c7ba-112">Il existe deux options pour activer IIS :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="5c7ba-113">Activation d’IIS, option #1-Assistant Configuration de votre serveur</span><span class="sxs-lookup"><span data-stu-id="5c7ba-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="5c7ba-114">Windows Server 2003 fournit un nouvel Assistant Configurer votre serveur pour vous aider à configurer correctement votre serveur dans le mode souhaité.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="5c7ba-115">Pour démarrer l’Assistant-note, pour exécuter l’Assistant, vous devez être connecté en tant qu’administrateur-aller à : Démarrer | Programmes | Outils d’administration et sélectionnez « configurer votre serveur ».</span><span class="sxs-lookup"><span data-stu-id="5c7ba-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="5c7ba-116">Une fois l’option sélectionnée, vous devez voir l’écran d’ouverture de l’Assistant Configurer votre serveur :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="5c7ba-117">Cliquez sur « Next &gt;» :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="5c7ba-118">Cliquez sur « Next &gt;»</span><span class="sxs-lookup"><span data-stu-id="5c7ba-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="5c7ba-119">Sur cet écran, vous devez sélectionner « serveur d’applications (IIS, ASP.NET) » comme options à configurer.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="5c7ba-120">Cliquez sur « Next &gt;».</span><span class="sxs-lookup"><span data-stu-id="5c7ba-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="5c7ba-121">Une fois que vous avez choisi de configurer le serveur en tant que serveur d’applications, cet écran s’affiche et vous invite à indiquer les fonctionnalités supplémentaires à installer.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="5c7ba-122">Aucune option n’est sélectionnée par défaut.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-122">Neither option is selected by default.</span></span> <span data-ttu-id="5c7ba-123">Pour activer ASP.NET automatiquement, vous devez sélectionner «Activer ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="5c7ba-124">Cliquez sur « Next &gt;».</span><span class="sxs-lookup"><span data-stu-id="5c7ba-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="5c7ba-125">Cet écran affiche les options à installer.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="5c7ba-126">Cliquez sur « Next &gt;».</span><span class="sxs-lookup"><span data-stu-id="5c7ba-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="5c7ba-127">Cet écran s’affiche lorsque les options que vous avez sélectionnées sont en cours d’installation.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="5c7ba-128">Il est normal que d’autres boîtes de dialogue s’affichent lorsque les services sont en cours d’installation.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="5c7ba-129">Vous pouvez également être invité à indiquer l’emplacement du CD d’installation du serveur Windows 2003.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="5c7ba-130">Lorsque vous avez terminé, cliquez sur « Next &gt;».</span><span class="sxs-lookup"><span data-stu-id="5c7ba-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="5c7ba-131">Cliquez sur Finish (terminer)-le serveur Windows Server 2003 est maintenant configuré pour prendre en charge IIS 6,0 et ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="5c7ba-132">Activation d’IIS, option #2-Configuration manuelle d’IIS et de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5c7ba-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="5c7ba-133">Si vous ne souhaitez pas utiliser l’Assistant Configurer votre serveur, vous pouvez éventuellement installer IIS 6,0 et ASP.NET 1,1 à l’aide de la commande « Ajout/suppression de programmes » du panneau de configuration.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="5c7ba-134">Commencez par ouvrir le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="5c7ba-135">Ensuite, cliquez sur « Ajouter/supprimer des composants Windows » pour ouvrir l’Assistant composants de Windows :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="5c7ba-136">Mettez en surbrillance et cochez « serveur d’applications », puis cliquez sur « détails ».</span><span class="sxs-lookup"><span data-stu-id="5c7ba-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="5c7ba-137">bouton</span><span class="sxs-lookup"><span data-stu-id="5c7ba-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="5c7ba-138">Pour installer ASP.NET, activez la case à cocher ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="5c7ba-139">Cliquez sur OK pour revenir à l’Assistant composants de Windows.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="5c7ba-140">Cliquez sur « Next &gt;» dans l’Assistant Composants Windows pour commencer l’installation de :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="5c7ba-141">Il est normal que d’autres boîtes de dialogue s’affichent lorsque les services sont en cours d’installation.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="5c7ba-142">Vous pouvez également être invité à indiquer l’emplacement du CD d’installation du serveur Windows 2003.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="5c7ba-143">Une fois l’installation terminée, le dernier écran de l’Assistant Composants Windows s’affiche :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="5c7ba-144">IIS 6,0 et ASP.NET 1,1 sont maintenant configurés et disponibles.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="5c7ba-145">Paramètres recommandés</span><span class="sxs-lookup"><span data-stu-id="5c7ba-145">Recommended Settings</span></span>

<span data-ttu-id="5c7ba-146">Lors de l’exécution de ASP.NET 1,1 avec IIS 6,0, il est recommandé d’utiliser plusieurs paramètres de configuration pour bénéficier des performances optimales de ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="5c7ba-147">Configuration des limites de la mémoire du processus de travail</span><span class="sxs-lookup"><span data-stu-id="5c7ba-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="5c7ba-148">Configuration du recyclage du processus de travail</span><span class="sxs-lookup"><span data-stu-id="5c7ba-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="5c7ba-149">Configuration des limites de la mémoire du processus de travail</span><span class="sxs-lookup"><span data-stu-id="5c7ba-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="5c7ba-150">Par défaut, IIS 6,0 ne définit pas de limite quant à la quantité de mémoire qu’IIS est autorisée à utiliser.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="5c7ba-151">ASP. La fonctionnalité de cache de NET s’appuie sur une limitation de mémoire afin que le cache puisse supprimer de manière proactive les éléments inutilisés de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="5c7ba-152">Il est recommandé de configurer la fonctionnalité de recyclage de la mémoire d’IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="5c7ba-153">Pour configurer ce gestionnaire Open Internet Information Services Manager (Démarrer | Programmes | Outils d’administration | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="5c7ba-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="5c7ba-154">Une fois ouvert, développez le dossier « pools d’applications » :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="5c7ba-155">Pour chaque pool d’applications :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="5c7ba-156">Cliquez avec le bouton droit sur le pool d’applications, par exemple « DefaultAppPool », puis sélectionnez « Propriétés » :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="5c7ba-157">Ensuite, activez le recyclage de la mémoire en cliquant sur « mémoire maximale utilisée (en mégaoctets) : ».</span><span class="sxs-lookup"><span data-stu-id="5c7ba-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="5c7ba-158">La valeur ne doit pas être supérieure à la quantité de mémoire physique (non virtuelle) sur le serveur, une bonne approximation est 60% de la mémoire physique, c.-à-d. pour un serveur de 512 Mo de mémoire physique, sélectionnez 310.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="5c7ba-159">Il est également recommandé de ne pas dépasser le nombre maximal de 800 Mo lors de l’utilisation d’un espace d’adressage de 2 Go.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="5c7ba-160">Si l’espace d’adressage de mémoire du serveur est 3 Go, la limite de mémoire maximale pour le processus de travail peut être aussi élevée que 1 800 Mo :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="5c7ba-161">Cliquez sur appliquer et sur OK pour quitter la boîte de dialogue Propriétés.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="5c7ba-162">Répétez cette opération pour tous les pools d’applications disponibles.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="5c7ba-163">Configuration du recyclage de Worker</span><span class="sxs-lookup"><span data-stu-id="5c7ba-163">Configuring worker recycling</span></span>

<span data-ttu-id="5c7ba-164">Par défaut, IIS 6,0 est configuré pour recycler son processus de travail toutes les 29 heures.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="5c7ba-165">C’est un peu agressif pour une application exécutant ASP.NET et il est recommandé de désactiver le recyclage automatique des processus de travail.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="5c7ba-166">Pour désactiver le recyclage automatique des processus de travail, ouvrez d’abord le gestionnaire de Internet Information Services (Démarrer | Programmes | Outils d’administration | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="5c7ba-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="5c7ba-167">Une fois ouvert, développez le dossier « pools d’applications » :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="5c7ba-168">Pour chaque pool d’applications :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-168">For each application pool:</span></span>

1. <span data-ttu-id="5c7ba-169">Cliquez avec le bouton droit sur le pool d’applications, par exemple « DefaultAppPool », puis sélectionnez « Propriétés » :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="5c7ba-170">Désactivez la case à cocher recycler le processus de travail (en minutes) :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="5c7ba-171">Cliquez sur appliquer et sur OK pour quitter la boîte de dialogue Propriétés.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="5c7ba-172">Répétez cette opération pour tous les pools d’applications disponibles.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="5c7ba-173">Octroi d’un accès en écriture au système de fichiers</span><span class="sxs-lookup"><span data-stu-id="5c7ba-173">Granting write access to the file system</span></span>

<span data-ttu-id="5c7ba-174">Si votre application nécessite un accès en écriture au système de fichiers et que vous utilisez NTFS, vous devez modifier une liste de Access Control (ACL) sur le dossier ou le fichier auquel accorder l’accès ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="5c7ba-175">Par exemple, pour accorder à ASP.NET l’accès en écriture à c:\inetpub\wwwroot First Explorer Open Explorer et accéder au répertoire :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="5c7ba-176">Ensuite, cliquez avec le bouton droit sur le répertoire, par exemple « wwwroot », puis sélectionnez Propriétés.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="5c7ba-177">Une fois que la boîte de dialogue Propriétés s’affiche, sélectionnez l’onglet « sécurité » :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="5c7ba-178">Le répertoire c:\inetpub\wwwroot\ est un répertoire spécial dans le fait que le groupe IIS 6,0 spécial « IIS\_WPG » reçoit déjà les autorisations lire &amp; exécuter, répertorier le contenu du dossier et lecture.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="5c7ba-179">Toutefois, pour accorder l’autorisation d’écriture, vous devez cliquer sur la case à cocher Autoriser pour l’écriture :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="5c7ba-180">IIS 6,0 dispose à présent d’une autorisation en écriture sur ce dossier.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="5c7ba-181">Pour accorder des autorisations en écriture sur d’autres dossiers, procédez comme suit : vous devrez peut-être ajouter le groupe IIS\_WPG s’il n’existe pas déjà.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="5c7ba-182">L’octroi d’une autorisation d’accès en écriture à IIS\_WPG permettra à toute application ASP.NET d’écrire dans ce répertoire.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="5c7ba-183">Prise en charge de l’authentification intégrée avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="5c7ba-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="5c7ba-184">L’authentification intégrée permet à SQL Server de tirer parti de l’authentification Windows NT pour valider SQL Server comptes d’ouverture de session.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="5c7ba-185">Cela permet à l’utilisateur de contourner le processus d’ouverture de session SQL Server standard.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="5c7ba-186">Avec cette approche, un utilisateur réseau peut accéder à une base de données SQL Server sans fournir d’identification ou de mot de passe d’ouverture de session distincte, car SQL Server obtient les informations d’utilisateur et de mot de passe à partir du processus de sécurité réseau de Windows NT.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="5c7ba-187">Le choix de l’authentification intégrée pour les applications ASP.NET est un bon choix, car aucune information d’identification n’est jamais stockée dans votre chaîne de connexion pour votre application.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="5c7ba-188">Au lieu de cela, la chaîne de connexion utilisée pour se connecter à SQL se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="5c7ba-189">Cette chaîne de connexion indique à SQL Server d’utiliser les informations d’identification Windows de l’application qui tente d’accéder à SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="5c7ba-190">Dans le cas de ASP.NET/IIS 6, il s’agit d’un compte dans le groupe IIS\_WPG.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="5c7ba-191">Pour activer l’authentification intégrée entre SQL Server et ASP.NET, vous devez d’abord vous assurer que SQL Server est configuré pour l’authentification intégrée ou l’authentification en mode mixte-Vérifiez auprès de votre administrateur de bases de l’administrateur pour déterminer cela.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="5c7ba-192">Si SQL Server est dans l’un de ces deux modes, vous pouvez utiliser l’authentification intégrée.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="5c7ba-193">Ouvrez le gestionnaire de SQL Server Entreprise (Démarrer | Programmes | Microsoft SQL Server | Enterprise Manager), sélectionnez le serveur approprié, puis développez le dossier sécurité :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="5c7ba-194">Si le groupe « BUILTINT\IIS\_WPG » n’est pas listé, cliquez avec le bouton droit sur connexions et sélectionnez Nouvelle connexion :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="5c7ba-195">Dans la zone de texte « Nom : », entrez « [nom de serveur/domaine] \IIS\_WPG » ou cliquez sur le bouton de sélection pour ouvrir le sélecteur d’utilisateur/de groupe Windows NT :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="5c7ba-196">Sélectionnez le groupe IIS\_WPG de l’ordinateur actuel, cliquez sur « Ajouter », puis sur OK pour fermer le sélecteur.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="5c7ba-197">Vous devez ensuite également définir la base de données par défaut et les autorisations d’accès à la base de données.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="5c7ba-198">Pour définir la base de données par défaut, choisissez dans la liste déroulante, par exemple, sous Northwind est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="5c7ba-199">Ensuite, cliquez sur l’onglet accès à la base de données :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="5c7ba-200">Cliquez sur la case à cocher Autoriser pour chaque base de données à laquelle vous souhaitez autoriser l’accès.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="5c7ba-201">Vous devez également sélectionner des rôles de base de données, en vérifiant la base de données\_propriétaire, afin de garantir que votre connexion dispose de toutes les autorisations nécessaires pour gérer et utiliser la base de données sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="5c7ba-202">Cliquez sur OK pour quitter la boîte de dialogue des propriétés.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="5c7ba-203">Votre application ASP.NET est maintenant configurée pour prendre en charge l’authentification SQL Server intégrée.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="5c7ba-204">N’exécutez pas ASP.NET 1,0 en mode natif IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="5c7ba-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="5c7ba-205">ASP.NET 1,0 sur IIS 6,0 est pris en charge uniquement en mode de compatibilité IIS 5.</span><span class="sxs-lookup"><span data-stu-id="5c7ba-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="5c7ba-206">Pour configurer ASP.NET 1,0 pour qu’il s’exécute en mode de compatibilité IIS 5,0, ouvrez Gestionnaire des services Internet puis cliquez avec le bouton droit sur sites Web et sélectionnez Propriétés :</span><span class="sxs-lookup"><span data-stu-id="5c7ba-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="5c7ba-207">Basculer vers l’onglet service et vérifier ? Exécuter le service WWW en mode d’isolation IIS 5,0 ?:</span><span class="sxs-lookup"><span data-stu-id="5c7ba-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
