---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Regroupement et minimisation de ressources dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: microsoft
description: Le regroupement et la minimisation sont des moyens de rendre votre site plus rapide. Le regroupement vous permet de combiner plusieurs fichiers JavaScript (. js) ou plusieurs feuilles de style en cascade (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5e42111ad71ec65581e56c73822e23ecd5fcbd58
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635708"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="f6893-104">Bundles et minimisation des ressources dans un site ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="f6893-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="f6893-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f6893-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f6893-106">Le regroupement et la minimisation sont des moyens de rendre votre site plus rapide.</span><span class="sxs-lookup"><span data-stu-id="f6893-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="f6893-107">Le regroupement vous permet de combiner plusieurs fichiers JavaScript ( *. js*) ou plusieurs fichiers de feuille de style en cascade ( *. CSS*) afin qu’ils puissent être téléchargés en tant qu’unité, plutôt qu’un à la fois.</span><span class="sxs-lookup"><span data-stu-id="f6893-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="f6893-108">La minimisation élimine l’espace blanc et effectue d’autres types de compression pour rendre les fichiers téléchargés aussi petits que possible.</span><span class="sxs-lookup"><span data-stu-id="f6893-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="f6893-109">La version RC de pages Web ASP.NET 2 ne prend pas en charge le regroupement et la minimisation, car le package qui contient les éléments requis n’est pas encore disponible dans Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="f6893-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="f6893-110">Veuillez nous excuser pour ce désagrément.</span><span class="sxs-lookup"><span data-stu-id="f6893-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="f6893-111">Le package doit être disponible dans la version finale de pages Web ASP.NET 2 et WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="f6893-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
