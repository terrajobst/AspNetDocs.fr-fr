---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Héberger OWIN dans un rôle de travail Azure | Microsoft Docs
author: MikeWasson
description: Ce didacticiel montre comment auto-héberger des OWIN dans un rôle de travail Microsoft Azure. Open Web interface for .NET (OWIN) définit une abstraction entre le serveur Web .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 59d2e0d549427093f8a2424b17af81169b78ef30
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584615"
---
# <a name="host-owin-in-an-azure-worker-role"></a>Héberger OWIN dans un rôle worker Azure

par [Mike Wasson](https://github.com/MikeWasson)

> Ce didacticiel montre comment auto-héberger des OWIN dans un rôle de travail Microsoft Azure.
>
> [Open Web interface for .net](http://owin.org/) (OWIN) définit une abstraction entre les serveurs Web .net et les applications Web. OWIN découple l’application Web du serveur, ce qui fait de OWIN idéal pour l’auto-hébergement d’une application Web dans votre propre processus, en dehors d’IIS, par exemple, à l’intérieur d’un rôle de travail Azure.
>
> Dans ce didacticiel, vous allez apprendre à auto-héberger des applications OWIN à l’intérieur d’un rôle de travail Microsoft Azure. Pour en savoir plus sur les rôles de travail, consultez [modèles d’exécution Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [Kit de développement logiciel (SDK) Azure pour .NET 2,3](https://azure.microsoft.com/downloads/)
> - [2.1.0 Microsoft. Owin. selfHost](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)

## <a name="create-a-microsoft-azure-project"></a>Créer un projet Microsoft Azure

Démarrez Visual Studio avec des privilèges d’administrateur. Des privilèges d’administrateur sont nécessaires pour déboguer l’application localement, à l’aide de l’émulateur de calcul Azure.

Dans le menu **fichier** , cliquez sur **nouveau**, puis sur **projet**. Dans **modèles installés**, sous Visual C#, cliquez sur **Cloud** , puis sur **service Cloud Windows Azure**. Nommez le projet « AzureApp », puis cliquez sur **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

Dans la boîte de dialogue **nouveau service Cloud Windows Azure** , double-cliquez sur **rôle de travail**. Laissez le nom par défaut (« WorkerRole1 »). Cette étape ajoute un rôle de travail à la solution. Cliquez sur **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

La solution Visual Studio créée contient deux projets :

- &quot;AzureApp&quot; définit les rôles et la configuration de l’application Azure.
- &quot;WorkerRole1&quot; contient le code du rôle de travail.

En général, une application Azure peut contenir plusieurs rôles, bien que ce didacticiel utilise un rôle unique.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Ajouter les packages d’auto-hébergement OWIN

Dans le menu **Outils** , cliquez sur **Gestionnaire de package NuGet**, puis sur **console du gestionnaire de package**.

Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Ajouter un point de terminaison HTTP

Dans Explorateur de solutions, développez le projet AzureApp. Développez le nœud rôles, cliquez avec le bouton droit sur WorkerRole1, puis sélectionnez **Propriétés**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Cliquez sur **Points de terminaison**, puis sur **Ajouter un point de terminaison**.

Dans la liste déroulante **protocole** , sélectionnez « http ». Dans **port public** et **port privé**, tapez 80. Ces derniers peuvent être différents. Le port public est utilisé par les clients lorsqu’ils envoient une demande au rôle.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Créer la classe de démarrage OWIN

Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet WorkerRole1 et sélectionnez **Ajouter** une / **classe** pour ajouter une nouvelle classe. Nommez la classe `Startup`.

Remplacez tout le code réutilisable par ce qui suit :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

La méthode d’extension `UseWelcomePage` ajoute une page HTML simple à votre application pour vérifier que le site fonctionne.

## <a name="start-the-owin-host"></a>Démarrer l’hôte OWIN

Ouvrez le fichier WorkerRole.cs. Cette classe définit le code qui s’exécute lorsque le rôle de travail est démarré et arrêté.

Ajoutez les instructions using suivantes :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Ajoutez un membre **IDisposable** à la classe `WorkerRole` :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

Dans la méthode `OnStart`, ajoutez le code suivant pour démarrer l’hôte :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

La méthode **WebApp. Start** démarre l’hôte OWIN. Le nom de la classe `Startup` est un paramètre de type de la méthode. Par Convention, l’hôte appellera la méthode `Configure` de cette classe.

Remplacez le `OnStop` pour supprimer l’instance d' *application\_* :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Voici le code complet pour WorkerRole.cs :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Générez la solution, puis appuyez sur F5 pour exécuter l’application localement dans l’émulateur de calcul Azure. Selon les paramètres de votre pare-feu, vous devrez peut-être autoriser l’émulateur via votre pare-feu.

L’émulateur de calcul affecte une adresse IP locale au point de terminaison. Vous pouvez trouver l’adresse IP en affichant l’interface utilisateur de l’émulateur de calcul. Cliquez avec le bouton droit sur l’icône de l’émulateur dans la zone de notification de la barre des tâches, puis sélectionnez **afficher l’interface utilisateur de l’émulateur de calcul**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Recherchez l’adresse IP sous déploiements du service, déploiement [id], Détails du service. Ouvrez un navigateur Web et accédez à http :\/\/*adresse*, où *adresse* est l’adresse IP affectée par l’émulateur de calcul ; par exemple, `http://127.0.0.1:80`. Vous devez voir la page d’accueil OWIN :

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Déployer sur Azure

Pour cette étape, vous devez disposer d’un compte Azure. Si vous n’en avez pas déjà un, vous pouvez créer un compte d’évaluation gratuit en quelques minutes. Pour plus d’informations, consultez [Microsoft Azure version d’évaluation gratuite](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet AzureApp. Sélectionnez **Publier**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Si vous n’êtes pas connecté à votre compte Azure, cliquez sur **se connecter**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Une fois que vous êtes connecté, choisissez un abonnement et cliquez sur **suivant**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Entrez un nom pour le service Cloud et choisissez une région. Cliquez sur **Créer**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Cliquez sur **Publier**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

La fenêtre Journal d’activité Azure affiche la progression du déploiement. Lorsque l’application est déployée, accédez à `http://appname.cloudapp.net/`, où *appname* est le nom de votre service Cloud.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Vue d’ensemble du projet Katana](an-overview-of-project-katana.md)
- [Projet Katana sur GitHub](https://github.com/aspnet/AspNetKatana/)
