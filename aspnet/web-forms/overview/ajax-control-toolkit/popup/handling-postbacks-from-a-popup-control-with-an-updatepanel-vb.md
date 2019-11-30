---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Gestion des publications (postback) à partir d’un contrôle Popup avec un UpdatePanel (VB) | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé. Une attention particulière doit être prise...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: dd045ae56696c7944df98cf805ba812fde1bb4ff
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598822"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="382e7-104">Gestion des publications (postback) à partir d’un contrôle de fenêtre contextuelle avec un UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="382e7-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>

<span data-ttu-id="382e7-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="382e7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="382e7-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="382e7-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="382e7-107">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="382e7-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="382e7-108">Une attention particulière doit être prise lorsqu’une publication (postback) se produit dans une telle fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="382e7-108">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="overview"></a><span data-ttu-id="382e7-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="382e7-109">Overview</span></span>

<span data-ttu-id="382e7-110">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle quand un autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="382e7-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="382e7-111">Une attention particulière doit être prise lorsqu’une publication (postback) se produit dans une telle fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="382e7-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="382e7-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="382e7-112">Steps</span></span>

<span data-ttu-id="382e7-113">Lors de l’utilisation d’un `PopupControl` avec une publication (postback), un `UpdatePanel` peut empêcher l’actualisation de la page causée par la publication (postback).</span><span class="sxs-lookup"><span data-stu-id="382e7-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="382e7-114">Le balisage suivant définit quelques éléments importants :</span><span class="sxs-lookup"><span data-stu-id="382e7-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="382e7-115">Un contrôle `ScriptManager` pour que la boîte à outils de contrôle ASP.NET AJAX fonctionne</span><span class="sxs-lookup"><span data-stu-id="382e7-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="382e7-116">Deux contrôles `TextBox` qui déclenchent tous deux un popup</span><span class="sxs-lookup"><span data-stu-id="382e7-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="382e7-117">Contrôle de `Panel` qui fera office de fenêtre contextuelle</span><span class="sxs-lookup"><span data-stu-id="382e7-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="382e7-118">Dans le panneau, un contrôle de `Calendar` est incorporé dans un contrôle `UpdatePanel`</span><span class="sxs-lookup"><span data-stu-id="382e7-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="382e7-119">Deux contrôles `PopupControlExtender` qui affectent le panneau aux zones de texte</span><span class="sxs-lookup"><span data-stu-id="382e7-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="382e7-120">Notez que l’attribut `OnSelectionChanged` du contrôle `Calendar` est défini.</span><span class="sxs-lookup"><span data-stu-id="382e7-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="382e7-121">Par conséquent, lorsque l’utilisateur sélectionne une date dans le calendrier, une publication (postback) se produit et la méthode côté serveur `c1_SelectionChanged()` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="382e7-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="382e7-122">Dans cette méthode, la date actuelle doit être extraite et réécrite dans la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="382e7-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="382e7-123">La syntaxe de est la suivante : tout d’abord, un objet proxy pour l' `PopupControlExtender` sur la page doit être généré.</span><span class="sxs-lookup"><span data-stu-id="382e7-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="382e7-124">ASP.NET AJAX Control Toolkit offre la méthode `GetProxyForCurrentPopup()`.</span><span class="sxs-lookup"><span data-stu-id="382e7-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="382e7-125">L’objet retourné par cette méthode prend en charge la méthode `Commit()` qui renvoie une valeur au contrôle qui a déclenché la fenêtre contextuelle (et non le contrôle qui a déclenché l’appel de méthode !).</span><span class="sxs-lookup"><span data-stu-id="382e7-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="382e7-126">Le code suivant fournit la date sélectionnée comme argument pour la méthode `Commit()`, ce qui amène le code à réécrire la date sélectionnée dans la zone de texte :</span><span class="sxs-lookup"><span data-stu-id="382e7-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="382e7-127">À présent, lorsque vous cliquez sur une date du calendrier, la date sélectionnée apparaît dans la zone de texte associée et crée un contrôle de sélecteur de dates qui se trouve actuellement sur de nombreux sites Web.</span><span class="sxs-lookup"><span data-stu-id="382e7-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>

<span data-ttu-id="382e7-128">[![le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="382e7-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="382e7-129">Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="382e7-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>

<span data-ttu-id="382e7-130">[![cliquant sur une date l’insère dans la zone de texte](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="382e7-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="382e7-131">Cliquez sur une date pour la placer dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="382e7-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="382e7-132">[Précédent](using-multiple-popup-controls-vb.md)
> [Suivant](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="382e7-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
