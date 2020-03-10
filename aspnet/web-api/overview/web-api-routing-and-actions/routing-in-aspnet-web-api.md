---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routage dans API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557609"
---
# <a name="routing-in-aspnet-web-api"></a>Routing in ASP.NET Web API (Routage dans l’API Web ASP.NET)

par [Mike Wasson](https://github.com/MikeWasson)

Cet article explique comment API Web ASP.NET achemine les requêtes HTTP vers les contrôleurs.

> [!NOTE]
> Si vous êtes familiarisé avec ASP.NET MVC, le routage de l’API Web est très similaire au routage MVC. La principale différence est que l’API Web utilise le verbe HTTP, et non le chemin d’accès de l’URI, pour sélectionner l’action. Vous pouvez également utiliser le routage de type MVC dans l’API Web. Cet article ne suppose aucune connaissance de ASP.NET MVC.

## <a name="routing-tables"></a>Tables de routage

Dans API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes http. Les méthodes publiques du contrôleur sont appelées *méthodes d’action* ou simplement des *actions*. Lorsque l’infrastructure de l’API Web reçoit une demande, elle route la demande vers une action.

Pour déterminer l’action à appeler, l’infrastructure utilise une *table de routage*. Le modèle de projet Visual Studio pour l’API Web crée un itinéraire par défaut :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Cet itinéraire est défini dans le fichier *WebApiConfig.cs* , qui est placé dans l' *application\_* répertoire de démarrage :

![](routing-in-aspnet-web-api/_static/image1.png)

Pour plus d’informations sur la classe `WebApiConfig`, consultez [Configuration des API Web ASP.net](../advanced/configuring-aspnet-web-api.md).

Si vous auto-hébergez une API Web, vous devez définir la table de routage directement sur l’objet `HttpSelfHostConfiguration`. Pour plus d’informations, consultez [auto-host a Web API](../older-versions/self-host-a-web-api.md).

Chaque entrée de la table de routage contient un *modèle*de routage. Le modèle de routage par défaut pour l’API Web est &quot;API/{Controller}/{ID}&quot;. Dans ce modèle, &quot;&quot; d’API est un segment de chemin d’accès littéral, et {Controller} et {ID} sont des variables d’espace réservé.

Lorsque l’infrastructure de l’API Web reçoit une requête HTTP, elle tente de faire correspondre l’URI à l’un des modèles de route de la table de routage. Si aucun itinéraire ne correspond, le client reçoit une erreur 404. Par exemple, les URI suivants correspondent à l’itinéraire par défaut :

- /api/contacts
- /api/contacts/1
- /api/products/gizmo1

Toutefois, l’URI suivant ne correspond pas, car il ne dispose pas de l’API &quot;&quot; segment :

- /contacts/1

> [!NOTE]
> La raison de l’utilisation de « API » dans l’itinéraire consiste à éviter les collisions avec le routage ASP.NET MVC. De cette façon, vous pouvez avoir &quot;/contacts&quot; accéder à un contrôleur MVC et &quot;/API/contacts&quot; accéder à un contrôleur d’API Web. Bien entendu, si vous n’aimez pas cette Convention, vous pouvez modifier la table de routage par défaut.

Une fois qu’un itinéraire correspondant est trouvé, l’API Web sélectionne le contrôleur et l’action :

- Pour trouver le contrôleur, l’API Web ajoute &quot;contrôleur&quot; à la valeur de la variable *{Controller}* .
- Pour trouver l’action, l’API Web examine le verbe HTTP, puis recherche une action dont le nom commence par ce nom de verbe HTTP. Par exemple, avec une demande d’accès, l’API Web recherche une action portant le préfixe &quot;obtenir&quot;, par exemple &quot;GetContact&quot; ou &quot;GetAllContacts&quot;. Cette Convention s’applique uniquement aux verbes d’extraction, de publication, de placement, de suppression, d’en-tête, d’OPTIONS et de correctif. Vous pouvez activer d’autres verbes HTTP à l’aide d’attributs sur votre contrôleur. Nous verrons un exemple de ce qui est plus tard.
- Les autres variables d’espace réservé dans le modèle de routage, telles que *{ID},* sont mappées aux paramètres d’action.

Intéressons-nous à un exemple. Supposons que vous définissiez le contrôleur suivant :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Voici quelques requêtes HTTP possibles, ainsi que l’action qui est appelée pour chaque :

| Verbe HTTP | Chemin de l’URI | Action | Paramètre |
| --- | --- | --- | --- |
| GET | API/produits | GetAllProducts | *None* |
| GET | API/produits/4 | GetProductById | 4 |
| Suppression | API/produits/4 | DeleteProduct | 4 |
| POST | API/produits | *(aucune correspondance)* |  |

Notez que le segment *{ID}* de l’URI, s’il est présent, est mappé au paramètre *ID* de l’action. Dans cet exemple, le contrôleur définit deux méthodes d’extraction, l’une avec un paramètre *ID* et l’autre sans paramètres.

Notez également que la demande de publication échouera, car le contrôleur ne définit pas une méthode &quot;de publication...&quot;.

## <a name="routing-variations"></a>Variations de routage

La section précédente a décrit le mécanisme de routage de base pour API Web ASP.NET. Cette section décrit certaines variations.

### <a name="http-verbs"></a>verbes HTTP

Au lieu d’utiliser la Convention d’affectation de noms pour les verbes HTTP, vous pouvez spécifier explicitement le verbe HTTP pour une action en décorant la méthode d’action avec l’un des attributs suivants :

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

Dans l’exemple suivant, la méthode `FindProduct` est mappée aux demandes d’extraction :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Pour autoriser plusieurs verbes HTTP pour une action, ou pour autoriser les verbes HTTP autres que obtenir, PUT, poster, supprimer, HEAD, OPTIONS et PATCH, utilisez l’attribut `[AcceptVerbs]`, qui prend une liste de verbes HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routage par nom d’action

Avec le modèle de routage par défaut, l’API Web utilise le verbe HTTP pour sélectionner l’action. Toutefois, vous pouvez également créer un itinéraire où le nom de l’action est inclus dans l’URI :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

Dans ce modèle de routage, le paramètre *{action}* nomme la méthode d’action sur le contrôleur. Avec ce style de routage, utilisez des attributs pour spécifier les verbes HTTP autorisés. Par exemple, supposons que votre contrôleur ait la méthode suivante :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

Dans ce cas, une requête d’extraction pour « API/Products/Details/1 » est mappée à la méthode `Details`. Ce style de routage est similaire à ASP.NET MVC et peut convenir à une API de style RPC.

Vous pouvez remplacer le nom de l’action à l’aide de l’attribut `[ActionName]`. Dans l’exemple suivant, deux actions sont mappées à &quot;API/Products/thumbnail/*ID*. L’un prend en charge l’extraction et l’autre prend en charge la publication :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Non-actions

Pour empêcher une méthode d’être appelée en tant qu’action, utilisez l’attribut `[NonAction]`. Cela signale à l’infrastructure que la méthode n’est pas une action, même si elle correspondrait autrement aux règles de routage.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>informations supplémentaires

Cette rubrique a fourni une vue d’ensemble du routage. Pour plus d’informations, consultez [routage et sélection des actions](routing-and-action-selection.md), qui décrit exactement comment l’infrastructure correspond à un URI à un itinéraire, sélectionne un contrôleur, puis sélectionne l’action à appeler.
