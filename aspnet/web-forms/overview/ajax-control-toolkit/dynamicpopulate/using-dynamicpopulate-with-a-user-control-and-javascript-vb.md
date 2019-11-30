---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Utilisation de DynamicPopulate avec un contrôle utilisateur et JavaScript (VB) | Microsoft Docs
author: wenz
description: Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: ee5923ad6d8b101f689a0564aef8b1e0e00a7639
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599143"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="bfd2a-103">Utilisation de DynamicPopulate avec un contrôle utilisateur et JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="bfd2a-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>

<span data-ttu-id="bfd2a-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bfd2a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bfd2a-105">[Télécharger le code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="bfd2a-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="bfd2a-106">Le contrôle DynamicPopulate dans la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="bfd2a-107">Il est également possible de déclencher le remplissage à l’aide de code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="bfd2a-108">Toutefois, une attention particulière doit être prise lorsque l’extendeur réside dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-108">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="overview"></a><span data-ttu-id="bfd2a-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="bfd2a-109">Overview</span></span>

<span data-ttu-id="bfd2a-110">Le contrôle `DynamicPopulate` de la boîte à outils de contrôle ASP.NET AJAX appelle un service Web (ou une méthode de page) et remplit la valeur résultante dans un contrôle cible sur la page, sans actualisation de page.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="bfd2a-111">Il est également possible de déclencher le remplissage à l’aide de code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="bfd2a-112">Toutefois, une attention particulière doit être prise lorsque l’extendeur réside dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="bfd2a-113">Étapes</span><span class="sxs-lookup"><span data-stu-id="bfd2a-113">Steps</span></span>

<span data-ttu-id="bfd2a-114">Tout d’abord, vous avez besoin d’un service Web ASP.NET qui implémente la méthode à appeler par le contrôle `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="bfd2a-115">Le service Web implémente la méthode `getDate()` qui attend un argument de type String, appelée `contextKey`, puisque le contrôle `DynamicPopulate` envoie une partie des informations de contexte avec chaque appel de service Web.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="bfd2a-116">Voici le code (fichiers `DynamicPopulate.vb.asmx`) qui récupère la date actuelle dans l’un des trois formats suivants :</span><span class="sxs-lookup"><span data-stu-id="bfd2a-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="bfd2a-117">À l’étape suivante, créez un nouveau contrôle utilisateur (fichier`.ascx`), indiqué par la déclaration suivante sur sa première ligne :</span><span class="sxs-lookup"><span data-stu-id="bfd2a-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="bfd2a-118">Un &lt;`label`élément &gt; sera utilisé pour afficher les données provenant du serveur.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="bfd2a-119">Dans le fichier de contrôle utilisateur, nous allons également utiliser trois cases d’option, chacune représentant l’un des trois formats de date possibles pris en charge par le service Web.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="bfd2a-120">Quand l’utilisateur clique sur l’une des cases d’option, le navigateur exécute du code JavaScript qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="bfd2a-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="bfd2a-121">Ce code permet d’accéder à la `DynamicPopulateExtender` (ne vous inquiétez pas encore de l’ID étrange, cette opération sera traitée plus tard) et déclenchera le remplissage dynamique avec les données.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="bfd2a-122">Dans le contexte de la case d’option active, `this.value` fait référence à sa valeur qui est `format1`, `format2` ou `format3` exactement ce que la méthode Web attend.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="bfd2a-123">La seule chose manquante dans le contrôle utilisateur est le contrôle `DynamicPopulateExtender` qui lie les cases d’option au service Web.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="bfd2a-124">Là encore, vous pouvez noter l’ID étrange utilisé dans le contrôle : `mcd1$myDate` au lieu de `myDate`.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="bfd2a-125">Précédemment, le code JavaScript utilisé `mcd1_dpe1` pour accéder au `DynamicPopulateExtender` au lieu de `dpe1`. Cette stratégie de nommage est une exigence spéciale lors de l’utilisation de `DynamicPopulateExtender` dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="bfd2a-126">En outre, vous devez incorporer le contrôle utilisateur d’une manière spécifique pour le faire fonctionner.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-126">Furthermore, you have to embed the user control in a specific way to make it all work.</span></span> <span data-ttu-id="bfd2a-127">Créez une nouvelle page ASP.NET et enregistrez un préfixe de balise pour le contrôle utilisateur que vous venez d’implémenter :</span><span class="sxs-lookup"><span data-stu-id="bfd2a-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="bfd2a-128">Incluez ensuite le contrôle de `ScriptManager` AJAX ASP.NET sur la nouvelle page :</span><span class="sxs-lookup"><span data-stu-id="bfd2a-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="bfd2a-129">Enfin, ajoutez le contrôle utilisateur à la page.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="bfd2a-130">Il vous suffit de définir son attribut `ID` (et `runat="server"`, bien évidemment), mais vous devez également le définir sur un nom spécifique : `mcd1` puisque c’est le préfixe utilisé dans le contrôle utilisateur pour y accéder à l’aide de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="bfd2a-131">Et voilà !</span><span class="sxs-lookup"><span data-stu-id="bfd2a-131">And that's it!</span></span> <span data-ttu-id="bfd2a-132">La page se comporte comme prévu : un utilisateur clique sur l’une des cases d’option, le contrôle de la boîte à outils appelle le service Web et affiche la date actuelle au format souhaité.</span><span class="sxs-lookup"><span data-stu-id="bfd2a-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>

<span data-ttu-id="bfd2a-133">[![les cases d’option résident dans un contrôle utilisateur](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bfd2a-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="bfd2a-134">Les cases d’option résident dans un contrôle utilisateur ([cliquez pour afficher l’image en taille réelle](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bfd2a-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bfd2a-135">Précédent</span><span class="sxs-lookup"><span data-stu-id="bfd2a-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
