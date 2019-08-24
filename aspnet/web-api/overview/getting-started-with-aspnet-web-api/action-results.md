---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Résultats des actions dans l’API Web 2-ASP.NET 4. x
author: MikeWasson
description: Décrit comment API Web ASP.NET convertit la valeur de retour d’une action de contrôleur en message de réponse HTTP dans ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 1eaaf8e87168096683212fa66d3ddf415ad6b22b
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000716"
---
# <a name="action-results-in-web-api-2"></a>Résultats des actions dans l’API Web 2

Cette rubrique décrit comment API Web ASP.NET convertit la valeur de retour d’une action de contrôleur en message de réponse HTTP.

Une action du contrôleur d’API Web peut retourner l’un des éléments suivants:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Autre type

En fonction de la valeur renvoyée, l’API Web utilise un mécanisme différent pour créer la réponse HTTP.

| Type de retour | Comment l’API Web crée la réponse |
| --- | --- |
| void | Retourne un 204 vide (aucun contenu) |
| **HttpResponseMessage** | Convertissez directement en message de réponse HTTP. |
| **IHttpActionResult** | Appelez **ExecuteAsync** pour créer un **HttpResponseMessage**, puis convertissez-le en message de réponse http. |
| Autre type | Écrivez la valeur de retour sérialisée dans le corps de la réponse; retourne 200 (OK). |

Le reste de cette rubrique décrit chaque option plus en détail.

## <a name="void"></a>void

Si le type de retour `void`est, l’API Web retourne simplement une réponse http vide avec le code d’État 204 (aucun contenu).

Exemple de contrôleur:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Réponse HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Si l’action retourne un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), l’API Web convertit la valeur de retour directement en message de réponse http, à l’aide des propriétés de l’objet **HttpResponseMessage** pour remplir la réponse.

Cette option vous donne un grand contrôle sur le message de réponse. Par exemple, l’action de contrôleur suivante définit l’en-tête Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Réponse :

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Si vous transmettez un modèle de domaine à la méthode **CreateResponse** , l’API Web utilise un [formateur multimédia](../formats-and-model-binding/media-formatters.md) pour écrire le modèle sérialisé dans le corps de la réponse.

[!code-csharp[Main](action-results/samples/sample5.cs)]

L’API Web utilise l’en-tête Accept dans la demande pour choisir le formateur. Pour plus d’informations, consultez [négociation de contenu](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

L’interface **IHttpActionResult** a été introduite dans l’API Web 2. Fondamentalement, il définit une fabrique **HttpResponseMessage** . Voici quelques avantages de l’utilisation de l’interface **IHttpActionResult** :

- Simplifie le [test unitaire](../testing-and-debugging/unit-testing-controllers-in-web-api.md) de vos contrôleurs.
- Déplace la logique commune pour créer des réponses HTTP dans des classes distinctes.
- Rend l’objectif de l’action du contrôleur plus clair, en masquant les détails de bas niveau de la construction de la réponse.

**IHttpActionResult** contient une méthode unique, **ExecuteAsync**, qui crée de manière asynchrone une instance **HttpResponseMessage** .

[!code-csharp[Main](action-results/samples/sample6.cs)]

Si une action de contrôleur retourne un **IHttpActionResult**, l’API Web appelle la méthode **ExecuteAsync** pour créer un **HttpResponseMessage**. Il convertit ensuite le **HttpResponseMessage** en message de réponse http.

Voici une implémentation simple de **IHttpActionResult** qui crée une réponse en texte brut:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Exemple d’action de contrôleur:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Réponse :

[!code-console[Main](action-results/samples/sample9.cmd)]

Plus souvent, vous utilisez les implémentations **IHttpActionResult** définies dans l’espace de noms **[System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** . La classe **ApiController** définit des méthodes d’assistance qui retournent ces résultats d’action intégrés.

Dans l’exemple suivant, si la demande ne correspond pas à un ID de produit existant, le contrôleur appelle [ApiController. NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) pour créer une réponse 404 (introuvable). Dans le cas contraire, le contrôleur appelle [ApiController. OK](https://msdn.microsoft.com/library/dn314591.aspx), qui crée une réponse 200 (OK) qui contient le produit.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Autres types de retour

Pour tous les autres types de retour, l’API Web utilise un [formateur multimédia](../formats-and-model-binding/media-formatters.md) pour sérialiser la valeur de retour. L’API Web écrit la valeur sérialisée dans le corps de la réponse. Le code d’état de la réponse est 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

L’inconvénient de cette approche est que vous ne pouvez pas retourner directement un code d’erreur, tel que 404. Toutefois, vous pouvez lever un **HttpResponseException** pour les codes d’erreur. Pour plus d’informations, consultez [gestion des exceptions dans API Web ASP.net](../error-handling/exception-handling.md).

L’API Web utilise l’en-tête Accept dans la demande pour choisir le formateur. Pour plus d’informations, consultez [négociation de contenu](../formats-and-model-binding/content-negotiation.md).

Exemple de requête

[!code-console[Main](action-results/samples/sample12.cmd)]

Exemple de réponse

[!code-console[Main](action-results/samples/sample13.cmd)]
