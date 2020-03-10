---
uid: signalr/overview/older-versions/signalr-performance
title: Performances de signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Performances de SignalR
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 915fd822caae9bbcb0a688c6dd7a5b2bda12c9df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579610"
---
# <a name="signalr-performance-signalr-1x"></a>Performances de SignalR (SignalR 1.x)

de [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique explique comment concevoir, mesurer et améliorer les performances dans une application Signalr.

Pour obtenir une présentation récente des performances et de la mise à l’échelle de Signalr, consultez [mise à l’échelle du Web en temps réel avec ASP.net signalr](https://channel9.msdn.com/Events/Build/2013/3-502).

Cette rubrique contient les sections suivantes :

- [Remarques relatives à la conception](#design)
- [Réglage des performances de votre serveur Signalr](#tuning)
- [Résolution des problèmes de performances](#troubleshooting)
- [Utilisation des compteurs de performance Signalr](#perfcounters)
- [Utilisation d’autres compteurs de performances](#othercounters)
- [Autres ressources](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Remarques relatives à la conception

Cette section décrit les modèles qui peuvent être implémentés lors de la conception d’une application Signalr, afin de garantir que les performances ne sont pas gênées par la génération d’un trafic réseau inutile.

### <a name="throttling-message-frequency"></a>Limitation de la fréquence des messages

Même dans une application qui envoie des messages à une fréquence élevée (par exemple, une application de jeu en temps réel), la plupart des applications n’ont pas besoin d’envoyer plus de quelques messages par seconde. Pour réduire le volume de trafic généré par chaque client, il est possible d’implémenter une boucle de messages qui met en file d’attente et envoie des messages plus fréquemment qu’un taux fixe (autrement dit, jusqu’à un certain nombre de messages sont envoyés chaque seconde, s’il y a des messages dans ce délai en onstruction à envoyer). Pour obtenir un exemple d’application qui limite les messages à un certain taux (à partir du client et du serveur), consultez [temps réel haute fréquence avec signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Réduction de la taille des messages

Vous pouvez réduire la taille d’un message Signalr en réduisant la taille de vos objets sérialisés. Dans le code serveur, si vous envoyez un objet qui contient des propriétés qui n’ont pas besoin d’être transmises, empêchez la sérialisation de ces propriétés à l’aide de l’attribut `JsonIgnore`. Les noms des propriétés sont également stockés dans le message. vous pouvez raccourcir les noms des propriétés à l’aide de l’attribut `JsonProperty`. L’exemple de code suivant montre comment exclure une propriété de l’envoi au client et comment raccourcir les noms de propriété :

**Code du serveur .NET illustrant l’attribut JsonIgnore pour empêcher l’envoi de données au client et l’attribut JsonProperty pour réduire la taille des messages**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Afin de conserver la lisibilité et la maintenabilité dans le code client, les noms de propriété abrégés peuvent être remappés à des noms conviviaux une fois le message reçu. L’exemple de code suivant montre une manière possible de remapper les noms abrégés à des noms plus longs, en définissant un contrat de message (mappage) et en utilisant la fonction `reMap` pour appliquer le contrat à la classe de message optimisée :

**Code JavaScript côté client qui remappe les noms de propriété abrégés à des noms explicites**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Il est également possible de raccourcir les noms des messages du client au serveur, en utilisant la même méthode.

La réduction de l’encombrement mémoire (c’est-à-dire la quantité de mémoire utilisée pour le message) de l’objet message peut également améliorer les performances. Par exemple, si la plage complète d’un `int` n’est pas nécessaire, un `short` ou `byte` peut être utilisé à la place.

Étant donné que les messages sont stockés dans le bus de messages dans la mémoire du serveur, la réduction de la taille des messages peut également résoudre les problèmes de mémoire du serveur.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Réglage des performances de votre serveur Signalr

Les paramètres de configuration suivants peuvent être utilisés pour adapter votre serveur afin d’améliorer les performances dans une application Signalr. Pour obtenir des informations générales sur l’amélioration des performances dans une application ASP.NET, consultez [amélioration des performances des ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).

**Paramètres de configuration de signalr**

- **DefaultMessageBufferSize**: par défaut, signalr conserve 1000 messages en mémoire par concentrateur par connexion. Si des messages volumineux sont utilisés, cela peut créer des problèmes de mémoire qui peuvent être atténués en réduisant cette valeur. Ce paramètre peut être défini dans le gestionnaire d’événements `Application_Start` dans une application ASP.NET ou dans la méthode `Configuration` d’une classe de démarrage OWIN dans une application auto-hébergée. L’exemple suivant montre comment réduire cette valeur afin de réduire l’encombrement mémoire de votre application afin de réduire la quantité de mémoire du serveur utilisée :

    **Code du serveur .NET dans global. asax pour la réduction de la taille de la mémoire tampon des messages par défaut**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Paramètres de configuration IIS**

- Nombre **maximal de demandes simultanées par application**: l’augmentation du nombre de demandes IIS simultanées augmente les ressources serveur disponibles pour traiter les demandes. La valeur par défaut est 5000 ; pour augmenter ce paramètre, exécutez les commandes suivantes dans une invite de commandes avec élévation de privilèges :

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**Paramètres de configuration de ASP.NET**

Cette section comprend des paramètres de configuration qui peuvent être définis dans le fichier `aspnet.config`. Ce fichier se trouve dans l’un des deux emplacements, en fonction de la plateforme :

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Les paramètres ASP.NET qui peuvent améliorer les performances de signaler sont les suivants :

- **Nombre maximal de demandes simultanées par UC**: l’amélioration de ce paramètre peut réduire les goulots d’étranglement de performances. Pour augmenter ce paramètre, ajoutez le paramètre de configuration suivant au fichier `aspnet.config` :

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Limite de la file d’attente des demandes**: lorsque le nombre total de connexions dépasse le paramètre `maxConcurrentRequestsPerCPU`, ASP.net commence à limiter les demandes à l’aide d’une file d’attente. Pour augmenter la taille de la file d’attente, vous pouvez augmenter le paramètre `requestQueueLimit`. Pour ce faire, ajoutez le paramètre de configuration suivant au nœud `processModel` dans `config/machine.config` (plutôt que `aspnet.config`) :

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Résolution des problèmes de performances

Cette section décrit les méthodes permettant de trouver des goulots d’étranglement au niveau des performances dans votre application.

### <a name="verifying-that-websocket-is-being-used"></a>Vérification de l’utilisation de WebSocket

Alors que Signalr peut utiliser un grand nombre de transports pour la communication entre le client et le serveur, WebSocket présente un avantage significatif en termes de performances et doit être utilisé si le client et le serveur le prennent en charge. Pour déterminer si votre client et votre serveur satisfont à la configuration requise pour WebSocket, consultez [transports et secours](../getting-started/introduction-to-signalr.md#transports). Pour déterminer le transport utilisé dans votre application, vous pouvez utiliser les outils de développement du navigateur et examiner les journaux pour voir quel transport est utilisé pour la connexion. Pour plus d’informations sur l’utilisation des outils de développement de navigateur dans Internet Explorer et chrome, consultez [transports et secours](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Utilisation des compteurs de performance Signalr

Cette section décrit comment activer et utiliser les compteurs de performance Signalr, qui se trouvent dans le package `Microsoft.AspNet.SignalR.Utils`.

### <a name="installing-signalrexe"></a>Installation de signalr. exe

Les compteurs de performances peuvent être ajoutés au serveur à l’aide d’un utilitaire appelé Signalr. exe. Pour installer cet utilitaire, procédez comme suit :

1. Dans Visual Studio, sélectionnez **outils** > **Gestionnaire de package NuGet** > **gérer les packages NuGet pour la solution**
2. Recherchez **signalr. utils**, puis sélectionnez Installer.

    ![](signalr-performance/_static/image1.png)
3. Acceptez le contrat de licence pour installer le package.
4. Signalr. exe sera installé dans `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Installation des compteurs de performances avec Signalr. exe

Pour installer les compteurs de performance Signalr, exécutez Signalr. exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Pour supprimer les compteurs de performance Signalr, exécutez Signalr. exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Compteurs de performance signalr

Le package utilitaires installe les compteurs de performances suivants. Les compteurs « total » mesurent le nombre d’événements depuis le dernier redémarrage du pool d’applications ou du serveur.

**Métriques de connexion**

Les métriques suivantes mesurent les événements de durée de vie de la connexion qui se produisent. Pour plus d’informations, consultez [comprendre et gérer les événements de durée de vie des connexions](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Connexions connectées**
- **Connexions reconnectées**
- **Connexions déconnectées**
- **Connexions en cours**

**Métriques du message**

Les métriques suivantes mesurent le trafic de messages généré par Signalr.

- **Nombre total de messages de connexion reçus**
- **Nombre total de messages de connexion envoyés**
- **Messages de connexion reçus/s**
- **Messages de connexion envoyés/s**

**Métriques du bus de messages**

Les métriques suivantes mesurent le trafic via le bus de messages Signalr interne, la file d’attente dans laquelle tous les messages entrants et sortants Signalr sont placés. Un message est **publié** lors de son envoi ou de sa diffusion. Un **abonné** dans ce contexte est un abonnement sur le bus de messages ; Cela doit être égal au nombre de clients plus le serveur lui-même. Un **Worker alloué** est un composant qui envoie des données à des connexions actives. un **Worker occupé est un Worker** qui envoie activement un message.

- **Nombre total de messages reçus par le bus de messages**
- **Messages du bus de messages reçus/s**
- **Nombre total de messages publiés par le bus de messages**
- **Messages du bus de messages publiés/s**
- **Abonnés au bus de messages actuel**
- **Nombre total d’abonnés au bus de messages**
- **Abonnés au bus de messages/s**
- **Threads de messagerie alloués**
- **Threads occupés par le bus de messages**
- **Rubriques du bus de messages en cours**

**Métriques d’erreur**

Les métriques suivantes mesurent les erreurs générées par le trafic des messages Signalr. Des erreurs de **résolution de concentrateur** se produisent lorsqu’une méthode de concentrateur ou de concentrateur ne peut pas être résolue. Les erreurs d' **appel de concentrateur** sont des exceptions levées lors de l’appel d’une méthode de concentrateur. Les erreurs de **transport** sont des erreurs de connexion levées au cours d’une requête ou d’une réponse http.

- **Erreurs : total**
- **Erreurs : toutes les/s**
- **Erreurs : total de résolution de concentrateur**
- **Erreurs : résolution de concentrateur/s**
- **Erreurs : total d’invocation de concentrateur**
- **Erreurs : appel de Hub/s**
- **Erreurs : transport total**
- **Erreurs : transport/s**

**Mesures ScaleOut**

Les métriques suivantes mesurent le trafic et les erreurs générées par le fournisseur ScaleOut. Un **flux** dans ce contexte est une unité d’échelle utilisée par le fournisseur ScaleOut. Il s’agit d’une table si SQL Server est utilisé, une rubrique si Service Bus est utilisé et un abonnement si Redims est utilisé. Par défaut, un seul flux est utilisé, mais il peut être augmenté via la configuration de SQL Server et Service Bus. Un flux de **mise en mémoire tampon** est un flux qui est entré dans un état d’erreur ; Lorsque le flux est dans l’état Faulted, tous les messages envoyés au backplane échouent immédiatement jusqu’à ce que le flux ne génère plus de défaillance. La **longueur de la file d’attente d’envoi** est le nombre de messages qui ont été publiés mais pas encore envoyés.

- **Messages du bus des messages ScaleOut reçus/s**
- **Total des flux ScaleOut**
- **Flux ScaleOut ouverts**
- **Mise en mémoire tampon des flux ScaleOut**
- **Nombre total d’erreurs ScaleOut**
- **Erreurs de ScaleOut/s**
- **Longueur de la file d’attente d’envoi ScaleOut**

Pour plus d’informations sur la mesure de ces compteurs, consultez [signalr ScaleOut avec Azure Service bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Utilisation d’autres compteurs de performances

Les compteurs de performances suivants peuvent également être utiles pour analyser les performances de votre application.

**Mémoire**

- Mémoire CLR .NET nombre d’octets dans tous les tas (pour w3wp)

**ASP.NET**

- ASP. asp.net \ requêtes actuel
- ASP.NET\Queued
- ASP. NET\Rejected

**Processeur**

- Temps de Information\Processor du processeur

**TCP/IP**

- TCPv6/connexions établies
- TCPv4/connexions établies

**Service Web**

- Connexions Web Web\connexions
- Connexions Web Service\Maximum

**Thread**

- \# CLR .NET LocksAndThreads de threads logiques actuels
- Threads .NET CLR LocksAnd\# de threads physiques actuels

<a id="otherresources"></a>

## <a name="other-resources"></a>Autres ressources

Pour plus d’informations sur l’analyse et le réglage des performances ASP.NET, consultez les rubriques suivantes :

- [Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Utilisation des threads ASP.NET sur IIS 7,5, IIS 7,0 et IIS 6,0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt;, élément (paramètres Web)](https://msdn.microsoft.com/library/dd560842.aspx)
