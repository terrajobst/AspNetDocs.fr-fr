---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Signalr ScaleOut avec Azure Service Bus | Microsoft Docs
author: bradygaster
description: Versions logicielles utilisées dans cette rubrique Visual Studio 2013 .NET 4,5 Signalr version 2 versions précédentes de cette rubrique pour la version Signalr 1. x de cette rubrique,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579176"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a>Scale-out de SignalR avec Azure Service Bus

par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Dans ce didacticiel, vous allez déployer une application Signalr sur un rôle Web Windows Azure, à l’aide de la Service Bus fond de panier pour distribuer des messages à chaque instance de rôle. (Vous pouvez également utiliser la Service Bus fond de panier avec les [applications Web dans Azure App service](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Conditions préalables :

- Un compte Windows Azure.
- Le [Kit de développement logiciel (SDK) Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 ou 2013.

Le fond de panier service bus est également compatible avec [service bus pour Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1,1. Toutefois, il n’est pas compatible avec la version 1,0 de Service Bus pour Windows Server.

## <a name="pricing"></a>Prix

Le fond de panier Service Bus utilise les rubriques pour envoyer des messages. Pour obtenir les dernières informations sur la tarification, consultez [service bus](https://azure.microsoft.com/pricing/details/service-bus/). Au moment de la rédaction de cet article, vous pouvez envoyer 1 million messages par mois pour moins de $1. Le fond de panier envoie un message service bus pour chaque appel d’une méthode de concentrateur Signalr. Il existe également des messages de contrôle pour les connexions, les déconnexions, la jonction ou la sortie de groupes, etc. Dans la plupart des applications, la majorité du trafic de messages est l’appel de méthode de concentrateur.

## <a name="overview"></a>Présentation

Avant d’accéder au didacticiel détaillé, voici un aperçu rapide de ce que vous allez faire.

1. Utilisez le Portail Azure Windows pour créer un espace de noms Service Bus.
2. Ajoutez ces packages NuGet à votre application : 

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. Aspnet. signalr. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) ou [Microsoft. Aspnet. Signalr. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Créer une application Signalr.
4. Ajoutez le code suivant à Startup.cs pour configurer le fond de panier : 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Ce code configure le fond de panier avec les valeurs par défaut pour [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) et [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Pour plus d’informations sur la modification de ces valeurs, consultez [performance de signalr : mesures ScaleOut](signalr-performance.md#scaleout_metrics).

Pour chaque application, choisissez une autre valeur pour « nomapp ». N’utilisez pas la même valeur pour plusieurs applications.

## <a name="create-the-azure-services"></a>Créer les services Azure

Créez un service Cloud, comme décrit dans [comment créer et déployer un service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Suivez les étapes de la section « Procédure : créer un service Cloud à l’aide de la création rapide ». Pour ce didacticiel, vous n’avez pas besoin de télécharger un certificat.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Créez un espace de noms Service Bus, comme décrit dans [How to Use service bus topics/subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Suivez les étapes de la section « créer un espace de noms de service ».

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Veillez à sélectionner la même région pour le service Cloud et l’espace de noms Service Bus.

## <a name="create-the-visual-studio-project"></a>Créer le projet Visual Studio

Démarrez Visual Studio. Dans le menu **Fichier**, cliquez sur **Nouveau projet**.

Dans la boîte de dialogue **nouveau projet** , développez **visuel C#** . Sous **modèles installés**, sélectionnez **Cloud** , puis sélectionnez **service Cloud Windows Azure**. Conservez la valeur par défaut .NET Framework 4,5. Nommez l’application ChatService, puis cliquez sur **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

Dans la boîte de dialogue **nouveau service Cloud Windows Azure** , sélectionnez rôle Web ASP.net. Cliquez sur le bouton de flèche droite ( **&gt;** ) pour ajouter le rôle à votre solution.

Placez le curseur de la souris sur le nouveau rôle pour afficher l’icône de crayon. Cliquez sur cette icône pour renommer le rôle. Nommez le rôle « SignalRChat », puis cliquez sur **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez **MVC**, puis cliquez sur OK.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

L’Assistant projet crée deux projets :

- ChatService : ce projet est l’application Windows Azure. Il définit les rôles Azure et d’autres options de configuration.
- SignalRChat : ce projet est votre projet ASP.NET MVC 5.

## <a name="create-the-signalr-chat-application"></a>Créer l’application Signalr chat

Pour créer l’application de conversation, suivez les étapes décrites dans le didacticiel [prise en main avec signalr et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Utilisez NuGet pour installer les bibliothèques requises. Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre **console du gestionnaire de package** , entrez les commandes suivantes :

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Utilisez l’option `-ProjectName` pour installer les packages dans le projet MVC ASP.NET, plutôt que dans le projet Windows Azure.

## <a name="configure-the-backplane"></a>Configurer le fond de panier

Dans le fichier Startup.cs de votre application, ajoutez le code suivant :

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Vous devez maintenant accéder à votre chaîne de connexion service bus. Dans le Portail Azure, sélectionnez l’espace de noms service bus que vous avez créé, puis cliquez sur l’icône de clé d’accès.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Copiez la chaîne de connexion dans le presse-papiers, puis collez-la dans la variable *ConnectionString* .

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Déployer sur Azure

Dans Explorateur de solutions, développez le dossier **rôles** à l’intérieur du projet ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Cliquez avec le bouton droit sur le rôle SignalRChat et sélectionnez **Propriétés**. Sélectionnez l’onglet **configuration** . Sous **instances** , sélectionnez 2. Vous pouvez également définir la taille de la machine virtuelle sur **très petite**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Enregistrez les modifications.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet ChatService. Sélectionnez **Publier**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

S’il s’agit de votre première publication sur Windows Azure, vous devez télécharger vos informations d’identification. Dans l’Assistant **publication** , cliquez sur « se connecter pour télécharger les informations d’identification ». Vous êtes alors invité à vous connecter au Portail Azure Windows et à télécharger un fichier de paramètres de publication.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Cliquez sur **Importer** , puis sélectionnez le fichier de paramètres de publication que vous avez téléchargé.

Cliquez sur **Next**. Dans la boîte de dialogue **paramètres de publication** , sous **service Cloud**, sélectionnez le service Cloud que vous avez créé précédemment.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Cliquez sur **Publier**. Le déploiement de l’application et le démarrage des machines virtuelles peuvent prendre quelques minutes.

Désormais, lorsque vous exécutez l’application de conversation, les instances de rôle communiquent via Azure Service Bus, à l’aide d’une rubrique Service Bus. Une rubrique est une file d’attente de messages qui autorise plusieurs abonnés.

Le fond de panier crée automatiquement la rubrique et les abonnements. Pour afficher les abonnements et l’activité des messages, ouvrez le Portail Azure, sélectionnez l’espace de noms Service Bus, puis cliquez sur « rubriques ».

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Quelques minutes sont nécessaires pour que l’activité des messages apparaisse dans le tableau de bord.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

Signalr gère la durée de vie de la rubrique. Tant que votre application est déployée, n’essayez pas de supprimer manuellement des rubriques ou de modifier les paramètres de la rubrique.

## <a name="troubleshooting"></a>Résolution des problèmes

**System. InvalidOperationException "la seule IsolationLevel prise en charge est’IsolationLevel. Serializable'."**

Cette erreur peut se produire si le niveau de transaction d’une opération est défini sur une valeur autre que `Serializable`. Vérifiez qu’aucune opération n’est effectuée avec d’autres niveaux de transaction.
