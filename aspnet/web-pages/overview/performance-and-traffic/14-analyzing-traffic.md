---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Suivi des informations du visiteur (analytique) pour un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Une fois que vous avez obtenu votre site Web, vous souhaiterez peut-être analyser le trafic de votre site Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525185"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="f660a-103">Suivi des informations du visiteur (analytique) pour un site pages Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="f660a-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="f660a-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f660a-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f660a-105">Cet article explique comment utiliser une application d’assistance pour ajouter des analyses de site Web aux pages d’un site Web pages Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="f660a-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="f660a-106">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="f660a-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="f660a-107">Comment envoyer des informations sur le trafic de votre site Web à un fournisseur d’analyse.</span><span class="sxs-lookup"><span data-stu-id="f660a-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="f660a-108">Voici les fonctionnalités de programmation ASP.NET présentées dans l’article :</span><span class="sxs-lookup"><span data-stu-id="f660a-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="f660a-109">Le programme d’assistance `Analytics`.</span><span class="sxs-lookup"><span data-stu-id="f660a-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f660a-110">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="f660a-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f660a-111">Pages Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="f660a-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="f660a-112">Bibliothèque d’applications auxiliaires Web ASP.NET (package NuGet)</span><span class="sxs-lookup"><span data-stu-id="f660a-112">ASP.NET Web Helpers Library (NuGet package)</span></span>

<span data-ttu-id="f660a-113">Analytics est un terme général pour les technologies qui mesure le trafic sur votre site Web, ce qui vous permet de comprendre comment les utilisateurs utilisent le site.</span><span class="sxs-lookup"><span data-stu-id="f660a-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="f660a-114">De nombreux services d’analyse sont disponibles, y compris les services de Google, Yahoo, StatCounter et d’autres.</span><span class="sxs-lookup"><span data-stu-id="f660a-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="f660a-115">Le fonctionnement d’Analytics est que vous vous inscrivez à un compte avec le fournisseur Analytics, où vous inscrivez le site dont vous souhaitez effectuer le suivi. Le fournisseur vous envoie un extrait de code JavaScript qui comprend un ID ou un code de suivi pour votre compte.</span><span class="sxs-lookup"><span data-stu-id="f660a-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="f660a-116">Vous ajoutez l’extrait de code JavaScript aux pages Web sur le site dont vous souhaitez effectuer le suivi. (En général, vous ajoutez l’extrait de code Analytics à un pied de page ou à une page de disposition ou à une autre balise HTML qui apparaît sur chaque page de votre site.) Lorsque les utilisateurs demandent une page contenant l’un de ces extraits de code JavaScript, l’extrait de code envoie des informations sur la page actuelle au fournisseur d’analyse, qui enregistre divers détails sur la page.</span><span class="sxs-lookup"><span data-stu-id="f660a-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="f660a-117">Lorsque vous souhaitez consulter les statistiques de votre site, vous vous connectez au site Web du fournisseur d’analyse.</span><span class="sxs-lookup"><span data-stu-id="f660a-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="f660a-118">Vous pouvez ensuite afficher toutes sortes de rapports sur votre site, par exemple :</span><span class="sxs-lookup"><span data-stu-id="f660a-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="f660a-119">Nombre de consultations de page pour des pages individuelles.</span><span class="sxs-lookup"><span data-stu-id="f660a-119">The number of page views for individual pages.</span></span> <span data-ttu-id="f660a-120">Cela vous indique (à peu près) le nombre de personnes visitant le site et les pages de votre site les plus populaires.</span><span class="sxs-lookup"><span data-stu-id="f660a-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="f660a-121">La durée pendant laquelle les utilisateurs consacrent des pages spécifiques.</span><span class="sxs-lookup"><span data-stu-id="f660a-121">How long people spend on specific pages.</span></span> <span data-ttu-id="f660a-122">Cela peut vous indiquer si votre page d’hébergement est l’intérêt des gens.</span><span class="sxs-lookup"><span data-stu-id="f660a-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="f660a-123">Les sites sur lesquels les utilisateurs se trouvaient avant la visite de votre site.</span><span class="sxs-lookup"><span data-stu-id="f660a-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="f660a-124">Cela vous permet de savoir si votre trafic provient de liens, de recherches, etc.</span><span class="sxs-lookup"><span data-stu-id="f660a-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="f660a-125">Lorsque les utilisateurs visitent votre site et leur durée de séjour.</span><span class="sxs-lookup"><span data-stu-id="f660a-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="f660a-126">Les pays d’où proviennent vos visiteurs.</span><span class="sxs-lookup"><span data-stu-id="f660a-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="f660a-127">Les navigateurs et systèmes d’exploitation que vos visiteurs utilisent.</span><span class="sxs-lookup"><span data-stu-id="f660a-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="f660a-129">Utilisation d’une application d’assistance pour ajouter Analytics à une page</span><span class="sxs-lookup"><span data-stu-id="f660a-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="f660a-130">Pages Web ASP.NET comprend plusieurs applications auxiliaires d’analyse (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`et `Analytics.GetStatCounterHtml`) qui facilitent la gestion des extraits de code JavaScript utilisés pour l’analyse.</span><span class="sxs-lookup"><span data-stu-id="f660a-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="f660a-131">Au lieu de déterminer comment et où placer le code JavaScript, il vous suffit d’ajouter l’application auxiliaire à une page.</span><span class="sxs-lookup"><span data-stu-id="f660a-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="f660a-132">Les seules informations que vous devez fournir sont le nom de votre compte, votre ID ou votre code de suivi.</span><span class="sxs-lookup"><span data-stu-id="f660a-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="f660a-133">(Pour StatCounter, vous devez également fournir quelques valeurs supplémentaires.)</span><span class="sxs-lookup"><span data-stu-id="f660a-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="f660a-134">Dans cette procédure, vous allez créer une page de disposition qui utilise le programme d’assistance `GetGoogleHtml`.</span><span class="sxs-lookup"><span data-stu-id="f660a-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="f660a-135">Si vous disposez déjà d’un compte avec l’un des autres fournisseurs d’analyse, vous pouvez utiliser ce compte à la place et effectuer de légers ajustements en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="f660a-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="f660a-136">Lorsque vous créez un compte Analytics, vous enregistrez l’URL du site que vous souhaitez suivre.</span><span class="sxs-lookup"><span data-stu-id="f660a-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="f660a-137">Si vous testez tout sur votre ordinateur local, vous ne suivez pas le trafic réel (le seul trafic est vous), vous ne pourrez donc pas enregistrer et afficher les statistiques de site.</span><span class="sxs-lookup"><span data-stu-id="f660a-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="f660a-138">Toutefois, cette procédure montre comment ajouter une application auxiliaire Analytics à une page.</span><span class="sxs-lookup"><span data-stu-id="f660a-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="f660a-139">Lorsque vous publiez votre site, le site actif envoie des informations à votre fournisseur d’analyse.</span><span class="sxs-lookup"><span data-stu-id="f660a-139">When you publish your site, the live site will send information to your analytics provider.</span></span>

1. <span data-ttu-id="f660a-140">Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas encore fait.</span><span class="sxs-lookup"><span data-stu-id="f660a-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="f660a-141">Créez un compte avec Google Analytics et enregistrez le nom du compte.</span><span class="sxs-lookup"><span data-stu-id="f660a-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="f660a-142">Créez une page de disposition nommée *Analytics. cshtml* et ajoutez le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="f660a-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="f660a-143">Vous devez placer l’appel au programme d’assistance `Analytics` dans le corps de votre page Web (avant la balise `</body>`).</span><span class="sxs-lookup"><span data-stu-id="f660a-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="f660a-144">Dans le cas contraire, le navigateur n’exécutera pas le script.</span><span class="sxs-lookup"><span data-stu-id="f660a-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="f660a-145">Si vous utilisez un autre fournisseur d’analyse, utilisez plutôt l’une des applications d’assistance suivantes :</span><span class="sxs-lookup"><span data-stu-id="f660a-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="f660a-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="f660a-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="f660a-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="f660a-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="f660a-148">Remplacez `myaccount` par le nom du compte, de l’ID ou du code de suivi que vous avez créé à l’étape 1.</span><span class="sxs-lookup"><span data-stu-id="f660a-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="f660a-149">Exécutez la page dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="f660a-149">Run the page in the browser.</span></span> <span data-ttu-id="f660a-150">(Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.)</span><span class="sxs-lookup"><span data-stu-id="f660a-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="f660a-151">Dans le navigateur, affichez la page source.</span><span class="sxs-lookup"><span data-stu-id="f660a-151">In the browser, view the page source.</span></span> <span data-ttu-id="f660a-152">Vous pouvez voir le code d’analyse rendu :</span><span class="sxs-lookup"><span data-stu-id="f660a-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="f660a-153">Connectez-vous au site Google Analytics et examinez les statistiques de votre site.</span><span class="sxs-lookup"><span data-stu-id="f660a-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="f660a-154">Si vous exécutez la page sur un site actif, vous voyez une entrée qui enregistre la visite sur votre page.</span><span class="sxs-lookup"><span data-stu-id="f660a-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="f660a-155">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f660a-155">Additional Resources</span></span>

- [<span data-ttu-id="f660a-156">Site Google Analytics</span><span class="sxs-lookup"><span data-stu-id="f660a-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="f660a-157">Site Web Analytics Yahoo !</span><span class="sxs-lookup"><span data-stu-id="f660a-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="f660a-158">Site StatCounter</span><span class="sxs-lookup"><span data-stu-id="f660a-158">StatCounter site</span></span>](http://statcounter.com/)
