---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Comment faire :] Contrôler la mise en cache d’une page ASP.NET basée sur des informations personnalisées | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo, Chris pixels montre comment contrôler les critères de mise en cache d’une page ASP.NET en fonction d’informations personnalisées. Une page d’exemple est créée, puis l’O...
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: 676bc8f234a6e517104d07fd58a0ff14aec73e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624732"
---
# <a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="f6718-104">[Comment faire :] Contrôler la mise en cache d’une page ASP.NET basée sur des informations personnalisées</span><span class="sxs-lookup"><span data-stu-id="f6718-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>

<span data-ttu-id="f6718-105">par [Chris pixels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="f6718-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="f6718-106">Dans cette vidéo, Chris pixels montre comment contrôler les critères de mise en cache d’une page ASP.NET en fonction d’informations personnalisées.</span><span class="sxs-lookup"><span data-stu-id="f6718-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="f6718-107">Une page d’exemple est créée, puis la directive OutputCache est utilisée avec l’attribut VaryByCustom qui contient une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="f6718-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="f6718-108">Ensuite, la méthode GetVaryCustomByString () est remplacée dans le module global. asax qui fournit la gestion de l’attribut personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f6718-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="f6718-109">Dans cette méthode, une chaîne est retournée qui identifie de façon unique la version mise en cache de la page.</span><span class="sxs-lookup"><span data-stu-id="f6718-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="f6718-110">Enfin, il existe une discussion sur la façon dont la mise en cache à l’aide d’une valeur personnalisée peut être utilisée de plusieurs façons pour un site Web.</span><span class="sxs-lookup"><span data-stu-id="f6718-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="f6718-111">&#9654;Regarder la vidéo (12 minutes)</span><span class="sxs-lookup"><span data-stu-id="f6718-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
