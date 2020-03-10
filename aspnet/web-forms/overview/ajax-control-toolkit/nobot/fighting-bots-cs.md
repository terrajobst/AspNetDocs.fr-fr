---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Lutte contre lesC#robots () | Microsoft Docs
author: wenz
description: Les robots automatisés mettent en plâtre des journaux et d’autres sites Web avec spam, en envoyant des formulaires de commentaires sans aucune intervention de l’utilisateur. Le contrôle NoBot dans le ASP.NET AJAX con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: fef55edf12a024e4dd66e2a18ea371ab4dac861f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627413"
---
# <a name="fighting-bots-c"></a><span data-ttu-id="266b6-104">Lutte contre les robots (C#)</span><span class="sxs-lookup"><span data-stu-id="266b6-104">Fighting Bots (C#)</span></span>

<span data-ttu-id="266b6-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="266b6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="266b6-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="266b6-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="266b6-107">Les robots automatisés mettent en plâtre des journaux et d’autres sites Web avec spam, en envoyant des formulaires de commentaires sans aucune intervention de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="266b6-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="266b6-108">Le contrôle NoBot dans ASP.NET AJAX Control Toolkit peut aider à combattre ces robots.</span><span class="sxs-lookup"><span data-stu-id="266b6-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="overview"></a><span data-ttu-id="266b6-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="266b6-109">Overview</span></span>

<span data-ttu-id="266b6-110">Les robots automatisés mettent en plâtre des journaux et d’autres sites Web avec spam, en envoyant des formulaires de commentaires sans aucune intervention de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="266b6-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="266b6-111">Le contrôle NoBot dans ASP.NET AJAX Control Toolkit peut aider à combattre ces robots.</span><span class="sxs-lookup"><span data-stu-id="266b6-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="266b6-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="266b6-112">Steps</span></span>

<span data-ttu-id="266b6-113">Une approche courante pour empêcher les robots est d’utiliser des tests Turing publics entièrement automatisés pour signaler les ordinateurs et les êtres humains.</span><span class="sxs-lookup"><span data-stu-id="266b6-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="266b6-114">Un test Turing était à l’origine un test dans lequel quelqu’un devait décider si un partenaire de communication est un homme ou un ordinateur.</span><span class="sxs-lookup"><span data-stu-id="266b6-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="266b6-115">Sur le Web, un CAPTCHA se compose généralement d’une image avec des lettres distordues.</span><span class="sxs-lookup"><span data-stu-id="266b6-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="266b6-116">L’idée est que seul un humain peut lire les lettres sur l’image, alors que les algorithmes OCR échouent.</span><span class="sxs-lookup"><span data-stu-id="266b6-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="266b6-117">Cette approche présente plusieurs avantages et inconvénients, mais une discussion n’entre pas dans le cadre de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="266b6-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="266b6-118">Il existe toutefois un contrôle dans la boîte à outils de contrôle ASP.NET AJAX qui offre une approche similaire : `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="266b6-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="266b6-119">Il est plus facile à surmonter qu’un CAPTCHA, mais il est très facile à utiliser et les tarifs sont extrêmement bien sur les sites Web tels que les blogs où il est considéré comme un succès si la plupart des tentatives de courrier indésirable échouent, ce que le contrôle `NoBot` peut faire.</span><span class="sxs-lookup"><span data-stu-id="266b6-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="266b6-120">`NoBot` intercepte la publication du formulaire Web ASP.NET en cours si au moins l’une de ces conditions est remplie :</span><span class="sxs-lookup"><span data-stu-id="266b6-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="266b6-121">Le navigateur ne parvient pas à résoudre un puzzle JavaScript (par exemple, lorsque JavaScript est désactivé)</span><span class="sxs-lookup"><span data-stu-id="266b6-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="266b6-122">L’utilisateur a soumis le formulaire à rapide</span><span class="sxs-lookup"><span data-stu-id="266b6-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="266b6-123">L’adresse IP du client a envoyé le formulaire trop souvent dans un laps de temps donné.</span><span class="sxs-lookup"><span data-stu-id="266b6-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="266b6-124">Pour vérifier ces conditions, le contrôle `NoBot` requiert ces attributs (tous facultatifs) :</span><span class="sxs-lookup"><span data-stu-id="266b6-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="266b6-125">`ResponseMinimumDelaySeconds` le nombre minimal de secondes entre les publications</span><span class="sxs-lookup"><span data-stu-id="266b6-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="266b6-126">`CutoffWindowSeconds` durée de l’intervalle de temps pendant laquelle les publications d’une adresse IP sont des mesures</span><span class="sxs-lookup"><span data-stu-id="266b6-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="266b6-127">`CutoffMaximumInstances` nombre maximal de secondes par intervalle de temps</span><span class="sxs-lookup"><span data-stu-id="266b6-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="266b6-128">La balise suivante exige qu’au moins deux secondes s’écoulent entre les publications et qu’il n’y ait que cinq publications (postbacks) ou moins dans un intervalle de 30 secondes :</span><span class="sxs-lookup"><span data-stu-id="266b6-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="266b6-129">Puis, comme d’habitude, veillez à inclure la `ScriptManager` dans la page afin que la bibliothèque AJAX ASP.NET soit chargée et que la boîte à outils de contrôle puisse être utilisée :</span><span class="sxs-lookup"><span data-stu-id="266b6-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="266b6-130">Dans la mesure où la plupart des vérifications `NoBot` se produisent côté serveur, vous devez vérifier le résultat de ces validations.</span><span class="sxs-lookup"><span data-stu-id="266b6-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="266b6-131">Pour ce faire, vous pouvez appeler la méthode `IsValid()` de `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="266b6-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="266b6-132">Elle possède un argument (en tant que paramètre `out``ByRef` paramètre) qui est de type `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="266b6-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="266b6-133">Sa représentation sous forme de chaîne contient la raison de l’échec de la vérification et `Valid` dans le cas contraire.</span><span class="sxs-lookup"><span data-stu-id="266b6-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="266b6-134">Le code suivant génère un message en fonction du résultat de `NoBot`:</span><span class="sxs-lookup"><span data-stu-id="266b6-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="266b6-135">Enfin, vous avez besoin d’un formulaire à envoyer et d’un élément label pour générer le message, et vous avez terminé !</span><span class="sxs-lookup"><span data-stu-id="266b6-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="266b6-136">Quand vous exécutez ce script et que vous désactivez JavaScript ou que vous envoyez le formulaire au cours des deux premières secondes ou que vous envoyez le formulaire sept fois dans les trente secondes, vous recevez un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="266b6-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="266b6-137">Toutefois, utilisez ce contrôle avec prudence, car seulement environ 90-95% des utilisateurs ont activé JavaScript, par conséquent 5-10% des utilisateurs échoueront `NoBot`test.</span><span class="sxs-lookup"><span data-stu-id="266b6-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>

<span data-ttu-id="266b6-138">[![ce message d’erreur a pu être provoqué par un bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="266b6-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="266b6-139">Ce message d’erreur peut avoir été provoqué par un bot ([cliquez pour afficher l’image en taille réelle](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="266b6-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="266b6-140">Next</span><span class="sxs-lookup"><span data-stu-id="266b6-140">Next</span></span>](fighting-bots-vb.md)
