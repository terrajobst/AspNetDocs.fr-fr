---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimiser les performances de génération de solution
author: AngelosP
description: Optimiser les performances de génération de solution
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622639"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="7a797-103">Optimiser les performances de génération de solution</span><span class="sxs-lookup"><span data-stu-id="7a797-103">Optimize build performance for solution</span></span>

<span data-ttu-id="7a797-104">Visual Studio 2017 15,8 ou version ultérieure inclut un élément de menu : **Build** > **compilation ASP.net** > **optimiser les performances de génération de la solution**.</span><span class="sxs-lookup"><span data-stu-id="7a797-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![Capture d’écran de l’élément de menu nouveau](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="7a797-106">ASP.NET compile ses vues lors de l’exécution, ce qui signifie qu’un projet ASP.NET y fait une copie du compilateur.</span><span class="sxs-lookup"><span data-stu-id="7a797-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="7a797-107">Toutefois, sur un ordinateur de développement lorsque la copie du compilateur ne correspond pas à la copie de Visual Studio, les performances de génération sont affectées sur l’ordre de 1-3 secondes par Build incrémentielle.</span><span class="sxs-lookup"><span data-stu-id="7a797-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="7a797-108">Cette fonctionnalité met à jour la copie de votre projet de façon à ce qu’elle corresponde à Visual Studio, ce qui accélère généralement les builds incrémentielles.</span><span class="sxs-lookup"><span data-stu-id="7a797-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="7a797-109">**Cela s’applique uniquement aux projets ASP.NET Framework 4.7.1 ou versions ultérieures, il ne s’applique pas à ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="7a797-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
