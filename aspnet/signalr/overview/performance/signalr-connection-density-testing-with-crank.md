---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Test de densité de connexion signalr avec manivelle | Microsoft Docs
author: bradygaster
description: Test de la densité des connexions SignalR avec Crank
ms.author: bradyg
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 901e039fbb81651ed18d560c99745b7e7f716e01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558337"
---
# <a name="signalr-connection-density-testing-with-crank"></a>Test de la densité des connexions SignalR avec Crank

par [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article explique comment utiliser l’outil manivelle pour tester une application avec plusieurs clients simulés.

Une fois que votre application s’exécute dans son environnement d’hébergement (rôle Web Azure, IIS ou auto-hébergé à l’aide de Owin), vous pouvez tester la réponse de l’application à un niveau élevé de densité de connexion à l’aide de l’outil manivelle. L’environnement d’hébergement peut être un serveur Internet Information Services (IIS), un hôte Owin ou un rôle Web Azure. (Remarque : les compteurs de performance ne sont pas disponibles sur Azure App Service Web Apps. vous ne pourrez donc pas obtenir de données de performances à partir d’un test de densité de connexion.)

La densité de connexion fait référence au nombre de connexions TCP simultanées qui peuvent être établies sur un serveur. Chaque connexion TCP entraîne une surcharge, et l’ouverture d’un grand nombre de connexions inactives finit par créer un goulot d’étranglement de mémoire.

[Le code base de signalr](https://github.com/signalr/signalr) comprend un outil de test de charge appelé **manivelle**. La dernière version de manivelle se trouve dans [la branche dev](https://github.com/SignalR/signalr/tree/dev) sur GitHub. Vous pouvez télécharger une archive zip de la branche dev de la base de code de Signalr [ici](https://github.com/SignalR/SignalR/archive/dev.zip).

La manivelle peut être utilisée pour saturer entièrement la mémoire du serveur afin de calculer le nombre total de connexions inactives possibles sur le matériel du serveur. Vous pouvez également utiliser la manivelle pour tester la charge du serveur sous une certaine quantité de sollicitation de la mémoire, en rampant les connexions jusqu’à ce qu’un nombre spécifique ou un seuil de mémoire spécifique soit atteint.

Lors du test, il est important d’utiliser des clients distants pour éviter toute concurrence pour les ressources (par exemple, les connexions TCP et la mémoire). Surveillez le ou les clients pour vous assurer qu’ils n’atteignent pas les goulots d’étranglement susceptibles d’empêcher le serveur d’atteindre sa capacité maximale (mémoire ou UC). Vous devrez peut-être augmenter le nombre de clients afin de charger entièrement le serveur.

### <a name="running-a-connection-density-test"></a>Exécution d’un test de densité de connexion

Cette section décrit les étapes nécessaires à l’exécution d’un test de densité de connexion sur une application Signalr.

1. Téléchargez et générez la [branche dev du code base de signalr](https://github.com/SignalR/SignalR/archive/dev.zip). Dans une invite de commandes, accédez à &lt;répertoire du projet&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Déployez votre application dans son environnement d’hébergement prévu. Prenez note du point de terminaison que votre application utilise ; par exemple, dans l’application créée dans le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md), le point de terminaison est `http://<yourhost>:8080/signalr`.
3. Installez les [compteurs de performance signalr](signalr-performance.md#perfcounters) sur le serveur. Si votre application s’exécute sur Azure, consultez [utilisation des compteurs de performance signalr dans un rôle Web Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Une fois que vous avez téléchargé et créé le code base et installé les compteurs de performance sur votre hôte, l’outil de ligne de commande manivelle se trouve dans le dossier `src\Microsoft.AspNet.SignalR.Crank\bin\Debug`.

Les options disponibles pour l’outil manivelle sont les suivantes :

- **/ ?** : Affiche l’écran d’aide. Les options disponibles sont également affichées si le paramètre **URL** est omis.
- **/URL**: URL des connexions signalr. Ce paramètre est obligatoire. Pour une application Signalr utilisant le mappage par défaut, le chemin d’accès se termine par « /signalr ».
- **/** : Nom du transport utilisé. La valeur par défaut est `auto`, qui sélectionne le meilleur protocole disponible. Les options incluent `WebSockets`, `ServerSentEvents`et `LongPolling` (`ForeverFrame` n’est pas une option pour manivelle, puisque le client .NET plutôt qu’Internet Explorer est utilisé). Pour plus d’informations sur la façon dont Signalr sélectionne les transports, consultez [transports et secours](../getting-started/introduction-to-signalr.md#transports).
- **/BatchSize**: nombre de clients ajoutés dans chaque lot. La valeur par défaut est de 50.
- **/ConnectInterval**: intervalle, en millisecondes, entre les ajouts de connexions. La valeur par défaut est 500.
- **/Connections**: nombre de connexions utilisées pour le test de charge de l’application. La valeur par défaut est 100 000.
- **/ConnectTimeout**: délai d’attente en secondes avant l’abandon du test. La valeur par défaut est 300.
- **MinServerMBytes**: le nombre minimal de mégaoctets de serveur à atteindre. La valeur par défaut est 500.
- **SendBytes**: taille de la charge utile envoyée au serveur en octets. La valeur par défaut est 0.
- **SendInterval**: délai en millisecondes entre les messages sur le serveur. La valeur par défaut est 500.
- **SendTimeout**: délai d’expiration, en millisecondes, des messages sur le serveur. La valeur par défaut est 300.
- **ControllerUrl**: URL où un client hébergera un concentrateur de contrôleur. La valeur par défaut est null (pas de concentrateur de contrôleur). Le concentrateur de contrôleur est démarré au démarrage de la session de manivelle. aucun contact supplémentaire entre le hub de contrôleur et le manivelle n’est effectué.
- **NumClients**: nombre de clients simulés pour la connexion à l’application. La valeur par défaut est un.
- **Logfile**: nom de fichier du fichier journal de la série de tests. La valeur par défaut est `crank.csv`.
- **SampleInterval**: durée en millisecondes entre les exemples de compteurs de performances. La valeur par défaut est 1000.
- **SignalRInstance**: nom de l’instance des compteurs de performance sur le serveur. La valeur par défaut consiste à utiliser l’état de la connexion cliente.

### <a name="example"></a>Exemple

La commande suivante teste un site appelé `pfsignalr` sur Azure qui héberge une application sur le port 8080 avec un concentrateur nommé « ControllerHub », à l’aide de connexions 100.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
