---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Application auxiliaire Twitter avec pages Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Cette rubrique et cette application montrent comment ajouter un programme d’assistance Twitter à votre projet WebMatrix 3. Il contient le code d’assistance Twitter et montre comment appeler le programme d’assistance...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638557"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="2a25f-104">Helper Twitter avec ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="2a25f-104">Twitter Helper with ASP.NET Web Pages</span></span>

<span data-ttu-id="2a25f-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2a25f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a25f-106">Les applications d’assistance Twitter sont obsolètes.</span><span class="sxs-lookup"><span data-stu-id="2a25f-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="2a25f-107">Pour obtenir les derniers outils d’engagement de Twitter pour les sites Web, consultez [vue d’ensemble de Twitter pour les sites Web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="2a25f-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="2a25f-108">Cette rubrique et cette application montrent comment ajouter un programme d’assistance Twitter à votre projet WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="2a25f-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="2a25f-109">Il contient le code d’assistance Twitter et montre comment appeler les méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="2a25f-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="2a25f-110">Ce code pour le fichier Twitter. cshtml a été développé par **Tian Pan** de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2a25f-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2a25f-111">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="2a25f-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2a25f-112">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2a25f-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2a25f-113">Ce didacticiel fonctionne également avec pages Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="2a25f-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="introduction"></a><span data-ttu-id="2a25f-114">Introduction</span><span class="sxs-lookup"><span data-stu-id="2a25f-114">Introduction</span></span>

<span data-ttu-id="2a25f-115">Cette rubrique montre comment ajouter un programme d’assistance Twitter à votre application et utiliser syntaxe Razor pour appeler les méthodes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="2a25f-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="2a25f-116">Le programme d’assistance Twitter facilite l’incorporation de boutons et de widgets Twitter dans votre application.</span><span class="sxs-lookup"><span data-stu-id="2a25f-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="2a25f-117">Pour utiliser un widget Twitter, tel que la chronologie d’un utilisateur ou les résultats de la recherche pour un mot-dièse, vous devez d’abord créer le [widget sur Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="2a25f-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="2a25f-118">Après avoir créé votre widget, vous recevrez un ID de widget. Vous transmettez cet ID de widget en tant que paramètre lors de l’appel des méthodes d’assistance qui affichent le widget.</span><span class="sxs-lookup"><span data-stu-id="2a25f-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="2a25f-119">Cette rubrique a été écrite pour la version 1,1 de l’API Twitter.</span><span class="sxs-lookup"><span data-stu-id="2a25f-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="2a25f-120">En ajoutant directement le code d’assistance Twitter à votre projet, vous pouvez mettre à jour le code d’assistance Si l’API Twitter change.</span><span class="sxs-lookup"><span data-stu-id="2a25f-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="2a25f-121">Pour plus d’informations sur l’installation de WebMatrix, consultez [Présentation de pages Web ASP.NET 2-prise en main](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="2a25f-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="2a25f-122">Ajouter l’assistance Twitter à votre projet</span><span class="sxs-lookup"><span data-stu-id="2a25f-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="2a25f-123">Pour ajouter le programme d’assistance Twitter, commencez par ajouter un dossier nommé **App\_code** à votre projet.</span><span class="sxs-lookup"><span data-stu-id="2a25f-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="2a25f-124">Créez ensuite un fichier nommé **Twitter. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="2a25f-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Dossier App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="2a25f-126">Remplacez le code par défaut dans Twitter. cshtml par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="2a25f-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="2a25f-127">Appeler des méthodes Twitter à partir de vos pages Web</span><span class="sxs-lookup"><span data-stu-id="2a25f-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="2a25f-128">L’exemple suivant montre comment utiliser les méthodes d’assistance Twitter à partir d’une page de votre projet.</span><span class="sxs-lookup"><span data-stu-id="2a25f-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="2a25f-129">Dans votre projet, vous souhaiterez remplacer les valeurs de paramètre par des valeurs qui correspondent à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="2a25f-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="2a25f-130">Vous pouvez utiliser les ID de widget fournis pour explorer le fonctionnement des méthodes, mais vous souhaiterez générer vos propres widgets pour votre projet.</span><span class="sxs-lookup"><span data-stu-id="2a25f-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="2a25f-131">Tous les paramètres indiqués ci-dessous ne sont pas requis.</span><span class="sxs-lookup"><span data-stu-id="2a25f-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="2a25f-132">Les paramètres facultatifs sont utilisés pour personnaliser l’affichage du bouton ou du widget.</span><span class="sxs-lookup"><span data-stu-id="2a25f-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="2a25f-133">Par exemple, le bouton suivant nécessite uniquement le nom d’utilisateur, mais l’exemple montre comment inclure le nombre d’abonnés et comment spécifier la taille du bouton et de la langue.</span><span class="sxs-lookup"><span data-stu-id="2a25f-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="2a25f-134">Consulter les résultats</span><span class="sxs-lookup"><span data-stu-id="2a25f-134">See the results</span></span>

<span data-ttu-id="2a25f-135">Le code ci-dessus produit les boutons et les widgets suivants.</span><span class="sxs-lookup"><span data-stu-id="2a25f-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="2a25f-136">Ces boutons et widgets sont des captures d’écran entièrement fonctionnelles.</span><span class="sxs-lookup"><span data-stu-id="2a25f-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="2a25f-137">Le bouton suivant est affiché en espagnol, car le paramètre de langue était défini sur **es**.</span><span class="sxs-lookup"><span data-stu-id="2a25f-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="2a25f-138">Bouton suivant</span><span class="sxs-lookup"><span data-stu-id="2a25f-138">Follow Button</span></span>

<span data-ttu-id="2a25f-139">[Suivez @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="2a25f-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="2a25f-140">Bouton Tweet</span><span class="sxs-lookup"><span data-stu-id="2a25f-140">Tweet Button</span></span>

<span data-ttu-id="2a25f-141">`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>` [Tweet](https://twitter.com/share)</span><span class="sxs-lookup"><span data-stu-id="2a25f-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="2a25f-142">Chronologie de l’utilisateur (profil)</span><span class="sxs-lookup"><span data-stu-id="2a25f-142">User Timeline (Profile)</span></span>

<span data-ttu-id="2a25f-143">[Tweets par @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2a25f-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="2a25f-144">Favoris</span><span class="sxs-lookup"><span data-stu-id="2a25f-144">Favorites</span></span>

<span data-ttu-id="2a25f-145">Les [tweets préférés par @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2a25f-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="2a25f-146">List</span><span class="sxs-lookup"><span data-stu-id="2a25f-146">List</span></span>

<span data-ttu-id="2a25f-147">[Tweets à partir de @Microsoft/MS\_des bandes\_de consommateurs](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2a25f-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="2a25f-148">Rechercher</span><span class="sxs-lookup"><span data-stu-id="2a25f-148">Search</span></span>

<span data-ttu-id="2a25f-149">[Tweets sur &quot;#asp&quot;.net](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2a25f-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
