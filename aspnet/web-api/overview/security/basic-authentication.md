---
uid: web-api/overview/security/basic-authentication
title: Authentification de base dans API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Décrit l’utilisation de l’authentification de base dans API Web ASP.NET.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555726"
---
# <a name="basic-authentication-in-aspnet-web-api"></a>Authentification de base dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

L’authentification de base est définie dans le [document RFC 2617, authentification http : authentification d’accès de base et Digest](http://www.ietf.org/rfc/rfc2617.txt).

Inconvénients

- Les informations d’identification de l’utilisateur sont envoyées dans la demande.
- Les informations d’identification sont envoyées en texte clair.
- Les informations d’identification sont envoyées avec chaque demande.
- Aucun moyen de se déconnecter, sauf en mettant fin à la session du navigateur.
- Vulnérable à la falsification de requête intersites (CSRF); requiert des mesures anti-CSRF.

Avantages

- Internet standard.
- Pris en charge par tous les principaux navigateurs.
- Protocole relativement simple.

L’authentification de base fonctionne comme suit :

1. Si une demande requiert une authentification, le serveur retourne 401 (non autorisé). La réponse comprend un en-tête WWW-Authenticate, indiquant que le serveur prend en charge l’authentification de base.
2. Le client envoie une autre demande, avec les informations d’identification du client dans l’en-tête Authorization. Les informations d’identification sont mises en forme en tant que chaîne « nom : mot de passe », encodée en base64. Les informations d’identification ne sont pas chiffrées.

L’authentification de base est effectuée dans le contexte d’un « domaine ». Le serveur comprend le nom du domaine dans l’en-tête WWW-Authenticate. Les informations d’identification de l’utilisateur sont valides dans ce domaine. La portée exacte d’un domaine est définie par le serveur. Par exemple, vous pouvez définir plusieurs domaines afin de partitionner les ressources.

![](basic-authentication/_static/image1.png)

Étant donné que les informations d’identification sont envoyées non chiffrées, l’authentification de base est sécurisée uniquement via HTTPs. Consultez [utilisation de SSL dans l’API Web](working-with-ssl-in-web-api.md).

L’authentification de base est également vulnérable aux attaques CSRF. Une fois que l’utilisateur a entré les informations d’identification, le navigateur les envoie automatiquement aux demandes suivantes au même domaine, pour la durée de la session. Cela comprend les demandes AJAX. Consultez la page [prévention des attaques de falsification de requête intersites (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Authentification de base avec IIS

IIS prend en charge l’authentification de base, mais il y a un inconvénient : l’utilisateur est authentifié par rapport à ses informations d’identification Windows. Cela signifie que l’utilisateur doit disposer d’un compte sur le domaine du serveur. Pour un site Web public, vous souhaitez généralement vous authentifier auprès d’un fournisseur d’appartenances ASP.NET.

Pour activer l’authentification de base à l’aide d’IIS, définissez le mode d’authentification sur « Windows » dans le fichier Web. config de votre projet ASP.NET :

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

Dans ce mode, IIS utilise les informations d’identification Windows pour s’authentifier. En outre, vous devez activer l’authentification de base dans IIS. Dans le gestionnaire des services Internet, accédez à l’affichage des fonctionnalités, sélectionnez authentification, puis activez l’authentification de base.

![](basic-authentication/_static/image2.png)

Dans votre projet d’API Web, ajoutez l’attribut `[Authorize]` pour toutes les actions de contrôleur qui nécessitent une authentification.

Un client s’authentifie lui-même en définissant l’en-tête d’autorisation dans la demande. Les clients de navigateur effectuent cette étape automatiquement. Les clients qui ne sont pas des navigateurs doivent définir l’en-tête.

## <a name="basic-authentication-with-custom-membership"></a>Authentification de base avec appartenance personnalisée

Comme mentionné précédemment, l’authentification de base intégrée à IIS utilise les informations d’identification Windows. Cela signifie que vous devez créer des comptes pour vos utilisateurs sur le serveur d’hébergement. Toutefois, pour une application Internet, les comptes d’utilisateurs sont généralement stockés dans une base de données externe.

Le code suivant montre comment un module HTTP qui effectue l’authentification de base. Vous pouvez facilement intégrer un fournisseur d’appartenances ASP.NET en remplaçant la méthode `CheckPassword`, qui est une méthode factice dans cet exemple.

Dans l’API Web 2, vous devez envisager d’écrire un [filtre d’authentification](authentication-filters.md) ou un [intergiciel (middleware) OWIN](../../../aspnet/overview/owin-and-katana/index.md)à la place d’un module http.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Pour activer le module HTTP, ajoutez ce qui suit à votre fichier Web. config dans la section **System. webServer** :

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Remplacez « YourAssemblyName » par le nom de l’assembly (sans l’extension « dll »).

Vous devez désactiver d’autres schémas d’authentification, tels que les formulaires ou l’authentification Windows.
