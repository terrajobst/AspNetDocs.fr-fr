---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prévention des attaques par falsification de requête intersites (CSRF) dans ASP.NET MVC
author: MikeWasson
description: Décrit l’attaque de falsification de requête intersites (CSRF) et comment implémenter des mesures anti-CSRF dans ASP.NET Web MVC.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5fb0f8bcc9e587ba4fbbf2b857d3bf7adcaafb94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555117"
---
# <a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>Prévention des attaques par falsification de requête intersites (CSRF) dans l’application ASP.NET MVC

par [Mike Wasson](https://github.com/MikeWasson)

La falsification de requête intersites (CSRF) est une attaque dans laquelle un site malveillant envoie une demande à un site vulnérable dans lequel l’utilisateur est actuellement connecté.

Voici un exemple d’attaque CSRF :

1. Un utilisateur se connecte à `www.example.com` à l’aide de l’authentification par formulaire.
2. Le serveur authentifie l’utilisateur. La réponse du serveur comprend un cookie d’authentification.
3. Sans déconnexion, l’utilisateur visite un site Web malveillant. Ce site malveillant contient le formulaire HTML suivant : 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Notez que l’action de formulaire publie sur le site vulnérable, et non sur le site malveillant. Il s’agit de la partie « inter-sites » de CSRF.
4. L’utilisateur clique sur le bouton Envoyer. Le navigateur comprend le cookie d’authentification avec la demande.
5. La demande s’exécute sur le serveur avec le contexte d’authentification de l’utilisateur et peut faire tout ce qu’un utilisateur authentifié est autorisé à faire.

Bien que cet exemple exige que l’utilisateur clique sur le bouton de formulaire, la page malveillante peut tout aussi facilement exécuter un script qui envoie le formulaire automatiquement. En outre, l’utilisation de SSL n’empêche pas une attaque CSRF, car le site malveillant peut envoyer une requête « https:// ».

En général, les attaques CSRF sont possibles sur les sites Web qui utilisent des cookies pour l’authentification, car les navigateurs envoient tous les cookies pertinents au site Web de destination. Toutefois, les attaques CSRF ne sont pas limitées à l’exploitation des cookies. Par exemple, l’authentification de base et Digest est également vulnérable. Une fois qu’un utilisateur se connecte avec l’authentification de base ou Digest. le navigateur envoie automatiquement les informations d’identification jusqu’à la fin de la session.

## <a name="anti-forgery-tokens"></a>Jetons anti-contrefaçon

Pour prévenir les attaques CSRF, ASP.NET MVC utilise des jetons anti-contrefaçon, également appelés *jetons de vérification de demande*.

1. Le client demande une page HTML qui contient un formulaire.
2. Le serveur comprend deux jetons dans la réponse. Un jeton est envoyé sous la forme d’un cookie. L’autre est placée dans un champ de formulaire masqué. Les jetons sont générés de manière aléatoire afin qu’un adversaire ne puisse pas deviner les valeurs.
3. Lorsque le client envoie le formulaire, il doit renvoyer les deux jetons au serveur. Le client envoie le jeton de cookie en tant que cookie et envoie le jeton de formulaire à l’intérieur des données de formulaire. (Un client navigateur effectue automatiquement cette modification lorsque l’utilisateur envoie le formulaire.)
4. Si une demande n’inclut pas les deux jetons, le serveur rejette la demande.

Voici un exemple de formulaire HTML avec un jeton de formulaire masqué :

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Les jetons anti-contrefaçon fonctionnent parce que la page malveillante ne peut pas lire les jetons de l’utilisateur, en raison des stratégies de même origine. ([Les stratégies de même origine](http://www.w3.org/Security/wiki/Same_Origin_Policy) empêchent les documents hébergés sur deux sites différents d’accéder au contenu de chacun d’eux. Ainsi, dans l’exemple précédent, la page malveillante peut envoyer des demandes à example.com, mais elle ne peut pas lire la réponse.)

Pour empêcher les attaques CSRF, utilisez des jetons anti-contrefaçon avec n’importe quel protocole d’authentification où le navigateur envoie les informations d’identification en mode silencieux une fois que l’utilisateur se connecte. Cela comprend des protocoles d’authentification par cookie, tels que l’authentification par formulaires, ainsi que des protocoles tels que l’authentification de base et Digest.

Vous devez exiger des jetons anti-contrefaçon pour toute méthode non sécurisée (publication, placement, suppression). En outre, assurez-vous que les méthodes sécurisées (obtenir, en-tête) n’ont pas d’effets secondaires. De plus, si vous activez la prise en charge inter-domaines, telle que CORS ou JSONP, les méthodes sécurisées telles que sont potentiellement vulnérables aux attaques CSRF, ce qui permet à l’attaquant de lire des données potentiellement sensibles.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Jetons anti-contrefaçon dans ASP.NET MVC

Pour ajouter les jetons anti-contrefaçon à une page Razor, utilisez la méthode d’assistance **HtmlHelper. AntiForgeryToken** :

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Cette méthode ajoute le champ de formulaire masqué et définit également le jeton de cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF et AJAX

le jeton de formulaire peut se révéler problématique pour les requêtes AJAX, car une requête AJAX risque d’envoyer des données JSON, et non des données de formulaire HTML. L’une des solutions consiste à envoyer les jetons dans un en-tête HTTP personnalisé. Le code ci-après utilise la syntaxe Razor pour générer les jetons, puis ajoute les jetons à une demande AJAX. Les jetons sont générés sur le serveur en appelant anti- **contrefaçon. GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Lorsque vous traitez la demande, extrayez les jetons de l’en-tête de la demande. Appelez ensuite la méthode anti- **contrefaçon. Validate** pour valider les jetons. La méthode **Validate** lève une exception si les jetons ne sont pas valides.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
