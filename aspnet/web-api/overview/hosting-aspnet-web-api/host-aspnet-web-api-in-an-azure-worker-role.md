---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hôte API Web ASP.NET 2 dans un rôle de travail Azure-ASP.NET 4. x
author: MikeWasson
description: 'Didacticiel : héberger API Web ASP.NET dans un rôle de travail Azure, à l’aide de OWIN pour auto-héberger l’infrastructure de l’API Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556629"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Hôte API Web ASP.NET 2 dans un rôle de travail Azure

par [Mike Wasson](https://github.com/MikeWasson)

> Ce didacticiel montre comment héberger des API Web ASP.NET dans un rôle de travail Azure, à l’aide de OWIN pour auto-héberger l’infrastructure de l’API Web.
>
> [Open Web interface for .net](http://owin.org/) (OWIN) définit une abstraction entre les serveurs Web .net et les applications Web. OWIN découple l’application Web du serveur, ce qui fait de OWIN idéal pour l’auto-hébergement d’une application Web dans votre propre processus, en dehors d’IIS, par exemple, à l’intérieur d’un rôle de travail Azure.
>
> Dans ce didacticiel, vous allez utiliser le package Microsoft. Owin. Host. HttpListener, qui fournit un serveur HTTP utilisé pour héberger des applications OWIN.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - API Web 2
> - [Kit de développement logiciel (SDK) Azure pour .NET 2,3](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a>Créer un projet Microsoft Azure

Démarrez Visual Studio avec des privilèges d’administrateur. Des privilèges d’administrateur sont nécessaires pour déboguer l’application localement, à l’aide de l’émulateur de calcul Azure.

Dans le menu **fichier** , cliquez sur **nouveau**, puis sur **projet**. Dans **modèles installés**, sous Visual C#, cliquez sur **Cloud** , puis sur **service Cloud Windows Azure**. Nommez le projet « AzureApp », puis cliquez sur **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

Dans la boîte de dialogue **nouveau service Cloud Windows Azure** , double-cliquez sur **rôle de travail**. Laissez le nom par défaut (« WorkerRole1 »). Cette étape ajoute un rôle de travail à la solution. Cliquez sur **OK**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

La solution Visual Studio créée contient deux projets :

- &quot;AzureApp&quot; définit les rôles et la configuration de l’application Azure.
- &quot;WorkerRole1&quot; contient le code du rôle de travail.

En général, une application Azure peut contenir plusieurs rôles, bien que ce didacticiel utilise un rôle unique.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Ajouter l’API Web et les packages OWIN

Dans le menu **Outils** , cliquez sur **Gestionnaire de package NuGet**, puis sur **console du gestionnaire de package**.

Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Ajouter un point de terminaison HTTP

Dans Explorateur de solutions, développez le projet AzureApp. Développez le nœud rôles, cliquez avec le bouton droit sur WorkerRole1, puis sélectionnez **Propriétés**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Cliquez sur **Points de terminaison**, puis sur **Ajouter un point de terminaison**.

Dans la liste déroulante **protocole** , sélectionnez « http ». Dans **port public** et **port privé**, tapez 80. Ces derniers peuvent être différents. Le port public est utilisé par les clients lorsqu’ils envoient une demande au rôle.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Configurer l’API Web pour l’auto-hébergement

Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet WorkerRole1 et sélectionnez **Ajouter** une / **classe** pour ajouter une nouvelle classe. Nommez la classe `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Remplacez tout le code réutilisable dans ce fichier par ce qui suit :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Ajouter un contrôleur d’API Web

Ensuite, ajoutez une classe de contrôleur d’API Web. Cliquez avec le bouton droit sur le projet WorkerRole1 et sélectionnez **ajouter** / **classe**. Nommez la classe TestController. Remplacez tout le code réutilisable dans ce fichier par ce qui suit :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Par souci de simplicité, ce contrôleur définit simplement deux méthodes d’extraction qui retournent du texte brut.

## <a name="start-the-owin-host"></a>Démarrer l’hôte OWIN

Ouvrez le fichier WorkerRole.cs. Cette classe définit le code qui s’exécute lorsque le rôle de travail est démarré et arrêté.

Ajoutez les instructions using suivantes :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Ajoutez un membre **IDisposable** à la classe `WorkerRole` :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

Dans la méthode `OnStart`, ajoutez le code suivant pour démarrer l’hôte :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

La méthode **WebApp. Start** démarre l’hôte OWIN. Le nom de la classe `Startup` est un paramètre de type de la méthode. Par Convention, l’hôte appellera la méthode `Configure` de cette classe.

Remplacez le `OnStop` pour supprimer l’instance d' *application\_* :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Voici le code complet pour WorkerRole.cs :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Générez la solution, puis appuyez sur F5 pour exécuter l’application localement dans l’émulateur de calcul Azure. Selon les paramètres de votre pare-feu, vous devrez peut-être autoriser l’émulateur via votre pare-feu.

> [!NOTE]
> Si vous obtenez une exception semblable à la suivante, veuillez consulter ce billet de [blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) pour obtenir une solution de contournement. «Impossible de charger le fichier ou l’assembly’Microsoft. Owin, version = 2.0.2.0, culture = neutral, PublicKeyToken = 31bf3856ad364e35 'ou l’une de ses dépendances. La définition du manifeste de l’assembly trouvé ne correspond pas à la référence de l’assembly. (Exception de HRESULT : 0x80131040)»

L’émulateur de calcul affecte une adresse IP locale au point de terminaison. Vous pouvez trouver l’adresse IP en affichant l’interface utilisateur de l’émulateur de calcul. Cliquez avec le bouton droit sur l’icône de l’émulateur dans la zone de notification de la barre des tâches, puis sélectionnez **afficher l’interface utilisateur de l’émulateur de calcul**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Recherchez l’adresse IP sous déploiements du service, déploiement [id], Détails du service. Ouvrez un navigateur Web et accédez à l'<em>adresse</em>http:///test/1, où <em>adresse</em> est l’adresse IP assignée par l’émulateur de calcul. par exemple, `http://127.0.0.1:80/test/1`. Vous devez voir la réponse du contrôleur d’API Web :

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Déployer sur Azure

Pour cette étape, vous devez disposer d’un compte Azure. Si vous n’en avez pas déjà un, vous pouvez créer un compte d’évaluation gratuit en quelques minutes. Pour plus d’informations, consultez [Microsoft Azure version d’évaluation gratuite](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet AzureApp. Sélectionnez **Publier**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Si vous n’êtes pas connecté à votre compte Azure, cliquez sur **se connecter**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Une fois que vous êtes connecté, choisissez un abonnement et cliquez sur **suivant**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Entrez un nom pour le service Cloud et choisissez une région. Cliquez sur **Créer**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Cliquez sur **Publier**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

La fenêtre Journal d’activité Azure affiche la progression du déploiement. Lorsque l’application est déployée, accédez à http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Ressources supplémentaires

- [Vue d’ensemble du projet Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Projet Katana sur GitHub](https://github.com/aspnet/AspNetKatana)
