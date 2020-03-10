---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateurs de médias dans API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: Montre comment prendre en charge des formats de média supplémentaires dans API Web ASP.NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557252"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a>Formateurs de médias dans API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

Ce didacticiel montre comment prendre en charge des formats de média supplémentaires dans API Web ASP.NET.

## <a name="internet-media-types"></a>Types de médias Internet

Un type de média, également appelé type MIME, identifie le format d’un élément de données. Dans HTTP, les types de média décrivent le format du corps du message. Un type de média se compose de deux chaînes, un type et un sous-type. Exemple :

- texte/html
- image/png
- application/json

Lorsqu’un message HTTP contient un corps d’entité, l’en-tête Content-type spécifie le format du corps du message. Cela indique au récepteur comment analyser le contenu du corps du message.

Par exemple, si une réponse HTTP contient une image PNG, la réponse peut contenir les en-têtes suivants.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Lorsque le client envoie un message de demande, il peut inclure un en-tête Accept. L’en-tête Accept indique au serveur le ou les types de médias que le client souhaite obtenir du serveur. Exemple :

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Cet en-tête indique au serveur que le client veut HTML, XHTML ou XML.

Le type de média détermine la façon dont l’API Web sérialise et désérialise le corps du message HTTP. L’API Web offre une prise en charge intégrée des données XML, JSON, BSON et form-urlencoded, et vous pouvez prendre en charge des types de média supplémentaires en écrivant un *formateur multimédia*.

Pour créer un formateur multimédia, dérivez de l’une des classes suivantes :

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Cette classe utilise des méthodes de lecture et d’écriture asynchrones.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Cette classe dérive de **MediaTypeFormatter** , mais utilise des méthodes de lecture/écriture synchrones.

La dérivation à partir de **BufferedMediaTypeFormatter** est plus simple, car il n’y a pas de code asynchrone, mais cela signifie également que le thread appelant peut se bloquer pendant les e/s.

## <a name="example-creating-a-csv-media-formatter"></a>Exemple : création d’un formateur de média CSV

L’exemple suivant montre un formateur de type de média qui peut sérialiser un objet Product dans un format CSV (valeurs séparées par des virgules). Cet exemple utilise le type de produit défini dans le didacticiel [création d’une API Web qui prend en charge les opérations CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Voici la définition de l’objet Product :

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Pour implémenter un formateur CSV, définissez une classe qui dérive de **BufferedMediaTypeFormatter**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Dans le constructeur, ajoutez les types de médias pris en charge par le formateur. Dans cet exemple, le formateur prend en charge un type de média unique, &quot;texte/CSV&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Substituez la méthode **CanWriteType** pour indiquer les types que le formateur peut sérialiser :

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

Dans cet exemple, le formateur peut sérialiser des objets `Product` uniques ainsi que des collections d’objets `Product`.

De même, substituez la méthode **CanReadType** pour indiquer les types que le formateur peut désérialiser. Dans cet exemple, le formateur ne prend pas en charge la désérialisation, donc la méthode retourne simplement **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Enfin, remplacez la méthode **WriteToStream** . Cette méthode sérialise un type en l’écrivant dans un flux. Si votre formateur prend en charge la désérialisation, substituez également la méthode **readFromStream** .

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Ajout d’un formateur multimédia au pipeline de l’API Web

Pour ajouter un formateur de type de média au pipeline de l’API Web, utilisez la propriété **Formatters** sur l’objet **HttpConfiguration** .

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Encodages de caractères

Un formateur de média peut éventuellement prendre en charge plusieurs encodages de caractères, comme UTF-8 ou ISO 8859-1.

Dans le constructeur, ajoutez un ou plusieurs types [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) à la collection **SupportedEncodings** . Placez d’abord l’encodage par défaut.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

Dans les méthodes **WriteToStream** et **readFromStream** , appelez [MediaTypeFormatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) pour sélectionner l’encodage de caractères par défaut. Cette méthode correspond aux en-têtes de requête par rapport à la liste des encodages pris en charge. Utilisez l' **encodage** retourné lors de la lecture ou de l’écriture à partir du flux :

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
