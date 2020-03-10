---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Affichage des cartes dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment afficher des cartes interactives sur les pages d’un site Web pages Web ASP.NET (Razor) en fonction des services de mappage fournis par Bing, Google, ma...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638676"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="d0b18-103">Affichage des cartes dans un site pages Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="d0b18-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="d0b18-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d0b18-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d0b18-105">Cet article explique comment afficher des cartes interactives sur les pages d’un site Web pages Web ASP.NET (Razor) en fonction des services de mappage fournis par Bing, Google, MapQuest et Yahoo.</span><span class="sxs-lookup"><span data-stu-id="d0b18-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="d0b18-106">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="d0b18-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d0b18-107">Comment générer un mappage basé sur une adresse.</span><span class="sxs-lookup"><span data-stu-id="d0b18-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="d0b18-108">Comment générer une carte en fonction des coordonnées de latitude et de longitude.</span><span class="sxs-lookup"><span data-stu-id="d0b18-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="d0b18-109">Comment inscrire un compte de développeur Bing Maps et obtenir une clé à utiliser avec Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="d0b18-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="d0b18-110">Il s’agit de la fonctionnalité ASP.NET introduite dans l’article :</span><span class="sxs-lookup"><span data-stu-id="d0b18-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="d0b18-111">Le programme d’assistance `Maps`.</span><span class="sxs-lookup"><span data-stu-id="d0b18-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d0b18-112">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="d0b18-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d0b18-113">Pages Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="d0b18-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="d0b18-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="d0b18-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="d0b18-115">Ce didacticiel fonctionne également avec WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="d0b18-115">This tutorial also works with WebMatrix 3.</span></span>

<span data-ttu-id="d0b18-116">Dans les pages Web, vous pouvez afficher les cartes sur une page à l’aide d' `Maps` Helper.</span><span class="sxs-lookup"><span data-stu-id="d0b18-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="d0b18-117">Vous pouvez générer des mappages basés sur une adresse ou sur un ensemble de coordonnées de longitude et de latitude.</span><span class="sxs-lookup"><span data-stu-id="d0b18-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="d0b18-118">La classe `Maps` vous permet d’appeler des moteurs de cartes populaires, notamment Bing, Google, MapQuest et Yahoo.</span><span class="sxs-lookup"><span data-stu-id="d0b18-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="d0b18-119">Les étapes d’ajout d’un mappage à une page sont les mêmes, quel que soit le moteur de carte que vous appelez.</span><span class="sxs-lookup"><span data-stu-id="d0b18-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="d0b18-120">Il vous suffit d’ajouter une référence de fichier JavaScript qui met à disposition les méthodes permettant d’afficher le mappage, puis d’appeler les méthodes du programme d’assistance `Maps`.</span><span class="sxs-lookup"><span data-stu-id="d0b18-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="d0b18-121">Vous choisissez un service de mappage basé sur les `Maps` méthode d’assistance que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="d0b18-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="d0b18-122">Vous pouvez utiliser l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="d0b18-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="d0b18-123">Installation des composants dont vous avez besoin</span><span class="sxs-lookup"><span data-stu-id="d0b18-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="d0b18-124">Pour afficher les mappages, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="d0b18-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="d0b18-125">Le programme d’assistance `Maps`.</span><span class="sxs-lookup"><span data-stu-id="d0b18-125">The `Maps` helper.</span></span> <span data-ttu-id="d0b18-126">Cette application auxiliaire est dans la version 2 de la bibliothèque d’applications auxiliaires Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d0b18-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="d0b18-127">Si vous n’avez pas encore ajouté la bibliothèque, vous pouvez l’installer dans votre site en tant que package NuGet.</span><span class="sxs-lookup"><span data-stu-id="d0b18-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="d0b18-128">Pour plus d’informations, consultez [installation des applications d’assistance dans un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="d0b18-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="d0b18-129">(Dans la Galerie, recherchez le package `microsoft-web-helpers`.)</span><span class="sxs-lookup"><span data-stu-id="d0b18-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="d0b18-130">Bibliothèque jQuery.</span><span class="sxs-lookup"><span data-stu-id="d0b18-130">The jQuery library.</span></span> <span data-ttu-id="d0b18-131">Plusieurs modèles de site WebMatrix incluent déjà des bibliothèques jQuery dans leurs dossiers de *script* .</span><span class="sxs-lookup"><span data-stu-id="d0b18-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="d0b18-132">Si vous ne disposez pas de ces bibliothèques, vous pouvez télécharger la dernière bibliothèque jQuery directement à partir du site [jQuery.org](http://jQuery.org) .</span><span class="sxs-lookup"><span data-stu-id="d0b18-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="d0b18-133">Ou vous pouvez créer un nouveau site à l’aide d’un modèle (par exemple, le modèle **Starter Site** ), puis copier les fichiers jQuery à partir de ce site vers votre site actuel.</span><span class="sxs-lookup"><span data-stu-id="d0b18-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="d0b18-134">Enfin, si vous souhaitez utiliser Bing Maps, vous devez d’abord créer un compte (gratuit) et obtenir une clé.</span><span class="sxs-lookup"><span data-stu-id="d0b18-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="d0b18-135">Pour obtenir une clé, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="d0b18-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="d0b18-136">Créez un compte sur le [compte de développeur Bing Maps](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="d0b18-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="d0b18-137">Vous devez également disposer d’un compte Microsoft (Windows Live ID).</span><span class="sxs-lookup"><span data-stu-id="d0b18-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="d0b18-138">Vous pouvez spécifier que vous souhaitez utiliser la clé à des fins d' **évaluation et de test**.</span><span class="sxs-lookup"><span data-stu-id="d0b18-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="d0b18-139">Si vous testez la fonction de mappage sur votre propre ordinateur à l’aide de WebMatrix et IIS Express, accédez à l’espace de travail du **site** et notez l’URL de votre site (par exemple, `http://localhost:50408`, bien que votre numéro de port soit probablement différent).</span><span class="sxs-lookup"><span data-stu-id="d0b18-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="d0b18-140">Vous pouvez utiliser cette adresse *localhost* comme site lorsque vous vous inscrivez.</span><span class="sxs-lookup"><span data-stu-id="d0b18-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="d0b18-141">Une fois que vous êtes inscrit à un compte, accédez au centre des comptes Bing Maps, puis cliquez sur **créer ou afficher les clés**:</span><span class="sxs-lookup"><span data-stu-id="d0b18-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mappage-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="d0b18-143">Enregistrez la clé créée par Bing.</span><span class="sxs-lookup"><span data-stu-id="d0b18-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="d0b18-144">Création d’une carte basée sur une adresse (à l’aide de Google)</span><span class="sxs-lookup"><span data-stu-id="d0b18-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="d0b18-145">L’exemple suivant montre comment créer une page qui restitue un mappage basé sur une adresse.</span><span class="sxs-lookup"><span data-stu-id="d0b18-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="d0b18-146">Cet exemple montre comment utiliser Google Maps.</span><span class="sxs-lookup"><span data-stu-id="d0b18-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="d0b18-147">Créez un fichier nommé *mapaddress. cshtml* à la racine du site.</span><span class="sxs-lookup"><span data-stu-id="d0b18-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="d0b18-148">Cette page génère un mappage basé sur une adresse que vous lui transmettez.</span><span class="sxs-lookup"><span data-stu-id="d0b18-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="d0b18-149">Copiez le code suivant dans le fichier, en remplaçant le contenu existant.</span><span class="sxs-lookup"><span data-stu-id="d0b18-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="d0b18-150">Notez les fonctionnalités suivantes de la page :</span><span class="sxs-lookup"><span data-stu-id="d0b18-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="d0b18-151">Élément `<script>` dans l’élément `<head>`.</span><span class="sxs-lookup"><span data-stu-id="d0b18-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="d0b18-152">Dans l’exemple, l’élément `<script>` fait référence au fichier *jQuery-1.6.4. min. js* , qui est une version minimisés (compressée) de la bibliothèque jQuery, version 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="d0b18-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="d0b18-153">Notez que la référence suppose que le fichier *. js* se trouve dans le dossier *scripts* de votre site.</span><span class="sxs-lookup"><span data-stu-id="d0b18-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="d0b18-154">Si vous utilisez une autre version de la bibliothèque jQuery, assurez-vous simplement que vous pointez correctement sur cette version.</span><span class="sxs-lookup"><span data-stu-id="d0b18-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="d0b18-155">L’appel à la `@Maps.GetGoogleHtml` dans le corps de la page.</span><span class="sxs-lookup"><span data-stu-id="d0b18-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="d0b18-156">Pour mapper une adresse, vous devez passer une chaîne d’adresse.</span><span class="sxs-lookup"><span data-stu-id="d0b18-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="d0b18-157">Les méthodes pour les autres moteurs de mappage fonctionnent de la même façon (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="d0b18-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="d0b18-158">Exécutez la page et entrez une adresse.</span><span class="sxs-lookup"><span data-stu-id="d0b18-158">Run the page and enter an address.</span></span> <span data-ttu-id="d0b18-159">La page affiche une carte, basée sur Google Maps, qui indique l’emplacement que vous avez spécifié.</span><span class="sxs-lookup"><span data-stu-id="d0b18-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![mappage-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="d0b18-161">Création d’une carte en fonction des coordonnées de latitude et de longitude (à l’aide de Bing)</span><span class="sxs-lookup"><span data-stu-id="d0b18-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="d0b18-162">Cet exemple montre comment créer un mappage basé sur des coordonnées.</span><span class="sxs-lookup"><span data-stu-id="d0b18-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="d0b18-163">Cet exemple montre comment utiliser Bing Maps et comment inclure votre clé Bing.</span><span class="sxs-lookup"><span data-stu-id="d0b18-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="d0b18-164">(Vous pouvez créer un mappage basé sur les coordonnées à l’aide des autres moteurs de mappage, sans utiliser de clé Bing.)</span><span class="sxs-lookup"><span data-stu-id="d0b18-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="d0b18-165">Créez un fichier nommé *MapCoordinates. cshtml* à la racine du site et remplacez le contenu existant par le code et le balisage suivants :</span><span class="sxs-lookup"><span data-stu-id="d0b18-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="d0b18-166">Remplacez `your-key-here` par la clé Bing Maps que vous avez générée précédemment.</span><span class="sxs-lookup"><span data-stu-id="d0b18-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="d0b18-167">Exécutez la page *MapCoordinates. cshtml* , entrez les coordonnées de latitude et de longitude, puis cliquez sur la **carte** .</span><span class="sxs-lookup"><span data-stu-id="d0b18-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="d0b18-168">disproportionnée.</span><span class="sxs-lookup"><span data-stu-id="d0b18-168">button.</span></span> <span data-ttu-id="d0b18-169">(Si vous ne connaissez aucune coordonnée, essayez ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="d0b18-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="d0b18-170">Il s’agit d’un emplacement sur le campus Microsoft Redmond.)</span><span class="sxs-lookup"><span data-stu-id="d0b18-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="d0b18-171">Latitude : 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="d0b18-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="d0b18-172">Longitude :-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="d0b18-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="d0b18-173">La page est affichée à l’aide des coordonnées que vous avez spécifiées.</span><span class="sxs-lookup"><span data-stu-id="d0b18-173">The page is displayed using the coordinates that you specified.</span></span>

     ![mappage-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d0b18-175">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d0b18-175">Additional Resources</span></span>

[<span data-ttu-id="d0b18-176">Informations de référence sur l’API Microsoft. Maps</span><span class="sxs-lookup"><span data-stu-id="d0b18-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
