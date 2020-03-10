---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Remplissage dynamique d’un contrôle (C#) | Microsoft Docs
author: wenz
description: Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 24f88e44e0f878127314774d4e8846f80133413e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535804"
---
# <a name="dynamically-populating-a-control-c"></a><span data-ttu-id="3375c-103">Remplissage dynamique d’un contrôle (C#)</span><span class="sxs-lookup"><span data-stu-id="3375c-103">Dynamically Populating a Control (C#)</span></span>

<span data-ttu-id="3375c-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3375c-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3375c-105">[Télécharger le code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3375c-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="3375c-106">Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page.</span><span class="sxs-lookup"><span data-stu-id="3375c-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>

## <a name="overview"></a><span data-ttu-id="3375c-107">Présentation</span><span class="sxs-lookup"><span data-stu-id="3375c-107">Overview</span></span>

<span data-ttu-id="3375c-108">Le contrôle `DynamicPopulate` de la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page.</span><span class="sxs-lookup"><span data-stu-id="3375c-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="3375c-109">Ce didacticiel montre comment le configurer.</span><span class="sxs-lookup"><span data-stu-id="3375c-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="3375c-110">Étapes</span><span class="sxs-lookup"><span data-stu-id="3375c-110">Steps</span></span>

<span data-ttu-id="3375c-111">Tout d’abord, vous avez besoin d’un service Web ASP.NET qui implémente la méthode à appeler par `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="3375c-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="3375c-112">La classe de service Web requiert l’attribut `ScriptService` qui est défini dans `Microsoft.Web.Script.Services`; Sinon, ASP.NET AJAX ne peut pas créer le proxy JavaScript côté client pour le service Web, qui est à son tour requis par `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="3375c-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="3375c-113">La méthode Web doit s’attendre à un argument de type String, appelé `contextKey`, puisque le contrôle `DynamicPopulate` envoie une partie des informations de contexte avec chaque appel de service Web.</span><span class="sxs-lookup"><span data-stu-id="3375c-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="3375c-114">Le service Web suivant retourne la date actuelle dans un format représenté par l’argument `contextKey` :</span><span class="sxs-lookup"><span data-stu-id="3375c-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="3375c-115">Le service Web est ensuite enregistré en tant que `DynamicPopulate.cs.asmx`.</span><span class="sxs-lookup"><span data-stu-id="3375c-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="3375c-116">Vous pouvez également implémenter la méthode `getDate()` en tant que méthode de page dans la page ASP.NET réelle avec le contrôle `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="3375c-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="3375c-117">À l’étape suivante, créez un nouveau fichier ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3375c-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="3375c-118">Comme toujours, la première étape consiste à inclure la `ScriptManager` dans la page actuelle pour charger la bibliothèque AJAX ASP.NET et pour faire fonctionner le jeu d’outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="3375c-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="3375c-119">Ensuite, ajoutez un contrôle Label (par exemple à l’aide du contrôle HTML du même nom, ou le &lt;`asp:Label` /&gt; contrôle Web) qui affichera ultérieurement le résultat de l’appel de service Web.</span><span class="sxs-lookup"><span data-stu-id="3375c-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="3375c-120">Un bouton HTML (comme un contrôle HTML, puisque nous n’avons pas besoin d’une publication sur le serveur) sera ensuite utilisé pour déclencher le remplissage dynamique :</span><span class="sxs-lookup"><span data-stu-id="3375c-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="3375c-121">Enfin, nous avons besoin du contrôle `DynamicPopulateExtender` pour relier les choses.</span><span class="sxs-lookup"><span data-stu-id="3375c-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="3375c-122">Les attributs suivants seront définis (en plus de ceux qui sont évidents, `ID` et `runat`=`"server"`) :</span><span class="sxs-lookup"><span data-stu-id="3375c-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="3375c-123">`TargetControlID` où placer le résultat de l’appel de service Web</span><span class="sxs-lookup"><span data-stu-id="3375c-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="3375c-124">`ServicePath` chemin d’accès au service Web (omettre si vous souhaitez utiliser une méthode de page)</span><span class="sxs-lookup"><span data-stu-id="3375c-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="3375c-125">`ServiceMethod` le nom de la méthode Web ou de la méthode de page</span><span class="sxs-lookup"><span data-stu-id="3375c-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="3375c-126">`ContextKey` les informations de contexte à envoyer au service Web</span><span class="sxs-lookup"><span data-stu-id="3375c-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="3375c-127">`PopulateTriggerControlID` élément qui déclenche l’appel de service Web</span><span class="sxs-lookup"><span data-stu-id="3375c-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="3375c-128">`ClearContentsDuringUpdate` s’il faut vider l’élément cible pendant l’appel du service Web</span><span class="sxs-lookup"><span data-stu-id="3375c-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="3375c-129">Comme vous pouvez le voir, le contrôle nécessite des informations, mais tout mettre en place est tout à fait simple.</span><span class="sxs-lookup"><span data-stu-id="3375c-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="3375c-130">Voici le balisage du contrôle `DynamicPopulateExtender` dans le scénario actuel :</span><span class="sxs-lookup"><span data-stu-id="3375c-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="3375c-131">Exécutez la page ASP.NET dans le navigateur, puis cliquez sur le bouton. vous recevrez la date actuelle au format mois-jour-année.</span><span class="sxs-lookup"><span data-stu-id="3375c-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>

<span data-ttu-id="3375c-132">[![un clic sur le bouton récupère la date à partir du serveur](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3375c-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="3375c-133">Un clic sur le bouton permet de récupérer la date à partir du serveur ([cliquez pour afficher l’image en taille réelle](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3375c-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3375c-134">Next</span><span class="sxs-lookup"><span data-stu-id="3375c-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
