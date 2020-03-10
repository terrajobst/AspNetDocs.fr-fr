---
uid: web-api/overview/security/integrated-windows-authentication
title: Authentification Windows intégrée | Microsoft Docs
author: MikeWasson
description: Décrit l’utilisation de l’authentification Windows intégrée dans API Web ASP.NET.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: e4f31f191f3c0fabff308ea5dadb0f1d9ce7d448
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621743"
---
# <a name="integrated-windows-authentication"></a>Authentification Windows intégrée

par [Mike Wasson](https://github.com/MikeWasson)

L’authentification Windows intégrée permet aux utilisateurs de se connecter avec leurs informations d’identification Windows, à l’aide de Kerberos ou NTLM. Le client envoie les informations d’identification dans l’en-tête Authorization. L’authentification Windows est idéale pour un environnement intranet. Pour plus d’informations, consultez [Authentification Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Avantages | Inconvénients |
| --- | --- |
| -Intégré à IIS. -N’envoie pas les informations d’identification de l’utilisateur dans la demande. -Si l’ordinateur client appartient au domaine (par exemple, une application intranet), l’utilisateur n’a pas besoin d’entrer des informations d’identification. | -Non recommandé pour les applications Internet. -Requiert la prise en charge de Kerberos ou NTLM dans le client. -Le client doit se trouver dans le domaine Active Directory. |

> [!NOTE]
> Si votre application est hébergée sur Azure et que vous disposez d’un domaine Active Directory sur site, envisagez de fédérer votre annuaire Active Directory sur site avec Azure Active Directory. Ainsi, les utilisateurs peuvent se connecter avec leurs informations d’identification locales, mais l’authentification est effectuée par Azure AD. Pour plus d’informations, consultez [authentification Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).

Pour créer une application qui utilise l’authentification Windows intégrée, sélectionnez le modèle « application intranet » dans l’Assistant projet MVC 4. Ce modèle de projet place le paramètre suivant dans le fichier Web. config :

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Côté client, l’authentification Windows intégrée fonctionne avec n’importe quel navigateur prenant en charge le schéma d’authentification [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , qui comprend la plupart des principaux navigateurs. Pour les applications clientes .NET, la classe **httpclient** prend en charge l’authentification Windows :

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

L’authentification Windows est vulnérable aux attaques de falsification de requête intersites (CSRF). Consultez la page [prévention des attaques de falsification de requête intersites (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).
