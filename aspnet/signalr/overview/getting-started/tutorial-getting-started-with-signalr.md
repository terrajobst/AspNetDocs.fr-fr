---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Didacticiel : conversation en temps réel avec Signalr 2 | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel. Vous ajoutez Signalr à une application Web ASP.NET vide.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600468"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Didacticiel : conversation en temps réel avec Signalr 2

Ce didacticiel vous montre comment utiliser Signalr pour créer une application de conversation en temps réel. Vous ajoutez Signalr à une application Web ASP.NET vide et créez une page HTML pour envoyer et afficher des messages.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configurer le projet
> * Exécuter l'exemple
> * Examiner le code

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Configuration requise

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail de **développement Web et ASP.net** .

## <a name="set-up-the-project"></a>Configurer le projet

Cette section montre comment utiliser Visual Studio 2017 et Signalr 2 pour créer une application Web ASP.NET vide, ajouter Signalr et créer l’application de conversation.

1. Dans Visual Studio, créez une application Web ASP.NET.

    ![Créer un site Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. Dans la fenêtre **nouveau projet ASP.net-SignalRChat** , laissez **vide** sélectionné et sélectionnez **OK**.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.

1. Dans **Ajouter un nouvel élément-SignalRChat**, **Sélectionnez installé** > **Visual C#**  > **Web** > **signalr** , puis sélectionnez **classe de concentrateur signalr (v2)** .

1. Nommez la classe *ChatHub* et ajoutez-la au projet.

    Cette étape crée le fichier de classe *ChatHub.cs* et ajoute un ensemble de fichiers de script et de références d’assembly qui prennent en charge signalr au projet.

1. Remplacez le code dans le nouveau fichier de classe *ChatHub.cs* par le code suivant :

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.

1. Dans **Ajouter un nouvel élément-SignalRChat** , sélectionnez **installé** > **Visual C#**  > **Web** , puis sélectionnez **classe de démarrage OWIN**.

1. Nommez le *démarrage* de la classe et ajoutez-le au projet.

1. Remplacez le code par défaut de la classe *Startup* par le code suivant :

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **page HTML**.

1. Nommez le nouvel *index* de page et sélectionnez **OK**.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page HTML que vous avez créée, puis sélectionnez **définir comme page de démarrage**.

1. Remplacez le code par défaut de la page HTML par ce code :

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. Dans **Explorateur de solutions**, développez **scripts**.

    Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.

    > [!IMPORTANT]
    > Le gestionnaire de package peut avoir installé une version plus récente des scripts Signalr.

1. Vérifiez que les références de script dans le bloc de code correspondent aux versions des fichiers de script dans le projet.

    Références de script à partir du bloc de code d’origine :

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Si elles ne correspondent pas, mettez à jour le fichier *. html* .

1. Dans la barre de menus, sélectionnez **fichier** > **enregistrer tout**.

## <a name="run-the-sample"></a>Exécuter l’exemple

1. Dans la barre d’outils, activez le **débogage de script** , puis sélectionnez le bouton de lecture pour exécuter l’exemple en mode débogage.

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr/_static/image3.png)

1. Lorsque le navigateur s’ouvre, entrez un nom pour votre identité de conversation.

1. Copiez l’URL à partir du navigateur, ouvrez deux autres navigateurs, puis collez les URL dans les barres d’adresses.

1. Dans chaque navigateur, entrez un nom unique.

1. Maintenant, ajoutez un commentaire et sélectionnez **Envoyer**. Répétez cette opération dans les autres navigateurs. Les commentaires s’affichent en temps réel.

    > [!NOTE]
    > Cette simple application de conversation ne gère pas le contexte de discussion sur le serveur. Le concentrateur diffuse des commentaires à tous les utilisateurs actuels. Les utilisateurs qui rejoignent la conversation voient les messages ajoutés à partir du moment où ils se joignent.

    Découvrez comment l’application de conversation s’exécute dans trois navigateurs différents. Lorsque Tom, Anand et Suzanne envoient des messages, tous les navigateurs sont mis à jour en temps réel :

    ![Les trois navigateurs affichent le même historique de conversation](tutorial-getting-started-with-signalr/_static/image4.png)

1. Dans **Explorateur de solutions**, examinez le nœud **documents de script** pour l’application en cours d’exécution. Un fichier de script nommé *hubs* est généré par la bibliothèque signalr au moment de l’exécution. Ce fichier gère la communication entre le script jQuery et le code côté serveur.

    ![script hubs généré automatiquement dans le nœud documents de script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Examiner le code

L’application SignalRChat illustre deux tâches de développement Signalr de base. Il vous montre comment créer un Hub. Le serveur utilise ce concentrateur comme objet de coordination principal. Le Hub utilise la bibliothèque jQuery Signalr pour envoyer et recevoir des messages.

### <a name="signalr-hubs-in-the-chathubcs"></a>Concentrateurs signalr dans le ChatHub.cs

Dans l’exemple de code ci-dessus, la classe `ChatHub` dérive de la classe `Microsoft.AspNet.SignalR.Hub`. La dérivation de la classe `Hub` est un moyen utile de générer une application Signalr. Vous pouvez créer des méthodes publiques sur votre classe de concentrateur, puis utiliser ces méthodes en les appelant à partir de scripts dans une page Web.

Dans le code de conversation, les clients appellent la méthode `ChatHub.Send` pour envoyer un nouveau message. Le Hub envoie ensuite le message à tous les clients en appelant `Clients.All.broadcastMessage`.

La méthode `Send` illustre plusieurs concepts de concentrateur :

* Déclarez les méthodes publiques sur un concentrateur afin que les clients puissent les appeler.

* Utilisez la propriété dynamique `Microsoft.AspNet.SignalR.Hub.Clients` pour communiquer avec tous les clients connectés à ce concentrateur.

* Appelez une fonction sur le client (comme la fonction `broadcastMessage`) pour mettre à jour les clients.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>Signalr et jQuery dans index. html

La page *index. html* de l’exemple de code montre comment utiliser la bibliothèque jQuery de signalr pour communiquer avec un concentrateur signalr. Le code effectue de nombreuses tâches importantes. Elle déclare un proxy pour référencer le concentrateur, déclare une fonction que le serveur peut appeler pour envoyer le contenu aux clients et démarre une connexion pour envoyer des messages au Hub.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> En JavaScript, la référence à la classe de serveur et à ses membres doit être la casse mixte. L’exemple de code fait C# référence à la classe *ChatHub* dans JavaScript en tant que `chatHub`.

Dans ce bloc de code, vous créez une fonction de rappel dans le script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

La classe de concentrateur sur le serveur appelle cette fonction pour transmettre les mises à jour de contenu à chaque client. Les deux lignes qui encodent le contenu au format HTML avant de l’afficher sont facultatives et présentent un bon moyen d’empêcher l’injection de script.

Ce code ouvre une connexion avec le concentrateur.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Cette approche garantit que le code établit une connexion avant l’exécution du gestionnaire d’événements.

Le code démarre la connexion, puis lui passe une fonction pour gérer l’événement de clic sur le bouton **Envoyer** dans la page html.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations sur Signalr, consultez les ressources suivantes :

* [Projet signalr](http://signalr.net)

* [Signalr GitHub et exemples](https://github.com/SignalR/SignalR)

* [Wiki signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Étapes suivantes :

Dans ce didacticiel, vous allez :

> [!div class="checklist"]
> * Configurer le projet
> * Exécution de l’exemple
> * Examen du code

Passez à l’article suivant pour apprendre à utiliser Signalr et MVC 5.
> [!div class="nextstepaction"]
> [Signalr 2 et MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)