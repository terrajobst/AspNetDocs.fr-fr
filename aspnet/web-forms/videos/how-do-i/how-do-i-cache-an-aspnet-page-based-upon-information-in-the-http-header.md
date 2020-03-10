---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[Comment faire :]  Mettre en cache une page ASP.NET en fonction des informations contenues dans l’en-tête HTTP | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo, Chris pixels montre comment conserver une page dans le cache de sortie ASP.NET en fonction des informations contenues dans l’en-tête HTTP de la page. Tout d’abord, le affich HTTP potentiel...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 79c27f39793a4a3a94ea412838fb3844579e874d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624970"
---
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="64b2e-104">[Comment faire :]  Mettre en cache une page ASP.NET en fonction des informations contenues dans l’en-tête HTTP</span><span class="sxs-lookup"><span data-stu-id="64b2e-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>

<span data-ttu-id="64b2e-105">par [Chris pixels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="64b2e-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="64b2e-106">Dans cette vidéo, Chris pixels montre comment conserver une page dans le cache de sortie ASP.NET en fonction des informations contenues dans l’en-tête HTTP de la page.</span><span class="sxs-lookup"><span data-stu-id="64b2e-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="64b2e-107">Tout d’abord, les valeurs d’en-tête HTTP potentielles sont examinées.</span><span class="sxs-lookup"><span data-stu-id="64b2e-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="64b2e-108">Ensuite, une page d’exemple est créée, puis la directive OutputCache est utilisée avec l’attribut VaryByHeader qui contient une valeur « Accept-Language », un en-tête HTTP, pour contrôler la mise en cache en fonction de la langue du navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="64b2e-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="64b2e-109">L’exemple de page est affiché dans IE, qui est défini sur anglais, puis dans FireFox qui est défini pour utiliser le français.</span><span class="sxs-lookup"><span data-stu-id="64b2e-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="64b2e-110">Enfin, l’option permettant de déplacer la définition du cache vers un CacheProfile dans le fichier Web. config est présentée.</span><span class="sxs-lookup"><span data-stu-id="64b2e-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="64b2e-111">&#9654;Regarder la vidéo (12 minutes)</span><span class="sxs-lookup"><span data-stu-id="64b2e-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
