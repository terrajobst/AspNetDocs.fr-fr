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
# <a name="optimize-build-performance-for-solution"></a>Optimiser les performances de génération de solution

Visual Studio 2017 15,8 ou version ultérieure inclut un élément de menu : **Build** > **compilation ASP.net** > **optimiser les performances de génération de la solution**.

![Capture d’écran de l’élément de menu nouveau](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET compile ses vues lors de l’exécution, ce qui signifie qu’un projet ASP.NET y fait une copie du compilateur. Toutefois, sur un ordinateur de développement lorsque la copie du compilateur ne correspond pas à la copie de Visual Studio, les performances de génération sont affectées sur l’ordre de 1-3 secondes par Build incrémentielle. Cette fonctionnalité met à jour la copie de votre projet de façon à ce qu’elle corresponde à Visual Studio, ce qui accélère généralement les builds incrémentielles.

**Cela s’applique uniquement aux projets ASP.NET Framework 4.7.1 ou versions ultérieures, il ne s’applique pas à ASP.NET Core.**
