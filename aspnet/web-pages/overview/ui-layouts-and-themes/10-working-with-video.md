---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Affichage de vidéos dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre explique comment afficher une vidéo dans un pages Web ASP.NET avec syntaxe Razor page.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628946"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="354b7-103">Affichage de vidéos dans un site pages Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="354b7-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="354b7-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="354b7-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="354b7-105">Cet article explique comment utiliser un lecteur vidéo (multimédia) dans un site Web pages Web ASP.NET (Razor) pour permettre aux utilisateurs d’afficher les vidéos stockées sur le site.</span><span class="sxs-lookup"><span data-stu-id="354b7-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="354b7-106">Pages Web ASP.NET avec syntaxe Razor vous permet de lire des vidéos flash ( *. swf*), Media Player ( *. wmv*) et Silverlight ( *. xap*).</span><span class="sxs-lookup"><span data-stu-id="354b7-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="354b7-107">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="354b7-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="354b7-108">Comment choisir un lecteur vidéo.</span><span class="sxs-lookup"><span data-stu-id="354b7-108">How to choose a video player.</span></span>
> - <span data-ttu-id="354b7-109">Comment ajouter une vidéo à une page Web.</span><span class="sxs-lookup"><span data-stu-id="354b7-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="354b7-110">Comment définir les attributs d’un lecteur vidéo.</span><span class="sxs-lookup"><span data-stu-id="354b7-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="354b7-111">Voici les fonctionnalités des pages Razor ASP.NET présentées dans l’article :</span><span class="sxs-lookup"><span data-stu-id="354b7-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="354b7-112">Le programme d’assistance `Video`.</span><span class="sxs-lookup"><span data-stu-id="354b7-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="354b7-113">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="354b7-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="354b7-114">Pages Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="354b7-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="354b7-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="354b7-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="354b7-116">Ce didacticiel fonctionne également avec WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="354b7-116">This tutorial also works with WebMatrix 3.</span></span>

## <a name="introduction"></a><span data-ttu-id="354b7-117">Introduction</span><span class="sxs-lookup"><span data-stu-id="354b7-117">Introduction</span></span>

<span data-ttu-id="354b7-118">Vous souhaiterez peut-être afficher une vidéo sur votre site.</span><span class="sxs-lookup"><span data-stu-id="354b7-118">You might want to display a video on your site.</span></span> <span data-ttu-id="354b7-119">Pour ce faire, vous pouvez créer un lien vers un site qui a déjà la vidéo, par exemple YouTube.</span><span class="sxs-lookup"><span data-stu-id="354b7-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="354b7-120">Si vous souhaitez incorporer une vidéo à partir de ces sites directement dans vos propres pages, vous pouvez généralement obtenir le balisage HTML à partir du site, puis le copier dans votre page.</span><span class="sxs-lookup"><span data-stu-id="354b7-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="354b7-121">Par exemple, l’exemple suivant montre comment incorporer une vidéo YouTube :</span><span class="sxs-lookup"><span data-stu-id="354b7-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="354b7-122">Si vous souhaitez lire une vidéo qui se trouve sur votre propre site Web (et non sur un site de partage vidéo public), vous ne pouvez pas le lier directement à l’aide d’un balisage incorporé comme celui-ci.</span><span class="sxs-lookup"><span data-stu-id="354b7-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="354b7-123">Toutefois, vous pouvez lire des vidéos à partir de votre site à l’aide du programme d’assistance `Video`, qui restitue un lecteur multimédia directement dans une page.</span><span class="sxs-lookup"><span data-stu-id="354b7-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="354b7-124">Choix d’un lecteur vidéo</span><span class="sxs-lookup"><span data-stu-id="354b7-124">Choosing a Video Player</span></span>

<span data-ttu-id="354b7-125">Il existe un grand nombre de formats pour les fichiers vidéo, et chaque format requiert généralement un joueur différent et une autre façon de configurer le lecteur.</span><span class="sxs-lookup"><span data-stu-id="354b7-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="354b7-126">Dans les pages Razor ASP.NET, vous pouvez lire une vidéo dans une page Web à l’aide du programme d’assistance `Video`.</span><span class="sxs-lookup"><span data-stu-id="354b7-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="354b7-127">Le `Video` Helper simplifie le processus d’incorporation de vidéos dans une page Web, car il génère automatiquement les éléments HTML `object` et `embed` qui sont normalement utilisés pour ajouter de la vidéo à la page.</span><span class="sxs-lookup"><span data-stu-id="354b7-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="354b7-128">Le programme d’assistance `Video` prend en charge les lecteurs multimédias suivants :</span><span class="sxs-lookup"><span data-stu-id="354b7-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="354b7-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="354b7-129">Adobe Flash</span></span>
- <span data-ttu-id="354b7-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="354b7-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="354b7-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="354b7-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="354b7-132">Le lecteur flash</span><span class="sxs-lookup"><span data-stu-id="354b7-132">The Flash Player</span></span>

<span data-ttu-id="354b7-133">Le lecteur `Flash` du programme d’assistance `Video` vous permet de lire des vidéos flash (fichiers *. swf* ) dans une page Web.</span><span class="sxs-lookup"><span data-stu-id="354b7-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="354b7-134">Au minimum, vous devez fournir un chemin d’accès au fichier vidéo.</span><span class="sxs-lookup"><span data-stu-id="354b7-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="354b7-135">Si vous ne spécifiez rien, à l’exception du chemin d’accès, le lecteur utilise les valeurs par défaut définies par la version actuelle de Flash.</span><span class="sxs-lookup"><span data-stu-id="354b7-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="354b7-136">Les paramètres par défaut sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="354b7-136">Typical default settings are:</span></span>

- <span data-ttu-id="354b7-137">La vidéo est affichée à l’aide de sa largeur et de sa hauteur par défaut et sans couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="354b7-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="354b7-138">La vidéo est lue automatiquement lors du chargement de la page.</span><span class="sxs-lookup"><span data-stu-id="354b7-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="354b7-139">La vidéo s’exécute en continu jusqu’à ce qu’elle soit explicitement arrêtée.</span><span class="sxs-lookup"><span data-stu-id="354b7-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="354b7-140">La vidéo est mise à l’échelle pour afficher toute la vidéo, au lieu de la rogner pour l’adapter à une taille spécifique.</span><span class="sxs-lookup"><span data-stu-id="354b7-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="354b7-141">La vidéo est lue dans une fenêtre.</span><span class="sxs-lookup"><span data-stu-id="354b7-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="354b7-142">Le lecteur MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="354b7-142">The MediaPlayer Player</span></span>

<span data-ttu-id="354b7-143">Le lecteur `MediaPlayer` du programme d’assistance `Video` vous permet de lire des vidéos Windows Media (fichiers *. wmv* ), des fichiers audio Windows Media (fichiers *. WMA* ) et des fichiers MP3 ( *. mp3* ) dans une page Web.</span><span class="sxs-lookup"><span data-stu-id="354b7-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="354b7-144">Vous devez inclure le chemin d’accès du fichier multimédia à lire ; tous les autres paramètres sont facultatifs.</span><span class="sxs-lookup"><span data-stu-id="354b7-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="354b7-145">Si vous spécifiez uniquement un chemin d’accès, le lecteur utilise les paramètres par défaut définis par la version actuelle de MediaPlayer, par exemple :</span><span class="sxs-lookup"><span data-stu-id="354b7-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="354b7-146">La vidéo est affichée en utilisant sa largeur et sa hauteur par défaut.</span><span class="sxs-lookup"><span data-stu-id="354b7-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="354b7-147">La vidéo est lue automatiquement lors du chargement de la page.</span><span class="sxs-lookup"><span data-stu-id="354b7-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="354b7-148">La vidéo est lue une seule fois (elle n’est pas en boucle).</span><span class="sxs-lookup"><span data-stu-id="354b7-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="354b7-149">Le lecteur affiche le jeu complet de contrôles dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="354b7-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="354b7-150">La vidéo est lue dans une fenêtre.</span><span class="sxs-lookup"><span data-stu-id="354b7-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="354b7-151">Le lecteur Silverlight</span><span class="sxs-lookup"><span data-stu-id="354b7-151">The Silverlight Player</span></span>

<span data-ttu-id="354b7-152">Le lecteur `Silverlight` du programme d’assistance `Video` vous permet de lire des Windows Media Video (fichiers *. wmv* ), des Windows Media audio (fichiers *. WMA* ) et des fichiers MP3 ( *. mp3* ).</span><span class="sxs-lookup"><span data-stu-id="354b7-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="354b7-153">Vous devez définir le paramètre path pour qu’il pointe vers un package d’application Silverlight (fichier *. xap* ).</span><span class="sxs-lookup"><span data-stu-id="354b7-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="354b7-154">Vous devez également définir les paramètres de largeur et de hauteur.</span><span class="sxs-lookup"><span data-stu-id="354b7-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="354b7-155">Tous les autres paramètres sont facultatifs.</span><span class="sxs-lookup"><span data-stu-id="354b7-155">All other parameters are optional.</span></span> <span data-ttu-id="354b7-156">Quand vous utilisez le lecteur Silverlight pour la vidéo, si vous définissez uniquement les paramètres requis, le lecteur Silverlight affiche la vidéo sans couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="354b7-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="354b7-157">Si vous ne connaissez pas encore Silverlight : le fichier *. xap* est un fichier compressé qui contient des instructions de mise en page dans un fichier *. Xaml* , du code managé dans les assemblys et des ressources facultatives.</span><span class="sxs-lookup"><span data-stu-id="354b7-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="354b7-158">Vous pouvez créer un fichier *. xap* dans Visual Studio en tant que projet d’application Silverlight.</span><span class="sxs-lookup"><span data-stu-id="354b7-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>

<span data-ttu-id="354b7-159">Le lecteur vidéo `Silverlight` utilise à la fois les paramètres que vous fournissez pour le lecteur et les paramètres fournis dans le fichier *. xap* .</span><span class="sxs-lookup"><span data-stu-id="354b7-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="354b7-160">Types MIME</span><span class="sxs-lookup"><span data-stu-id="354b7-160">MIME Types</span></span>
> 
> <span data-ttu-id="354b7-161">Quand un navigateur télécharge un fichier, le navigateur s’assure que le type de fichier correspond au type MIME spécifié pour le document en cours de rendu.</span><span class="sxs-lookup"><span data-stu-id="354b7-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="354b7-162">Le type MIME est le type de contenu ou le type de média d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="354b7-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="354b7-163">Le programme d’assistance `Video` utilise les types MIME suivants :</span><span class="sxs-lookup"><span data-stu-id="354b7-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="354b7-164">Vidéos flash (. swf)</span><span class="sxs-lookup"><span data-stu-id="354b7-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="354b7-165">Cette procédure vous montre comment lire une vidéo Flash nommée *Sample. swf*.</span><span class="sxs-lookup"><span data-stu-id="354b7-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="354b7-166">La procédure suppose que vous disposez d’un dossier nommé *Media* sur votre site et que le fichier *. swf* se trouve dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="354b7-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="354b7-167">Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas encore fait.</span><span class="sxs-lookup"><span data-stu-id="354b7-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="354b7-168">Dans le site Web, ajoutez une page et nommez-la *FlashVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="354b7-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="354b7-169">Ajoutez le balisage suivant à la page :</span><span class="sxs-lookup"><span data-stu-id="354b7-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="354b7-170">Exécutez la page dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="354b7-170">Run the page in a browser.</span></span> <span data-ttu-id="354b7-171">(Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.) La page s’affiche et la vidéo est lue automatiquement.</span><span class="sxs-lookup"><span data-stu-id="354b7-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="354b7-172">![image](10-working-with-video/_static/image1.jpg "ch08_video -1. jpg")</span><span class="sxs-lookup"><span data-stu-id="354b7-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="354b7-173">Vous pouvez définir le paramètre `quality` pour une vidéo Flash sur `low`, `autolow`, `autohigh`, `medium`, `high`et `best`:</span><span class="sxs-lookup"><span data-stu-id="354b7-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="354b7-174">Vous pouvez modifier la vidéo Flash pour qu’elle s’exécute à une taille spécifique à l’aide du paramètre `scale`, que vous pouvez définir comme suit :</span><span class="sxs-lookup"><span data-stu-id="354b7-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="354b7-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="354b7-175">`showall`.</span></span> <span data-ttu-id="354b7-176">Cela rend l’intégralité de la vidéo visible tout en conservant les proportions d’origine.</span><span class="sxs-lookup"><span data-stu-id="354b7-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="354b7-177">Toutefois, vous pouvez vous retrouver avec des bordures de chaque côté.</span><span class="sxs-lookup"><span data-stu-id="354b7-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="354b7-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="354b7-178">`noorder`.</span></span> <span data-ttu-id="354b7-179">Cela met à l’échelle la vidéo tout en conservant les proportions d’origine, mais elle peut être rognée.</span><span class="sxs-lookup"><span data-stu-id="354b7-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="354b7-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="354b7-180">`exactfit`.</span></span> <span data-ttu-id="354b7-181">Cela rend l’intégralité de la vidéo visible sans conserver les proportions d’origine, mais la distorsion peut se produire.</span><span class="sxs-lookup"><span data-stu-id="354b7-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="354b7-182">Si vous ne spécifiez pas de paramètre `scale`, la vidéo entière est visible et les proportions d’origine sont conservées sans rognage.</span><span class="sxs-lookup"><span data-stu-id="354b7-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="354b7-183">L’exemple suivant montre comment utiliser le paramètre `scale` :</span><span class="sxs-lookup"><span data-stu-id="354b7-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="354b7-184">Le lecteur Flash Player prend en charge un paramètre de mode vidéo nommé `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="354b7-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="354b7-185">Vous pouvez définir cette valeur sur `window`, `opaque`et `transparent`.</span><span class="sxs-lookup"><span data-stu-id="354b7-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="354b7-186">Par défaut, le `windowMode` est défini sur `window`, qui affiche la vidéo dans une fenêtre distincte sur la page Web.</span><span class="sxs-lookup"><span data-stu-id="354b7-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="354b7-187">Le paramètre `opaque` masque tout ce qui se trouve derrière la vidéo sur la page Web.</span><span class="sxs-lookup"><span data-stu-id="354b7-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="354b7-188">Le paramètre `transparent` permet à l’arrière-plan de la page Web de s’afficher dans la vidéo, en supposant qu’une partie de la vidéo est transparente.</span><span class="sxs-lookup"><span data-stu-id="354b7-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="354b7-189">Vidéos MediaPlayer ( *. wmv*)</span><span class="sxs-lookup"><span data-stu-id="354b7-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="354b7-190">La procédure suivante montre comment lire une vidéo multimédia Windows nommée *Sample. wmv* qui se trouve dans le dossier *Media* .</span><span class="sxs-lookup"><span data-stu-id="354b7-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="354b7-191">Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.</span><span class="sxs-lookup"><span data-stu-id="354b7-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="354b7-192">Créez une nouvelle page nommée *MediaPlayerVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="354b7-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="354b7-193">Ajoutez le balisage suivant à la page :</span><span class="sxs-lookup"><span data-stu-id="354b7-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="354b7-194">Exécutez la page dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="354b7-194">Run the page in a browser.</span></span> <span data-ttu-id="354b7-195">La vidéo se charge et se lance automatiquement.</span><span class="sxs-lookup"><span data-stu-id="354b7-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="354b7-196">![image](10-working-with-video/_static/image2.jpg "ch08_video-2. jpg")</span><span class="sxs-lookup"><span data-stu-id="354b7-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="354b7-197">Vous pouvez définir `playCount` sur un entier qui indique le nombre de fois que la vidéo est automatiquement lue :</span><span class="sxs-lookup"><span data-stu-id="354b7-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="354b7-198">Le paramètre `uiMode` vous permet de spécifier les contrôles qui apparaissent dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="354b7-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="354b7-199">Vous pouvez définir `uiMode` sur `invisible`, `none`, `mini`ou `full`.</span><span class="sxs-lookup"><span data-stu-id="354b7-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="354b7-200">Si vous ne spécifiez pas de paramètre `uiMode`, la vidéo sera affichée à l’aide de la fenêtre d’État, de la barre de recherche, des boutons de contrôle et des contrôles de volume, en plus de la fenêtre vidéo.</span><span class="sxs-lookup"><span data-stu-id="354b7-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="354b7-201">Ces contrôles s’affichent également si vous utilisez le lecteur pour lire un fichier audio.</span><span class="sxs-lookup"><span data-stu-id="354b7-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="354b7-202">Voici un exemple d’utilisation du paramètre `uiMode` :</span><span class="sxs-lookup"><span data-stu-id="354b7-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="354b7-203">Par défaut, l’audio est activé lors de la lecture de la vidéo.</span><span class="sxs-lookup"><span data-stu-id="354b7-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="354b7-204">Vous pouvez désactiver l’audio en affectant au paramètre `mute` la valeur true :</span><span class="sxs-lookup"><span data-stu-id="354b7-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="354b7-205">Vous pouvez contrôler le niveau audio de la vidéo de MediaPlayer en définissant le paramètre `volume` sur une valeur comprise entre 0 et 100.</span><span class="sxs-lookup"><span data-stu-id="354b7-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="354b7-206">La valeur par défaut est 50.</span><span class="sxs-lookup"><span data-stu-id="354b7-206">The default value is 50.</span></span> <span data-ttu-id="354b7-207">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="354b7-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="354b7-208">Vidéos Silverlight</span><span class="sxs-lookup"><span data-stu-id="354b7-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="354b7-209">Cette procédure vous montre comment lire la vidéo contenue dans une page Silverlight *. xap* qui se trouve dans un dossier nommé *Media*.</span><span class="sxs-lookup"><span data-stu-id="354b7-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="354b7-210">Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.</span><span class="sxs-lookup"><span data-stu-id="354b7-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="354b7-211">Créez une nouvelle page nommée *SilverlightVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="354b7-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="354b7-212">Ajoutez le balisage suivant à la page :</span><span class="sxs-lookup"><span data-stu-id="354b7-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="354b7-213">Exécutez la page dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="354b7-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="354b7-214">![image](10-working-with-video/_static/image3.jpg "ch08_video-3. jpg")</span><span class="sxs-lookup"><span data-stu-id="354b7-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="354b7-215">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="354b7-215">Additional Resources</span></span>

<span data-ttu-id="354b7-216">[Présentation de Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="354b7-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="354b7-217">Attributs d’objet Flash et de balise EMBED</span><span class="sxs-lookup"><span data-stu-id="354b7-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="354b7-218">[Balises PARAM du SDK Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="354b7-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
