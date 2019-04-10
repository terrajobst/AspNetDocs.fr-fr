---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Regroupement et minimisation des ressources dans une application Web Pages (Razor) Site | Microsoft Docs
author: microsoft
description: Regroupement et minimisation manières pour accélérer votre site. Regroupement de permet de combiner plusieurs fichiers JavaScript (.js) ou plusieurs styles CSS (...)
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5e42111ad71ec65581e56c73822e23ecd5fcbd58
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400449"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="1dc95-104">Bundles et minimisation des ressources dans un site ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="1dc95-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="1dc95-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1dc95-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1dc95-106">Regroupement et minimisation manières pour accélérer votre site.</span><span class="sxs-lookup"><span data-stu-id="1dc95-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="1dc95-107">Regroupement vous permet de combiner plusieurs JavaScript (*.js*) fichiers ou la feuille de style en cascade multiples (*.css*) de sorte qu’ils peuvent être téléchargés comme une unité, plutôt qu’une à la fois.</span><span class="sxs-lookup"><span data-stu-id="1dc95-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="1dc95-108">Minimisation resserre les espaces blancs et effectue d’autres types de compression pour rendre les fichiers téléchargés en tant que petit un éventuel.</span><span class="sxs-lookup"><span data-stu-id="1dc95-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="1dc95-109">La version RC de ASP.NET Web Pages 2 ne prend pas en charge regroupement et minimisation, car le package qui contient les éléments requis n’est pas encore disponible dans Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="1dc95-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="1dc95-110">Veuillez nous excuser pour ce désagrément.</span><span class="sxs-lookup"><span data-stu-id="1dc95-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="1dc95-111">Le package est censé être disponible dans la version finale d’ASP.NET Web Pages 2 et WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="1dc95-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
