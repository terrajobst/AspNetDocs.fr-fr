---
uid: webhooks/receiving/dependencies
title: Dépendances du récepteur ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Dépendances du récepteur et l’injection de dépendances dans ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048726"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dépendances du récepteur WebHooks ASP.NET

Microsoft ASP.NET WebHooks est conçu avec l’injection de dépendance à l’esprit. La plupart des dépendances dans le système peut être remplacé par d’autres implémentations à l’aide d’un moteur de l’injection de dépendance.

Consultez [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) pour obtenir la liste de dépendances du récepteur. Si aucune dépendance n’a été inscrit, une implémentation par défaut est utilisée. Consultez [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) pour obtenir la liste des implémentations par défaut.
