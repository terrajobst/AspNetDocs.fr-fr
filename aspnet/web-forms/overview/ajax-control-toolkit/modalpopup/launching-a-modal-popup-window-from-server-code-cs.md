---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Lancement d’une fenêtre contextuelle modale à partirC#du code serveur () | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Toutefois, certains scénarios requièrent que t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: fec0ce2cdd24333f65201301718440e1a09d930e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613294"
---
# <a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="bdc9c-104">Lancement d’une fenêtre contextuelle modale à partir de code serveur (C#)</span><span class="sxs-lookup"><span data-stu-id="bdc9c-104">Launching a Modal Popup Window from Server Code (C#)</span></span>

<span data-ttu-id="bdc9c-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bdc9c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bdc9c-106">[Télécharger le code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bdc9c-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="bdc9c-107">Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="bdc9c-108">Toutefois, certains scénarios requièrent que l’ouverture de la fenêtre contextuelle modale soit déclenchée côté serveur.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="overview"></a><span data-ttu-id="bdc9c-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="bdc9c-109">Overview</span></span>

<span data-ttu-id="bdc9c-110">Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="bdc9c-111">Toutefois, certains scénarios requièrent que l’ouverture de la fenêtre contextuelle modale soit déclenchée côté serveur.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="bdc9c-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="bdc9c-112">Steps</span></span>

<span data-ttu-id="bdc9c-113">Tout d’abord, un contrôle Web ASP.NET Button est requis pour illustrer le fonctionnement du contrôle ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="bdc9c-114">Ajoutez un bouton de ce type dans le &lt;formulaire&gt; élément sur une nouvelle page :</span><span class="sxs-lookup"><span data-stu-id="bdc9c-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="bdc9c-115">Ensuite, vous avez besoin du balisage pour la fenêtre contextuelle que vous souhaitez créer.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="bdc9c-116">Définissez-le en tant que contrôle de `<asp:Panel>` et assurez-vous qu’il comprend un contrôle bouton.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="bdc9c-117">Le contrôle ModalPopup offre la fonctionnalité permettant de fermer un bouton de ce type dans la fenêtre contextuelle. dans le cas contraire, il n’existe aucun moyen simple de la laisser disparaître.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="bdc9c-118">Ajoutez ensuite le contrôle ModalPopup de ASP.NET AJAX Toolkit à la page.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="bdc9c-119">Définissez les propriétés du bouton qui charge le contrôle, le bouton qui le fait disparaître et l’ID de la fenêtre contextuelle réelle.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="bdc9c-120">Comme avec toutes les pages Web basées sur ASP.NET AJAX ; le gestionnaire de scripts est nécessaire pour charger les bibliothèques JavaScript nécessaires pour les différents navigateurs cibles :</span><span class="sxs-lookup"><span data-stu-id="bdc9c-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="bdc9c-121">Exécutez l’exemple dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-121">Run the example in the browser.</span></span> <span data-ttu-id="bdc9c-122">Lorsque vous cliquez sur le bouton, la fenêtre contextuelle modale s’affiche.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="bdc9c-123">Pour obtenir le même effet à l’aide du code côté serveur, un nouveau bouton est requis :</span><span class="sxs-lookup"><span data-stu-id="bdc9c-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="bdc9c-124">Comme vous pouvez le voir, un clic sur le bouton génère une publication et exécute la méthode `ServerButton_Click()` sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="bdc9c-125">Dans cette méthode, une fonction JavaScript appelée `launchModal()` est exécutée comme exacte, la fonction JavaScript est exécutée une fois que la page a été chargée :</span><span class="sxs-lookup"><span data-stu-id="bdc9c-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="bdc9c-126">La tâche de `launchModal()` consiste à afficher ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="bdc9c-127">La fonction `launchModal()` est exécutée une fois que la page HTML complète a été chargée.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="bdc9c-128">Toutefois, à ce stade, l’infrastructure AJAX ASP.NET n’a pas encore été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="bdc9c-129">Par conséquent, la fonction `launchModal()` définit simplement une variable sur laquelle le contrôle ModalPopup doit être affiché plus tard :</span><span class="sxs-lookup"><span data-stu-id="bdc9c-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="bdc9c-130">La fonction JavaScript `pageLoad()` est une fonction spéciale qui est exécutée une fois ASP.NET AJAX entièrement chargé.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="bdc9c-131">Par conséquent, nous ajoutons du code à cette fonction pour afficher le contrôle ModalPopup, mais uniquement si `launchModal()` a été appelée avant :</span><span class="sxs-lookup"><span data-stu-id="bdc9c-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="bdc9c-132">La fonction `$find()` recherche un élément nommé sur la page et attend l’ID côté serveur en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="bdc9c-133">Par conséquent, `$find("mpe")` retourne la représentation cliente du contrôle ModalPopup ; sa méthode `show()` permet d’afficher la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="bdc9c-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>

<span data-ttu-id="bdc9c-134">[![la fenêtre contextuelle modale s’affiche lorsque vous cliquez sur l’un des boutons](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bdc9c-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="bdc9c-135">La fenêtre contextuelle modale s’affiche lorsque l’utilisateur clique sur l’un des boutons ([cliquez pour afficher l’image en taille réelle](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bdc9c-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bdc9c-136">Next</span><span class="sxs-lookup"><span data-stu-id="bdc9c-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
