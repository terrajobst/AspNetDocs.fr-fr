---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notes de publication pour ASP.NET et Web Tools 2013,1 pour Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Ce document décrit la version de ASP.NET et Web Tools 2013,1 pour Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578441"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="8ad1b-103">ASP.NET et Web Tools 2013.1 pour Visual Studio 2012 - Notes de publication</span><span class="sxs-lookup"><span data-stu-id="8ad1b-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<span data-ttu-id="8ad1b-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8ad1b-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="8ad1b-105">Ce document décrit la version de ASP.NET et Web Tools 2013,1 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

## <a name="contents"></a><span data-ttu-id="8ad1b-106">Contenu</span><span class="sxs-lookup"><span data-stu-id="8ad1b-106">Contents</span></span>

- [<span data-ttu-id="8ad1b-107">Notes d’installation</span><span class="sxs-lookup"><span data-stu-id="8ad1b-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="8ad1b-108">Configuration logicielle requise</span><span class="sxs-lookup"><span data-stu-id="8ad1b-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="8ad1b-109">Nouvelles fonctionnalités de ASP.NET et Web Tools 2013,1 pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8ad1b-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="8ad1b-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="8ad1b-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="8ad1b-111">Modèles</span><span class="sxs-lookup"><span data-stu-id="8ad1b-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="8ad1b-112">Modèle ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="8ad1b-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="8ad1b-113">Modèle API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="8ad1b-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="8ad1b-114">Modèles d’élément</span><span class="sxs-lookup"><span data-stu-id="8ad1b-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="8ad1b-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8ad1b-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="8ad1b-116">Génération de modèles automatique ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ad1b-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="8ad1b-117">Éditeur Razor</span><span class="sxs-lookup"><span data-stu-id="8ad1b-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="8ad1b-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="8ad1b-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="8ad1b-119">Problèmes connus et modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="8ad1b-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="8ad1b-120">Génération de modèles automatique ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ad1b-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="8ad1b-121">Génération de modèles automatique MVC et API Web-erreur HTTP 404, introuvable</span><span class="sxs-lookup"><span data-stu-id="8ad1b-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="8ad1b-122">Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément de génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="8ad1b-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="8ad1b-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="8ad1b-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="8ad1b-124">L’affichage du fichier cshtml avec Browse avec ou F5 provoque une erreur de serveur</span><span class="sxs-lookup"><span data-stu-id="8ad1b-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="8ad1b-125">Réécriture d’URL et tilde (~)</span><span class="sxs-lookup"><span data-stu-id="8ad1b-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="8ad1b-126">Modèles</span><span class="sxs-lookup"><span data-stu-id="8ad1b-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="8ad1b-127">Notes d'installation</span><span class="sxs-lookup"><span data-stu-id="8ad1b-127">Installation Notes</span></span>

<span data-ttu-id="8ad1b-128">[Installer](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET et Web Tools 2013,1 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="8ad1b-129">Configuration logicielle</span><span class="sxs-lookup"><span data-stu-id="8ad1b-129">Software Requirements</span></span>

<span data-ttu-id="8ad1b-130">Vous devez disposer de Visual Studio 2012 ou Visual Studio Express 2012 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="8ad1b-131">Nouvelles fonctionnalités de ASP.NET et Web Tools 2013,1 pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8ad1b-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="8ad1b-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="8ad1b-132">Bootstrap</span></span>

<span data-ttu-id="8ad1b-133">Lorsque vous générez des contrôleurs et des vues MVC 5, le balisage des vues utilise [bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="8ad1b-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="8ad1b-134">Modèles</span><span class="sxs-lookup"><span data-stu-id="8ad1b-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="8ad1b-135">Modèle ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="8ad1b-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="8ad1b-136">Nous avons ajouté un nouveau modèle MVC 5.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="8ad1b-137">Il référence les derniers packages NuGet MVC 5, et vous pouvez utiliser la génération de modèles automatique pour ajouter des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="8ad1b-138">Modèle API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="8ad1b-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="8ad1b-139">Nous avons ajouté un nouveau modèle Web API 2.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="8ad1b-140">Il fait référence aux derniers packages NuGet de l’API Web 2, et vous pouvez utiliser la génération de modèles automatique pour ajouter des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="8ad1b-141">Modèles d'élément</span><span class="sxs-lookup"><span data-stu-id="8ad1b-141">Item Templates</span></span>

<span data-ttu-id="8ad1b-142">Nous avons ajouté de nouveaux modèles d’élément pour les vues MVC 5, les pages Web (Razor 3) et les contrôleurs Web API 2.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="8ad1b-143">Ils installent les packages NuGet associés dans le projet lors de l’ajout de nouveaux éléments.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="8ad1b-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8ad1b-144">Entity Framework 6</span></span>

<span data-ttu-id="8ad1b-145">Lors de la génération de modèles automatique d’un contrôleur MVC ou d’API Web à l’aide de Entity Framework, nous utilisons Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="8ad1b-146">Pour plus d’informations sur Entity Framework, consultez l' [historique des versions Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="8ad1b-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="8ad1b-147">Vous pouvez également télécharger et installer le Entity Framework 6 Tools pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="8ad1b-148">Consultez la [Entity Framework obtenir](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="8ad1b-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="8ad1b-149">Génération de modèles automatique ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ad1b-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="8ad1b-150">La génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="8ad1b-151">Il facilite l’ajout de code réutilisable à votre projet qui interagit avec un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="8ad1b-152">Dans les versions précédentes de Visual Studio, la génération de modèles automatique était limitée aux projets ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="8ad1b-153">Avec cette mise à jour, vous pouvez désormais utiliser la génération de modèles automatique pour tous les projets ASP.NET, y compris Web Forms.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="8ad1b-154">Cette mise à jour ne prend pas en charge la génération de pages pour un projet de Web Forms, mais vous pouvez toujours utiliser la génération de modèles automatique avec Web Forms en ajoutant des dépendances MVC au projet.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="8ad1b-155">La prise en charge de la génération de pages pour Web Forms sera ajoutée dans une prochaine mise à jour.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="8ad1b-156">Lors de l’utilisation de la génération de modèles automatique, nous garantissons que toutes les dépendances requises sont installées dans le projet.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="8ad1b-157">Par exemple, si vous démarrez avec un projet ASP.NET Web Forms et que vous utilisez ensuite la génération de modèles automatique pour ajouter un contrôleur d’API Web, les références et packages NuGet requis sont ajoutés automatiquement à votre projet.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="8ad1b-158">Pour ajouter la génération de modèles automatique MVC à un projet Web Forms, ajoutez un **nouvel élément de génération de modèles** automatique et sélectionnez **dépendances MVC 5** dans la fenêtre de dialogue.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="8ad1b-159">Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="8ad1b-160">Si vous sélectionnez minimal, seuls les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="8ad1b-161">Si vous sélectionnez l’option complète, les dépendances minimales sont ajoutées, ainsi que les fichiers de contenu requis pour un projet MVC.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="8ad1b-162">La prise en charge de la génération de modèles automatique des contrôleurs asynchrones utilise les nouvelles fonctionnalités Async de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="8ad1b-163">Pour plus d’informations et des didacticiels, consultez [vue d’ensemble](../2013/aspnet-scaffolding-overview.md)de la génération de modèles automatique ASP.net.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="8ad1b-164">Ces didacticiels illustrent la génération de modèles automatique avec Visual Studio 2013, mais ils s’appliquent également à ASP.NET et Web Tools 2013,1 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="8ad1b-165">Éditeur Razor</span><span class="sxs-lookup"><span data-stu-id="8ad1b-165">Razor Editor</span></span>

<span data-ttu-id="8ad1b-166">Avec cette mise à jour, Visual Studio 2012 prend désormais en charge les outils/édition Razor 3.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="8ad1b-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="8ad1b-167">NuGet 2.7</span></span>

<span data-ttu-id="8ad1b-168">NuGet 2,7 comprend un ensemble complet de nouvelles fonctionnalités qui sont décrites en détail dans les [notes de publication de nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="8ad1b-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="8ad1b-169">Cette version de NuGet évite aux utilisateurs d’autoriser explicitement NuGet à restaurer les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="8ad1b-170">Lors de l’installation de NuGet 2,7, les utilisateurs consentent implicitement à restaurer automatiquement les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="8ad1b-171">Les utilisateurs peuvent explicitement refuser la restauration des packages à l’aide des paramètres NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="8ad1b-172">Cette modification simplifie le fonctionnement de la restauration des packages.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="8ad1b-173">Problèmes connus et modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="8ad1b-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="8ad1b-174">Génération de modèles automatique ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ad1b-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="8ad1b-175">Génération de modèles automatique MVC et API Web-erreur HTTP 404, introuvable</span><span class="sxs-lookup"><span data-stu-id="8ad1b-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="8ad1b-176">Si vous rencontrez une erreur lors de l’ajout d’un élément de génération de modèles automatique à un projet, il est possible que votre projet reste dans un état incohérent.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="8ad1b-177">Certaines des modifications apportées à la génération de modèles automatique seront annulées, mais d’autres modifications, telles que les packages NuGet installés, ne seront pas annulées.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="8ad1b-178">Si les modifications de configuration de routage sont annulées, les utilisateurs reçoivent une erreur HTTP 404 lors de la navigation vers des éléments de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="8ad1b-179">Pour corriger cette erreur pour MVC, ajoutez un nouvel élément de génération de modèles automatique et sélectionnez dépendances MVC 5 (minimale ou complète).</span><span class="sxs-lookup"><span data-stu-id="8ad1b-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="8ad1b-180">Ce processus ajoutera toutes les modifications requises à votre projet.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="8ad1b-181">Pour corriger cette erreur pour l’API Web :</span><span class="sxs-lookup"><span data-stu-id="8ad1b-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="8ad1b-182">Ajoutez la classe WebApiConfig suivante à votre projet.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="8ad1b-183">Configurez WebApiConfig. Register dans l’application\_méthode Start dans global. asax comme suit :</span><span class="sxs-lookup"><span data-stu-id="8ad1b-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="8ad1b-184">Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément de génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="8ad1b-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="8ad1b-185">Si Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément de génération de modèles automatique avec Entity Framework (par exemple, un contrôleur d’API Web 2 avec des actions, à l’aide d’Entity Framework), il est possible que Visual Studio Express n’ait pas pu charger l’image native d’un assembly dépend de System. Web. extensions.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="8ad1b-186">Pour corriger ce problème, configurez Visual Studio Express pour qu’il fonctionne avec l’image MSIL de System. Web. extensions :</span><span class="sxs-lookup"><span data-stu-id="8ad1b-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="8ad1b-187">Ouvrez l’invite de commandes en mode administrateur.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="8ad1b-188">Accédez à%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE ou% ProgramFiles (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (pour Windows 64 bits).</span><span class="sxs-lookup"><span data-stu-id="8ad1b-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="8ad1b-189">Ouvrez VWDExpress. exe. config dans un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="8ad1b-190">Ajoutez la ligne suivante sous l’élément de configuration &lt;&gt;/&lt;Runtime&gt; :</span><span class="sxs-lookup"><span data-stu-id="8ad1b-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="8ad1b-191">Redémarrez Visual Studio Express 2012 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="8ad1b-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="8ad1b-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a><span data-ttu-id="8ad1b-193">L’affichage du fichier cshtml avec Browse avec ou F5 provoque une erreur de serveur</span><span class="sxs-lookup"><span data-stu-id="8ad1b-193">Viewing cshtml file with Browse With or F5 causes a server error</span></span>

<span data-ttu-id="8ad1b-194">Quand vous créez un projet MVC 5 dans Visual Studio 2012 (ou que vous ouvrez dans Visual Studio 2012 un projet MVC 5 créé dans Visual Studio 2013) et que vous tentez d’afficher un fichier cshtml à l’aide de la fonction parcourir avec ou F5, vous recevez un message d’erreur indiquant que le serveur indique une **erreur de serveur dans l’application'/'** .</span><span class="sxs-lookup"><span data-stu-id="8ad1b-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="8ad1b-195">Le serveur tente de naviguer jusqu’à `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="8ad1b-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="8ad1b-196">Pour résoudre ce problème, changez le paramètre d' **action de démarrage** de votre projet en **page spécifique**.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="8ad1b-197">Vous n’avez pas besoin de fournir une valeur pour la page.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="8ad1b-198">Après avoir apporté cette modification, la sélection de la touche F5 permet de naviguer jusqu’à la racine de votre application (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="8ad1b-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="8ad1b-199">Ce comportement est différent du comportement pour les projets MVC 5 dans Visual Studio 2013, où le paramètre de la **page active** lance la page ouverte.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="8ad1b-200">Réécriture d’URL et tilde (~)</span><span class="sxs-lookup"><span data-stu-id="8ad1b-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="8ad1b-201">Après la mise à niveau vers ASP.NET Razor 3 ou ASP.NET MVC 5, la notation tilde (~) risque de ne plus fonctionner correctement si vous utilisez la réécriture d’URL.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="8ad1b-202">La réécriture d’URL affecte la notation tilde (~) dans les éléments HTML tels que &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;lien/&gt;, et par conséquent, le tilde n’est plus mappé au répertoire racine.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="8ad1b-203">Par exemple, si vous réécrivez les demandes pour **ASP.net/content** à **ASP.net**, l’attribut href dans &lt;A href = "~/Content/"/&gt; se résout en **/content/content/** au lieu de **/** .</span><span class="sxs-lookup"><span data-stu-id="8ad1b-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="8ad1b-204">Pour supprimer cette modification, vous pouvez définir le contexte **WasUrlRewritten IIS\_** sur false dans chaque page Web ou dans l' **application\_BeginRequest** dans global. asax.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="8ad1b-205">Modèles</span><span class="sxs-lookup"><span data-stu-id="8ad1b-205">Templates</span></span>

<span data-ttu-id="8ad1b-206">Quand vous créez des projets MVC ASP.NET avec Visual Studio 2012 sur Windows 8.1 ou Windows Server 2012 R2, Visual Studio affiche un message d’erreur indiquant « la configuration du Web [URL] pour ASP.NET 4,5 a échoué ».</span><span class="sxs-lookup"><span data-stu-id="8ad1b-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![erreur de configuration](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="8ad1b-208">Vous voyez cette erreur, car Visual Studio 2012 n’active pas la fonctionnalité ASP.NET 4,5 lorsqu’elle est installée sur ces versions de Windows.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="8ad1b-209">Pour activer ASP.NET 4,5, effectuez les étapes décrites dans [activer ou désactiver des fonctionnalités Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="8ad1b-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![activer ou désactiver les fonctionnalités Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="8ad1b-211">Vous pouvez également activer ASP.NET 4,5 via la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="8ad1b-212">Ouvrez l’invite de commandes en mode administrateur.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="8ad1b-213">Exécutez la commande suivante pour activer ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="8ad1b-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
