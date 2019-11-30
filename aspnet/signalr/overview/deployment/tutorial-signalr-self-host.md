---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Didacticiel : auto-hôte Signalr | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment créer un serveur auto-hébergé Signalr 2 et comment s’y connecter avec un client JavaScript. Versions logicielles utilisées dans le didacticiel V...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74578575"
---
# <a name="tutorial-signalr-self-host"></a>Didacticiel : auto-hôte Signalr

de [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Télécharger le projet terminé](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Ce didacticiel montre comment créer un serveur auto-hébergé Signalr 2 et comment s’y connecter avec un client JavaScript.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr version 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Utilisation de Visual Studio 2012 avec ce didacticiel
>
>
> Pour utiliser Visual Studio 2012 dans ce didacticiel, procédez comme suit :
>
> - Mettez à jour votre [Gestionnaire de package](http://docs.nuget.org/docs/start-here/installing-nuget) avec la dernière version.
> - Installez le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - Dans le Web Platform Installer, recherchez et installez **ASP.NET et Web Tools 2013,1 pour Visual Studio 2012**. Cela permet d’installer les modèles Visual Studio pour les classes Signalr telles que **Hub**.
> - Certains modèles (par exemple, la **classe de démarrage OWIN**) ne sont pas disponibles. pour ceux-ci, utilisez plutôt un fichier de classe.
>
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Vue d'ensemble de

Un serveur Signalr est généralement hébergé dans une application ASP.NET dans IIS, mais il peut également être auto-hébergé (comme dans une application console ou un service Windows) à l’aide de la bibliothèque auto-hôte. Cette bibliothèque, comme la totalité de Signalr 2, repose sur OWIN ([Open Web interface pour .net](http://owin.org)). OWIN définit une abstraction entre les serveurs Web .NET et les applications Web. OWIN découple l’application Web du serveur, ce qui fait de OWIN idéal pour l’auto-hébergement d’une application Web dans votre propre processus, en dehors d’IIS.

Les raisons pour lesquelles l’hébergement n’est pas dans IIS sont les suivantes :

- Environnements où IIS n’est pas disponible ou souhaitable, par exemple une batterie de serveurs existante sans IIS.
- La surcharge de performances d’IIS doit être évitée.
- La fonctionnalité signalr doit être ajoutée à une application existante qui s’exécute dans un service Windows, un rôle de travail Azure ou tout autre processus.

Si une solution est développée comme auto-hébergée pour des raisons de performances, il est recommandé de tester également l’application hébergée dans IIS pour déterminer l’avantage en matière de performances.

Ce didacticiel contient les sections suivantes :

- [Création du serveur](#server)
- [Accès au serveur avec un client JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Création du serveur

Dans ce didacticiel, vous allez créer un serveur hébergé dans une application console, mais le serveur peut être hébergé dans n’importe quel processus, tel qu’un service Windows ou un rôle de travail Azure. Pour obtenir un exemple de code pour l’hébergement d’un serveur Signalr dans un service Windows, consultez [auto-hébergement signalr dans un service Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Ouvrez Visual Studio 2013 avec des privilèges d’administrateur. Sélectionnez **fichier**, **nouveau projet**. Sélectionnez **fenêtres** sous le **nœud C# visuel** dans le volet **modèles** , puis sélectionnez le modèle **application console** . Nommez le nouveau projet « SignalRSelfHost », puis cliquez sur **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Ouvrez la console du gestionnaire de package NuGet en sélectionnant **outils** > **Gestionnaire de package NuGet** > **console du gestionnaire de package**.
3. Dans la console du gestionnaire de package, entrez la commande suivante :

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Cette commande ajoute les bibliothèques d’auto-hébergement Signalr 2 au projet.
4. Dans la console du gestionnaire de package, entrez la commande suivante :

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Cette commande ajoute la bibliothèque Microsoft. Owin. cors au projet. Cette bibliothèque sera utilisée pour la prise en charge inter-domaines, qui est requise pour les applications qui hébergent Signalr et un client de page Web dans des domaines différents. Étant donné que vous hébergerez le serveur Signalr et le client Web sur des ports différents, cela signifie que l’activation de la communication entre domaines doit être activée entre les composants.
5. Remplacez le contenu de Program.cs par le code suivant.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Le code ci-dessus comprend trois classes :

    - **, Y**compris la méthode **main** définissant le chemin d’accès principal d’exécution. Dans cette méthode, une application Web de type **Startup** est démarrée à l’URL spécifiée (`http://localhost:8080`). Si la sécurité est requise sur le point de terminaison, vous pouvez implémenter le protocole SSL. Pour plus d’informations, consultez [procédure : configurer un port avec un certificat SSL](https://msdn.microsoft.com/library/ms733791.aspx) .
    - **Startup**, la classe contenant la configuration du serveur signalr (la seule configuration utilisée par ce didacticiel est l’appel à `UseCors`) et l’appel à `MapSignalR`, qui crée des itinéraires pour tous les objets Hub du projet.
    - **Monconcentrateur**, la classe de concentrateur signalr que l’application fournira aux clients. Cette classe possède une méthode unique, **Envoyer**, que les clients appellera pour diffuser un message à tous les autres clients connectés.
6. Compilez et exécutez l'application. L’adresse que le serveur exécute doit s’afficher dans une fenêtre de console.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Si l’exécution échoue avec l’exception `System.Reflection.TargetInvocationException was unhandled`, vous devrez redémarrer Visual Studio avec des privilèges d’administrateur.
8. Arrêtez l’application avant de passer à la section suivante.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Accès au serveur avec un client JavaScript

Dans cette section, vous allez utiliser le même client JavaScript que le [didacticiel prise en main](../getting-started/tutorial-getting-started-with-signalr.md). Nous allons uniquement apporter une seule modification au client, ce qui permet de définir explicitement l’URL du Hub. Avec une application auto-hébergée, le serveur ne doit pas nécessairement se trouver à la même adresse que l’URL de connexion (en raison des proxies inverses et des équilibreurs de charge), de sorte que l’URL doit être définie explicitement.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution et sélectionnez **Ajouter**, **nouveau projet**. Sélectionnez le nœud **Web** , puis sélectionnez le modèle **application Web ASP.net** . Nommez le projet « JavascriptClient », puis cliquez sur **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Sélectionnez le modèle **vide** et laissez les autres options désélectionnées. Sélectionnez **créer un projet**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. Dans la console du gestionnaire de package, sélectionnez le projet « JavascriptClient » dans la liste déroulante **projet par défaut** , puis exécutez la commande suivante :

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Cette commande installe les bibliothèques Signalr et JQuery dont vous aurez besoin dans le client.
4. Cliquez avec le bouton droit sur votre projet et sélectionnez **Ajouter**, **nouvel élément**. Sélectionnez le nœud **Web** , puis page html. Nommez la page **default. html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Remplacez le contenu de la nouvelle page HTML par le code suivant. Vérifiez que les références de script correspondent aux scripts dans le dossier scripts du projet.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Le code suivant (mis en surbrillance dans l’exemple de code ci-dessus) est l’ajout que vous avez apporté au client utilisé dans le didacticiel obtenir des étoiles (en plus de la mise à niveau du code vers Signalr version 2 bêta). Cette ligne de code définit explicitement l’URL de connexion de base pour Signalr sur le serveur.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Cliquez avec le bouton droit sur la solution, puis sélectionnez **définir les projets de démarrage...** . Sélectionnez la case d’option **plusieurs projets de démarrage** et définissez l' **action** les deux projets sur **Démarrer**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Cliquez avec le bouton droit sur « default. html » et sélectionnez **définir comme page de démarrage**.
8. Exécutez l'application. Le serveur et la page seront lancés. Vous devrez peut-être recharger la page Web (ou sélectionner **Continuer** dans le débogueur) si la page se charge avant le démarrage du serveur.
9. Dans le navigateur, indiquez un nom d’utilisateur lorsque vous y êtes invité. Copiez l’URL de la page dans un autre onglet ou une autre fenêtre de navigateur et fournissez un nom d’utilisateur différent. Vous serez en mesure d’envoyer des messages d’un volet de navigateur à l’autre, comme dans le didacticiel Prise en main.
