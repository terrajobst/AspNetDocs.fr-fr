---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Signalr ScaleOut avec SQL Server | Microsoft Docs
author: bradygaster
description: Versions logicielles utilisées dans cette rubrique Visual Studio 2013 .NET 4,5 Signalr version 2 versions précédentes de cette rubrique pour plus d’informations sur les versions antérieures de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 709a9ebf8f3396842bee0d87e621c00ae1418ec1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579183"
---
# <a name="signalr-scaleout-with-sql-server"></a>Scale-out de SignalR avec SQL Server

par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans cette rubrique
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr version 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versions précédentes de cette rubrique
>
> Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

Dans ce didacticiel, vous allez utiliser SQL Server pour distribuer des messages à travers une application Signalr déployée dans deux instances IIS distinctes. Vous pouvez également exécuter ce didacticiel sur un seul ordinateur de test, mais pour obtenir un effet complet, vous devez déployer l’application Signalr sur deux serveurs ou plus. Vous devez également installer SQL Server sur l’un des serveurs, ou sur un serveur dédié distinct. Une autre option consiste à exécuter le didacticiel à l’aide de machines virtuelles sur Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Conditions préalables requises

Microsoft SQL Server 2005 ou version ultérieure. Le fond de panier prend en charge les éditions Desktop et Server de SQL Server. Il ne prend pas en charge l’édition ou la Azure SQL Database SQL Server Compact. (Si votre application est hébergée sur Azure, envisagez plutôt le Service Bus backplane.)

## <a name="overview"></a>Présentation

Avant d’accéder au didacticiel détaillé, voici un aperçu rapide de ce que vous allez faire.

1. Créez une nouvelle base de données vide. Le fond de panier crée les tables nécessaires dans cette base de données.
2. Ajoutez ces packages NuGet à votre application :

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signalr. SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Créer une application Signalr.
4. Ajoutez le code suivant à Startup.cs pour configurer le fond de panier :

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   Ce code configure le fond de panier avec les valeurs par défaut de [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) et [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Pour plus d’informations sur la modification de ces valeurs, consultez [performance de signalr : mesures ScaleOut](signalr-performance.md#scaleout_metrics).

## <a name="configure-the-database"></a>Configurer la base de données

Déterminez si l’application doit utiliser l’authentification Windows ou l’authentification SQL Server pour accéder à la base de données. Quelle que soit la valeur, assurez-vous que l’utilisateur de base de données dispose des autorisations nécessaires pour se connecter, créer des schémas et créer des tables.

Créez une nouvelle base de données pour le fond de panier à utiliser. Vous pouvez attribuer à la base de données n’importe quel nom. Vous n’avez pas besoin de créer de tables dans la base de données ; le fond de panier crée les tables nécessaires.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Activer Service Broker

Il est recommandé d’activer Service Broker pour la base de données de fond de panier. Service Broker fournit une prise en charge native de la messagerie et de la mise en file d’attente dans SQL Server, ce qui permet au fond de panier de recevoir des mises à jour plus efficacement. (Toutefois, le fond de panier fonctionne également sans Service Broker.)

Pour vérifier si la Service Broker est activée, interrogez la colonne **is\_Broker\_activée** de l’affichage catalogue **sys. databases** .

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Pour activer Service Broker, utilisez la requête SQL suivante :

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Si cette requête semble se bloquer, assurez-vous qu’aucune application n’est connectée à la base de la base de la.

Si vous avez activé le suivi, les traces indiquent également si Service Broker est activé.

## <a name="create-a-signalr-application"></a>Créer une application Signalr

Créez une application Signalr en suivant l’un des didacticiels suivants :

- [Prise en main avec Signalr 2,0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Prise en main avec Signalr 2,0 et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Ensuite, nous allons modifier l’application de conversation pour prendre en charge ScaleOut avec SQL Server. Tout d’abord, ajoutez le package NuGet Signalr. SqlServer à votre projet. Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Ensuite, ouvrez le fichier Startup.cs. Ajoutez le code suivant à la méthode **configure** :

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Déployer et exécuter l’application

Préparez vos instances de Windows Server pour déployer l’application Signalr.

Ajoutez le rôle IIS. Inclut des fonctionnalités de développement d’applications, notamment le protocole WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Incluez également le service de gestion (listé sous outils d’administration).

![](scaleout-with-sql-server/_static/image5.png)

**Installez Web Deploy 3,0.** Lorsque vous exécutez le gestionnaire des services Internet, vous êtes invité à installer Microsoft Web Platform ou vous pouvez [Télécharger le programme d’installation](https://go.microsoft.com/fwlink/?LinkId=255386). Dans le programme d’installation de la plateforme, recherchez Web Deploy et installez Web Deploy 3,0

![](scaleout-with-sql-server/_static/image6.png)

Vérifiez que le service de gestion Web est en cours d’exécution. Si ce n’est pas le cas, démarrez le service. (Si vous ne voyez pas service de gestion Web dans la liste des services Windows, assurez-vous que vous avez installé le service de gestion lorsque vous avez ajouté le rôle IIS.)

Enfin, ouvrez le port 8172 pour TCP. Il s’agit du port utilisé par l’outil Web Deploy.

Vous êtes maintenant prêt à déployer le projet Visual Studio à partir de votre ordinateur de développement sur le serveur. Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution, puis cliquez sur **publier**.

Pour obtenir une documentation plus détaillée sur le déploiement Web, consultez [mappage de contenu de déploiement Web pour Visual Studio et ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).

Si vous déployez l’application sur deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent tous les messages Signalr de l’autre. (Bien sûr, dans un environnement de production, les deux serveurs se trouvent derrière un équilibreur de charge.)

![](scaleout-with-sql-server/_static/image7.png)

Après avoir exécuté l’application, vous pouvez voir que Signalr a créé automatiquement des tables dans la base de données :

![](scaleout-with-sql-server/_static/image8.png)

Signalr gère les tables. Tant que votre application est déployée, ne supprimez pas les lignes, modifiez la table, et ainsi de suite.
