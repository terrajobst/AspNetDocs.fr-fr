---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendu des sites pages Web ASP.NET (Razor) pour les appareils mobiles | Microsoft Docs
author: Rick-Anderson
description: 'Cet article explique comment créer des pages dans un site pages Web ASP.NET (Razor) qui s’affichent correctement sur les périphériques mobiles. Ce que vous allez apprendre : comment...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563566"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="63be5-104">Rendu des sites pages Web ASP.NET (Razor) pour les appareils mobiles</span><span class="sxs-lookup"><span data-stu-id="63be5-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>

<span data-ttu-id="63be5-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="63be5-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="63be5-106">Cet article explique comment créer des pages dans un site pages Web ASP.NET (Razor) qui s’affichent correctement sur les périphériques mobiles.</span><span class="sxs-lookup"><span data-stu-id="63be5-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="63be5-107">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="63be5-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="63be5-108">Comment utiliser une convention d’affectation de noms pour spécifier qu’une page est conçue spécifiquement pour les périphériques mobiles.</span><span class="sxs-lookup"><span data-stu-id="63be5-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="63be5-109">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="63be5-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="63be5-110">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="63be5-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="63be5-111">Ce didacticiel fonctionne également avec pages Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="63be5-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="63be5-112">Pages Web ASP.NET vous permet de créer des affichages personnalisés pour le rendu du contenu sur des appareils mobiles ou autres.</span><span class="sxs-lookup"><span data-stu-id="63be5-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="63be5-113">La façon la plus simple de créer une page spécifique à un appareil dans un site pages Web ASP.NET consiste à utiliser un modèle de nom de fichier comme celui-ci : *nom_fichier. mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="63be5-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.Mobile.cshtml*.</span></span> <span data-ttu-id="63be5-114">Vous pouvez créer deux versions d’une page (par exemple, l’une nommée *MyFile. cshtml* et l’autre nommée *MyFile. mobile. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="63be5-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="63be5-115">Au moment de l’exécution, quand un appareil mobile demande *MyFile. cshtml*, ASP.NET restitue le contenu à partir de *MyFile. mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="63be5-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="63be5-116">Dans le cas contraire, *MyFile. cshtml* est restitué.</span><span class="sxs-lookup"><span data-stu-id="63be5-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="63be5-117">L’exemple suivant montre comment activer le rendu mobile en ajoutant une page de contenu pour les périphériques mobiles.</span><span class="sxs-lookup"><span data-stu-id="63be5-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="63be5-118">*Page1. cshtml* contient du contenu plus un encadré de navigation.</span><span class="sxs-lookup"><span data-stu-id="63be5-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="63be5-119">*Page1. mobile. cshtml* contient le même contenu, mais omet la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="63be5-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="63be5-120">Dans un site pages Web ASP.NET, créez un fichier nommé *Page1. cshtml* et remplacez le contenu actuel par le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="63be5-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="63be5-121">Créez un fichier nommé *Page1. mobile. cshtml* et remplacez le contenu existant par le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="63be5-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="63be5-122">Notez que la version mobile de la page omet la section de navigation pour un meilleur rendu sur un écran plus petit.</span><span class="sxs-lookup"><span data-stu-id="63be5-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="63be5-123">Exécutez un navigateur de bureau et accédez à *Page1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="63be5-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="63be5-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="63be5-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="63be5-125">Exécutez un navigateur mobile (ou un émulateur d’appareil mobile) et accédez à *Page1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="63be5-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="63be5-126">(Notez que vous n’incluez pas *. mobile.*</span><span class="sxs-lookup"><span data-stu-id="63be5-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="63be5-127">dans le cadre de l’URL.) Même si la requête est dans *Page1. cshtml*, ASP.NET affiche *Page1. mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="63be5-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="63be5-129">Pour tester les pages mobiles, vous pouvez utiliser un simulateur d’appareil mobile qui s’exécute sur un ordinateur de bureau.</span><span class="sxs-lookup"><span data-stu-id="63be5-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="63be5-130">Cet outil vous permet de tester les pages Web telles qu’elles apparaissent sur les périphériques mobiles (c’est-à-dire, généralement avec une zone d’affichage bien plus petite).</span><span class="sxs-lookup"><span data-stu-id="63be5-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="63be5-131">L’un des exemples d’un simulateur est le [Sélecteur d’agent utilisateur](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) pour Mozilla Firefox, qui vous permet d’émuler différents navigateurs mobiles à partir d’une version de bureau de Firefox.</span><span class="sxs-lookup"><span data-stu-id="63be5-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="63be5-132">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="63be5-132">Additional Resources</span></span>

<span data-ttu-id="63be5-133">[Émulateur Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="63be5-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
