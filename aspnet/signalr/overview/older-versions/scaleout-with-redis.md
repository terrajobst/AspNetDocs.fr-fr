---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Signalr ScaleOut avec des ReDim (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 5079cf3fa773faeac911eddc75dc66e94ad98bb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536560"
---
# <a name="signalr-scaleout-with-redis-signalr-1x"></a>Scale-out de SignalR avec Redis (SignalR 1.x)

par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Dans ce didacticiel, vous allez utiliser des [redims](http://redis.io/) pour distribuer des messages à travers une application signalr déployée sur deux instances IIS distinctes.

Redims est un magasin clé-valeur en mémoire. Il prend également en charge un système de messagerie avec un modèle de publication/abonnement. Le fond de panier ReDim de Signalr utilise la fonctionnalité Pub/Sub pour transférer les messages vers d’autres serveurs.

![](scaleout-with-redis/_static/image1.png)

Pour ce didacticiel, vous allez utiliser trois serveurs :

- Deux serveurs exécutant Windows, que vous utiliserez pour déployer une application Signalr.
- Un serveur exécutant Linux, que vous utiliserez pour exécuter des ReDim. Pour les captures d’écran de ce didacticiel, j’ai utilisé le protocole TLS Ubuntu 12,04.

Si vous n’avez pas trois serveurs physiques à utiliser, vous pouvez créer des machines virtuelles sur Hyper-V. Une autre option consiste à créer des machines virtuelles sur Azure.

Bien que ce didacticiel utilise l’implémentation de ReDim officielle, il existe également un [port Windows de redims](https://github.com/MSOpenTech/redis) de MSOpenTech. L’installation et la configuration sont différentes, mais les étapes sont identiques dans le cas contraire.

> [!NOTE] 
> 
> Signalr ScaleOut avec Redims ne prend pas en charge les clusters ReDim.

## <a name="overview"></a>Présentation

Avant d’accéder au didacticiel détaillé, voici un aperçu rapide de ce que vous allez faire.

1. Installez les ReDim et démarrez le serveur ReDim.
2. Ajoutez ces packages NuGet à votre application : 

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signalr. Redims](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Créer une application Signalr.
4. Ajoutez le code suivant à global. asax pour configurer le fond de panier : 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu sur Hyper-V

À l’aide de Windows Hyper-V, vous pouvez facilement créer une machine virtuelle Ubuntu sur Windows Server.

Téléchargez le fichier ISO Ubuntu à partir de [http://www.ubuntu.com](http://www.ubuntu.com/).

Dans Hyper-V, ajoutez une nouvelle machine virtuelle. Dans l’étape **connecter un disque dur virtuel** , sélectionnez **créer un disque dur virtuel**.

![](scaleout-with-redis/_static/image2.png)

À l’étape **options d’installation** , sélectionnez **fichier image (. ISO)** , cliquez sur **Parcourir**, puis accédez à l’ISO d’installation Ubuntu.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Installer les ReDim

Suivez les étapes de [http://redis.io/download](http://redis.io/download) pour télécharger et générer des instructions ReDim.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Cela génère les fichiers binaires ReDim dans le répertoire `src`.

Par défaut, Redims ne requiert pas de mot de passe. Pour définir un mot de passe, modifiez le fichier `redis.conf`, qui se trouve dans le répertoire racine du code source. (Effectuez une copie de sauvegarde du fichier avant de le modifier) Ajoutez la directive suivante à `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

À présent, démarrez le serveur Redims :

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Ouvrez le port 6379, qui est le port par défaut sur lequel ReDim écoute. (Vous pouvez modifier le numéro de port dans le fichier de configuration.)

## <a name="create-the-signalr-application"></a>Créer l’application Signalr

Créez une application Signalr en suivant l’un des didacticiels suivants :

- [Prise en main avec Signalr](../getting-started/tutorial-getting-started-with-signalr.md)
- [Prise en main avec Signalr et MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Ensuite, nous allons modifier l’application de conversation pour prendre en charge ScaleOut avec Redims. Tout d’abord, ajoutez le package NuGet Signalr. Redims à votre projet. Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Ensuite, ouvrez le fichier global. asax. Ajoutez le code suivant à l' **Application\_méthode Start** :

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- « Server » est le nom du serveur qui exécute les insertions.
- *port* est le numéro de port
- « Password » est le mot de passe que vous avez défini dans le fichier redims. conf.
- « AppName » est une chaîne quelconque. Signalr crée un sous-canal de Pub/Sub portant ce nom.

Exemple :

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Déployer et exécuter l’application

Préparez vos instances de Windows Server pour déployer l’application Signalr.

Ajoutez le rôle IIS. Inclut des fonctionnalités de développement d’applications, notamment le protocole WebSocket.

![](scaleout-with-redis/_static/image5.png)

Incluez également le service de gestion (listé sous outils d’administration).

![](scaleout-with-redis/_static/image6.png)

**Installez Web Deploy 3,0.** Lorsque vous exécutez le gestionnaire des services Internet, vous êtes invité à installer Microsoft Web Platform ou vous pouvez [Télécharger le programme d’installation](https://go.microsoft.com/fwlink/?LinkId=255386). Dans le programme d’installation de la plateforme, recherchez Web Deploy et installez Web Deploy 3,0

![](scaleout-with-redis/_static/image7.png)

Vérifiez que le service de gestion Web est en cours d’exécution. Si ce n’est pas le cas, démarrez le service. (Si vous ne voyez pas service de gestion Web dans la liste des services Windows, assurez-vous que vous avez installé le service de gestion lorsque vous avez ajouté le rôle IIS.)

Par défaut, le service de gestion Web écoute le port TCP 8172. Dans le pare-feu Windows, créez une nouvelle règle de trafic entrant pour autoriser le trafic TCP sur le port 8172. Pour plus d’informations, consultez [configuration de règles de pare-feu](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Si vous hébergez les machines virtuelles sur Azure, vous pouvez le faire directement dans le Portail Azure. Consultez [Comment configurer des points de terminaison sur une machine virtuelle](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Vous êtes maintenant prêt à déployer le projet Visual Studio à partir de votre ordinateur de développement sur le serveur. Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution, puis cliquez sur **publier**.

Pour obtenir une documentation plus détaillée sur le déploiement Web, consultez [mappage de contenu de déploiement Web pour Visual Studio et ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).

Si vous déployez l’application sur deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent tous les messages Signalr de l’autre. (Bien sûr, dans un environnement de production, les deux serveurs se trouvent derrière un équilibreur de charge.)

![](scaleout-with-redis/_static/image8.png)

Si vous êtes curieux de voir les messages envoyés aux ReDim, vous pouvez utiliser le client **redims-CLI** , qui est installé avec des inversions.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
