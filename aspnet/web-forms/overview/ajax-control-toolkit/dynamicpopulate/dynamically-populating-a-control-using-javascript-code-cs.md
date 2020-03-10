---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Remplissage dynamique d’un contrôle à l’aide deC#code JavaScript () | Microsoft Docs
author: wenz
description: Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535741"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="57f54-103">Remplissage dynamique d’un contrôle avec du code JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="57f54-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>

<span data-ttu-id="57f54-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="57f54-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="57f54-105">[Télécharger le code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="57f54-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="57f54-106">Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page.</span><span class="sxs-lookup"><span data-stu-id="57f54-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="57f54-107">Il est également possible de déclencher le remplissage à l’aide de code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="57f54-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="57f54-108">Présentation</span><span class="sxs-lookup"><span data-stu-id="57f54-108">Overview</span></span>

<span data-ttu-id="57f54-109">Le contrôle `DynamicPopulate` de la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page.</span><span class="sxs-lookup"><span data-stu-id="57f54-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="57f54-110">Il est également possible de déclencher le remplissage à l’aide de code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="57f54-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="57f54-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="57f54-111">Steps</span></span>

<span data-ttu-id="57f54-112">Tout d’abord, vous avez besoin d’un service Web ASP.NET qui implémente la méthode à appeler par le contrôle `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="57f54-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="57f54-113">Le service Web implémente la méthode `getDate()` qui attend un argument de type String, appelée `contextKey`, puisque le contrôle `DynamicPopulate` envoie une partie des informations de contexte avec chaque appel de service Web.</span><span class="sxs-lookup"><span data-stu-id="57f54-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="57f54-114">Voici le code (`DynamicPopulate.cs.asmx`de fichier) qui récupère la date actuelle dans l’un des trois formats suivants :</span><span class="sxs-lookup"><span data-stu-id="57f54-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="57f54-115">À l’étape suivante, créez un nouveau site ASP.NET et commencez avec le contrôle ASP.NET AJAX ScriptManager :</span><span class="sxs-lookup"><span data-stu-id="57f54-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="57f54-116">Ensuite, ajoutez un contrôle Label (par exemple à l’aide du contrôle HTML du même nom ou du contrôle Web `<asp:Label />`) qui affichera par la suite le résultat de l’appel du service Web.</span><span class="sxs-lookup"><span data-stu-id="57f54-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="57f54-117">Ensuite, incluez un contrôle de `DynamicPopulateExtender` et fournissez des informations sur le service Web, le contrôle cible, mais pas le nom du contrôle qui déclenche le remplissage que cette opération sera effectuée ultérieurement, à l’aide de JavaScript personnalisé !</span><span class="sxs-lookup"><span data-stu-id="57f54-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="57f54-118">Maintenant, à la partie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="57f54-118">Now to the JavaScript part.</span></span> <span data-ttu-id="57f54-119">La fonction `$find()`, définie par la bibliothèque AJAX ASP.NET, retourne une référence aux objets côté serveur de la boîte à outils de contrôle ASP.NET AJAX, comme `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="57f54-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="57f54-120">Dans le fichier actif, `$find("dpe")` retourne une référence à l’un des `DynamicPopulateExtender` contrôle dans la page.</span><span class="sxs-lookup"><span data-stu-id="57f54-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="57f54-121">Il expose une méthode appelée `populate()` qui déclenche le processus de remplissage dynamique.</span><span class="sxs-lookup"><span data-stu-id="57f54-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="57f54-122">La méthode `populate()` nécessite un argument : la clé de contexte qui servira d’argument à la méthode Web `getDate()`.</span><span class="sxs-lookup"><span data-stu-id="57f54-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="57f54-123">Par exemple, `$find("dpe").populate("format1")` remplit l’étiquette avec la date actuelle au format mois-jour-année.</span><span class="sxs-lookup"><span data-stu-id="57f54-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="57f54-124">Pour que l’exemple soit un peu plus flexible, l’utilisateur peut désormais choisir entre plusieurs formats de date.</span><span class="sxs-lookup"><span data-stu-id="57f54-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="57f54-125">Pour chacun d’eux, une case d’option est affichée.</span><span class="sxs-lookup"><span data-stu-id="57f54-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="57f54-126">Une fois que l’utilisateur clique sur une case d’option, le code JavaScript remplit dynamiquement l’étiquette avec le format de date sélectionné.</span><span class="sxs-lookup"><span data-stu-id="57f54-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="57f54-127">Les cases d’option sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="57f54-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="57f54-128">Notez que dans le contexte d’une case d’option, l’expression JavaScript `this.value` fait référence à la valeur du bouton actuel, qui se présente exactement les mêmes informations que celles que la méthode `getDate()` peut utiliser.</span><span class="sxs-lookup"><span data-stu-id="57f54-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>

<span data-ttu-id="57f54-129">[![un clic sur le bouton récupère la date du serveur, au format spécifié](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="57f54-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="57f54-130">Un clic sur le bouton permet de récupérer la date du serveur, au format spécifié ([cliquez pour afficher l’image en taille réelle](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="57f54-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="57f54-131">[Précédent](dynamically-populating-a-control-cs.md)
> [Suivant](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="57f54-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
