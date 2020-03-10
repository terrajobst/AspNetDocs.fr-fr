---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Création d’URL lisibles dans des sites pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article décrit le routage dans un site Web pages Web ASP.NET (Razor) et explique comment cela vous permet d’utiliser des URL plus lisibles et mieux adaptées à SEO. Ce que vous allez faire...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628393"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="66c3b-104">Création d’URL lisibles dans des sites pages Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="66c3b-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="66c3b-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="66c3b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="66c3b-106">Cet article décrit le routage dans un site Web pages Web ASP.NET (Razor) et explique comment cela vous permet d’utiliser des URL plus lisibles et mieux adaptées à SEO.</span><span class="sxs-lookup"><span data-stu-id="66c3b-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="66c3b-107">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="66c3b-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="66c3b-108">Comment ASP.NET utilise le routage pour vous permettre d’utiliser des URL plus lisibles et pouvant faire l’objet d’une recherche.</span><span class="sxs-lookup"><span data-stu-id="66c3b-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="66c3b-109">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="66c3b-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="66c3b-110">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="66c3b-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="66c3b-111">Ce didacticiel fonctionne également avec pages Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="66c3b-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="about-routing"></a><span data-ttu-id="66c3b-112">À propos du routage</span><span class="sxs-lookup"><span data-stu-id="66c3b-112">About Routing</span></span>

<span data-ttu-id="66c3b-113">Les URL des pages de votre site peuvent avoir un impact sur le fonctionnement du site.</span><span class="sxs-lookup"><span data-stu-id="66c3b-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="66c3b-114">Une URL &quot;&quot; conviviale peut faciliter l’utilisation du site par les personnes.</span><span class="sxs-lookup"><span data-stu-id="66c3b-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="66c3b-115">Il peut également être utile avec l’optimisation du moteur de recherche (SEO) pour le site.</span><span class="sxs-lookup"><span data-stu-id="66c3b-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="66c3b-116">Les sites Web ASP.NET incluent la possibilité d’utiliser automatiquement des URL conviviales.</span><span class="sxs-lookup"><span data-stu-id="66c3b-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="66c3b-117">ASP.NET vous permet de créer des URL explicites qui décrivent les actions des utilisateurs au lieu de simplement pointer vers un fichier sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="66c3b-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="66c3b-118">Considérez ces URL pour un blog fictif :</span><span class="sxs-lookup"><span data-stu-id="66c3b-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="66c3b-119">Comparez ces URL aux éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="66c3b-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="66c3b-120">Dans la première paire, un utilisateur doit savoir que le blog s’affiche à l’aide de la page *blog. cshtml* , puis créer une chaîne de requête qui obtient la bonne catégorie ou plage de dates.</span><span class="sxs-lookup"><span data-stu-id="66c3b-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="66c3b-121">Le deuxième ensemble d’exemples est beaucoup plus facile à comprendre et à créer.</span><span class="sxs-lookup"><span data-stu-id="66c3b-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="66c3b-122">Les URL du premier exemple pointent également directement vers un fichier spécifique (*blog. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="66c3b-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="66c3b-123">Si, pour une raison quelconque, le blog a été déplacé vers un autre dossier sur le serveur, ou si le blog a été réécrit pour utiliser une autre page, les liens seraient erronés.</span><span class="sxs-lookup"><span data-stu-id="66c3b-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="66c3b-124">Le deuxième ensemble d’URL ne pointe pas vers une page spécifique. ainsi, même si l’implémentation du blog ou l’emplacement change, les URL sont toujours valides.</span><span class="sxs-lookup"><span data-stu-id="66c3b-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="66c3b-125">Dans pages Web ASP.NET, vous pouvez créer des URL plus conviviales comme celles des exemples ci-dessus, car ASP.NET utilise le *routage*.</span><span class="sxs-lookup"><span data-stu-id="66c3b-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="66c3b-126">Le routage crée un mappage logique à partir d’une URL vers une page (ou des pages) qui peut traiter la demande.</span><span class="sxs-lookup"><span data-stu-id="66c3b-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="66c3b-127">Étant donné que le mappage est logique (pas physique, vers un fichier spécifique), le routage offre une grande flexibilité dans la façon dont vous définissez les URL de votre site.</span><span class="sxs-lookup"><span data-stu-id="66c3b-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="66c3b-128">Fonctionnement du routage</span><span class="sxs-lookup"><span data-stu-id="66c3b-128">How Routing Works</span></span>

<span data-ttu-id="66c3b-129">Quand ASP.NET traite une requête, il lit l’URL pour déterminer comment l’acheminer.</span><span class="sxs-lookup"><span data-stu-id="66c3b-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="66c3b-130">ASP.NET tente de faire correspondre des segments individuels de l’URL à des fichiers sur le disque, en allant de gauche à droite.</span><span class="sxs-lookup"><span data-stu-id="66c3b-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="66c3b-131">S’il y a une correspondance, tout ce qui reste dans l’URL est passé à la page en tant qu' *informations de chemin d’accès*.</span><span class="sxs-lookup"><span data-stu-id="66c3b-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="66c3b-132">Imaginez qu’une personne effectue une demande à l’aide de cette URL :</span><span class="sxs-lookup"><span data-stu-id="66c3b-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="66c3b-133">La recherche se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="66c3b-133">The search goes like this:</span></span>

1. <span data-ttu-id="66c3b-134">Existe-t-il un fichier avec le chemin d’accès et le nom de */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="66c3b-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="66c3b-135">Dans ce cas, exécutez cette page et ne lui transmettez aucune information.</span><span class="sxs-lookup"><span data-stu-id="66c3b-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="66c3b-136">Sinon...</span><span class="sxs-lookup"><span data-stu-id="66c3b-136">Otherwise ...</span></span>
2. <span data-ttu-id="66c3b-137">Existe-t-il un fichier avec le chemin d’accès et le nom de */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="66c3b-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="66c3b-138">Dans ce cas, exécutez cette page et transmettez la valeur `c` à celle-ci.</span><span class="sxs-lookup"><span data-stu-id="66c3b-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="66c3b-139">Sinon...</span><span class="sxs-lookup"><span data-stu-id="66c3b-139">Otherwise …</span></span>
3. <span data-ttu-id="66c3b-140">Existe-t-il un fichier avec le chemin d’accès et le nom de */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="66c3b-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="66c3b-141">Dans ce cas, exécutez cette page et transmettez la valeur `b/c` à celle-ci.</span><span class="sxs-lookup"><span data-stu-id="66c3b-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="66c3b-142">Si la recherche n’a trouvé aucune correspondance exacte pour les fichiers *. cshtml* dans leurs dossiers spécifiés, ASP.NET continue à rechercher ces fichiers à tour de fonction :</span><span class="sxs-lookup"><span data-stu-id="66c3b-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="66c3b-143">*/a/b/c/default.cshtml* (aucune information sur le chemin d’accès).</span><span class="sxs-lookup"><span data-stu-id="66c3b-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="66c3b-144">*/a/b/c/index.cshtml* (aucune information sur le chemin d’accès).</span><span class="sxs-lookup"><span data-stu-id="66c3b-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="66c3b-145">Pour être clair, les demandes de pages spécifiques (autrement dit, les demandes incluant l’extension de nom de fichier *. cshtml* ) fonctionnent comme prévu.</span><span class="sxs-lookup"><span data-stu-id="66c3b-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="66c3b-146">Une demande comme `http://www.contoso.com/a/b.cshtml` exécutera la page *b. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="66c3b-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>

<span data-ttu-id="66c3b-147">À l’intérieur d’une page, vous pouvez obtenir les informations de chemin d’accès via la propriété `UrlData` de la page, qui est un dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="66c3b-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="66c3b-148">Imaginez que vous avez un fichier nommé *ViewCustomers. cshtml* et que votre site reçoit cette demande :</span><span class="sxs-lookup"><span data-stu-id="66c3b-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="66c3b-149">Comme décrit dans les règles ci-dessus, la demande est envoyée à votre page.</span><span class="sxs-lookup"><span data-stu-id="66c3b-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="66c3b-150">À l’intérieur de la page, vous pouvez utiliser un code semblable au suivant pour obtenir et afficher les informations relatives au chemin (dans ce cas, la valeur &quot;1000&quot;) :</span><span class="sxs-lookup"><span data-stu-id="66c3b-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="66c3b-151">Étant donné que le routage n’implique pas de noms de fichiers complets, il peut y avoir une ambiguïté si vous avez des pages qui ont le même nom mais des extensions de nom de fichier différentes (par exemple, *MyPage. cshtml* et *MyPage. html*).</span><span class="sxs-lookup"><span data-stu-id="66c3b-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="66c3b-152">Afin d’éviter les problèmes de routage, il est préférable de s’assurer que vous n’avez pas de pages dans votre site dont les noms diffèrent uniquement par leur extension.</span><span class="sxs-lookup"><span data-stu-id="66c3b-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="66c3b-153">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="66c3b-153">Additional Resources</span></span>

<span data-ttu-id="66c3b-154">[WebMatrix : URL, URLData et routage pour SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="66c3b-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="66c3b-155">Cette entrée de blog de Mike salée fournit des informations supplémentaires sur le fonctionnement du routage dans pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="66c3b-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
