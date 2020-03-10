---
uid: webhooks/source
title: ASP.NET webhooks code source et packages NuGet | Microsoft Docs
author: rick-anderson
description: Liens vers le code source et les packages NuGet ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633062"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET webhooks code source et packages NuGet

Microsoft ASP.NET webhooks fait partie de la famille de modules Microsoft ASP.NET et est hébergé en tant que [projet open source sur GitHub](https://github.com/aspnet/WebHooks). Cela signifie que nous acceptons les contributions, mais examinez les [recommandations de contribution](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) avant de soumettre une demande de tirage (pull request).

Cette documentation en ligne que vous lisez maintenant est également hébergée en tant que [Open source sur GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) et accepte également les contributions.

## <a name="nuget-packages"></a>Packages NuGet

Les [packages NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sont divisés en trois parties :

* [Commun](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un package commun qui est partagé entre les expéditeurs et les destinataires.

* [Expéditeur](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): ensemble de packages prenant en charge l’envoi de vos propres webhooks à d’autres utilisateurs. La fonctionnalité d’envoi de webhook est décrite plus en détail dans la rubrique [envoi de webhooks](sending/senders.md).

* [Destinataires](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): ensemble de packages prenant en charge la réception de webhooks d’autres utilisateurs. La fonctionnalité de réception de webhook est décrite plus en détail dans [réception de webhooks](receiving/index.md).
