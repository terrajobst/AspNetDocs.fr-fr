---
uid: web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
title: 'Comment faire : utiliser des URL dans le routage ASP.NET ? | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo, Chris pixels montre comment spécifier les URL d’un site Web qui utilise le routage ASP.NET. Tout d’abord, un site Web est créé et le routage est défini dans le GL...
ms.author: riande
ms.date: 10/15/2010
ms.assetid: 08f9d0a7-cfa0-4914-a672-8a64295d7ba8
msc.legacyurl: /web-forms/videos/net-4/routing/how-do-i-work-with-urls-in-aspnet-routing
msc.type: video
ms.openlocfilehash: eb1060fd9cc9469dc2b1d2e918823316c36840cb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78565127"
---
# <a name="how-do-i-work-with-urls-in-aspnet-routing"></a><span data-ttu-id="8eb18-105">Comment faire : utiliser des URL dans le routage ASP.NET ?</span><span class="sxs-lookup"><span data-stu-id="8eb18-105">How Do I: Work with URLs in ASP.NET Routing?</span></span>

<span data-ttu-id="8eb18-106">par [Chris pixels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="8eb18-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="8eb18-107">Dans cette vidéo, Chris pixels montre comment spécifier les URL d’un site Web qui utilise le routage ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8eb18-107">In this video Chris Pels shows how to specify URLs in a web site that utilizes ASP.NET routing.</span></span> <span data-ttu-id="8eb18-108">Tout d’abord, un site Web est créé et le routage est défini dans la classe d’application globale (. asax).</span><span class="sxs-lookup"><span data-stu-id="8eb18-108">First, a web site is created and routing is defined in the Global Application Class (.asax).</span></span> <span data-ttu-id="8eb18-109">Ensuite, un exemple de page Web est créé et une URL basée sur un itinéraire défini est ajoutée à la page à l’aide de l’approche standard « codée en dur », par exemple « ~/Stats/Visitors ».</span><span class="sxs-lookup"><span data-stu-id="8eb18-109">Next, a sample web page is created and a URL based upon a defined route is added to the page using the standard "hard coded" approach, e.g., "~/Stats/Visitors".</span></span> <span data-ttu-id="8eb18-110">Un autre lien est ensuite ajouté à la page, ce qui génère dynamiquement la même URL dans le balisage à l’aide de la méthode RouteValue qui accepte le nom et les paramètres de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="8eb18-110">Another link is then added to the page which dynamically generates the same URL in markup using the RouteValue method which accepts the route name and parameters.</span></span> <span data-ttu-id="8eb18-111">La même URL est ensuite implémentée à l’aide du code plutôt que du balisage directement dans la page.</span><span class="sxs-lookup"><span data-stu-id="8eb18-111">The same URL is then implemented using code rather than markup directly in the page.</span></span> <span data-ttu-id="8eb18-112">L’itinéraire d’origine et l’emplacement de la page physique sont ensuite modifiés, ce qui signifie que le lien codé en dur ne fonctionne plus, tandis que les liens générés dynamiquement fonctionnent correctement.</span><span class="sxs-lookup"><span data-stu-id="8eb18-112">The original route and physical page location are then changed, resulting in the hard coded link no longer working whereas both dynamically generated links function properly.</span></span> <span data-ttu-id="8eb18-113">Enfin, la valeur des liens générés de manière dynamique est ensuite abordée.</span><span class="sxs-lookup"><span data-stu-id="8eb18-113">Finally, the value of dynamically generated links is then discussed.</span></span>

[<span data-ttu-id="8eb18-114">&#9654;Regarder la vidéo (20 minutes)</span><span class="sxs-lookup"><span data-stu-id="8eb18-114">&#9654; Watch video (20 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-work-with-urls-in-aspnet-routing)

> [!div class="step-by-step"]
> [<span data-ttu-id="8eb18-115">Précédent</span><span class="sxs-lookup"><span data-stu-id="8eb18-115">Previous</span></span>](how-do-i-use-routing-with-aspnet-web-forms.md)
