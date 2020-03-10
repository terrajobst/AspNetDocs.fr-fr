---
uid: signalr/overview/getting-started/supported-platforms
title: Plateformes prises en charge | Microsoft Docs
author: bradygaster
description: Cet article décrit ce que les clients et les serveurs sont pris en charge par Signalr.
ms.author: bradyg
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 89730e591bb94022d16fe1a78a4350b38e0bf7a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558631"
---
# <a name="supported-platforms"></a>Plateformes prises en charge

de [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article décrit ce que les clients et les serveurs sont pris en charge par Signalr. 
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

Signalr est pris en charge sous une variété de configurations de serveur et de client. En outre, chaque option de transport a un ensemble d’exigences propres. Si la configuration requise pour un transport n’est pas disponible, Signalr effectue un basculement normal vers d’autres transports. Pour plus d’informations sur les transports pris en charge par Signalr, consultez [transports et secours](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Configuration requise pour le serveur

Le composant serveur Signalr peut être hébergé sur diverses configurations de serveur. Cette section décrit les versions prises en charge des systèmes d’exploitation, .NET Framework, Internet Information Server et d’autres composants.

### <a name="supported-server-operating-systems"></a>Systèmes d'exploitation serveurs pris en charge

Le composant serveur Signalr peut être hébergé dans les systèmes d’exploitation serveur ou client suivants. Notez que pour que Signalr utilise WebSockets, Windows Server 2012, Windows Server 2016 ou Windows 8 est requis (WebSocket peut être utilisé sur les sites Web Windows Azure, tant que la version du .NET Framework du site est définie sur 4,5 et que Web Sockets est activé dans le Page Configuration).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 R2
- Windows 10
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>Version de .NET Framework de serveur prise en charge

Signalr 2 est uniquement pris en charge sur .NET Framework 4,5. Consultez la section [mises à jour recommandées](#updates) pour connaître les mises à jour qui améliorent la fiabilité, la compatibilité, la stabilité et les performances.

### <a name="supported-server-iis-versions"></a>Versions de serveur IIS prises en charge

Lorsque Signalr est hébergé dans IIS, les versions suivantes sont prises en charge. Notez qu’en cas d’utilisation d’un système d’exploitation client, par exemple pour le développement (Windows 8 ou Windows 7), il est préférable de ne pas utiliser de versions complètes d’IIS ou de Cassini, car il y aura une limite de 10 connexions simultanées imposées, qui sera rapidement atteinte depuis les connexions. sont temporaires, souvent rétablies et ne sont pas supprimés immédiatement lorsqu’ils ne sont plus utilisés. IIS Express doit être utilisé sur les systèmes d’exploitation clients.

Notez également que pour que Signalr utilise WebSocket, IIS 8 ou IIS 8 Express doit être utilisé, le serveur doit utiliser Windows 8, Windows Server 2012 ou une version ultérieure, et WebSocket doit être activé dans IIS. Pour plus d’informations sur l’activation de WebSocket dans IIS, consultez [prise en charge du protocole WebSocket iis 8,0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 ou IIS 8 Express.
- IIS 7 et 7,5. La prise en charge des [URL sans extension](https://support.microsoft.com/kb/980368) est requise.
- IIS doit s’exécuter en mode intégré ; le mode classique n’est pas pris en charge. Les retards de message pouvant atteindre 30 secondes peuvent être rencontrés si IIS est exécuté en mode classique à l’aide du transport des événements envoyés par le serveur.
- L’application d’hébergement doit s’exécuter en mode de confiance totale.

## <a name="client-system-requirements"></a>Configuration système requise du client

Signalr peut être utilisé dans diverses plateformes clientes. Cette section décrit la configuration système requise pour l’utilisation de Signalr dans des navigateurs Web, des applications de bureau Windows, des applications Silverlight et des appareils mobiles.

### <a name="web-browsers"></a>Navigateurs Web

Signalr peut être utilisé dans différents navigateurs Web, mais en général, seules les deux dernières versions sont prises en charge.

Les applications qui utilisent Signalr dans les navigateurs doivent utiliser jQuery version 1.6.4 ou les versions majeures ultérieures (telles que 1.7.2, 1.8.2 ou 1.9.1).

Signalr peut être utilisé dans les navigateurs suivants :

- Microsoft Internet Explorer versions 8, 9, 10 et 11. Les versions Modern, Desktop et mobile sont prises en charge.
- Mozilla Firefox : version actuelle-1, versions Windows et Mac.
- Google Chrome : version actuelle-1, versions Windows et Mac.
- Safari : version actuelle-1, versions Mac et iOS.
- Opera : version actuelle-1, Windows uniquement.
- Navigateur Android

En plus d’exiger certains navigateurs, les différents transports utilisés par signaler ont des exigences propres. Les transports suivants sont pris en charge dans les configurations suivantes :

<a id="browser"></a>

**Conditions requises pour le transport du navigateur Web**

| Transport | Internet Explorer | Chrome (Windows ou iOS) | Firefox | Safari (OSX ou iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | actuel-1 | actuel-1 | actuel-1 | N/A |
| Événements envoyés par le serveur | N/A | actuel-1 | actuel-1 | actuel-1 | N/A |
| ForeverFrame | 8+ | N/A | N/A | N/A | 4,1 |
| Interrogation longue (Long polling) | 8+ | actuel-1 | actuel-1 | actuel-1 | 4,1 |

\*: 6 + requis pour bénéficier de toutes les fonctionnalités.

#### <a name="unsupported-browsers"></a>Navigateurs non pris en charge

Bien que Signalr *puisse* s’exécuter sans problème majeur dans les anciennes versions de navigateur, nous ne les testons pas activement signalr et ne corrigent généralement pas les bogues qui peuvent y figurer.

### <a name="windows-desktop-and-silverlight-applications"></a>Applications de bureau Windows et Silverlight

En plus de s’exécuter dans un navigateur Web, Signalr peut être hébergé dans des applications clientes Windows autonomes ou Silverlight. Les applications de bureau Windows et Silverlight Signalr ont la configuration système suivante.

- Les applications utilisant .NET 4 sont prises en charge sur Windows XP SP3 ou version ultérieure.
- Les applications qui utilisent .NET Framework 4,5 sont prises en charge sur Windows Vista ou version ultérieure.

Outre la configuration requise du système d’exploitation et du .NET Framework, les transports disponibles pour signaler ont des exigences propres. Les transports suivants sont pris en charge dans les configurations suivantes :

**Conditions requises pour le transport Windows Desktop et Silverlight**

| Transport | Application .NET | Silverlight |
| --- | --- | --- |
| Web Sockets | Windows 8 + et .NET 4.5 + | N/A |
| Frame illimité | N/A | N/A |
| Événements envoyés par le serveur | .NET 4 + | 5+ |
| Interrogation longue (Long polling) | .NET 4 + | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Applications Windows Store et Windows Phone

Signalr peut être utilisé dans les applications du Windows Store et les applications Windows Phone 8. Les transports suivants sont pris en charge dans les configurations suivantes :

**Conditions requises pour le transport Windows Store et Windows Phone**

| Transport | Windows Store/.NET | Windows Store/JavaScript | Windows Phone/IE | Windows Phone/.NET |
| --- | --- | --- | --- | --- |
| WebSockets | N/A | WIN8 + | 8+ | N/A |
| Frame illimité | N/A | WIN8 + | 7.5+ | N/A |
| Événements envoyés par le serveur | WIN8 + | N/A | N/A | 8+ |
| Interrogation longue (Long polling) | WIN8 + | WIN8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Mises à jour recommandées

Les mises à jour suivantes sont recommandées pour les serveurs Signalr :

- Une mise à jour pour .NET Framework 4,5 est disponible [ici](https://support.microsoft.com/kb/2750149).
- Microsoft mettra périodiquement à jour les QFE pour ASP.NET. Celles-ci doivent être appliquées comme disponibles.
