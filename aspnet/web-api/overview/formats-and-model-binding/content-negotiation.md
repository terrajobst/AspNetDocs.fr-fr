---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Négociation de contenu dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Décrit comment API Web ASP.NET implémente la négociation de contenu HTTP pour ASP.NET 4. x.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622268"
---
# <a name="content-negotiation-in-aspnet-web-api"></a>Négociation de contenu dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

Cet article explique comment API Web ASP.NET implémente la négociation de contenu pour ASP.NET 4. x.

La spécification HTTP (RFC 2616) définit la négociation de contenu comme « le processus de sélection de la meilleure représentation pour une réponse donnée lorsqu’il y a plusieurs représentations disponibles ». Le mécanisme principal de négociation de contenu dans HTTP sont les en-têtes de requête suivants :

- **Accepter :** Les types de médias qui sont acceptables pour la réponse, tels que « application/JSON », « application/XML » ou un type de support personnalisé tel que &quot;application/vnd. example + XML&quot;
- **Accept-Charset :** Les jeux de caractères admis, par exemple UTF-8 ou ISO 8859-1.
- **Acceptation-encodage :** Les encodages de contenu acceptables, tels que gzip.
- **Accept-Language :** Langage naturel préféré, tel que « en-US ».

Le serveur peut également examiner d’autres parties de la requête HTTP. Par exemple, si la requête contient un en-tête X-requested-with, indiquant une requête AJAX, le serveur peut utiliser par défaut JSON en l’absence d’en-tête Accept.

Dans cet article, nous allons examiner comment l’API Web utilise les en-têtes Accept et Accept-Charset. (À ce stade, il n’existe pas de prise en charge intégrée pour Accept-Encoding ou Accept-Language.)

## <a name="serialization"></a>Sérialisation

Si un contrôleur d’API Web retourne une ressource en tant que type CLR, le pipeline sérialise la valeur de retour et l’écrit dans le corps de la réponse HTTP.

Par exemple, considérez l’action de contrôleur suivante :

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Un client peut envoyer cette requête HTTP :

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

En réponse, le serveur peut envoyer :

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

Dans cet exemple, le client a demandé JSON, JavaScript ou « tout » (\*/\*). Le serveur a répondu avec une représentation JSON de l’objet `Product`. Notez que l’en-tête Content-type dans la réponse est défini sur &quot;application/JSON&quot;.

Un contrôleur peut également retourner un objet **HttpResponseMessage** . Pour spécifier un objet CLR pour le corps de la réponse, appelez la méthode d’extension **CreateResponse** :

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Cette option vous donne davantage de contrôle sur les détails de la réponse. Vous pouvez définir le code d’État, ajouter des en-têtes HTTP, etc.

L’objet qui sérialise la ressource est appelé *formateur multimédia*. Les formateurs multimédias dérivent de la classe **MediaTypeFormatter** . L’API Web fournit des formateurs multimédias pour XML et JSON, et vous pouvez créer des formateurs personnalisés pour prendre en charge d’autres types de média. Pour plus d’informations sur l’écriture d’un formateur personnalisé, consultez [formateurs de médias](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Fonctionnement de la négociation de contenu

Tout d’abord, le pipeline obtient le service **IContentNegotiator** à partir de l’objet **HttpConfiguration** . Elle obtient également la liste des formateurs de média à partir de la collection **HttpConfiguration. Formatters** .

Ensuite, le pipeline appelle **IContentNegotiator. Negotiate**, en passant :

- Type d’objet à sérialiser.
- Collection de formateurs de médias
- La requête HTTP

La méthode **Negotiate** retourne deux informations :

- Formateur à utiliser
- Type de média pour la réponse

Si aucun formateur n’est trouvé, la méthode **Negotiate** retourne la **valeur null**et le client reçoit l’erreur http 406 (non acceptable).

Le code suivant montre comment un contrôleur peut appeler directement la négociation de contenu :

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Ce code équivaut à ce que le pipeline effectue automatiquement.

## <a name="default-content-negotiator"></a>Négociateur de contenu par défaut

La classe **DefaultContentNegotiator** fournit l’implémentation par défaut de **IContentNegotiator**. Il utilise plusieurs critères pour sélectionner un formateur.

Tout d’abord, le formateur doit être en mesure de sérialiser le type. Cette vérification est effectuée en appelant **MediaTypeFormatter. CanWriteType**.

Ensuite, le négociateur de contenu examine chaque formateur et évalue la manière dont il correspond à la requête HTTP. Pour évaluer la correspondance, le négociateur de contenu examine deux choses sur le formateur :

- Collection **SupportedMediaTypes** , qui contient la liste des types de média pris en charge. Le négociateur de contenu tente de faire correspondre cette liste avec l’en-tête d’acceptation de la demande. Notez que l’en-tête Accept peut inclure des plages. Par exemple, « texte/brut » est une correspondance pour le texte/la\* ou \*/\*.
- Collection **MediaTypeMappings** , qui contient une liste d’objets **MediaTypeMapping** . La classe **MediaTypeMapping** offre un moyen générique de faire correspondre les requêtes http avec les types de média. Par exemple, il peut mapper un en-tête HTTP personnalisé à un type de média particulier.

S’il existe plusieurs correspondances, la correspondance avec le facteur de qualité le plus élevé gagne. Exemple :

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

Dans cet exemple, application/JSON a un facteur de qualité implicite de 1,0, donc il est préférable à application/XML.

Si aucune correspondance n’est trouvée, le négociateur de contenu tente de faire correspondre le type de média du corps de la demande, le cas échéant. Par exemple, si la requête contient des données JSON, le négociateur de contenu recherche un formateur JSON.

S’il n’y a toujours aucune correspondance, le négociateur de contenu sélectionne simplement le premier formateur qui peut sérialiser le type.

## <a name="selecting-a-character-encoding"></a>Sélection d’un encodage de caractères

Une fois qu’un formateur est sélectionné, le négociateur de contenu choisit le meilleur encodage de caractères en examinant la propriété **SupportedEncodings** du formateur et en la faisant correspondre à l’en-tête Accept-Charset dans la requête (le cas échéant).
