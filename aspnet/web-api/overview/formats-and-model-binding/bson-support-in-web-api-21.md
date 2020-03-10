---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Prise en charge de BSON dans API Web ASP.NET 2,1-ASP.NET 4. x
author: MikeWasson
description: montre comment utiliser BSON dans un contrôleur d’API Web (côté serveur) et dans une application cliente .NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: ccbc0372120301b1cd8d4cdc86bd9fba9404d8ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622296"
---
# <a name="bson-support-in-aspnet-web-api-21"></a>Prise en charge de BSON dans API Web ASP.NET 2,1

par [Mike Wasson](https://github.com/MikeWasson)

Cette rubrique montre comment utiliser BSON dans votre contrôleur d’API Web (côté serveur) et dans une application cliente .NET. L’API Web 2,1 introduit la prise en charge de BSON. 

## <a name="what-is-bson"></a>Qu’est-ce que BSON ?

[BSON](http://bsonspec.org/) est un format de sérialisation binaire. « BSON » signifie « JSON binaire », mais BSON et JSON sont sérialisés très différemment. BSON est de type JSON, car les objets sont représentés en tant que paires nom-valeur, similaire à JSON. Contrairement à JSON, les types de données numériques sont stockés sous la forme d’octets et non de chaînes

BSON a été conçu pour être léger, facile à analyser et rapide à encoder/décoder.

- BSON a une taille comparable à JSON. Selon les données, une charge utile BSON peut être plus modeste ou plus volumineuse qu’une charge utile JSON. Pour sérialiser des données binaires, telles qu’un fichier image, BSON est plus petit que JSON, car les données binaires ne sont pas encodées en base64.
- Les documents BSON sont faciles à analyser, car les éléments sont précédés d’un champ de longueur, de sorte qu’un analyseur peut ignorer les éléments sans les décoder.
- L’encodage et le décodage sont efficaces, car les types de données numériques sont stockés sous la forme de nombres, et non de chaînes.

Les clients natifs, tels que les applications clientes .NET, peuvent tirer parti de l’utilisation de BSON à la place de formats textuels tels que JSON ou XML. Pour les clients de navigateur, vous souhaiterez sans doute utiliser JSON, car JavaScript peut directement convertir la charge utile JSON.

Heureusement, l’API Web utilise la [négociation de contenu](content-negotiation.md). par conséquent, votre API peut prendre en charge les deux formats et laisser le client choisir.

## <a name="enabling-bson-on-the-server"></a>Activation de BSON sur le serveur

Dans la configuration de votre API Web, ajoutez **BsonMediaTypeFormatter** à la collection Formatters.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Désormais, si le client demande « application/BSON », l’API Web utilise le formateur BSON.

Pour associer des BSON à d’autres types de médias, ajoutez-les à la collection SupportedMediaTypes. Le code suivant ajoute « application/vnd. contoso » aux types de média pris en charge :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Exemple de session HTTP

Pour cet exemple, nous allons utiliser la classe de modèle suivante et un contrôleur d’API Web simple :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Un client peut envoyer la requête HTTP suivante :

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Voici la réponse :

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Ici, j’ai remplacé les données binaires par &quot;.&quot; caractères. La capture d’écran suivante de Fiddler montre les valeurs hex brutes.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Utilisation de BSON avec HttpClient

Les applications clientes .NET peuvent utiliser le formateur BSON avec **httpclient**. Pour plus d’informations sur **httpclient**, consultez [appel d’une API Web à partir d’un client .net](../advanced/calling-a-web-api-from-a-net-client.md).

Le code suivant envoie une requête d’extraction qui accepte BSON, puis désérialise la charge utile BSON dans la réponse.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Pour demander BSON à partir du serveur, définissez l’en-tête Accept sur « application/BSON » :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Pour désérialiser le corps de la réponse, utilisez **BsonMediaTypeFormatter**. Ce formateur n’est pas dans la collection de formateurs par défaut. vous devez donc le spécifier lorsque vous lisez le corps de la réponse :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

L’exemple suivant montre comment envoyer une demande de publication contenant BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Une grande partie de ce code est identique à l’exemple précédent. Mais dans la méthode **PostAsync** , spécifiez **BsonMediaTypeFormatter** comme formateur :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Sérialisation des types primitifs de niveau supérieur

Chaque document BSON est une liste de paires clé/valeur. La spécification BSON ne définit pas de syntaxe pour la sérialisation d’une seule valeur brute, telle qu’un entier ou une chaîne.

Pour contourner cette limitation, **BsonMediaTypeFormatter** traite les types primitifs comme un cas spécial. Avant de sérialiser, elle convertit la valeur en une paire clé/valeur avec la clé « value ». Par exemple, supposons que votre contrôleur d’API retourne un entier :

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Avant de sérialiser, le formateur BSON le convertit en paire clé/valeur suivante :

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Lorsque vous désérialisez, le formateur rétablit la valeur d’origine des données. Toutefois, les clients qui utilisent un autre analyseur BSON doivent gérer ce cas, si votre API Web retourne des valeurs brutes. En général, vous devez envisager de retourner des données structurées plutôt que des valeurs brutes.

## <a name="additional-resources"></a>Ressources supplémentaires

[API Web BSON, exemple](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[Formateurs de médias](media-formatters.md)
