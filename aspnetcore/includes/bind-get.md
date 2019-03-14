---
ms.openlocfilehash: 545448e3673b02abc7e685bd987f2cf5f71375b4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57051956"
---
> [!WARNING]
> Pour des raisons de sécurité, vous devez choisir de lier les données de requête `GET` aux propriétés du modèle de page. Vérifiez l’entrée utilisateur avant de la mapper à des propriétés. Le choix de la liaison `GET` est utile pour les scénarios qui reposent sur des valeurs de routage ou de chaîne de requête.
>
> Pour lier une propriété sur des requêtes `GET`, affectez à la propriété `SupportsGet` de l’attribut [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) la valeur `true` : `[BindProperty(SupportsGet = true)]`
