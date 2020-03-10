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
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="75c6c-103">Dépendances du récepteur webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="75c6c-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="75c6c-104">Microsoft ASP.NET webhook est conçu avec l’injection de dépendance à l’esprit.</span><span class="sxs-lookup"><span data-stu-id="75c6c-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="75c6c-105">La plupart des dépendances dans le système peuvent être remplacées par d’autres implémentations à l’aide d’un moteur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="75c6c-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="75c6c-106">Pour obtenir la liste des dépendances du récepteur, consultez [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) .</span><span class="sxs-lookup"><span data-stu-id="75c6c-106">See [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="75c6c-107">Si aucune dépendance n’a été inscrite, une implémentation par défaut est utilisée.</span><span class="sxs-lookup"><span data-stu-id="75c6c-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="75c6c-108">Pour obtenir la liste des implémentations par défaut, consultez [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) .</span><span class="sxs-lookup"><span data-stu-id="75c6c-108">See [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
