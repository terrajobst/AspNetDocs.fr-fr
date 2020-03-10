---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Afficher les détails de l’élément | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557322"
---
# <a name="display-item-details"></a><span data-ttu-id="7ba93-102">Afficher les détails des éléments</span><span class="sxs-lookup"><span data-stu-id="7ba93-102">Display Item Details</span></span>

<span data-ttu-id="7ba93-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7ba93-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7ba93-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="7ba93-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="7ba93-105">Dans cette section, vous allez ajouter la possibilité d’afficher les détails de chaque livre.</span><span class="sxs-lookup"><span data-stu-id="7ba93-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="7ba93-106">Dans App. js, ajoutez le code suivant au modèle de vue :</span><span class="sxs-lookup"><span data-stu-id="7ba93-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="7ba93-107">Dans views/domotique/index. cshtml, ajoutez un élément de liaison de données au lien Détails :</span><span class="sxs-lookup"><span data-stu-id="7ba93-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="7ba93-108">Cela lie le gestionnaire de clics pour le &lt;un élément&gt; à la fonction `getBookDetail` sur le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="7ba93-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="7ba93-109">Dans le même fichier, remplacez la marque suivante :</span><span class="sxs-lookup"><span data-stu-id="7ba93-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="7ba93-110">par le code :</span><span class="sxs-lookup"><span data-stu-id="7ba93-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="7ba93-111">Ce balisage crée une table qui est liée aux données des propriétés du `detail` observable dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="7ba93-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="7ba93-112">La syntaxe «&lt;!--Ko--&gt;&quot; vous permet d’inclure une liaison Knockout en dehors d’un élément DOM.</span><span class="sxs-lookup"><span data-stu-id="7ba93-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="7ba93-113">Dans ce cas, la liaison de `if` entraîne l’affichage de cette section du balisage uniquement lorsque `details` n’est pas null.</span><span class="sxs-lookup"><span data-stu-id="7ba93-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="7ba93-114">Maintenant, si vous exécutez l’application et cliquez sur l’une des &quot;détails&quot; les liens, l’application affiche les détails du livre.</span><span class="sxs-lookup"><span data-stu-id="7ba93-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="7ba93-115">[Précédent](part-7.md)
> [Suivant](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="7ba93-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
