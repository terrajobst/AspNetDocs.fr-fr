---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Helper Twitter avec ASP.NET Web Pages | Microsoft Docs
author: Rick-Anderson
description: Cette rubrique et l’application montrent comment ajouter une application auxiliaire Twitter à votre projet WebMatrix 3. Il contient le code de l’application auxiliaire Twitter et montre comment appeler l’assistance...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 5dda267b146f11355dd94181ef2926e4a304a3ad
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59399006"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="45430-104">Helper Twitter avec ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="45430-104">Twitter Helper with ASP.NET Web Pages</span></span>

<span data-ttu-id="45430-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="45430-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45430-106">Programmes d’assistance Twitter sont obsolètes.</span><span class="sxs-lookup"><span data-stu-id="45430-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="45430-107">Pour les outils de Twitter dernière engagement pour les sites Web, consultez [Twitter pour une vue d’ensemble de sites Web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="45430-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="45430-108">Cette rubrique et l’application montrent comment ajouter une application auxiliaire Twitter à votre projet WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="45430-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="45430-109">Il contient le code de l’application auxiliaire Twitter et montre comment appeler les méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="45430-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="45430-110">Ce code pour le fichier Twitter.cshtml a été développé par **Tian panoramique** de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="45430-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="45430-111">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="45430-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="45430-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="45430-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="45430-113">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="45430-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="45430-114">Introduction</span><span class="sxs-lookup"><span data-stu-id="45430-114">Introduction</span></span>

<span data-ttu-id="45430-115">Cette rubrique montre comment ajouter une application auxiliaire Twitter à votre application et utiliser la syntaxe Razor pour appeler les méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="45430-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="45430-116">L’application auxiliaire Twitter vous permet de facilement incorporer des widgets dans votre application et les boutons de Twitter.</span><span class="sxs-lookup"><span data-stu-id="45430-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="45430-117">Pour utiliser un widget de Twitter, telles que la chronologie d’un utilisateur ou les résultats de recherche pour un mot-dièse, vous devez d’abord créer le [widget sur Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="45430-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="45430-118">Après avoir créé votre widget, vous recevrez un id de widget. Vous passez cet id de widget en tant que paramètre lorsque vous appelez les méthodes d’assistance qui montrent le widget.</span><span class="sxs-lookup"><span data-stu-id="45430-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="45430-119">Cette rubrique a été écrit pour la version 1.1 de l’API Twitter.</span><span class="sxs-lookup"><span data-stu-id="45430-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="45430-120">En ajoutant directement le code de l’application auxiliaire Twitter à votre projet, vous pouvez mettre à jour le code d’assistance si l’API Twitter change.</span><span class="sxs-lookup"><span data-stu-id="45430-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="45430-121">Pour plus d’informations sur l’installation de WebMatrix, consultez [Introducing ASP.NET Web Pages 2 - mise en route](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="45430-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="45430-122">Ajouter l’application auxiliaire Twitter à votre projet</span><span class="sxs-lookup"><span data-stu-id="45430-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="45430-123">Pour ajouter l’application auxiliaire Twitter, tout d’abord, ajoutez un dossier nommé **application\_Code** à votre projet.</span><span class="sxs-lookup"><span data-stu-id="45430-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="45430-124">Ensuite, créez un fichier nommé **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="45430-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Dossier App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="45430-126">Remplacez le code par défaut dans Twitter.cshtml par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="45430-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="45430-127">Appelez les méthodes de Twitter à partir de vos pages web</span><span class="sxs-lookup"><span data-stu-id="45430-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="45430-128">L’exemple suivant montre comment utiliser les méthodes d’assistance de Twitter à partir d’une page dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="45430-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="45430-129">Dans votre projet, vous devez remplacer les valeurs des paramètres avec des valeurs qui correspondent à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="45430-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="45430-130">Vous pouvez utiliser les ID de widget fourni pour Explorer le fonctionnement des méthodes, mais vous pouvez générer vos propres widgets pour votre projet.</span><span class="sxs-lookup"><span data-stu-id="45430-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="45430-131">Tous les paramètres indiqués ci-dessous sont requis.</span><span class="sxs-lookup"><span data-stu-id="45430-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="45430-132">Les paramètres facultatifs sont utilisés pour personnaliser la façon dont le bouton ou un widget s’affiche.</span><span class="sxs-lookup"><span data-stu-id="45430-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="45430-133">Par exemple, le bouton suivre requiert uniquement le nom d’utilisateur à suivre, mais l’exemple montre comment inclure le nombre d’abonnés et comment spécifier la taille du bouton et la langue.</span><span class="sxs-lookup"><span data-stu-id="45430-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="45430-134">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="45430-134">See the results</span></span>

<span data-ttu-id="45430-135">Le code ci-dessus génère les boutons et les widgets suivants.</span><span class="sxs-lookup"><span data-stu-id="45430-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="45430-136">Ces boutons et les widgets sont entièrement fonctionnelles, pas des captures d’écran.</span><span class="sxs-lookup"><span data-stu-id="45430-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="45430-137">Le bouton suivre est affiché en espagnol, car le paramètre de langue a été défini sur **es**.</span><span class="sxs-lookup"><span data-stu-id="45430-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="45430-138">Suivez le bouton</span><span class="sxs-lookup"><span data-stu-id="45430-138">Follow Button</span></span>

[<span data-ttu-id="45430-139">Suivez @aspnet)</span><span class="sxs-lookup"><span data-stu-id="45430-139">Follow @aspnet)</span></span>](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a><span data-ttu-id="45430-140">Bouton de tweet</span><span class="sxs-lookup"><span data-stu-id="45430-140">Tweet Button</span></span>

[<span data-ttu-id="45430-141">Tweet</span><span class="sxs-lookup"><span data-stu-id="45430-141">Tweet</span></span>](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a><span data-ttu-id="45430-142">Chronologie de l’utilisateur (profil)</span><span class="sxs-lookup"><span data-stu-id="45430-142">User Timeline (Profile)</span></span>

[<span data-ttu-id="45430-143">Tweets par @aspnet</span><span class="sxs-lookup"><span data-stu-id="45430-143">Tweets by @aspnet</span></span>](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a><span data-ttu-id="45430-144">Favoris</span><span class="sxs-lookup"><span data-stu-id="45430-144">Favorites</span></span>

[<span data-ttu-id="45430-145">Favoris Tweets par @Microsoft</span><span class="sxs-lookup"><span data-stu-id="45430-145">Favorite Tweets by @Microsoft</span></span>](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a><span data-ttu-id="45430-146">Liste</span><span class="sxs-lookup"><span data-stu-id="45430-146">List</span></span>

[<span data-ttu-id="45430-147">À partir des tweets @Microsoft/MS \_consommateur\_bandes</span><span class="sxs-lookup"><span data-stu-id="45430-147">Tweets from @Microsoft/MS\_Consumer\_Bands</span></span>](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a><span data-ttu-id="45430-148">Rechercher</span><span class="sxs-lookup"><span data-stu-id="45430-148">Search</span></span>

[<span data-ttu-id="45430-149">Un Tweet sur &quot;#asp.net&quot;</span><span class="sxs-lookup"><span data-stu-id="45430-149">Tweets about &quot;#asp.net&quot;</span></span>](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
