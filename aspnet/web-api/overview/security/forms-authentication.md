---
uid: web-api/overview/security/forms-authentication
title: Authentification par formulaire dans API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Décrit l’utilisation de l’authentification par formulaire dans API Web ASP.NET.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 147bfab76e48497f35a72b28cd935f40ec4193bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598594"
---
# <a name="forms-authentication-in-aspnet-web-api"></a>Authentification par formulaire dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

L’authentification par formulaire utilise un formulaire HTML pour envoyer les informations d’identification de l’utilisateur au serveur. Il ne s’agit pas d’une norme Internet. L’authentification par formulaire est uniquement appropriée pour les API Web appelées à partir d’une application Web, afin que l’utilisateur puisse interagir avec le formulaire HTML.

| Avantages | Inconvénients |
| --- | --- |
| -Facile à implémenter : intégré à ASP.NET. -Utilise le fournisseur d’appartenances ASP.NET, qui facilite la gestion des comptes d’utilisateur. | -N’est pas un mécanisme d’authentification HTTP standard ; utilise des cookies HTTP au lieu de l’en-tête d’autorisation standard. -Nécessite un client navigateur. -Les informations d’identification sont envoyées en texte clair. -Vulnérable à la falsification de requête intersites (CSRF); requiert des mesures anti-CSRF. -Difficile à utiliser à partir de clients qui ne sont pas des navigateurs. La connexion requiert un navigateur. -Les informations d’identification de l’utilisateur sont envoyées dans la demande. -Certains utilisateurs désactivent les cookies. |

Brièvement, l’authentification par formulaire dans ASP.NET fonctionne de la manière suivante :

1. Le client demande une ressource qui requiert une authentification.
2. Si l’utilisateur n’est pas authentifié, le serveur retourne HTTP 302 (trouvé) et redirige vers une page de connexion.
3. L’utilisateur entre les informations d’identification et envoie le formulaire.
4. Le serveur renvoie un autre HTTP 302 qui redirige vers l’URI d’origine. Cette réponse comprend un cookie d’authentification.
5. Le client demande à nouveau la ressource. La demande comprend le cookie d’authentification, de sorte que le serveur accorde la demande.

![](forms-authentication/_static/image1.png)

Pour plus d’informations, consultez [vue d’ensemble de l’authentification par formulaire.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Utilisation de l’authentification par formulaire avec l’API Web

Pour créer une application qui utilise l’authentification par formulaire, sélectionnez le modèle « application Internet » dans l’Assistant projet MVC 4. Ce modèle permet de créer des contrôleurs MVC pour la gestion des comptes. Vous pouvez également utiliser le modèle « application à page unique », disponible dans la mise à jour ASP.NET automne 2012.

Dans vos contrôleurs d’API Web, vous pouvez restreindre l’accès à l’aide de l’attribut `[Authorize]`, comme décrit dans [utilisation de l’attribut [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

L’authentification par formulaire utilise un cookie de session pour authentifier les demandes. Les navigateurs envoient automatiquement tous les cookies pertinents au site Web de destination. Cette fonctionnalité rend l’authentification par formulaire potentiellement vulnérable aux attaques de falsification de requête intersites (CSRF), en [empêchant les attaques de falsification de requête intersite (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

L’authentification par formulaire ne chiffre pas les informations d’identification de l’utilisateur. Par conséquent, l’authentification par formulaire n’est pas sécurisée, sauf si elle est utilisée avec SSL. Consultez [utilisation de SSL dans l’API Web](working-with-ssl-in-web-api.md).
