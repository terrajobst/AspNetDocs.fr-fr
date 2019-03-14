---
ms.openlocfilehash: b6f39dd0dedb38961eb021cde355f7ec83283195
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57024866"
---
# <a name="aspnet-core-web-api-controller-sample"></a>Exemple de contrôleur d’API web ASP.NET Core

Cet exemple d’application est composé des projets suivants :

- **WebApiSample.Api.22*: Un projet ASP.NET Core 2.2 ciblant .NET Core 2.2.
- **WebApiSample.Api.21**: Un projet ASP.NET Core 2.1 ciblant .NET Core 2.1.
- **WebApiSample.Api.Pre21**: Un projet ASP.NET Core 2.0 ciblant .NET Core 2.0.
- **WebApiSample.DataAccess**: Une bibliothèque de classes .NET Standard 2.0 agissant comme une couche d’accès aux données pour les projets d’API Web 2.

Cet exemple illustre différentes façons de créer un contrôleur d’API web :

- [Dériver la classe de ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Annoter la classe avec ApiControllerAttribute](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
