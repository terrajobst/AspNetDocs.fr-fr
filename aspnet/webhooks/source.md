---
uid: webhooks/source
title: Code source de WebHooks d’ASP.NET et les packages NuGet | Microsoft Docs
author: rick-anderson
description: Des liens vers les WebHooks ASP.NET du code source et les packages NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: f88d9247f9d8aa0c5edc1ffc462be21d9319a725
ms.sourcegitcommit: dd0dc556a3d99a31d8fdbc763e9a2e53f3441b70
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67410797"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Code source de WebHooks d’ASP.NET et les packages NuGet

Microsoft ASP.NET WebHooks fait partie de la famille Microsoft ASP.NET de modules et est hébergé comme un [Open Source Project sur GitHub](https://github.com/aspnet/WebHooks). Cela signifie que nous accepter les contributions, mais que vous examinez le [marche à suivre pour](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) avant de soumettre une demande de tirage.

Cette documentation en ligne qui vous lisez maintenant est également hébergée en tant que [Open Source sur GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) et accepte également les contributions.

## <a name="nuget-packages"></a>Packages NuGet

Le [les packages NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sont divisées en trois parties :

* [Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Un package commun qui est partagé entre les expéditeurs et destinataires.

* [Expéditeur](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Un ensemble de packages prenant en charge l’envoi de vos propres WebHooks à d’autres personnes. La fonctionnalité pour l’envoi de WebHooks est décrite plus en détail dans [WebHooks envoi](sending/senders).

* [Récepteurs](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Un ensemble de packages prenant en charge la réception de WebHooks à partir d’autres. Les fonctionnalités pour la réception des WebHooks sont décrites plus en détail dans [WebHooks réception](receiving/index.md).
