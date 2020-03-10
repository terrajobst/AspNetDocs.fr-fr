---
uid: webhooks/receiving/dependencies
title: Dépendances du récepteur ASP.NET webhooks | Microsoft Docs
author: rick-anderson
description: Dépendances du récepteur et injection de dépendances dans les webhooks ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 477b8828209d0da1d485ef883b0f99b4e1b9b5bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637283"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dépendances du récepteur webhook ASP.NET

Microsoft ASP.NET webhook est conçu avec l’injection de dépendance à l’esprit. La plupart des dépendances dans le système peuvent être remplacées par d’autres implémentations à l’aide d’un moteur d’injection de dépendances.

Pour obtenir la liste des dépendances du récepteur, consultez [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) . Si aucune dépendance n’a été inscrite, une implémentation par défaut est utilisée. Pour obtenir la liste des implémentations par défaut, consultez [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) .
