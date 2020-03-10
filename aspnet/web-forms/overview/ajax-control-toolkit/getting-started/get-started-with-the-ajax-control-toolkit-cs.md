---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Prise en main de la boîte à outilsC#de contrôle AJAX () | Microsoft Docs
author: microsoft
description: Découvrez tout ce que vous devez savoir pour commencer à utiliser le kit d’outils de contrôle AJAX.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 7478b090ec52778572d70065983de6be8bdb4e6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621603"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="c2d6f-103">Bien démarrer avec AJAX Control Toolkit (C#)</span><span class="sxs-lookup"><span data-stu-id="c2d6f-103">Get Started with the AJAX Control Toolkit (C#)</span></span>

<span data-ttu-id="c2d6f-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c2d6f-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c2d6f-105">Découvrez tout ce que vous devez savoir pour commencer à utiliser le kit d’outils de contrôle AJAX.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>

<span data-ttu-id="c2d6f-106">Le kit d’outils de contrôle AJAX contient plus de 30 contrôles gratuits que vous pouvez utiliser dans vos applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="c2d6f-107">Dans ce didacticiel, vous allez apprendre à télécharger AJAX Control Toolkit et à ajouter les contrôles de boîte à outils à votre boîte à outils Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="c2d6f-108">Téléchargement de la boîte à outils de contrôle AJAX</span><span class="sxs-lookup"><span data-stu-id="c2d6f-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="c2d6f-109">[AJAX Control Toolkit](http://devexpress.com/act) est un projet open source développé par les membres de la communauté ASP.net et de l’équipe ASP.net.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 

<span data-ttu-id="c2d6f-110">[![téléchargement de la boîte à outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c2d6f-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="c2d6f-111">**Figure 01**: Téléchargement de la boîte à outils de contrôle AJAX ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c2d6f-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>

<span data-ttu-id="c2d6f-112">Après avoir téléchargé le fichier, vous devez débloquer le fichier.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="c2d6f-113">Cliquez avec le bouton droit sur le fichier, sélectionnez Propriétés, puis cliquez sur le bouton **débloquer** (voir figure 2).</span><span class="sxs-lookup"><span data-stu-id="c2d6f-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>

<span data-ttu-id="c2d6f-114">[![du déblocage du fichier ZIP du kit de commandes AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c2d6f-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="c2d6f-115">**Figure 02**: déblocage du fichier zip du kit de commandes Ajax ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="c2d6f-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>

<span data-ttu-id="c2d6f-116">Une fois le fichier débloqué, vous pouvez décompresser le fichier : cliquez avec le bouton droit sur le fichier et sélectionnez l’option de menu **extraire tout** .</span><span class="sxs-lookup"><span data-stu-id="c2d6f-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="c2d6f-117">Nous sommes maintenant prêts à ajouter le Toolkit à la boîte à outils Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="c2d6f-118">Ajout de la boîte à outils de contrôle AJAX à la boîte à outils</span><span class="sxs-lookup"><span data-stu-id="c2d6f-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="c2d6f-119">Le moyen le plus simple d’utiliser le kit d’outils de contrôle AJAX consiste à ajouter le Toolkit à votre boîte à outils Visual Studio/Visual Web Developer (voir figure 3).</span><span class="sxs-lookup"><span data-stu-id="c2d6f-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="c2d6f-120">De cette façon, vous pouvez simplement faire glisser un contrôle Toolkit sur une page lorsque vous souhaitez l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>

<span data-ttu-id="c2d6f-121">[![boîte à outils de contrôle AJAX s’affiche dans la boîte à outils](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c2d6f-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="c2d6f-122">**Figure 03**: la boîte à outils de contrôle AJAX s’affiche dans la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c2d6f-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>

<span data-ttu-id="c2d6f-123">Tout d’abord, vous devez ajouter un onglet AJAX Control Toolkit à la boîte à outils.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="c2d6f-124">Effectuez les opérations suivantes.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-124">Follow these steps.</span></span>

1. <span data-ttu-id="c2d6f-125">Créez un site Web ASP.NET en sélectionnant l’option de menu fichier, nouveau site Web.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="c2d6f-126">Double-cliquez sur le fichier default. aspx dans la fenêtre Explorateur de solutions pour ouvrir le fichier dans l’éditeur.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="c2d6f-127">Cliquez avec le bouton droit sur la boîte à outils sous l’onglet général, puis sélectionnez l’option de menu **Ajouter un onglet** (voir figure 4).</span><span class="sxs-lookup"><span data-stu-id="c2d6f-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="c2d6f-128">Entrez un nouvel onglet nommé AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-128">Enter a new tab named AJAX Control Toolkit.</span></span>

<span data-ttu-id="c2d6f-129">[![ajout d’un nouvel onglet](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c2d6f-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="c2d6f-130">**Figure 04**: ajout d’un nouvel onglet ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="c2d6f-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>

<span data-ttu-id="c2d6f-131">Ensuite, vous devez ajouter les contrôles de la boîte à outils du contrôle AJAX au nouvel onglet. procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="c2d6f-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="c2d6f-132">Cliquez avec le bouton droit sous l’onglet AJAX Control Toolkit et sélectionnez l’option de menu **choisir des éléments (voir figure 5)** .</span><span class="sxs-lookup"><span data-stu-id="c2d6f-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="c2d6f-133">Accédez à l’emplacement où vous avez décompressé la boîte à outils de contrôle AJAX et sélectionnez l’assembly AjaxControlToolkit. dll.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>

<span data-ttu-id="c2d6f-134">[![choisir les éléments à ajouter à la boîte à outils](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="c2d6f-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="c2d6f-135">**Figure 05**: sélectionner les éléments à ajouter à la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="c2d6f-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>

<span data-ttu-id="c2d6f-136">Une fois ces étapes terminées, tous les contrôles du kit d’outils s’affichent dans votre boîte à outils.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="c2d6f-137">Mise à niveau vers une nouvelle version de la boîte à outils</span><span class="sxs-lookup"><span data-stu-id="c2d6f-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="c2d6f-138">Si vous utilisiez une version antérieure de la boîte à outils et que vous devez maintenant passer à une version ultérieure, Voici les étapes recommandées :</span><span class="sxs-lookup"><span data-stu-id="c2d6f-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="c2d6f-139">Fichiers binaires : supprimez l’ancienne version de l’assembly AjaxControlToolkit. dll dans le dossier bin de votre site Web.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="c2d6f-140">Éléments de boîte à outils-supprimez l’onglet AJAX Control Toolkit et suivez les étapes ci-dessus pour recréer l’onglet avec la nouvelle version de l’assembly AjaxControlToolkit. dll.</span><span class="sxs-lookup"><span data-stu-id="c2d6f-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c2d6f-141">Next</span><span class="sxs-lookup"><span data-stu-id="c2d6f-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
