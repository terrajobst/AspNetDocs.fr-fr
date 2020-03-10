---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Comment faire : créer un programme d’assistance HTML personnalisé pour une application MVC | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo, Chris pixels montre comment créer un HtmlHelper personnalisé qui n’est pas disponible dans l’ensemble standard d’une application MVC. Tout d’abord, un exemple d’appliances MVC...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 60953243d3038667e4f729b1394e68f0c9d7c178
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559044"
---
# <a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="656ae-105">Comment faire : créer un programme d’assistance HTML personnalisé pour une application MVC</span><span class="sxs-lookup"><span data-stu-id="656ae-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>

<span data-ttu-id="656ae-106">par [Chris pixels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="656ae-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="656ae-107">Dans cette vidéo, Chris pixels montre comment créer un HtmlHelper personnalisé qui n’est pas disponible dans l’ensemble standard d’une application MVC.</span><span class="sxs-lookup"><span data-stu-id="656ae-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="656ae-108">Tout d’abord, un exemple d’application MVC est créé avec un contrôleur et une vue de démonstration pour tester le HtmlHelper personnalisé.</span><span class="sxs-lookup"><span data-stu-id="656ae-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="656ae-109">Ensuite, un module est créé avec une fonction publique qui est une méthode d’extension qui représente l’implémentation du HtmlHelper personnalisé.</span><span class="sxs-lookup"><span data-stu-id="656ae-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="656ae-110">L’application auxiliaire personnalisée permet de créer des balises `<img>` dans une page et reçoit plusieurs paramètres entrants, y compris l’ID, l’URL et le texte de remplacement pour la balise d’image.</span><span class="sxs-lookup"><span data-stu-id="656ae-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="656ae-111">La logique est ensuite ajoutée à la fonction pour retourner la balise `<img>` terminée avec les informations spécifiées.</span><span class="sxs-lookup"><span data-stu-id="656ae-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="656ae-112">Le HtmlHelper personnalisé est ensuite utilisé sur la page de démonstration pour afficher une image.</span><span class="sxs-lookup"><span data-stu-id="656ae-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="656ae-113">Enfin, le HtmlHelper personnalisé est développé pour inclure plusieurs substitutions de constructeur qui offrent une certaine flexibilité pour créer plus facilement différentes balises de `<img>`.</span><span class="sxs-lookup"><span data-stu-id="656ae-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="656ae-114">&#9654;Regarder la vidéo (18 minutes)</span><span class="sxs-lookup"><span data-stu-id="656ae-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="656ae-115">[Précédent](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Suivant](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="656ae-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
