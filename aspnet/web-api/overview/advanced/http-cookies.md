---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Décrit comment envoyer et recevoir des cookies HTTP dans l’API Web pour ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557686"
---
# <a name="http-cookies-in-aspnet-web-api"></a>Cookies HTTP dans l’API web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

Cette rubrique explique comment envoyer et recevoir des cookies HTTP dans l’API Web.

## <a name="background-on-http-cookies"></a>Arrière-plan sur les cookies HTTP

Cette section donne un bref aperçu de la façon dont les cookies sont implémentés au niveau HTTP. Pour plus d’informations, consultez la [RFC 6265](http://tools.ietf.org/html/rfc6265).

Un cookie est un élément de données qu’un serveur envoie dans la réponse HTTP. Le client stocke (éventuellement) le cookie et le retourne aux demandes suivantes. Cela permet au client et au serveur de partager l’État. Pour définir un cookie, le serveur comprend un en-tête Set-Cookie dans la réponse. Le format d’un cookie est une paire nom-valeur, avec des attributs facultatifs. Exemple :

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Voici un exemple avec des attributs :

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Pour retourner un cookie au serveur, le client comprend un en-tête de cookie dans les demandes ultérieures.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Une réponse HTTP peut inclure plusieurs en-têtes set-cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Le client retourne plusieurs cookies à l’aide d’un seul en-tête de cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

L’étendue et la durée d’un cookie sont contrôlées par les attributs suivants dans l’en-tête Set-Cookie :

- **Domain**: indique au client le domaine qui doit recevoir le cookie. Par exemple, si le domaine est « example.com », le client retourne le cookie à chaque sous-domaine de example.com. S’il n’est pas spécifié, le domaine est le serveur d’origine.
- **Path**: limite le cookie au chemin d’accès spécifié dans le domaine. S’il n’est pas spécifié, le chemin d’accès de l’URI de la demande est utilisé.
- **Expires**: définit une date d’expiration pour le cookie. Le client supprime le cookie lorsqu’il expire.
- **Max-age**: définit l’âge maximal du cookie. Le client supprime le cookie lorsqu’il atteint l’âge maximal.

Si `Expires` et `Max-Age` sont définis, `Max-Age` est prioritaire. Si aucun n’est défini, le client supprime le cookie lorsque la session active se termine. (La signification exacte de « session » est déterminée par l’agent utilisateur.)

Toutefois, sachez que les clients peuvent ignorer les cookies. Par exemple, un utilisateur peut désactiver les cookies pour des raisons de confidentialité. Les clients peuvent supprimer des cookies avant leur expiration ou limiter le nombre de cookies stockés. Pour des raisons de confidentialité, les clients rejettent souvent des cookies « tiers », où le domaine ne correspond pas au serveur d’origine. En bref, le serveur ne doit pas s’appuyer sur la récupération des cookies qu’il définit.

## <a name="cookies-in-web-api"></a>Cookies dans l’API Web

Pour ajouter un cookie à une réponse HTTP, créez une instance **CookieHeaderValue** qui représente le cookie. Appelez ensuite la méthode d’extension **AddCookies** , qui est définie dans le **System .net. http. Classe HttpResponseHeadersExtensions** , pour ajouter le cookie.

Par exemple, le code suivant ajoute un cookie au sein d’une action de contrôleur :

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Notez que **AddCookies** prend un tableau d’instances **CookieHeaderValue** .

Pour extraire les cookies d’une demande du client, appelez la méthode **GetCookies** :

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Un **CookieHeaderValue** contient une collection d’instances **CookieState** . Chaque **CookieState** représente un cookie. Utilisez la méthode d’indexeur pour obtenir un **CookieState** par nom, comme indiqué.

## <a name="structured-cookie-data"></a>Données de cookie structurées

De nombreux navigateurs limitent le nombre de cookies&#8212;qu’ils stockent à la fois le nombre total et le nombre par domaine. Par conséquent, il peut être utile de placer des données structurées dans un cookie unique, au lieu de définir plusieurs cookies.

> [!NOTE]
> La RFC 6265 ne définit pas la structure des données de cookie.

À l’aide de la classe **CookieHeaderValue** , vous pouvez passer une liste de paires nom-valeur pour les données de cookie. Ces paires nom-valeur sont encodées en tant que données de formulaire encodées URL dans l’en-tête Set-Cookie :

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Le code précédent génère l’en-tête Set-Cookie suivant :

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

La classe **CookieState** fournit une méthode d’indexeur pour lire les sous-valeurs d’un cookie dans le message de demande :

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Exemple : définir et récupérer des cookies dans un gestionnaire de messages

Les exemples précédents ont montré comment utiliser des cookies à partir d’un contrôleur d’API Web. Une autre option consiste à utiliser des [gestionnaires de messages](http-message-handlers.md). Les gestionnaires de messages sont appelés plus tôt dans le pipeline que les contrôleurs. Un gestionnaire de messages peut lire les cookies de la demande avant que la demande n’atteigne le contrôleur, ou ajouter des cookies à la réponse après que le contrôleur a généré la réponse.

![](http-cookies/_static/image2.png)

Le code suivant illustre un gestionnaire de messages pour créer des ID de session. L’ID de session est stocké dans un cookie. Le gestionnaire vérifie la requête pour le cookie de session. Si la demande n’inclut pas le cookie, le gestionnaire génère un nouvel ID de session. Dans les deux cas, le gestionnaire stocke l’ID de session dans le conteneur de propriétés **HttpRequestMessage. Properties** . Il ajoute également le cookie de session à la réponse HTTP.

Cette implémentation ne valide pas le fait que l’ID de session du client a été émis par le serveur. Ne l’utilisez pas comme une forme d’authentification ! Le point de l’exemple est de montrer la gestion des cookies HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Un contrôleur peut obtenir l’ID de session à partir du jeu de propriétés **HttpRequestMessage. Properties** .

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
