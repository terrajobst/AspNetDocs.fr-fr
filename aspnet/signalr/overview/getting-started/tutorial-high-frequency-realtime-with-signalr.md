---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Didacticiel : créer une application en temps réel haute fréquence avec Signalr 2 | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr pour fournir une fonctionnalité de messagerie à fréquence élevée.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600455"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Didacticiel : créer une application en temps réel haute fréquence avec Signalr 2

Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de messagerie à fréquence élevée. Dans ce cas, « messagerie à fréquence élevée » signifie que le serveur envoie des mises à jour à une fréquence fixe. Vous envoyez jusqu’à 10 messages par seconde.

L’application que vous créez affiche une forme que les utilisateurs peuvent faire glisser. Le serveur met à jour la position de la forme dans tous les navigateurs connectés pour qu’elle corresponde à la position de la forme glissée à l’aide des mises à jour chronométrées.

Les concepts présentés dans ce didacticiel ont des applications en temps réel et d’autres applications de simulation.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configurer le projet
> * Créer l’application de base
> * Mapper au concentrateur au démarrage de l’application
> * Ajouter le client
> * Exécuter l'application
> * Ajouter la boucle cliente
> * Ajouter la boucle serveur
> * Ajouter une animation lisse

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Configuration requise

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail de **développement Web et ASP.net** .

## <a name="set-up-the-project"></a>Configurer le projet

Dans cette section, vous allez créer le projet dans Visual Studio 2017.

Cette section montre comment utiliser Visual Studio 2017 pour créer une application Web ASP.NET vide et ajouter les bibliothèques Signalr et jQuery. UI.

1. Dans Visual Studio, créez une application Web ASP.NET.

    ![Créer un site Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. Dans la fenêtre **nouvelle application Web ASP.net-MoveShapeDemo** , laissez **vide** sélectionné, puis sélectionnez **OK**.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.

1. Dans **Ajouter un nouvel élément-MoveShapeDemo**, **Sélectionnez installé** > **Visual C#**  > **Web** > **signalr** , puis sélectionnez **classe de concentrateur signalr (v2)** .

1. Nommez la classe *MoveShapeHub* et ajoutez-la au projet.

    Cette étape crée le fichier de classe *MoveShapeHub.cs* . Simultanément, elle ajoute un ensemble de fichiers de script et de références d’assembly qui prennent en charge Signalr au projet.

1. Sélectionnez **outils** > **Gestionnaire de package NuGet** > **console du gestionnaire de package**.

1. Dans la **console du gestionnaire de package**, exécutez la commande suivante :

    ```console
    Install-Package jQuery.UI.Combined
    ```

    La commande installe la bibliothèque de l’interface utilisateur jQuery. Vous l’utilisez pour animer la forme.

1. Dans **Explorateur de solutions**, développez le nœud scripts.

    ![Références de la bibliothèque de scripts](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Les bibliothèques de scripts pour jQuery, jQueryUI et Signalr sont visibles dans le projet.

## <a name="create-the-base-application"></a>Créer l’application de base

Dans cette section, vous allez créer une application de navigateur. L’application envoie l’emplacement de la forme au serveur lors de chaque événement de déplacement de la souris. Le serveur diffuse ces informations à tous les autres clients connectés en temps réel. Vous en apprendrez davantage sur cette application dans les sections ultérieures.

1. Ouvrez le fichier *MoveShapeHub.cs* .

1. Remplacez le code du fichier *MoveShapeHub.cs* par le code suivant :

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Enregistrez le fichier.

La classe `MoveShapeHub` est une implémentation d’un concentrateur Signalr. Comme dans le didacticiel [prise en main avec signalr](tutorial-getting-started-with-signalr.md) , le Hub a une méthode que les clients appellent directement. Dans ce cas, le client envoie un objet avec les nouvelles coordonnées X et Y de la forme au serveur. Ces coordonnées sont diffusées à tous les autres clients connectés. Signalr sérialise automatiquement cet objet à l’aide de JSON.

L’application envoie l’objet `ShapeModel` au client. Il a des membres pour stocker la position de la forme. La version de l’objet sur le serveur a également un membre pour suivre les données du client qui sont stockées. Cet objet empêche le serveur de renvoyer les données d’un client à lui-même. Ce membre utilise l’attribut `JsonIgnore` pour empêcher l’application de sérialiser les données et de les renvoyer au client.

## <a name="map-to-the-hub-when-app-starts"></a>Mapper au concentrateur au démarrage de l’application

Ensuite, vous configurez le mappage au hub lorsque l’application démarre. Dans Signalr 2, l’ajout d’une classe de démarrage OWIN crée le mappage.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.

1. Dans **Ajouter un nouvel élément-MoveShapeDemo** , sélectionnez **installé** > **Visual C#**  > **Web** , puis sélectionnez **classe de démarrage OWIN**.

1. Nommez le *démarrage* de la classe, puis sélectionnez **OK**.

1. Remplacez le code par défaut dans le fichier *Startup.cs* par le code suivant :

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

La classe de démarrage OWIN appelle `MapSignalR` lorsque l’application exécute la méthode `Configuration`. L’application ajoute la classe au processus de démarrage de OWIN à l’aide de l’attribut d’assembly `OwinStartup`.

## <a name="add-the-client"></a>Ajouter le client

Ajoutez la page HTML pour le client.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **page HTML**.

1. Nommez la page **par défaut** , puis sélectionnez **OK**.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *default. html* , puis sélectionnez **définir comme page de démarrage**.

1. Remplacez le code par défaut dans le fichier *default. html* par le code suivant :

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. Dans **Explorateur de solutions**, développez **scripts**.

    Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.

    > [!IMPORTANT]
    > Le gestionnaire de package installe une version plus récente des scripts Signalr.

1. Mettez à jour les références de script dans le bloc de code afin qu’elles correspondent aux versions des fichiers de script dans le projet.

Ce code HTML et JavaScript crée une `div` rouge appelée `shape`. Il active le comportement de glissement de la forme à l’aide de la bibliothèque jQuery et utilise l’événement `drag` pour envoyer la position de la forme au serveur.

## <a name="run-the-app"></a>Exécuter l'application

Vous pouvez exécuter l’application pour se’e. Lorsque vous faites glisser la forme autour d’une fenêtre de navigateur, la forme se déplace également dans les autres navigateurs.

1. Dans la barre d’outils, activez le **débogage de script** , puis sélectionnez le bouton de lecture pour exécuter l’application en mode débogage.

    ![Capture d’écran de l’activation du mode de débogage par l’utilisateur et de la sélection de lecture.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Une fenêtre de navigateur s’ouvre avec la forme rouge dans le coin supérieur droit.

1. Copiez l’URL de la page.

1. Ouvrez un autre navigateur et collez l’URL dans la barre d’adresses.

1. Faites glisser la forme dans l’une des fenêtres du navigateur. La forme dans l’autre fenêtre de navigateur suit.

Bien que l’application fonctionne avec cette méthode, il ne s’agit pas d’un modèle de programmation recommandé. Il n’existe aucune limite supérieure pour le nombre de messages envoyés. Par conséquent, les clients et le serveur sont submergés par les messages et les dégradations de performances. En outre, l’application affiche une animation disjointe sur le client. Cette animation saccadée se produit parce que la forme se déplace instantanément par chaque méthode. Il est préférable que la forme se déplace correctement vers chaque nouvel emplacement. Ensuite, vous allez apprendre à résoudre ces problèmes.

## <a name="add-the-client-loop"></a>Ajouter la boucle cliente

L’envoi de l’emplacement de la forme sur chaque événement de déplacement de la souris crée un volume de trafic réseau inutile. L’application doit limiter les messages du client.

Utilisez la fonction JavaScript `setInterval` pour configurer une boucle qui envoie de nouvelles informations de position au serveur à un taux fixe. Cette boucle est une représentation de base d’une « boucle de jeu ». Il s’agit d’une fonction appelée à plusieurs reprises qui pilote toutes les fonctionnalités d’un jeu.

1. Remplacez le code client dans le fichier *default. html* par le code suivant :

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > Vous devez reremplacer les références de script. Ils doivent correspondre aux versions des scripts dans le projet.

    Ce nouveau code ajoute la fonction `updateServerModel`. Elle est appelée sur une fréquence fixe. La fonction envoie les données de position au serveur chaque fois que l’indicateur `moved` indique qu’il y a de nouvelles données de position à envoyer.

1. Sélectionner le bouton de lecture pour démarrer l’application

1. Copiez l’URL de la page.

1. Ouvrez un autre navigateur et collez l’URL dans la barre d’adresses.

1. Faites glisser la forme dans l’une des fenêtres du navigateur. La forme dans l’autre fenêtre de navigateur suit.

Étant donné que l’application limite le nombre de messages envoyés au serveur, l’animation ne s’affiche pas comme étant lisse au préalable.

## <a name="add-the-server-loop"></a>Ajouter la boucle serveur

Dans l’application actuelle, les messages envoyés depuis le serveur vers le client se déplacent aussi souvent qu’ils sont reçus. Ce trafic réseau présente un problème similaire, comme nous le voyons sur le client.

L’application peut envoyer des messages plus souvent que nécessaire. La connexion peut être saturée par conséquent. Cette section décrit comment mettre à jour le serveur pour ajouter un minuteur qui limite le taux des messages sortants.

1. Remplacez le contenu de `MoveShapeHub.cs` par ce code :

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Sélectionnez le bouton de lecture pour démarrer l’application.

1. Copiez l’URL de la page.

1. Ouvrez un autre navigateur et collez l’URL dans la barre d’adresses.

1. Faites glisser la forme dans l’une des fenêtres du navigateur.

Ce code développe le client pour ajouter la classe `Broadcaster`. La nouvelle classe limite les messages sortants à l’aide de la classe `Timer` à partir du .NET Framework.

Il est bon de savoir que le Hub lui-même est passager. Il est créé chaque fois qu’il est nécessaire. L’application crée donc le `Broadcaster` en tant que singleton. Elle utilise l’initialisation tardive pour différer la création de `Broadcaster`jusqu’à ce qu’elle soit nécessaire. Cela garantit que l’application crée la première instance de Hub complètement avant de démarrer la minuterie.

L’appel à la fonction `UpdateShape` des clients est ensuite déplacé hors de la méthode de `UpdateModel` du concentrateur. Elle n’est plus appelée immédiatement chaque fois que l’application reçoit des messages entrants. Au lieu de cela, l’application envoie les messages aux clients à un débit de 25 appels par seconde. Le processus est géré par le minuteur `_broadcastLoop` à partir de la classe `Broadcaster`.

Enfin, au lieu d’appeler directement la méthode du client à partir du concentrateur, la classe `Broadcaster` doit obtenir une référence au concentrateur `_hubContext` en cours d’exploitation. Elle obtient la référence avec l' `GlobalHost`.

## <a name="add-smooth-animation"></a>Ajouter une animation lisse

L’application est presque terminée, mais nous pourrions améliorer l’application. L’application déplace la forme sur le client en réponse aux messages du serveur. Au lieu de définir la position de la forme sur le nouvel emplacement donné par le serveur, utilisez la fonction `animate` de la bibliothèque de l’interface utilisateur JQuery. Il peut déplacer la forme en douceur entre sa position actuelle et la nouvelle position.

1. Mettez à jour la méthode de `updateShape` du client dans le fichier *default. html* pour qu’elle ressemble au code mis en surbrillance :

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Sélectionnez le bouton de lecture pour démarrer l’application.

1. Copiez l’URL de la page.

1. Ouvrez un autre navigateur et collez l’URL dans la barre d’adresses.

1. Faites glisser la forme dans l’une des fenêtres du navigateur.

Le mouvement de la forme dans l’autre fenêtre apparaît moins saccadé. L’application interpole ses mouvements dans le temps plutôt qu’une seule fois par message entrant.

Ce code déplace la forme de l’ancien emplacement vers le nouveau. Le serveur donne la position de la forme au cours de l’intervalle d’animation. Dans ce cas, il s’agit de 100 millisecondes. L’application efface toute animation précédente s’exécutant sur la forme avant le démarrage de la nouvelle animation.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a>Ressources supplémentaires

Le paradigme de communication que vous venez de découvrir est utile pour le développement de jeux en ligne et d’autres simulations, comme [le jeu de pousseurs créé avec signalr](https://shootr.azurewebsites.net/).

Pour plus d’informations sur Signalr, consultez les ressources suivantes :

* [Projet signalr](http://signalr.net)

* [Signalr GitHub et exemples](https://github.com/SignalR/SignalR)

* [Wiki signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Étapes suivantes :

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Configurer le projet
> * Création de l’application de base
> * Mappé au concentrateur au démarrage de l’application
> * Ajout du client
> * L’application a été exécutée
> * Ajout de la boucle client
> * Ajout de la boucle serveur
> * Animation lisse ajoutée

Passez à l’article suivant pour apprendre à créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de diffusion serveur.
> [!div class="nextstepaction"]
> [Signalr 2 et diffusion du serveur](tutorial-server-broadcast-with-signalr.md)