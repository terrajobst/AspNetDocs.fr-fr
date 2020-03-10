---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Temps réel haute fréquence avec Signalr 1. x | Microsoft Docs
author: bradygaster
description: Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr pour fournir une fonctionnalité de messagerie à fréquence élevée. Messagerie à fréquence élevée dans...
ms.author: bradyg
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: e37e63a410952ec170cbbaadeeb54eae7e82b3b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623500"
---
# <a name="high-frequency-realtime-with-signalr-1x"></a>Temps réel haute fréquence avec SignalR 1.x

de [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr pour fournir une fonctionnalité de messagerie à fréquence élevée. Dans ce cas, la messagerie à fréquence élevée signifie les mises à jour envoyées à un taux fixe. dans le cas de cette application, jusqu’à 10 messages par seconde.
> 
> L’application que vous allez créer dans ce didacticiel affiche une forme que les utilisateurs peuvent faire glisser. La position de la forme dans tous les autres navigateurs connectés est ensuite mise à jour pour correspondre à la position de la forme glissée à l’aide des mises à jour chronométrées.
> 
> Les concepts présentés dans ce didacticiel ont des applications en temps réel et d’autres applications de simulation.
> 
> Les commentaires sur ce didacticiel sont les bienvenus. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com).

## <a name="overview"></a>Présentation

Ce didacticiel montre comment créer une application qui partage l’état d’un objet avec d’autres navigateurs en temps réel. L’application que nous allons créer est appelée MoveShape. La page MoveShape affiche un élément div HTML que l’utilisateur peut faire glisser ; Lorsque l’utilisateur fait glisser la balise div, sa nouvelle position est envoyée au serveur, qui indique ensuite à tous les autres clients connectés de mettre à jour la position de la forme pour qu’elle corresponde.

![Fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

L’application créée dans ce didacticiel est basée sur une démonstration de Damian Edwards. Une vidéo contenant cette démonstration peut être consultée [ici](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Le didacticiel commence en montrant comment envoyer des messages Signalr à partir de chaque événement qui se déclenche lors de l’opération de déplacement de la forme. Chaque client connecté met ensuite à jour la position de la version locale de la forme chaque fois qu’un message est reçu.

Bien que l’application fonctionne avec cette méthode, il ne s’agit pas d’un modèle de programmation recommandé, étant donné qu’il n’y a pas de limite supérieure au nombre de messages envoyés. les clients et le serveur peuvent donc être submergés de messages et les performances se dégradent . L’animation affichée sur le client est également disjointe, car la forme est déplacée instantanément par chaque méthode, au lieu de se déplacer correctement vers chaque nouvel emplacement. Les sections suivantes du didacticiel expliquent comment créer une fonction de minuteur qui limite la vitesse maximale à laquelle les messages sont envoyés par le client ou le serveur, et comment déplacer la forme facilement entre les emplacements. La version finale de l’application créée dans ce didacticiel peut être téléchargée à partir de la [Galerie de code](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Ce didacticiel contient les sections suivantes :

- [Conditions préalables](#prerequisites)
- [Création du projet](#createtheproject)
- [Ajouter les packages NuGet ASP.NET Signalr et JQuery. UI](#nugetpackages)
- [Créer l’application de base](#baseapp)
- [Ajouter la boucle cliente](#clientloop)
- [Ajouter la boucle serveur](#serverloop)
- [Ajouter une animation lisse sur le client](#animation)
- [Autres étapes](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Conditions préalables requises

Ce didacticiel nécessite Visual Studio 2012 ou Visual Studio 2010. Si Visual Studio 2010 est utilisé, le projet utilise .NET Framework 4 au lieu de .NET Framework 4,5.

Si vous utilisez Visual Studio 2012, il est recommandé d’installer la [mise à jour ASP.NET et Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Cette mise à jour contient de nouvelles fonctionnalités, telles que des améliorations de la publication, de nouvelles fonctionnalités et de nouveaux modèles.

Si vous disposez de Visual Studio 2010, assurez-vous que [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) est installé.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Créer le projet

Dans cette section, nous allons créer le projet dans Visual Studio.

1. Dans le menu **Fichier**, cliquez sur **Nouveau projet**.
2. Dans la boîte de dialogue **nouveau projet** , **C#** développez sous **modèles** , puis sélectionnez **Web**.
3. Sélectionnez le modèle **application Web vide ASP.net** , nommez le projet *MoveShapeDemo*, puis cliquez sur **OK**.

    ![Création du projet](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Ajouter les packages NuGet Signalr et JQuery. UI

Vous pouvez ajouter une fonctionnalité Signalr à un projet en installant un package NuGet. Ce didacticiel utilise également le package JQuery. UI pour permettre le glissement et l’animation de la forme.

1. Cliquez sur **Outils | Gestionnaire de package NuGet | Console du gestionnaire de package**.
2. Dans le gestionnaire de package, entrez la commande suivante.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Le package Signalr installe un certain nombre d’autres packages NuGet en tant que dépendances. Une fois l’installation terminée, vous disposez de tous les composants serveur et client requis pour utiliser Signalr dans une application ASP.NET.
3. Entrez la commande suivante dans la console du gestionnaire de package pour installer les packages JQuery et JQuery. UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Créer l’application de base

Dans cette section, nous allons créer une application de navigateur qui envoie l’emplacement de la forme au serveur lors de chaque événement de déplacement de la souris. Le serveur diffuse ensuite ces informations à tous les autres clients connectés au fur et à mesure de leur réception. Nous développerons cette application dans les sections suivantes.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter**, **classe.** ... Nommez la classe **MoveShapeHub** , puis cliquez sur **Ajouter**.
2. Remplacez le code de la nouvelle classe **MoveShapeHub** par le code suivant.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    La classe `MoveShapeHub` ci-dessus est une implémentation d’un concentrateur Signalr. Comme dans le didacticiel [prise en main avec signalr](index.md) , le Hub a une méthode que les clients appellera directement. Dans ce cas, le client enverra un objet contenant les nouvelles coordonnées X et Y de la forme au serveur, qui est ensuite diffusé à tous les autres clients connectés. Signalr sérialise automatiquement cet objet à l’aide de JSON.

    L’objet qui sera envoyé au client (`ShapeModel`) contient des membres pour stocker la position de la forme. La version de l’objet sur le serveur contient également un membre pour suivre les données du client qui sont stockées, afin qu’un client donné n’envoie pas ses propres données. Ce membre utilise l’attribut `JsonIgnore` pour empêcher qu’il soit sérialisé et envoyé au client.
3. Ensuite, nous allons configurer le concentrateur au démarrage de l’application. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis cliquez sur **Ajouter | Classe d’application globale**. Acceptez le nom par défaut *Global* , puis cliquez sur **OK**.

    ![Ajouter une classe d’application globale](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Ajoutez l’instruction `using` suivante après les instructions **using** fournies dans la classe Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Ajoutez la ligne de code suivante à la méthode `Application_Start` de la classe globale pour enregistrer l’itinéraire par défaut pour Signalr.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Votre fichier global. asax doit ressembler à ce qui suit :

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Nous allons ensuite ajouter le client. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis cliquez sur **Ajouter | Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **page HTML**. Attribuez un nom approprié à la page ( **par exemple default. html**), puis cliquez sur **Ajouter**.
7. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la page que vous venez de créer, puis cliquez sur **définir comme page de démarrage**.
8. Remplacez le code par défaut dans la page HTML par l’extrait de code suivant.

    > [!NOTE]
    > Vérifiez que les références de script ci-dessous correspondent aux packages ajoutés à votre projet dans le dossier scripts. Dans Visual Studio 2010, la version de JQuery et Signalr ajoutée au projet peut ne pas correspondre aux numéros de version ci-dessous.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Le code HTML et JavaScript ci-dessus crée un div rouge appelé Shape, active le comportement de glissement de la forme à l’aide de la bibliothèque jQuery et utilise l’événement `drag` de la forme pour envoyer la position de la forme au serveur.
9. Démarrez l’application en appuyant sur F5. Copiez l’URL de la page, puis collez-la dans une deuxième fenêtre de navigateur. Faites glisser la forme dans l’une des fenêtres du navigateur ; la forme dans l’autre fenêtre de navigateur doit se déplacer.

    ![Fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Ajouter la boucle cliente

Étant donné que l’envoi de l’emplacement de la forme sur chaque événement de déplacement de la souris crée un volume de trafic réseau inutile, les messages du client doivent être limités. Nous allons utiliser la fonction JavaScript `setInterval` pour configurer une boucle qui envoie de nouvelles informations de position au serveur à un taux fixe. Cette boucle est une représentation très basique d’une « boucle de jeu », une fonction appelée à plusieurs reprises qui pilote toutes les fonctionnalités d’un jeu ou d’une autre simulation.

1. Mettez à jour le code client dans la page HTML pour qu’il corresponde à l’extrait de code suivant.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    La mise à jour ci-dessus ajoute la fonction `updateServerModel`, qui est appelée sur une fréquence fixe. Cette fonction envoie les données de position au serveur chaque fois que l’indicateur `moved` indique qu’il y a de nouvelles données de position à envoyer.
2. Démarrez l’application en appuyant sur F5. Copiez l’URL de la page, puis collez-la dans une deuxième fenêtre de navigateur. Faites glisser la forme dans l’une des fenêtres du navigateur ; la forme dans l’autre fenêtre de navigateur doit se déplacer. Étant donné que le nombre de messages envoyés au serveur sera limité, l’animation n’apparaîtra pas aussi lisse que dans la section précédente.

    ![Fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Ajouter la boucle serveur

Dans l’application actuelle, les messages envoyés depuis le serveur vers le client se déplacent aussi souvent qu’ils sont reçus. Cela présente un problème semblable à celui observé sur le client. les messages peuvent être envoyés plus souvent qu’ils ne le sont, et la connexion peut être envahie par conséquent. Cette section décrit comment mettre à jour le serveur pour implémenter un minuteur qui limite le taux des messages sortants.

1. Remplacez le contenu de `MoveShapeHub.cs` par l’extrait de code suivant.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Le code ci-dessus développe le client pour ajouter la classe `Broadcaster`, qui limite les messages sortants à l’aide de la classe `Timer` à partir du .NET Framework.

    Étant donné que le Hub lui-même est transitif (il est créé chaque fois qu’il est nécessaire), le `Broadcaster` est créé en tant que singleton. L’initialisation tardive (introduite dans .NET 4) est utilisée pour différer sa création jusqu’à ce qu’elle soit nécessaire, ce qui garantit que la première instance de concentrateur est créée complètement avant le démarrage du minuteur.

    L’appel à la fonction `UpdateShape` des clients est ensuite déplacé hors de la méthode de `UpdateModel` du concentrateur, afin qu’il ne soit plus appelé immédiatement chaque fois que des messages entrants sont reçus. Au lieu de cela, les messages aux clients seront envoyés à un taux de 25 appels par seconde, gérés par le minuteur `_broadcastLoop` à partir de la classe `Broadcaster`.

    Enfin, au lieu d’appeler directement la méthode du client à partir du concentrateur, la classe `Broadcaster` doit obtenir une référence au concentrateur en cours d’exploitation (`_hubContext`) à l’aide de l' `GlobalHost`.
2. Démarrez l’application en appuyant sur F5. Copiez l’URL de la page, puis collez-la dans une deuxième fenêtre de navigateur. Faites glisser la forme dans l’une des fenêtres du navigateur ; la forme dans l’autre fenêtre de navigateur doit se déplacer. Il n’y aura pas de différence visible dans le navigateur à partir de la section précédente, mais le nombre de messages envoyés au client sera limité.

    ![Fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Ajouter une animation lisse sur le client

L’application est presque terminée, mais nous pourrions améliorer la forme sur le client au fur et à mesure qu’elle est déplacée en réponse aux messages du serveur. Au lieu de définir la position de la forme sur le nouvel emplacement donné par le serveur, nous allons utiliser la fonction `animate` de la bibliothèque de l’interface utilisateur JQuery pour déplacer la forme en douceur entre sa position actuelle et la nouvelle position.

1. Mettez à jour la méthode de `updateShape` du client pour qu’elle ressemble au code mis en surbrillance ci-dessous :

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Le code ci-dessus déplace la forme de l’ancien emplacement vers la nouvelle donnée par le serveur au cours de l’intervalle d’animation (dans ce cas, 100 millisecondes). Toute animation précédente s’exécutant sur la forme est effacée avant le démarrage de la nouvelle animation.
2. Démarrez l’application en appuyant sur F5. Copiez l’URL de la page, puis collez-la dans une deuxième fenêtre de navigateur. Faites glisser la forme dans l’une des fenêtres du navigateur ; la forme dans l’autre fenêtre de navigateur doit se déplacer. Le mouvement de la forme dans l’autre fenêtre doit apparaître moins saccadé car son mouvement est interpolé dans le temps plutôt que d’être défini une fois par message entrant.

    ![Fenêtre d’application](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Autres étapes

Dans ce didacticiel, vous avez appris à programmer une application Signalr qui envoie des messages à fréquence élevée entre les clients et les serveurs. Ce paradigme de communication est utile pour le développement de jeux en ligne et d’autres simulations, telles que [le jeu de pousseurs créé avec signalr](http://shootr.signalr.net).

L’application complète créée dans ce didacticiel peut être téléchargée à partir de la [Galerie de code](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Pour en savoir plus sur les concepts de développement Signalr, visitez les sites suivants pour le code source et les ressources Signalr :

- [Projet signalr](http://signalr.net)
- [Signalr GitHub et exemples](https://github.com/SignalR/SignalR)
- [Wiki signalr](https://github.com/SignalR/SignalR/wiki)
