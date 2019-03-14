---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Comment faire] Contrôle la mise en cache d’une Page ASP.NET en fonction des informations personnalisées | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo Chris Pels montre comment contrôler les critères pour la mise en cache une page ASP.NET en fonction des informations personnalisées. Un exemple de page est créée et ensuite l’o...
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: a9ed2baad3460441bc57d97bf74f6de5977db0c9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032226"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="c6719-104">[Comment faire] Contrôle la mise en cache d’une Page ASP.NET en fonction des informations personnalisées</span><span class="sxs-lookup"><span data-stu-id="c6719-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="c6719-105">par [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="c6719-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="c6719-106">Dans cette vidéo Chris Pels montre comment contrôler les critères pour la mise en cache une page ASP.NET en fonction des informations personnalisées.</span><span class="sxs-lookup"><span data-stu-id="c6719-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="c6719-107">Un exemple de page est créé et ensuite la directive OutputCache est utilisée avec l’attribut VaryByCustom qui contient une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="c6719-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="c6719-108">Ensuite, la méthode GetVaryCustomByString() est remplacée dans le module de global.asax, qui fournit la gestion de l’attribut personnalisé.</span><span class="sxs-lookup"><span data-stu-id="c6719-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="c6719-109">Dans cette méthode, une chaîne est retournée qui identifie de façon unique la version mise en cache de la page.</span><span class="sxs-lookup"><span data-stu-id="c6719-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="c6719-110">Enfin, il existe une discussion sur la façon dont la mise en cache à l’aide d’une valeur personnalisée peut servir de plusieurs façons pour un site web.</span><span class="sxs-lookup"><span data-stu-id="c6719-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="c6719-111">&#9654;Regardez la vidéo (12 minutes)</span><span class="sxs-lookup"><span data-stu-id="c6719-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
