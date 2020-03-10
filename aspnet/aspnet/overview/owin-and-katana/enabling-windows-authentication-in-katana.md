---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Activation de l’authentification Windows dans Katana | Microsoft Docs
author: MikeWasson
description: 'Cet article montre comment activer l’authentification Windows dans Katana. Il aborde deux scénarios : l’utilisation d’IIS pour héberger Katana et l’utilisation de HttpListener pour auto-héberger des Kat...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617179"
---
# <a name="enabling-windows-authentication-in-katana"></a>Activation de l’authentification Windows dans Katana

par [Mike Wasson](https://github.com/MikeWasson)

> Cet article montre comment activer l’authentification Windows dans Katana. Il aborde deux scénarios : l’utilisation d’IIS pour héberger Katana et l’utilisation de HttpListener pour auto-héberger des Katana dans un processus personnalisé. Grâce à Barry Dorrans, David Matson et Chris Ross pour l’examen de cet article.

Katana est l’implémentation Microsoft de [OWIN](http://owin.org/), l’interface Web ouverte pour .net. Vous pouvez lire une présentation de OWIN et Katana [ici](an-overview-of-project-katana.md). L’architecture OWIN a plusieurs couches :

- Hôte : gère le processus dans lequel le pipeline OWIN s’exécute.
- Serveur : ouvre un socket réseau et écoute les demandes.
- Intergiciel (middleware) : traite la requête et la réponse HTTP.

Katana fournit actuellement deux serveurs, qui prennent tous deux en charge l’authentification intégrée de Windows :

- **Microsoft. Owin. Host. SystemWeb**. Utilise IIS avec le pipeline ASP.NET.
- **Microsoft. Owin. Host. HttpListener**. Utilise [System .net. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Ce serveur est actuellement l’option par défaut lors de l’auto-hébergement de Katana.

> [!NOTE]
> Katana ne fournit pas actuellement de OWIN middleware pour l’authentification Windows, car cette fonctionnalité est déjà disponible sur les serveurs.

## <a name="windows-authentication-in-iis"></a>Authentification Windows dans IIS

À l’aide de Microsoft. Owin. Host. SystemWeb, vous pouvez simplement activer l’authentification Windows dans IIS.

Commençons par créer une nouvelle application ASP.NET à l’aide du modèle de projet « application Web vide ASP.NET ».

![](enabling-windows-authentication-in-katana/_static/image1.png)

Ensuite, ajoutez des packages NuGet. Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

À présent, ajoutez une classe nommée `Startup` avec le code suivant :

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

C’est tout ce dont vous avez besoin pour créer une application « Hello World » pour OWIN, en cours d’exécution sur IIS. Appuyez sur F5 pour déboguer l'application. « Hello World! » doit dans la fenêtre du navigateur.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Nous allons ensuite activer l’authentification Windows dans IIS Express. Dans le menu **affichage** , sélectionnez **Propriétés**. Cliquez sur le nom du projet dans Explorateur de solutions pour afficher les propriétés du projet.

Dans la fenêtre **Propriétés** , affectez à **authentification anonyme** la valeur **désactivé** et à **authentification Windows** la valeur **activé**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Quand vous exécutez l’application à partir de Visual Studio, IIS Express nécessitera les informations d’identification Windows de l’utilisateur. Vous pouvez le voir à l’aide de [Fiddler](http://fiddler2.com/home) ou d’un autre outil de débogage http. Voici un exemple de réponse HTTP :

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Les en-têtes WWW-Authenticate dans cette réponse indiquent que le serveur prend en charge le protocole [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , qui utilise Kerberos ou NTLM.

Plus tard, lorsque vous déploierez l’application sur un serveur, procédez comme [suit](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) pour activer l’authentification Windows dans IIS sur ce serveur.

## <a name="windows-authentication-in-httplistener"></a>Authentification Windows dans HttpListener

Si vous utilisez Microsoft. Owin. Host. HttpListener pour l’auto-hébergement des Katana, vous pouvez activer l’authentification Windows directement sur l’instance **HttpListener** .

Tout d’abord, créez une application console. Ensuite, ajoutez des packages NuGet. Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

À présent, ajoutez une classe nommée `Startup` avec le code suivant :

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Cette classe implémente le même exemple « Hello World » que précédemment, mais définit également l’authentification Windows comme schéma d’authentification.

À l’intérieur de la fonction `Main`, démarrez le pipeline OWIN :

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Vous pouvez envoyer une demande dans Fiddler pour confirmer que l’application utilise l’authentification Windows :

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Rubriques connexes

[Vue d’ensemble du projet Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Fonctionnement de l’authentification par formulaire OWIN dans MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
