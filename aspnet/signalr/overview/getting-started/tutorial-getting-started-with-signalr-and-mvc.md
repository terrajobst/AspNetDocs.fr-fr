---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Didacticiel : conversation en temps réel avec Signalr 2 et MVC 5 | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment utiliser ASP.NET Signalr 2 pour créer une application de conversation en temps réel. Vous ajoutez Signalr à une application MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537043"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Didacticiel : conversation en temps réel avec Signalr 2 et MVC 5

Ce didacticiel montre comment utiliser ASP.NET Signalr 2 pour créer une application de conversation en temps réel. Vous ajoutez Signalr à une application MVC 5 et créez une vue conversation pour envoyer et afficher des messages.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configuration du projet
> * Exécution de l'exemple
> * Examiner le code

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Conditions préalables requises

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail **Développement ASP.NET et web**.

## <a name="set-up-the-project"></a>Configurer le projet

Cette section montre comment utiliser Visual Studio 2017 et Signalr 2 pour créer une application ASP.NET MVC 5 vide, ajouter la bibliothèque Signalr et créer l’application de conversation.

1. Dans Visual Studio, créez une C# application ASP.net qui cible .NET Framework 4,5, nommez-la SignalRChat, puis cliquez sur OK.

    ![Créer un site Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. Dans **nouvelle application Web ASP.net-SignalRMvcChat**, sélectionnez **MVC** , puis sélectionnez **modifier l’authentification**.

1. Dans **modifier l’authentification**, sélectionnez **aucune authentification** , puis cliquez sur **OK**.

    ![Sélectionner aucune authentification](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. Dans **nouvelle application Web ASP.net-SignalRMvcChat**, sélectionnez **OK**.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.

1. Dans **Ajouter un nouvel élément-SignalRChat**, **Sélectionnez installé** > **Visual C#**  > **Web** > **signalr** , puis sélectionnez **classe de concentrateur signalr (v2)** .

1. Nommez la classe *ChatHub* et ajoutez-la au projet.

    Cette étape crée le fichier de classe *ChatHub.cs* et ajoute un ensemble de fichiers de script et de références d’assembly qui prennent en charge signalr au projet.

1. Remplacez le code dans le nouveau fichier de classe *ChatHub.cs* par le code suivant :

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **classe**.

1. Nommez le nouveau *démarrage* de la classe et ajoutez-le au projet.

1. Remplacez le code dans le fichier de classe *Startup.cs* par le code suivant :

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. Dans **Explorateur de solutions**, sélectionnez **contrôleurs** > **HomeController.cs**.

1. Ajoutez cette méthode à *HomeController.cs*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Cette méthode retourne l’affichage **conversation** que vous avez créé lors d’une étape ultérieure.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **vues** > **page**d’affichage, puis sélectionnez **Ajouter** une **vue**de >  .

1. Dans **Ajouter une vue**, nommez la nouvelle vue **conversation** , puis sélectionnez **Ajouter**.

1. Remplacez le contenu de **conversation. cshtml** par le code suivant :

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. Dans **Explorateur de solutions**, développez **scripts**.

    Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.

    > [!IMPORTANT]
    > Le gestionnaire de package peut avoir installé une version plus récente des scripts Signalr.

1. Vérifiez que les références de script dans le bloc de code correspondent aux versions des fichiers de script dans le projet.

    Références de script à partir du bloc de code d’origine :

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Si elles ne correspondent pas, mettez à jour le fichier *. cshtml* .

1. Dans la barre de menus, sélectionnez **fichier** > **enregistrer tout**.

## <a name="run-the-sample"></a>Exécuter l’exemple

1. Dans la barre d’outils, activez le **débogage de script** , puis sélectionnez le bouton de lecture pour exécuter l’exemple en mode débogage.

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Lorsque le navigateur s’ouvre, entrez un nom pour votre identité de conversation.

1. Copiez l’URL à partir du navigateur, ouvrez deux autres navigateurs, puis collez les URL dans les barres d’adresses.

1. Dans chaque navigateur, entrez un nom unique.

1. Maintenant, ajoutez un commentaire et sélectionnez **Envoyer**. Répétez cette opération dans les autres navigateurs. Les commentaires s’affichent en temps réel.

    > [!NOTE]
    > Cette simple application de conversation ne gère pas le contexte de discussion sur le serveur. Le concentrateur diffuse des commentaires à tous les utilisateurs actuels. Les utilisateurs qui rejoignent la conversation voient les messages ajoutés à partir du moment où ils se joignent.

    Découvrez comment l’application de conversation s’exécute dans trois navigateurs différents. Lorsque Tom, Anand et Suzanne envoient des messages, tous les navigateurs sont mis à jour en temps réel :

    ![Les trois navigateurs affichent le même historique de conversation](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. Dans **Explorateur de solutions**, examinez le nœud **documents de script** pour l’application en cours d’exécution. Un fichier de script nommé *hubs* est généré par la bibliothèque signalr au moment de l’exécution. Ce fichier gère la communication entre le script jQuery et le code côté serveur.

    ![script hubs généré automatiquement dans le nœud documents de script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Examiner le code

L’application de conversation vocale Signalr illustre deux tâches de développement Signalr de base. Il vous montre comment créer un Hub. Le serveur utilise ce concentrateur comme objet de coordination principal. Le Hub utilise la bibliothèque jQuery Signalr pour envoyer et recevoir des messages.

### <a name="signalr-hubs-in-the-chathubcs"></a>Concentrateurs signalr dans le ChatHub.cs

Dans l’exemple de code, la classe `ChatHub` dérive de la classe `Microsoft.AspNet.SignalR.Hub`. La dérivation de la classe `Hub` est un moyen utile de générer une application Signalr. Vous pouvez créer des méthodes publiques sur votre classe de concentrateur, puis accéder à ces méthodes en les appelant à partir de scripts dans une page Web.

Dans le code de conversation, les clients appellent la méthode `ChatHub.Send` pour envoyer un nouveau message. Le Hub envoie ensuite le message à tous les clients en appelant `Clients.All.addNewMessageToPage`.

La méthode `Send` illustre plusieurs concepts de concentrateur :

* Déclarez les méthodes publiques sur un concentrateur afin que les clients puissent les appeler.

* Utilisez la propriété dynamique `Microsoft.AspNet.SignalR.Hub.Clients` pour communiquer avec tous les clients connectés à ce concentrateur.

* Appelez une fonction sur le client (comme la fonction `addNewMessageToPage`) pour mettre à jour les clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>Signalr et jQuery chat. cshtml

Le fichier de vue de *conversation. cshtml* dans l’exemple de code montre comment utiliser la bibliothèque jQuery signalr pour communiquer avec un concentrateur signalr.  Le code effectue de nombreuses tâches importantes. Elle crée une référence au proxy généré automatiquement pour le concentrateur, déclare une fonction que le serveur peut appeler pour envoyer le contenu aux clients et démarre une connexion pour envoyer des messages au Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> En JavaScript, la référence à la classe de serveur et à ses membres se trouve dans la casse mixte. L’exemple de code fait C# référence à la classe `ChatHub` dans JavaScript comme `chatHub`.

Dans ce bloc de code, vous créez une fonction de rappel dans le script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

La classe de concentrateur sur le serveur appelle cette fonction pour transmettre les mises à jour de contenu à chaque client. L’appel facultatif à la fonction `htmlEncode` montre comment encoder le contenu du message au format HTML avant de l’afficher dans la page. C’est un moyen d’empêcher l’injection de scripts.

Ce code ouvre une connexion avec le concentrateur.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Cette approche garantit que vous établissez une connexion avant que le gestionnaire d’événements s’exécute.

Le code démarre la connexion, puis lui passe une fonction pour gérer l’événement de clic sur le bouton **Envoyer** dans la page de conversation.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations sur Signalr, consultez les ressources suivantes :

* [Projet signalr](http://signalr.net)

* [Signalr GitHub et exemples](https://github.com/SignalR/SignalR)

* [Wiki signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configuration du projet
> * Exécution de l’exemple
> * Examen du code

Passez à l’article suivant pour apprendre à créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de messagerie à fréquence élevée.
> [!div class="nextstepaction"]
> [Application Web avec messagerie à fréquence élevée](tutorial-high-frequency-realtime-with-signalr.md)