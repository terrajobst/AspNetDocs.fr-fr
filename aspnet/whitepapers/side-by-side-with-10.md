---
uid: whitepapers/side-by-side-with-10
title: ASP.NET l’exécution côte à côte de .NET Framework 1,0 et 1,1 | Microsoft Docs
author: rick-anderson
description: Ce livre blanc explique comment installer .NET 1,0 et .NET 1,1 sur votre ordinateur, ce qui permet à une application Web ASP.NET de s’exécuter sur l’une ou l’autre des versions du minutage...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632971"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="5a0d9-103">ASP.NET - Exécution côte à côte de .NET Framework 1.0 et 1.1</span><span class="sxs-lookup"><span data-stu-id="5a0d9-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>

> <span data-ttu-id="5a0d9-104">Ce livre blanc explique comment installer .NET 1,0 et .NET 1,1 sur votre ordinateur, ce qui permet à une application Web ASP.NET de s’exécuter sur l’une ou l’autre des versions du Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="5a0d9-105">S’applique à ASP.NET 1,0 et ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="5a0d9-106">Dans ASP.NET, les applications sont dites exécutées côte à côte lorsqu’elles sont installées sur le même ordinateur, mais utilisent des versions différentes du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="5a0d9-107">La rubrique suivante explique comment configurer des applications ASP.NET pour l’exécution côte à côte et fournit des étapes détaillées pour :</span><span class="sxs-lookup"><span data-stu-id="5a0d9-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="5a0d9-108">Mettre à jour le mappage de votre application Web à .NET Framework version 1,0 lors de l’installation</span><span class="sxs-lookup"><span data-stu-id="5a0d9-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="5a0d9-109">Mapper une application Web à une version spécifique du .NET Framework</span><span class="sxs-lookup"><span data-stu-id="5a0d9-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="5a0d9-110">Rechercher la version du .NET Framework qu’un site Web utilise</span><span class="sxs-lookup"><span data-stu-id="5a0d9-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="5a0d9-111">Traditionnellement, lorsqu’un composant ou une application est mis à jour sur un ordinateur, l’ancienne version est supprimée et remplacée par la version plus récente.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="5a0d9-112">Si la nouvelle version n’est pas compatible avec la version précédente, cela interrompt généralement les autres applications qui utilisent le composant ou l’application.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="5a0d9-113">L' .NET Framework prend en charge l’exécution côte à côte, ce qui permet l’installation de plusieurs versions d’un assembly ou d’une application sur le même ordinateur en même temps.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="5a0d9-114">Étant donné que plusieurs versions peuvent être installées simultanément, les applications gérées peuvent sélectionner la version à utiliser sans affecter les applications qui utilisent une version différente.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="5a0d9-115">Par défaut, lors de l’installation du .NET Framework version 1,1, toutes les applications ASP.NET existantes sont automatiquement reconfigurées pour utiliser la version la plus récente du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="5a0d9-116">Si vous ne souhaitez pas que vos applications ASP.NET soient par défaut en .NET Framework 1,1, cliquez [ici](#1) pour savoir comment éviter cela pendant l’installation.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="5a0d9-117">Si vous mettez à jour votre serveur Web pour .NET Framework 1,1 et que vous souhaitez qu’une ou plusieurs applications Web s’exécutent .NET Framework 1,0, vous devez mettre à jour le mappage de scripts Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="5a0d9-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="5a0d9-118">Le mappage de script est le mécanisme permettant de mapper l’extension de fichier. aspx pour une application Web spécifique à une version du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="5a0d9-119">Cliquez [ici](#2) pour apprendre à mapper une application Web à une version spécifique du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="5a0d9-120">Vous pouvez utiliser Internet Information Manager ou l’outil d’inscription ASP.NET IIS (ASPNET\_regiis. exe) pour rechercher la version de .NET Framework qui exécute une application Web particulière.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="5a0d9-121">Cliquez [ici](#3) pour découvrir comment trouver la version de la .NET Framework qu’un site Web utilise.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="5a0d9-122">L’un des points à prendre en compte lors de la migration vers .NET Framework 1,1 est que chaque version du .NET Framework utilise son propre fichier machine. config.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="5a0d9-123">Par conséquent, si un administrateur Web a apporté des modifications au fichier machine. config, ces modifications doivent être migrées vers le fichier machine. config .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="5a0d9-124">Maintien du mappage de votre application Web à .NET Framework 1,0 lors de l’installation</span><span class="sxs-lookup"><span data-stu-id="5a0d9-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="5a0d9-125">Par défaut, toutes les applications ASP.NET existantes sont automatiquement reconfigurées au cours de l’installation pour utiliser la version plus récente du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="5a0d9-126">À l’aide de la version la plus récente du .NET Framework, les applications peuvent tirer pleinement parti des améliorations et des nouvelles fonctionnalités incluses dans la nouvelle version.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="5a0d9-127">En même temps, l’administrateur Web, qui peut avoir besoin d’un contrôle granulaire sur les applications mises à jour, peut empêcher le remappage automatique de toutes les applications ASP.NET existantes lors de l’installation du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="5a0d9-128">Pour empêcher le remappage automatique de la totalité de l’application ASP.NET vers la version la plus récente du .NET Framework, l’administrateur Web peut utiliser l’option de ligne de commande/noaspupgrade avec le programme d’installation de Dotnetfx. exe.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="5a0d9-129">**Pour empêcher le remappage total de l’application ASP.NET vers une version plus récente**</span><span class="sxs-lookup"><span data-stu-id="5a0d9-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="5a0d9-130">Cliquez sur **Démarrer**.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-130">Go to **Start**.</span></span>
2. <span data-ttu-id="5a0d9-131">Cliquez sur **exécuter**.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-131">Click on **run**.</span></span>
3. <span data-ttu-id="5a0d9-132">Tapez **cmd**.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-132">Type **cmd**.</span></span>
4. <span data-ttu-id="5a0d9-133">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="5a0d9-134">À partir de l’invite de commandes, tapez la ligne suivante pour commencer l’installation du .NET Framework : **Dotnetfx. exe/c : "installer/noaspupgrade ?** .</span><span class="sxs-lookup"><span data-stu-id="5a0d9-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="5a0d9-135">Cliquez sur **Oui** dans le programme d’installation de Microsoft .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="5a0d9-136">Cette opération démarre le processus d’installation de la .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="5a0d9-137">Mapper une application Web à une version spécifique du .NET Framework</span><span class="sxs-lookup"><span data-stu-id="5a0d9-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="5a0d9-138">Chaque version du .NET Framework contient une version de l’outil d’inscription ASP.NET IIS (ASPNET\_regiis. exe).</span><span class="sxs-lookup"><span data-stu-id="5a0d9-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="5a0d9-139">Cet outil permet aux administrateurs de spécifier qu’une application Web doit être exécutée sous une version particulière du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="5a0d9-140">On parle alors de mapper une application Web à une version du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="5a0d9-141">Les administrateurs doivent sélectionner le\_ASPNET regiis. exe qui correspond à la version du .NET Framework qui sera associé à l’application Web.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="5a0d9-142">Par exemple, un administrateur qui souhaite spécifier qu’un site Web utilise .NET Framework 1,1 doit utiliser le\_ASPNET regiis. exe fourni avec .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="5a0d9-143">Le\_ASPNET regiis. exe pour la version 1,0 se trouve à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="5a0d9-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="5a0d9-144">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\ASPNET\_regiis</span><span class="sxs-lookup"><span data-stu-id="5a0d9-144">C:\WINDOWS\Microsoft.NET\Framework\\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="5a0d9-145">Le\_ASPNET regiis. exe pour la version 1, 1 se trouve à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="5a0d9-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="5a0d9-146">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis</span><span class="sxs-lookup"><span data-stu-id="5a0d9-146">C:\WINDOWS\Microsoft.NET\Framework\\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="5a0d9-147">Le\_ASPNET regiis. exe fournit deux options pour le mappage de script d’une application Web :</span><span class="sxs-lookup"><span data-stu-id="5a0d9-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="5a0d9-148">**-s** définit le mappage de scripts dans le chemin d’accès et dans ses répertoires enfants.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="5a0d9-149">**-sn** définit le mappage de scripts uniquement dans le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="5a0d9-150">Le chemin d’accès définit le chemin des métadonnées IIS de l’application Web, qui est défini sous la forme W3SVC/ROOT/{WebSiteNumber}/{application\_Name}.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="5a0d9-151">Par exemple, pour une application Web appelée portail située sous le site Web par défaut, le chemin d’accès à la métabase est W3SVC/1/ROOT/Portal.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="5a0d9-152">Remarque Vous pouvez également utiliser un outil appelé éditeur de métabase pour obtenir le chemin d’accès de la métabase.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="5a0d9-153">Vous pouvez télécharger cet outil à partir du site Support Microsoft à l’adresse [https://support.microsoft.com/default.aspx?scid=kb; en-US ; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="5a0d9-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="5a0d9-154">Exécutez ASPNET\_regiis. exe-s W3SVC/1/ROOT/Portal pour mettre à jour le mappage de script IIS du portail et sa sous-application.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="5a0d9-155">Exécutez ASPNET\_regiis. exe-SN W3SVC/1/ROOT/Portal pour mettre à jour le mappage de script IIS du portail, sans affecter les applications dans les sous-répertoires du portail.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="5a0d9-156">Rechercher la version de .NET Framework qu’une application Web utilise</span><span class="sxs-lookup"><span data-stu-id="5a0d9-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="5a0d9-157">Un administrateur peut utiliser l’Service Manager Internet pour rechercher la version du .NET Framework qui exécute un site Web.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="5a0d9-158">Les différentes versions de système d’exploitation lancent Internet Service Manager différemment.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="5a0d9-159">Pour démarrer le gestionnaire de service, suivez les étapes indiquées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="5a0d9-160">**Pour démarrer le Service Manager Internet**</span><span class="sxs-lookup"><span data-stu-id="5a0d9-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="5a0d9-161">Cliquez sur **Démarrer**.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-161">Go to **Start**.</span></span>
2. <span data-ttu-id="5a0d9-162">Cliquez sur **exécuter**.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-162">Click on **run**.</span></span>
3. <span data-ttu-id="5a0d9-163">Tapez **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="5a0d9-164">À partir de l’Service Manager Internet, sélectionnez l’application Web dont vous souhaitez connaître la version du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="5a0d9-165">Cliquez avec le bouton droit sur l’application Web, puis cliquez sur **Propriétés.**</span><span class="sxs-lookup"><span data-stu-id="5a0d9-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="5a0d9-166">Dans la fenêtre des propriétés, sélectionnez **Configuration.**</span><span class="sxs-lookup"><span data-stu-id="5a0d9-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="5a0d9-167">Dans la table de mappage d’application, sélectionnez **. aspx**, puis cliquez sur **modifier**.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="5a0d9-168">Dans la zone de texte **exécutable** , examinez le répertoire de version en faisant défiler la liste.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="5a0d9-169">Si le répertoire de version est v. 1.1.4322, l’application est mappée à .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="5a0d9-170">À l’inverse, si le répertoire de version est v 1.0.3705, l’application est mappée à .NET Framework 1,0.</span><span class="sxs-lookup"><span data-stu-id="5a0d9-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
