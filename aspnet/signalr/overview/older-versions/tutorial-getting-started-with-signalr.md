---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Didacticiel : Prise en main avec Signalr 1. x | Microsoft Docs'
author: bradygaster
description: Utilisez ASP.NET Signalr pour créer une application de conversation en temps réel dans une page HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623542"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a>Didacticiel : Prise en main avec Signalr 1. x

de [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel. Vous allez ajouter Signalr à une application Web ASP.NET vide et créer une page HTML pour envoyer et afficher des messages.

## <a name="overview"></a>Présentation

Ce didacticiel présente le développement Signalr en expliquant comment créer une application de conversation simple basée sur un navigateur. Vous allez ajouter la bibliothèque Signalr à une application Web ASP.NET vide, créer une classe de concentrateur pour envoyer des messages aux clients et créer une page HTML qui permet aux utilisateurs d’envoyer et de recevoir des messages de conversation. Pour obtenir un didacticiel similaire qui montre comment créer une application de conversation dans MVC 4 à l’aide d’une vue MVC, consultez [prise en main avec signalr et MVC 4](index.md).

> [!NOTE]
> Ce didacticiel utilise la version Release (1. x) de Signalr. Pour plus d’informations sur les modifications apportées entre Signalr 1. x et 2,0, consultez [mise à niveau des projets signalr 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).

Signalr est une bibliothèque .NET Open source permettant de créer des applications Web nécessitant des mises à jour en temps réel des données ou des interactions avec l’utilisateur. Exemples : applications sociales, jeux multi-utilisateurs, collaboration professionnelle, Actualités, météo ou mises à jour financières. Il s’agit souvent d’applications en temps réel.

Signalr simplifie le processus de création d’applications en temps réel. Il comprend une bibliothèque de serveur ASP.NET et une bibliothèque cliente JavaScript pour faciliter la gestion des connexions client-serveur et l’envoi de mises à jour de contenu aux clients. Vous pouvez ajouter la bibliothèque Signalr à une application ASP.NET existante pour obtenir des fonctionnalités en temps réel.

Ce didacticiel présente les tâches de développement Signalr suivantes :

- Ajout de la bibliothèque Signalr à une application Web ASP.NET.
- Création d’une classe de concentrateur pour transmettre le contenu aux clients.
- Utilisation de la bibliothèque jQuery Signalr dans une page Web pour envoyer des messages et afficher des mises à jour à partir du Hub.

La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur. Chaque nouvel utilisateur peut poster des commentaires et voir les commentaires ajoutés après que l’utilisateur s’est joint à la conversation.

![Instances de conversation](tutorial-getting-started-with-signalr/_static/image1.png)

Sections

- [Configurer le projet](#setup)
- [Exécuter l’exemple](#run)
- [Examiner le code](#code)
- [Étapes suivantes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurer le projet

Cette section montre comment créer une application Web ASP.NET vide, ajouter Signalr et créer l’application de conversation.

Conditions préalables :

- Visual Studio 2010 SP1 ou 2012. Si vous ne disposez pas de Visual Studio, consultez [ASP.net downloads](https://www.asp.net/downloads) pour obtenir l’outil gratuit visual studio 2012 Express Development.
- [Microsoft ASP.NET et Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941). Pour Visual Studio 2012, ce programme d’installation ajoute de nouvelles fonctionnalités ASP.NET, notamment des modèles Signalr à Visual Studio. Pour Visual Studio 2010 SP1, un programme d’installation n’est pas disponible, mais vous pouvez suivre le didacticiel en installant le package NuGet Signalr, comme décrit dans la procédure d’installation.

Les étapes suivantes utilisent Visual Studio 2012 pour créer une application Web vide ASP.NET et ajouter la bibliothèque Signalr :

1. Dans Visual Studio, créez une application Web vide ASP.NET.

    ![Créer un site Web vide](tutorial-getting-started-with-signalr/_static/image2.png)
2. Ouvrez la **console du gestionnaire de package** en sélectionnant **Outils | Gestionnaire de package NuGet | Console du gestionnaire de package**. Entrez la commande suivante dans la fenêtre de console :

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Cette commande installe la dernière version de Signalr 1. x.
3. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter | Classe**. Nommez la nouvelle classe **ChatHub**.
4. Dans **Explorateur de solutions** développez le nœud scripts. Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.

    ![Références de bibliothèque](tutorial-getting-started-with-signalr/_static/image3.png)
5. Remplacez le code de la classe **ChatHub** par le code suivant.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis cliquez sur **Ajouter | Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **classe d’application globale** , puis cliquez sur **Ajouter**.

    ![Ajouter global](tutorial-getting-started-with-signalr/_static/image4.png)
7. Ajoutez les instructions `using` suivantes après les instructions `using` fournies dans la classe Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Ajoutez la ligne de code suivante à la méthode `Application_Start` de la classe globale pour enregistrer l’itinéraire par défaut pour les concentrateurs Signalr.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis cliquez sur **Ajouter | Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez page HTML, puis cliquez sur **Ajouter**.
10. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page HTML que vous venez de créer, puis cliquez sur **définir comme page de démarrage**.
11. Remplacez le code par défaut dans la page HTML par le code suivant.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Enregistrer tout** pour le projet.

<a id="run"></a>

## <a name="run-the-sample"></a>Exécuter l’exemple

1. Appuyez sur F5 pour exécuter le projet en mode débogage. La page HTML est chargée dans une instance de navigateur et demande un nom d’utilisateur.

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr/_static/image5.png)
2. Entrez un nom d’utilisateur.
3. Copiez l’URL à partir de la ligne d’adresse du navigateur et utilisez-la pour ouvrir deux autres instances de navigateur. Dans chaque instance de navigateur, entrez un nom d’utilisateur unique.
4. Dans chaque instance de navigateur, ajoutez un commentaire et cliquez sur **Envoyer**. Les commentaires doivent s’afficher dans toutes les instances de navigateur.

    > [!NOTE]
    > Cette simple application de conversation ne gère pas le contexte de discussion sur le serveur. Le concentrateur diffuse des commentaires à tous les utilisateurs actuels. Les utilisateurs qui rejoignent la conversation voient les messages ajoutés à partir du moment où ils se joignent.

    La capture d’écran suivante montre l’application de conversation en cours d’exécution dans trois instances de navigateur, toutes mises à jour lorsqu’une instance envoie un message :

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr/_static/image6.png)
5. Dans **Explorateur de solutions**, examinez le nœud **documents de script** pour l’application en cours d’exécution. Un fichier de script nommé **hubs** est généré dynamiquement au moment de l’exécution. Ce fichier gère la communication entre le script jQuery et le code côté serveur.

    ![Script de concentrateur généré](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examiner le code

L’application de conversation vocale Signalr montre deux tâches de développement Signalr de base : la création d’un hub en tant qu’objet de coordination principal sur le serveur et l’utilisation de la bibliothèque jQuery de Signalr pour envoyer et recevoir des messages.

### <a name="signalr-hubs"></a>Hubs SignalR

Dans l’exemple de code, la classe **ChatHub** dérive de la classe **Microsoft. Aspnet. signalr. Hub** . La dérivation à partir de la classe de **concentrateur** est un moyen utile de générer une application signalr. Vous pouvez créer des méthodes publiques sur votre classe de concentrateur, puis accéder à ces méthodes en les appelant à partir de scripts jQuery dans une page Web.

Dans le code de conversation, les clients appellent la méthode **ChatHub. Send** pour envoyer un nouveau message. Le Hub envoie ensuite le message à tous les clients en appelant **clients. All. broadcastMessage**.

La méthode **Send** illustre plusieurs concepts de concentrateur :

- Déclarez les méthodes publiques sur un concentrateur afin que les clients puissent les appeler.
- Utilisez la propriété dynamique **Microsoft. Aspnet. signalr. Hub. clients** pour accéder à tous les clients connectés à ce concentrateur.
- Appelez une fonction jQuery sur le client (par exemple, la fonction `broadcastMessage`) pour mettre à jour les clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Signalr et jQuery

La page HTML de l’exemple de code montre comment utiliser la bibliothèque jQuery Signalr pour communiquer avec un concentrateur Signalr. Les tâches essentielles du code déclarent un proxy pour faire référence au Hub, en déclarant une fonction que le serveur peut appeler pour envoyer du contenu aux clients et en commençant une connexion pour envoyer des messages au Hub.

Le code suivant déclare un proxy pour un concentrateur.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> Dans jQuery, la référence à la classe de serveur et à ses membres est en casse mixte. L’exemple de code fait C# référence à la classe **ChatHub** dans jQuery en tant que **ChatHub**.

Le code suivant montre comment créer une fonction de rappel dans le script. La classe de concentrateur sur le serveur appelle cette fonction pour transmettre les mises à jour de contenu à chaque client. Les deux lignes qui encodent le contenu au format HTML avant de l’afficher sont facultatives et montrent un moyen simple d’empêcher l’injection de script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Le code suivant montre comment ouvrir une connexion avec le Hub. Le code démarre la connexion, puis lui passe une fonction pour gérer l’événement de clic sur le bouton **Envoyer** dans la page html.

> [!NOTE]
> Cette approche garantit que la connexion est établie avant l’exécution du gestionnaire d’événements.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Étapes suivantes

Vous avez appris que Signalr est une infrastructure permettant de créer des applications Web en temps réel. Vous avez également appris plusieurs tâches de développement Signalr : comment ajouter Signalr à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du Hub.

Vous pouvez rendre l’exemple d’application dans ce didacticiel ou d’autres applications signaler disponibles sur Internet en les déployant sur un fournisseur d’hébergement. Microsoft offre un hébergement Web gratuit pour un maximum de 10 sites Web dans un [compte d’essai gratuit de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Pour obtenir une procédure pas à pas sur le déploiement de l’exemple d’application Signalr, consultez [publier l’exemple signalr prise en main en tant que site Web Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Pour plus d’informations sur le déploiement d’un projet Web Visual Studio sur un site Web Windows Azure, consultez [déploiement d’une Application ASP.net sur un site Web Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Remarque : le transport WebSocket n’est actuellement pas pris en charge pour les sites Web Windows Azure. Lorsque le transport WebSocket n’est pas disponible, Signalr utilise les autres transports disponibles, comme décrit dans la section Transports de la [rubrique Présentation de signalr](index.md).)

Pour en savoir plus sur les concepts d’évolution de Signalr plus avancés, visitez les sites suivants pour le code source et les ressources Signalr :

- [Projet signalr](http://signalr.net)
- [Signalr GitHub et exemples](https://github.com/SignalR/SignalR)
- [Wiki signalr](https://github.com/SignalR/SignalR/wiki)
