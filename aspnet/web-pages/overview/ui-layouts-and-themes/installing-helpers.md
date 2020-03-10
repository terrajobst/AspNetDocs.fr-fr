---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installation d’un programme d’assistance sur un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment installer une application auxiliaire sur un site Web pages Web ASP.NET (Razor). Une application auxiliaire est un composant réutilisable qui comprend le code et le balisage par...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638585"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="5c964-104">Installation d’un programme d’assistance sur un site pages Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="5c964-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="5c964-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5c964-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5c964-106">Cet article explique comment installer une application auxiliaire sur un site Web pages Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="5c964-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="5c964-107">Une *application auxiliaire* est un composant réutilisable qui comprend du code et des balises pour effectuer une tâche qui peut être fastidieuse ou complexe.</span><span class="sxs-lookup"><span data-stu-id="5c964-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="5c964-108">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="5c964-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="5c964-109">Comment installer une application d’assistance dans un site Web créé à l’aide de WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="5c964-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5c964-110">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="5c964-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5c964-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="5c964-111">WebMatrix 3</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="5c964-112">Vue d’ensemble des applications auxiliaires</span><span class="sxs-lookup"><span data-stu-id="5c964-112">Overview of Helpers</span></span>

<span data-ttu-id="5c964-113">Certaines tâches que les utilisateurs souhaitent souvent effectuer sur des pages Web nécessitent beaucoup de code ou nécessitent des connaissances supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="5c964-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="5c964-114">Les exemples incluent l’affichage d’un graphique pour les données ; Ajout d’un bouton « follow » Twitter sur une page ; envoi de courrier électronique à partir de votre site Web ; rognage ou redimensionnement d’images ; utilisation de PayPal pour votre site.</span><span class="sxs-lookup"><span data-stu-id="5c964-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="5c964-115">Pour faciliter ces types d’opérations, pages Web ASP.NET vous permet d’utiliser des *applications auxiliaires*.</span><span class="sxs-lookup"><span data-stu-id="5c964-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="5c964-116">Les programmes d’assistance sont des composants que vous installez pour un site et qui vous permettent d’effectuer des tâches courantes à l’aide d’une ou deux lignes de code Razor.</span><span class="sxs-lookup"><span data-stu-id="5c964-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="5c964-117">Pages Web ASP.NET dispose de quelques applications d’assistance intégrées.</span><span class="sxs-lookup"><span data-stu-id="5c964-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="5c964-118">Toutefois, de nombreuses applications d’assistance sont disponibles dans les packages (compléments) fournis à l’aide du gestionnaire de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="5c964-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="5c964-119">NuGet vous permet de sélectionner un package à installer, puis il prend en charge tous les détails de l’installation.</span><span class="sxs-lookup"><span data-stu-id="5c964-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="5c964-120">Installation d’une application d’assistance dans WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="5c964-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="5c964-121">Dans WebMatrix 3, cliquez sur le bouton **NuGet** .</span><span class="sxs-lookup"><span data-stu-id="5c964-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Boîte de dialogue galerie NuGet dans WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="5c964-123">Le gestionnaire de package NuGet est lancé et les packages disponibles s’affichent.</span><span class="sxs-lookup"><span data-stu-id="5c964-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="5c964-124">Dans la zone de recherche, entrez un mot clé pour l’application auxiliaire que vous souhaitez installer.</span><span class="sxs-lookup"><span data-stu-id="5c964-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Boîte de dialogue galerie NuGet dans WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="5c964-126">Sélectionnez le package, puis cliquez sur **installer**.</span><span class="sxs-lookup"><span data-stu-id="5c964-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="5c964-127">Cliquez sur **Oui** lorsque vous êtes invité à installer le package et indiquez que vous acceptez les termes du contrat.</span><span class="sxs-lookup"><span data-stu-id="5c964-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="5c964-128">S’il s’agit de la première fois que vous avez installé une application d’assistance, NuGet crée des dossiers dans votre site Web pour le code qui compose l’application d’assistance.</span><span class="sxs-lookup"><span data-stu-id="5c964-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="5c964-129">Pour désinstaller une application d’assistance, cliquez sur le bouton **Galerie** , cliquez sur l’onglet **installé** , puis sélectionnez le package que vous souhaitez désinstaller.</span><span class="sxs-lookup"><span data-stu-id="5c964-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="5c964-130">Installation du programme d’assistance Twitter</span><span class="sxs-lookup"><span data-stu-id="5c964-130">Installing the Twitter helper</span></span>

<span data-ttu-id="5c964-131">La dernière version de l’API Twitter n’est pas compatible avec l’application auxiliaire Twitter que vous installez par le biais de NuGet.</span><span class="sxs-lookup"><span data-stu-id="5c964-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="5c964-132">Pour plus d’informations sur la façon de configurer le programme d’assistance Twitter dans votre projet, consultez plutôt la rubrique [application auxiliaire Twitter avec WebMatrix](twitter-helper.md) .</span><span class="sxs-lookup"><span data-stu-id="5c964-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="5c964-133">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5c964-133">Additional Resources</span></span>

[<span data-ttu-id="5c964-134">Présentation de pages Web ASP.NET 2-notions de base de la programmation</span><span class="sxs-lookup"><span data-stu-id="5c964-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="5c964-135">Application auxiliaire Twitter avec WebMatrix</span><span class="sxs-lookup"><span data-stu-id="5c964-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
