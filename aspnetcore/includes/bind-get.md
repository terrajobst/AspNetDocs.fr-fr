---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051956"
---
> [!WARNING]
> <span data-ttu-id="3c3c1-101">Pour des raisons de sécurité, vous devez choisir de lier les données de requête `GET` aux propriétés du modèle de page.</span><span class="sxs-lookup"><span data-stu-id="3c3c1-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="3c3c1-102">Vérifiez l’entrée utilisateur avant de la mapper à des propriétés.</span><span class="sxs-lookup"><span data-stu-id="3c3c1-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="3c3c1-103">Le choix de la liaison `GET` est utile pour les scénarios qui reposent sur des valeurs de routage ou de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="3c3c1-103">Opting in to `GET` binding is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="3c3c1-104">Pour lier une propriété sur des requêtes `GET`, affectez à la propriété `SupportsGet` de l’attribut [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) la valeur `true` : `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="3c3c1-104">To bind a property on `GET` requests, set the [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>
