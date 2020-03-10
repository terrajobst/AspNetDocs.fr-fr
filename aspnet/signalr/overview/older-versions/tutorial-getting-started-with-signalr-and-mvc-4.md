---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Didacticiel : Prise en main avec Signalr 1. x et MVC 4 | Microsoft Docs'
author: bradygaster
description: Utilisez ASP.NET Signalr et ASP.NET MVC 4 pour créer une application de conversation en temps réel.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579568"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Didacticiel : Prise en main avec Signalr 1. x et MVC 4

de [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce didacticiel montre comment utiliser ASP.NET Signalr pour créer une application de conversation en temps réel. Vous allez ajouter Signalr à une application MVC 4 et créer une vue conversation pour envoyer et afficher des messages.

## <a name="overview"></a>Présentation

Ce didacticiel vous présente le développement d’applications Web en temps réel avec ASP.NET Signalr et ASP.NET MVC 4. Ce didacticiel utilise le même code d’application de conversation que le [didacticiel prise en main signalr](tutorial-getting-started-with-signalr.md), mais il montre comment l’ajouter à une application MVC 4 basée sur le modèle Internet.

Dans cette rubrique, vous allez apprendre les tâches de développement Signalr suivantes :

- Ajout de la bibliothèque Signalr à une application MVC 4.
- Création d’une classe de concentrateur pour transmettre le contenu aux clients.
- Utilisation de la bibliothèque jQuery Signalr dans une page Web pour envoyer des messages et afficher des mises à jour à partir du Hub.

La capture d’écran suivante montre l’application chat terminée s’exécutant dans un navigateur.

![Instances de conversation](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Sections

- [Configurer le projet](#setup)
- [Exécuter l’exemple](#run)
- [Examiner le code](#code)
- [Étapes suivantes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurer le projet

Conditions préalables :

- Visual Studio 2010 SP1, Visual Studio 2012 ou Visual Studio 2012 Express. Si vous ne disposez pas de Visual Studio, consultez [ASP.net downloads](https://www.asp.net/downloads) pour obtenir l’outil gratuit visual studio 2012 Express Development.
- Pour Visual Studio 2010, installez [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

Cette section montre comment créer une application ASP.NET MVC 4, ajouter la bibliothèque Signalr et créer l’application de conversation.

1. 1. Dans Visual Studio, créez une application ASP.NET MVC 4, nommez-la SignalRChat, puis cliquez sur OK.

        > [!NOTE]
        > Dans VS 2010, sélectionnez **.NET Framework 4** dans le contrôle de liste déroulante version du Framework. Le code signalr s’exécute sur .NET Framework versions 4 et 4,5.

        ![Créer un site Web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Sélectionnez le modèle application Internet, désactivez l’option de **création d’un projet de test unitaire**, puis cliquez sur OK.

         ![Créer un site Internet MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Ouvrez **outils > gestionnaire de package NuGet > console du gestionnaire de package** et exécutez la commande suivante. Cette étape ajoute au projet un ensemble de fichiers de script et de références d’assembly qui activent la fonctionnalité Signalr.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. Dans **Explorateur de solutions** développez le dossier scripts. Notez que les bibliothèques de scripts pour Signalr ont été ajoutées au projet.

         ![Références de bibliothèque](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter | Nouveau dossier**, puis ajoutez un nouveau dossier nommé **hubs**.
      6. Cliquez avec le bouton droit sur le dossier **hubs** , puis cliquez sur **Ajouter |** Et créez une nouvelle C# classe nommée **ChatHub.cs**. Vous allez utiliser cette classe comme concentrateur de serveur Signalr qui envoie des messages à tous les clients.

> [!NOTE]
> Si vous utilisez Visual Studio 2012 et que vous avez installé la [mise à jour ASP.NET et Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), vous pouvez utiliser le nouveau modèle d’élément signalr pour créer la classe de concentrateur. Pour ce faire, cliquez avec le bouton droit sur le dossier **hubs** , puis cliquez sur **Ajouter | Nouvel élément**, sélectionnez **classe de concentrateur signalr (v1)** et nommez la classe **ChatHub.cs**.

1. Remplacez le code de la classe **ChatHub** par le code suivant.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Ouvrez le fichier **global. asax** pour le projet, puis ajoutez un appel à la méthode `RouteTable.Routes.MapHubs();` en tant que première ligne de code dans la méthode `Application_Start`. Ce code inscrit l’itinéraire par défaut pour les concentrateurs Signalr et doit être appelé avant d’inscrire d’autres itinéraires. La méthode `Application_Start` terminée ressemble à l’exemple suivant.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Modifiez la classe `HomeController` trouvée dans **Controllers/HomeController. cs** et ajoutez la méthode suivante à la classe. Cette méthode retourne l’affichage **conversation** que vous allez créer dans une étape ultérieure.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Cliquez avec le bouton droit dans la méthode de `Chat` que vous venez de créer, puis cliquez sur **Ajouter une vue** pour créer un nouveau fichier de vue.
5. Dans la boîte de dialogue **Ajouter une vue** , assurez-vous que la case à cocher est activée pour **utiliser une disposition ou une page maître** (désactivez les autres cases à cocher), puis cliquez sur **Ajouter**.

    ![Ajouter une vue](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Modifiez le nouveau fichier de vue nommé **chat. cshtml**. Après la balise &lt;H2&gt;, collez la section de&gt; &lt;div suivante et `@section scripts` bloc de code dans la page. Ce script permet à la page d’envoyer des messages de conversation et d’afficher des messages à partir du serveur. Le code complet de l’affichage conversation s’affiche dans le bloc de code suivant.

    > [!IMPORTANT]
    > Quand vous ajoutez Signalr et d’autres bibliothèques de scripts à votre projet Visual Studio, le gestionnaire de package peut installer des versions des scripts qui sont plus récents que les versions présentées dans cette rubrique. Assurez-vous que les références de script dans votre code correspondent aux versions des bibliothèques de scripts installées dans votre projet.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Enregistrer tout** pour le projet.

<a id="run"></a>

## <a name="run-the-sample"></a>Exécuter l’exemple

1. Appuyez sur F5 pour exécuter le projet en mode débogage.
2. Dans la ligne d’adresse du navigateur, ajoutez **/Home/chat** à l’URL de la page par défaut du projet. La page de conversation est chargée dans une instance de navigateur et vous invite à entrer un nom d’utilisateur.

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Entrez un nom d’utilisateur.
4. Copiez l’URL à partir de la ligne d’adresse du navigateur et utilisez-la pour ouvrir deux autres instances de navigateur. Dans chaque instance de navigateur, entrez un nom d’utilisateur unique.
5. Dans chaque instance de navigateur, ajoutez un commentaire et cliquez sur **Envoyer**. Les commentaires doivent s’afficher dans toutes les instances de navigateur.

    > [!NOTE]
    > Cette simple application de conversation ne gère pas le contexte de discussion sur le serveur. Le concentrateur diffuse des commentaires à tous les utilisateurs actuels. Les utilisateurs qui rejoignent la conversation voient les messages ajoutés à partir du moment où ils se joignent.
6. La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur.

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. Dans **Explorateur de solutions**, examinez le nœud **documents de script** pour l’application en cours d’exécution. Ce nœud est visible en mode débogage si vous utilisez Internet Explorer comme navigateur. Un fichier de script nommé **hubs** est généré dynamiquement au moment de l’exécution. Ce fichier gère la communication entre le script jQuery et le code côté serveur. Si vous utilisez un navigateur autre qu’Internet Explorer, vous pouvez également accéder au fichier **hubs** dynamiques en y accédant directement, par exemple http://mywebsite/signalr/hubs.

    ![Script de concentrateur généré](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examiner le code

L’application de conversation vocale Signalr montre deux tâches de développement Signalr de base : la création d’un hub en tant qu’objet de coordination principal sur le serveur et l’utilisation de la bibliothèque jQuery de Signalr pour envoyer et recevoir des messages.

### <a name="signalr-hubs"></a>Hubs SignalR

Dans l’exemple de code, la classe **ChatHub** dérive de la classe **Microsoft. Aspnet. signalr. Hub** . La dérivation à partir de la classe de **concentrateur** est un moyen utile de générer une application signalr. Vous pouvez créer des méthodes publiques sur votre classe de concentrateur, puis accéder à ces méthodes en les appelant à partir de scripts jQuery dans une page Web.

Dans le code de conversation, les clients appellent la méthode **ChatHub. Send** pour envoyer un nouveau message. Le Hub envoie ensuite le message à tous les clients en appelant **clients. All. addNewMessageToPage**.

La méthode **Send** illustre plusieurs concepts de concentrateur :

- Déclarez les méthodes publiques sur un concentrateur afin que les clients puissent les appeler.
- Utilisez la propriété **Microsoft. Aspnet. signalr. Hub. clients** pour accéder à tous les clients connectés à ce concentrateur.
- Appelez une fonction jQuery sur le client (par exemple, la fonction `addNewMessageToPage`) pour mettre à jour les clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Signalr et jQuery

Le fichier de vue de **conversation. cshtml** dans l’exemple de code montre comment utiliser la bibliothèque jQuery signalr pour communiquer avec un concentrateur signalr. Les tâches essentielles du code sont la création d’une référence au proxy généré automatiquement pour le concentrateur, la déclaration d’une fonction que le serveur peut appeler pour transmettre du contenu aux clients et le démarrage d’une connexion pour envoyer des messages au Hub.

Le code suivant déclare un proxy pour un concentrateur.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> Dans jQuery, la référence à la classe de serveur et à ses membres est en casse mixte. L’exemple de code fait C# référence à la classe **ChatHub** dans jQuery en tant que **ChatHub**. Si vous souhaitez référencer la classe `ChatHub` dans jQuery avec la casse conventionnelle conventionnel comme vous C#le feriez dans, modifiez le fichier de classe ChatHub.cs. Ajoutez une instruction `using` pour référencer l’espace de noms `Microsoft.AspNet.SignalR.Hubs`. Ajoutez ensuite l’attribut `HubName` à la classe `ChatHub`, par exemple `[HubName("ChatHub")]`. Enfin, mettez à jour votre référence jQuery à la classe `ChatHub`.

Le code suivant montre comment créer une fonction de rappel dans le script. La classe de concentrateur sur le serveur appelle cette fonction pour transmettre les mises à jour de contenu à chaque client. L’appel facultatif à la fonction `htmlEncode` montre comment encoder le contenu du message au format HTML avant de l’afficher dans la page pour empêcher l’injection de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Le code suivant montre comment ouvrir une connexion avec le Hub. Le code démarre la connexion, puis lui passe une fonction pour gérer l’événement de clic sur le bouton **Envoyer** dans la page de conversation.

> [!NOTE]
> Cette approche garantit que la connexion est établie avant l’exécution du gestionnaire d’événements.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Étapes suivantes

Vous avez appris que Signalr est une infrastructure permettant de créer des applications Web en temps réel. Vous avez également appris plusieurs tâches de développement Signalr : comment ajouter Signalr à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du Hub.

Pour en savoir plus sur les concepts d’évolution de Signalr plus avancés, visitez les sites suivants pour le code source et les ressources Signalr :

- [Projet signalr](http://signalr.net)
- [Signalr GitHub et exemples](https://github.com/SignalR/SignalR)
- [Wiki signalr](https://github.com/SignalR/SignalR/wiki)
