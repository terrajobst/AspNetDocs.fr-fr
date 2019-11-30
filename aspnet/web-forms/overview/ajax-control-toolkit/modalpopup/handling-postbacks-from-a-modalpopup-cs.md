---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Gestion des publications (postback) àC#partir d’un ModalPopup () | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Une attention particulière doit être prise lorsqu’un POS...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599085"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="8598a-104">Gestion des publications (postback) à partir d’un ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="8598a-104">Handling Postbacks from a ModalPopup (C#)</span></span>

<span data-ttu-id="8598a-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8598a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8598a-106">[Télécharger le code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8598a-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="8598a-107">Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client.</span><span class="sxs-lookup"><span data-stu-id="8598a-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="8598a-108">Une attention particulière doit être prise lorsqu’une publication (postback) est créée à partir de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="8598a-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="8598a-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="8598a-109">Overview</span></span>

<span data-ttu-id="8598a-110">Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client.</span><span class="sxs-lookup"><span data-stu-id="8598a-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="8598a-111">Une attention particulière doit être prise lorsqu’une publication (postback) est créée à partir de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="8598a-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="8598a-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="8598a-112">Steps</span></span>

<span data-ttu-id="8598a-113">Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :</span><span class="sxs-lookup"><span data-stu-id="8598a-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="8598a-114">Ensuite, ajoutez un panneau qui sert de fenêtre contextuelle modale.</span><span class="sxs-lookup"><span data-stu-id="8598a-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="8598a-115">À partir de là, l’utilisateur peut entrer un nom et une adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="8598a-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="8598a-116">Un bouton est utilisé pour fermer la fenêtre contextuelle et enregistrer les informations.</span><span class="sxs-lookup"><span data-stu-id="8598a-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="8598a-117">Notez que l’attribut `OnClick` est défini de telle sorte qu’une publication (postback) se produit lorsque vous cliquez sur ce bouton :</span><span class="sxs-lookup"><span data-stu-id="8598a-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="8598a-118">La page elle-même se compose de deux étiquettes pour exactement les mêmes informations : nom et adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="8598a-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="8598a-119">Un bouton est utilisé pour déclencher la fenêtre contextuelle modale :</span><span class="sxs-lookup"><span data-stu-id="8598a-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="8598a-120">Pour que la fenêtre contextuelle s’affiche, ajoutez le contrôle `ModalPopupExtender`.</span><span class="sxs-lookup"><span data-stu-id="8598a-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="8598a-121">Affectez à l’attribut `PopupControlID` l’ID du panneau et `TargetControlID` à l’ID du bouton :</span><span class="sxs-lookup"><span data-stu-id="8598a-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="8598a-122">Désormais, chaque fois que le bouton `Save` dans la fenêtre contextuelle modale est cliqué, la méthode `SaveData()` côté serveur est exécutée.</span><span class="sxs-lookup"><span data-stu-id="8598a-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="8598a-123">À partir de là, vous pouvez enregistrer les données entrées dans un magasin de données.</span><span class="sxs-lookup"><span data-stu-id="8598a-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="8598a-124">Par souci de simplicité, les nouvelles données sont simplement sorties dans l’étiquette :</span><span class="sxs-lookup"><span data-stu-id="8598a-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="8598a-125">En outre, les contrôles TextBox dans la fenêtre contextuelle modale doivent être remplis avec le nom et l’adresse de messagerie actuels.</span><span class="sxs-lookup"><span data-stu-id="8598a-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="8598a-126">Toutefois, cela n’est nécessaire que si aucune publication (postback) ne se produit.</span><span class="sxs-lookup"><span data-stu-id="8598a-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="8598a-127">S’il existe une publication (postback), la fonctionnalité ASP.NET ViewState remplit automatiquement les zones de texte avec les valeurs appropriées.</span><span class="sxs-lookup"><span data-stu-id="8598a-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

<span data-ttu-id="8598a-128">[![la fenêtre contextuelle modale entraîne une publication (postback)](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8598a-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="8598a-129">La fenêtre contextuelle modale entraîne une publication ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8598a-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8598a-130">[Précédent](using-modalpopup-with-a-repeater-control-cs.md)
> [Suivant](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8598a-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
