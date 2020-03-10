---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Utilisation de Signalr avec Web Apps dans Azure App Service | Microsoft Docs
author: bradygaster
description: Ce document explique comment configurer une application Signalr qui s’exécute sur Microsoft Azure. Versions logicielles utilisées dans le didacticiel Visual Studio 2013 ou...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558701"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a>Utilisation de SignalR avec Web Apps dans Azure App Service

de [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document explique comment configurer une application Signalr qui s’exécute sur Microsoft Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) ou Visual Studio 2012
> - .NET 4.5
> - Signalr version 2
> - Kit de développement logiciel (SDK) Azure 2,3 pour Visual Studio 2013 ou 2012
>
>
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), ou sur les [forums Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).

## <a name="table-of-contents"></a>Sommaire

- [Introduction](#introduction)
- [Déploiement d’une application Web Signalr sur Azure App Service](#deploying)
- [Activation de WebSockets sur Azure App Service](#websocket)
- [Utilisation du fond de panier du cache Redims Azure](#backplane)
- [Étapes suivantes](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introduction

ASP.NET Signalr peut être utilisé pour mettre en place un nouveau niveau d’interactivité entre les serveurs et les clients Web ou .NET. Lorsqu’ils sont hébergés dans Azure, les applications Signalr peuvent tirer parti de l’environnement hautement disponible, évolutif et performant qui s’exécute dans le Cloud.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Déploiement d’une application Web Signalr sur Azure App Service

Signalr n’ajoute pas de complications particulières au déploiement d’une application sur Azure par rapport au déploiement sur un serveur local. Une application qui utilise Signalr peut être hébergée dans Azure sans aucune modification de configuration ou d’autres paramètres (Toutefois, pour la prise en charge de WebSockets, consultez [activation de WebSockets sur Azure App service](#websocket) ci-dessous). Pour ce didacticiel, vous allez déployer l’application créée dans le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md) sur Azure.

**Conditions préalables**

- Visual Studio 2013. Si vous n’avez pas Visual Studio, Visual Studio 2013 Express pour le Web est inclus dans l’installation du kit de développement logiciel (SDK) Azure.
- [Azure sdk 2,3 pour Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) ou le [Kit de développement logiciel (sdk) Azure 2,3 pour Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Pour suivre le didacticiel, vous devez disposer d'un abonnement Azure. Vous pouvez [activer les avantages de votre abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)ou vous [inscrire pour bénéficier d’un abonnement d’évaluation](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Déploiement d’une application Web Signalr sur Azure

1. Suivez le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md)ou téléchargez le projet terminé à partir de la [Galerie de code](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. Dans Visual Studio, sélectionnez **générer**, **publier signalr chat**.
3. Dans la boîte de dialogue publier le site Web, sélectionnez « sites Web Windows Azure ».

    ![Sélectionner les sites Web Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Si vous n’êtes pas connecté à votre compte Microsoft, cliquez sur **se connecter...** dans la boîte de dialogue « Sélectionner un site Web existant », puis connectez-vous.

    ![Sélectionner un site Web existant](using-signalr-with-azure-web-sites/_static/image2.png)    ![Se connecter à Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. Dans la boîte de dialogue « Sélectionner un site Web existant », cliquez sur **nouveau**.

    ![Nouveau site Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. Dans la boîte de dialogue « créer un site sur Windows Azure », entrez un nom d’application unique. Sélectionnez la région la plus proche de vous dans la liste déroulante région. Cliquez sur **Créer**.

    ![Créer un site sur Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. Dans la boîte de dialogue publier le site Web, cliquez sur **publier**.

    ![Publier le site](using-signalr-with-azure-web-sites/_static/image6.png)
8. Une fois que l’application a terminé la publication, l’application Signalr chat hébergée dans Azure App Service Web Apps s’ouvre dans un navigateur.

    ![Ouverture de site dans un navigateur](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Activation de WebSockets sur Azure App Service Web Apps

Les WebSockets doivent être explicitement activés dans votre application Web pour être utilisés dans une application Signalr. dans le cas contraire, d’autres protocoles seront utilisés (consultez [transports et secours](../getting-started/introduction-to-signalr.md#transports) pour plus d’informations).

Pour utiliser WebSockets sur Azure App Service Web Apps, activez-le dans la section de configuration de l’application Web. Pour ce faire, ouvrez votre application Web dans le [portail de gestion Azure](https://manage.windowsazure.com/), puis sélectionnez configurer.

![Onglet Configurer](using-signalr-with-azure-web-sites/_static/image8.png)

En haut de la page de configuration, assurez-vous que .NET 4,5 est utilisé pour votre application Web.

![Paramètre .NET Framework version 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

Sur la page Configuration, dans le paramètre **WebSockets** , sélectionnez **activé**.

![Paramètre WebSocket : activé](using-signalr-with-azure-web-sites/_static/image10.png)

En bas de la page Configuration, sélectionnez **Enregistrer** pour enregistrer vos modifications.

![Enregistrer les paramètres](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Utilisation du fond de panier du cache Redims Azure

Si vous utilisez plusieurs instances pour votre application Web et que les utilisateurs de ces instances doivent interagir les uns avec les autres (par exemple, les messages de conversation créés dans une instance peuvent atteindre les utilisateurs connectés à d’autres instances), le [fond de panier du cache](../performance/scaleout-with-redis.md) d’inversion Azure doit être implémenté dans votre application.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les Web Apps dans Azure App Service, consultez [Web Apps Overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
