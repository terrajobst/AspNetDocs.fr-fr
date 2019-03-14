---
ms.openlocfilehash: b6f39dd0dedb38961eb021cde355f7ec83283195
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57024866"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="5bcaa-101">Exemple de contrôleur d’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5bcaa-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="5bcaa-102">Cet exemple d’application est composé des projets suivants :</span><span class="sxs-lookup"><span data-stu-id="5bcaa-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="5bcaa-103">\**WebApiSample.Api.22*: Un projet ASP.NET Core 2.2 ciblant .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="5bcaa-103">\**WebApiSample.Api.22*: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="5bcaa-104">**WebApiSample.Api.21**: Un projet ASP.NET Core 2.1 ciblant .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5bcaa-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="5bcaa-105">**WebApiSample.Api.Pre21**: Un projet ASP.NET Core 2.0 ciblant .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5bcaa-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="5bcaa-106">**WebApiSample.DataAccess**: Une bibliothèque de classes .NET Standard 2.0 agissant comme une couche d’accès aux données pour les projets d’API Web 2.</span><span class="sxs-lookup"><span data-stu-id="5bcaa-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="5bcaa-107">Cet exemple illustre différentes façons de créer un contrôleur d’API web :</span><span class="sxs-lookup"><span data-stu-id="5bcaa-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="5bcaa-108">Dériver la classe de ControllerBase</span><span class="sxs-lookup"><span data-stu-id="5bcaa-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="5bcaa-109">Annoter la classe avec ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="5bcaa-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
