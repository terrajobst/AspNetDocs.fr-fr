---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profiler et déboguer votre application ASP.NET MVC avec l’aperçu | Microsoft Docs
author: Rick-Anderson
description: L’aperçu est une famille de packages NuGet Open source prospère et croissante qui fournit des informations détaillées sur les performances, le débogage et le diagnostic pour ASP.NET a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538534"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="7645c-103">Profiler et déboguer votre application ASP.NET MVC avec Glimpse</span><span class="sxs-lookup"><span data-stu-id="7645c-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>

<span data-ttu-id="7645c-104">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7645c-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="7645c-105">L’aperçu est une famille de packages NuGet Open source prospère et croissante qui fournit des informations détaillées sur les performances, le débogage et le diagnostic pour les applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7645c-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="7645c-106">Il est facile d’installer, léger, ultra-rapide et d’afficher des mesures de performances clés en bas de chaque page.</span><span class="sxs-lookup"><span data-stu-id="7645c-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="7645c-107">Elle vous permet d’accéder à votre application lorsque vous avez besoin de savoir ce qui se passe sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="7645c-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="7645c-108">L’aperçu fournit des informations précieuses, nous vous recommandons de l’utiliser tout au long de votre cycle de développement, y compris votre environnement de test Azure.</span><span class="sxs-lookup"><span data-stu-id="7645c-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="7645c-109">Bien que [Fiddler](http://www.telerik.com/fiddler) et les [outils de développement F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) fournissent une vue côté client, l’outil aperçu fournit une vue détaillée du serveur.</span><span class="sxs-lookup"><span data-stu-id="7645c-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="7645c-110">Ce didacticiel se concentre sur l’utilisation des packages ASP.NET MVC et EF, mais de nombreux autres packages sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="7645c-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="7645c-111">Dans la mesure du possible, je vais créer un lien vers les [documents d’aperçu](http://getglimpse.com/Docs/) appropriés que je vous aide à entretenir.</span><span class="sxs-lookup"><span data-stu-id="7645c-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="7645c-112">L’aperçu est un projet open source, vous pouvez également contribuer au code source et aux documents.</span><span class="sxs-lookup"><span data-stu-id="7645c-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>

- [<span data-ttu-id="7645c-113">Installation de l’aperçu</span><span class="sxs-lookup"><span data-stu-id="7645c-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="7645c-114">Activer l’Aperçu pour localhost</span><span class="sxs-lookup"><span data-stu-id="7645c-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="7645c-115">Onglet chronologie</span><span class="sxs-lookup"><span data-stu-id="7645c-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="7645c-116">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="7645c-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="7645c-117">Itinéraires</span><span class="sxs-lookup"><span data-stu-id="7645c-117">Routes</span></span>](#route)
- [<span data-ttu-id="7645c-118">Utilisation de l’aperçu sur Azure</span><span class="sxs-lookup"><span data-stu-id="7645c-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="7645c-119">Ressources supplémentaires pour MSBuild</span><span class="sxs-lookup"><span data-stu-id="7645c-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="7645c-120">Installation de l’aperçu</span><span class="sxs-lookup"><span data-stu-id="7645c-120">Installing Glimpse</span></span>

<span data-ttu-id="7645c-121">Vous pouvez installer l’aperçu à partir de la console du gestionnaire de package NuGet ou de la console **gérer les packages NuGet** .</span><span class="sxs-lookup"><span data-stu-id="7645c-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="7645c-122">Pour cette démonstration, je vais installer les packages Mvc5 et EF6 :</span><span class="sxs-lookup"><span data-stu-id="7645c-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![installer l’aperçu à partir de NuGet DLG](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="7645c-124">Recherche d' *aperçu. EF*</span><span class="sxs-lookup"><span data-stu-id="7645c-124">Search for *Glimpse.EF*</span></span>

![Aperçu. EF de NuGet install DLG](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="7645c-126">En sélectionnant **packages installés**, vous pouvez voir les modules dépendants d’aperçu installés :</span><span class="sxs-lookup"><span data-stu-id="7645c-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Packages d’aperçu installés à partir de la DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="7645c-128">Les commandes suivantes installent les modules MVC5 et EF6 à partir de la console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="7645c-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="7645c-129">Activer l’Aperçu pour localhost</span><span class="sxs-lookup"><span data-stu-id="7645c-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="7645c-130">Accédez à http://localhost:&lt;p Trier #&gt;/Glimpse.axd, puis cliquez sur le bouton <strong>activer l’aperçu</strong> .</span><span class="sxs-lookup"><span data-stu-id="7645c-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Page d’aperçu axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="7645c-132">Si votre barre de favoris est affichée, vous pouvez glisser-déplacer les boutons d’aperçu et les ajouter en tant que bookmarklets :</span><span class="sxs-lookup"><span data-stu-id="7645c-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE avec les bookmarklets d’aperçu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="7645c-134">Vous pouvez maintenant naviguer dans votre application et l' **affichage des têtes d’affichage** (HUD) s’affiche au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="7645c-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Page du gestionnaire de contacts avec HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="7645c-136">La [page Aperçu HUD](http://getglimpse.com/Docs/Heads-up-Display) détaille les informations de minutage indiquées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="7645c-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="7645c-137">Les données de performances discrètes affichées par HUD peuvent vous informer immédiatement d’un problème, avant d’aller au cycle de test.</span><span class="sxs-lookup"><span data-stu-id="7645c-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="7645c-138">En cliquant sur le &quot;g&quot; dans le coin inférieur droit, vous affichez le panneau d’aperçu :</span><span class="sxs-lookup"><span data-stu-id="7645c-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Panneau d’aperçu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="7645c-140">Dans l’image ci-dessus, l' [onglet exécution](http://getglimpse.com/Docs/Execution-Tab) est sélectionné, qui affiche les détails de la minuterie des actions et des filtres dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="7645c-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="7645c-141">Vous pouvez voir mon [minuteur de filtre Stop Watch](http://www.nuget.org/packages/StopWatch/) démarrer à l’étape 6 du pipeline.</span><span class="sxs-lookup"><span data-stu-id="7645c-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="7645c-142">Bien que mon minuteur clair puisse fournir des données de profil/minutage utiles, il manque tout le temps passé dans l’autorisation et le rendu de la vue.</span><span class="sxs-lookup"><span data-stu-id="7645c-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="7645c-143">Vous pouvez en savoir plus sur mon minuteur au niveau du [profil et l’heure de votre application ASP.NET MVC jusqu’à Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="7645c-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="7645c-144">La page [onglets](http://getglimpse.com/Docs/Tabs) fournit des liens vers des informations détaillées sur chaque onglet.</span><span class="sxs-lookup"><span data-stu-id="7645c-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="7645c-145">Onglet chronologie</span><span class="sxs-lookup"><span data-stu-id="7645c-145">The Timeline tab</span></span>

<span data-ttu-id="7645c-146">J’ai modifié le [didacticiel EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) en suspens de Tom Dykstra avec la modification de code suivante dans le contrôleur des enseignants :</span><span class="sxs-lookup"><span data-stu-id="7645c-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="7645c-147">Le code ci-dessus me permet de transmettre une chaîne de requête (`eager`) pour contrôler le chargement hâtif ou explicite des données.</span><span class="sxs-lookup"><span data-stu-id="7645c-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="7645c-148">Dans l’image ci-dessous, le chargement explicite est utilisé et la page minutage affiche chaque inscription chargée dans le `Index` méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="7645c-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![chargement explicite](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="7645c-150">Dans le code suivant, hâtif est spécifié et chaque inscription est extraite après l’appel de la vue `Index` :</span><span class="sxs-lookup"><span data-stu-id="7645c-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![hâtif est spécifié](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="7645c-152">Vous pouvez pointer sur un segment temporel pour obtenir des informations détaillées sur la temporisation :</span><span class="sxs-lookup"><span data-stu-id="7645c-152">You can hover over a time segment to get detailed timing information:</span></span>

![pointer pour voir le minutage détaillé](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="7645c-154">Liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="7645c-154">Model Binding</span></span>

<span data-ttu-id="7645c-155">L' [onglet liaison de modèle](http://getglimpse.com/Docs/Model-Binding-Tab) fournit une multitude d’informations pour vous aider à comprendre comment vos variables de formulaire sont liées et pourquoi certains ne sont pas liés comme prévu.</span><span class="sxs-lookup"><span data-stu-id="7645c-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="7645c-156">L’image ci-dessous montre les **?**</span><span class="sxs-lookup"><span data-stu-id="7645c-156">The image below shows the **?**</span></span> <span data-ttu-id="7645c-157">, sur laquelle vous pouvez cliquer pour afficher la page d’aide de l’aperçu de cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="7645c-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![vue de liaison du modèle aperçu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="7645c-159">Routes</span><span class="sxs-lookup"><span data-stu-id="7645c-159">Routes</span></span>

 <span data-ttu-id="7645c-160">L’onglet itinéraires d’aperçu peut vous aider à déboguer et à comprendre le routage.</span><span class="sxs-lookup"><span data-stu-id="7645c-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="7645c-161">Dans l’image ci-dessous, l’itinéraire du produit est sélectionné (et il s’affiche en vert, une convention d’aperçu).</span><span class="sxs-lookup"><span data-stu-id="7645c-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="7645c-162">![nom de produit sélectionné](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) les contraintes de routage, les zones et les jetons de données sont également affichés.</span><span class="sxs-lookup"><span data-stu-id="7645c-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="7645c-163">Pour plus d’informations, consultez [itinéraires d’aperçu](http://getglimpse.com/Docs/Routes-Tab) et [routage d’attributs dans ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) .</span><span class="sxs-lookup"><span data-stu-id="7645c-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="7645c-164">Utilisation de l’aperçu sur Azure</span><span class="sxs-lookup"><span data-stu-id="7645c-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="7645c-165">La stratégie de sécurité aperçu par défaut permet d’afficher uniquement les données d’aperçu à partir de l’hôte local.</span><span class="sxs-lookup"><span data-stu-id="7645c-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="7645c-166">Vous pouvez modifier cette stratégie de sécurité afin de pouvoir afficher ces données sur un serveur distant (par exemple, une application Web sur Azure).</span><span class="sxs-lookup"><span data-stu-id="7645c-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="7645c-167">Pour les environnements de test sur Azure, ajoutez la marque mise en surbrillance au bas du fichier *Web. config* pour activer l’aperçu :</span><span class="sxs-lookup"><span data-stu-id="7645c-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.config* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="7645c-168">Avec cette modification seule, n’importe quel utilisateur peut voir vos données d’aperçu sur un site distant.</span><span class="sxs-lookup"><span data-stu-id="7645c-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="7645c-169">Envisagez d’ajouter le balisage ci-dessus à un profil de publication afin qu’il ne soit déployé qu’un appliqué lorsque vous utilisez ce profil de publication (par exemple, votre profil test Azure). Pour limiter les données d’aperçu, nous allons ajouter le rôle `canViewGlimpseData` et autoriser uniquement les utilisateurs de ce rôle à afficher les données d’aperçu.</span><span class="sxs-lookup"><span data-stu-id="7645c-169">Consider adding the markup above to a publish profile so it's only deployed an applied when you use that publish profile (for example, your Azure test profile.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="7645c-170">Supprimez les commentaires du fichier *GlimpseSecurityPolicy.cs* et remplacez l’appel [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) `Administrator` par le rôle `canViewGlimpseData` :</span><span class="sxs-lookup"><span data-stu-id="7645c-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="7645c-171">Sécurité : les données enrichies fournies par l’aperçu peuvent exposer la sécurité de votre application.</span><span class="sxs-lookup"><span data-stu-id="7645c-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="7645c-172">Microsoft n’a pas effectué d’audit de sécurité sur l’Aperçu pour une utilisation sur les applications de production.</span><span class="sxs-lookup"><span data-stu-id="7645c-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>

<span data-ttu-id="7645c-173">Pour plus d’informations sur l’ajout de rôles, consultez le didacticiel mon [déploiement d’une application web ASP.NET MVC 5 sécurisée avec appartenance, OAuth et SQL Database dans Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .</span><span class="sxs-lookup"><span data-stu-id="7645c-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="7645c-174">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7645c-174">Additional Resources</span></span>

- [<span data-ttu-id="7645c-175">Déployer une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et SQL Database à Azure</span><span class="sxs-lookup"><span data-stu-id="7645c-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="7645c-176">[Configuration](http://getglimpse.com/Docs/Configuration) de l’aperçu-page document sur la configuration des onglets, de la stratégie Runtime, de la journalisation, etc.</span><span class="sxs-lookup"><span data-stu-id="7645c-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
