---
uid: webhooks/source
title: ASP.NET webhooks code source et packages NuGet | Microsoft Docs
author: rick-anderson
description: Liens vers le code source et les packages NuGet ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000703"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET webhooks code source et packages NuGet

Microsoft ASP.NET webhooks fait partie de la famille de modules Microsoft ASP.NET et est hébergé en tant que [projet open source sur GitHub](https://github.com/aspnet/WebHooks). Cela signifie que nous acceptons les contributions, mais examinez les [recommandations de contribution](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) avant de soumettre une demande de tirage (pull request).

Cette documentation en ligne que vous lisez maintenant est également hébergée en tant que [Open source sur GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) et accepte également les contributions.

## <a name="nuget-packages"></a>Packages NuGet

Les [packages NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sont divisés en trois parties:

* [Courant](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Package commun qui est partagé entre les expéditeurs et les destinataires.

* [Expéditeur](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Ensemble de packages prenant en charge l’envoi de vos propres webhooks à d’autres utilisateurs. La fonctionnalité d’envoi de webhook est décrite plus en détail dans la rubrique [envoi](sending/senders.md)de webhooks.

* [Destinataires](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Ensemble de packages prenant en charge la réception de webhooks d’autres utilisateurs. La fonctionnalité de réception de webhook est décrite plus en détail dans [réception](receiving/index.md)de webhooks.
