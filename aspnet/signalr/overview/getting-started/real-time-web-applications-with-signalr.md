---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Atelier pratique : applications Web en temps réel avec Signalr | Microsoft Docs'
author: bradygaster
description: Les applications Web en temps réel permettent d’envoyer du contenu côté serveur vers les clients connectés en temps réel. Pour les développeurs ASP.NET, ASP...
ms.author: bradyg
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 9e39fd3f2fc9d4e791002450085215096c222fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537099"
---
# <a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Atelier pratique : applications Web en temps réel avec Signalr

par l' [équipe Web camps](https://twitter.com/webcamps)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Télécharger le kit de formation Web camps, version 2015 d’octobre](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b)

> Les applications Web en temps réel permettent d’envoyer du contenu côté serveur vers les clients connectés en temps réel. Pour les développeurs ASP.NET, **ASP.net signalr** est une bibliothèque qui permet d’ajouter des fonctionnalités Web en temps réel à leurs applications. Elle tire parti de plusieurs transports, en sélectionnant automatiquement le meilleur transport disponible en fonction du meilleur transport disponible du client et du serveur. Elle tire parti de **WebSocket**, une API HTML5 qui permet une communication bidirectionnelle entre le navigateur et le serveur.
> 
> **Signalr** fournit également une API de haut niveau simple pour effectuer un RPC de serveur à client (appeler des fonctions JavaScript dans les navigateurs de vos clients à partir du code .net côté serveur) dans votre application ASP.net, ainsi qu’ajouter des hooks utiles pour la gestion des connexions, telles que les événements de connexion/déconnexion, le regroupement de connexions et l’autorisation.
> 
> **Signalr** est une abstraction sur certains des transports requis pour effectuer un travail en temps réel entre le client et le serveur. Une connexion **signalr** démarre en tant que http et est ensuite promue en connexion **WebSocket** si elle est disponible. **WebSocket** est le transport idéal pour **signalr**, car il optimise l’utilisation de la mémoire du serveur, présente la latence la plus faible et possède les fonctionnalités les plus sous-jacentes (telles que la communication duplex intégrale entre le client et le serveur), mais il présente également les exigences les plus strictes : **WebSocket** requiert que le serveur utilise **Windows server 2012** ou **Windows 8**, ainsi **4,5 .NET Framework**que Si ces exigences ne sont pas satisfaites, **signalr** tente d’utiliser d’autres transports pour effectuer ses connexions (par exemple, *interrogation longue Ajax*).
> 
> L’API **signalr** contient deux modèles pour la communication entre les clients et les serveurs : **connexions persistantes** et **concentrateurs**. Une **connexion** représente un point de terminaison simple pour l’envoi de messages de destinataire unique, groupés ou de diffusion. Un **Hub** est un pipeline plus élevé basé sur l’API de connexion qui permet à votre client et à votre serveur d’appeler directement des méthodes.
> 
> ![Architecture signalr](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, version 2015 d’octobre, disponible sur [https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b](https://github.com/Microsoft-Web/WebCampTrainingKit/releases/tag/v2015.10.13b).  Notez que le lien du programme d’installation sur cette page ne fonctionne plus ; Utilisez plutôt l’un des liens sous la section ressources.

<a id="Overview"></a>
## <a name="overview"></a>Présentation

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Envoyer des notifications du serveur au client à l’aide de Signalr.
- Scale Out votre application Signalr à l’aide de **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Les éléments suivants sont requis pour effectuer ce laboratoire pratique :

- [Visual Studio Express 2013 pour Web](https://www.microsoft.com/visualstudio/) ou version ultérieure

<a id="Setup"></a>
### <a name="setup"></a>Installation

Pour exécuter les exercices dans ce laboratoire pratique, vous devez d’abord configurer votre environnement.

1. Ouvrez une fenêtre de l’Explorateur Windows et accédez au dossier **source** du laboratoire.
2. Cliquez avec le bouton droit sur **Setup. cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui va configurer votre environnement et installer les extraits de code Visual Studio pour ce Lab.
3. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.

> [!NOTE]
> Vérifiez que vous avez vérifié toutes les dépendances de ce Lab avant d’exécuter le programme d’installation.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Utilisation des extraits de code

Tout au long du document Lab, vous serez invité à insérer des blocs de code. Pour plus de commodité, la majeure partie de ce code est fournie sous la forme d’extraits de Visual Studio Code, auxquels vous pouvez accéder à partir de Visual Studio 2013 pour éviter d’avoir à l’ajouter manuellement.

> [!NOTE]
> Chaque exercice est accompagné d’une solution de démarrage située dans le dossier **Begin** de l’exercice, qui vous permet de suivre chaque exercice indépendamment des autres. N’oubliez pas que les extraits de code ajoutés au cours d’un exercice sont absents de ces solutions de démarrage et peuvent ne pas fonctionner tant que vous n’avez pas terminé l’exercice. À l’intérieur du code source d’un exercice, vous trouverez également un dossier de **fin** contenant une solution Visual Studio avec le code qui résulte de l’exécution des étapes de l’exercice correspondant. Vous pouvez utiliser ces solutions comme conseils si vous avez besoin d’aide supplémentaire au fur et à mesure que vous travaillez dans ce laboratoire pratique.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique comprend les exercices suivants :

1. [Utilisation des données en temps réel à l’aide de Signalr](#Exercise1)
2. [Montée en charge à l’aide de SQL Server](#Exercise2)

Durée estimée pour effectuer ce laboratoire : **60 minutes**

> [!NOTE]
> Lorsque vous démarrez Visual Studio pour la première fois, vous devez sélectionner l’une des collections de paramètres prédéfinies. Chaque collection prédéfinie est conçue pour correspondre à un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, les extraits de code IntelliSense et les options de boîte de dialogue. Les procédures de ce laboratoire décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lors de l’utilisation de la collection de **paramètres de développement généraux** . Si vous choisissez une autre collection de paramètres pour votre environnement de développement, il peut y avoir des différences entre les étapes que vous devez prendre en compte.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Exercice 1 : utilisation des données en temps réel à l’aide de Signalr

Alors que la conversation est souvent utilisée comme exemple, vous pouvez en faire beaucoup plus avec la fonctionnalité Web en temps réel. Chaque fois qu’un utilisateur actualise une page Web pour voir les nouvelles données ou que la page implémente une interrogation longue AJAX pour récupérer de nouvelles données, vous pouvez utiliser Signalr.

Signalr prend en charge la fonctionnalité d’émission ou de **diffusion** de **serveur** ; Il gère automatiquement la gestion des connexions. Dans les connexions HTTP classiques pour la communication client-serveur, la connexion est rétablie pour chaque demande, mais Signalr assure une connexion permanente entre le client et le serveur. Dans Signalr, le code serveur appelle un code client dans le navigateur à l’aide d’appels de procédure distante (RPC), plutôt que du modèle de demande-réponse que nous savons aujourd’hui.

Dans cet exercice, vous allez configurer l’application de **quiz** pour utiliser signalr pour afficher le tableau de bord des statistiques avec les mesures mises à jour sans avoir à actualiser la page entière.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Tâche 1 : exploration de la page de statistiques des quiz de l’inversion

Dans cette tâche, vous allez parcourir l’application et vérifier comment la page statistiques s’affiche et comment vous pouvez améliorer la façon dont les informations sont mises à jour.

1. Ouvrez **Visual Studio Express 2013 pour le Web** et ouvrez la solution **GeekQuiz. sln** qui se trouve dans le dossier **Source\Ex1-WorkingWithRealTimeData\Begin** .
2. Appuyez sur **F5** pour exécuter la solution. La page **de connexion** doit s’afficher dans le navigateur.

    ![Exécution de la solution](real-time-web-applications-with-signalr/_static/image2.png "Exécution de la solution")

    *Exécution de la solution*
3. Cliquez sur **s’inscrire** dans le coin supérieur droit de la page pour créer un nouvel utilisateur dans l’application.

    ![Enregistrer le lien](real-time-web-applications-with-signalr/_static/image3.png "Enregistrer le lien")

    *Enregistrer le lien*
4. Dans la page **inscrire** , entrez un **nom d’utilisateur** et un **mot de passe**, puis cliquez sur **inscrire**.

    ![Inscription d’un utilisateur](real-time-web-applications-with-signalr/_static/image4.png "Inscription d’un utilisateur")

    *Inscription d’un utilisateur*
5. L’application inscrit le nouveau compte et l’utilisateur est authentifié et redirigé vers la page d’hébergement pour montrer la première question du quiz.
6. Ouvrez la page des **statistiques** dans une nouvelle fenêtre et placez la page d' **hébergement** et la page de **statistiques** côte à côte.

    ![Fenêtres côte à côte](real-time-web-applications-with-signalr/_static/image5.png "Fenêtres côte à côte")

    *Fenêtres côte à côte*
7. Dans la page d' **hébergement** , répondez à la question en cliquant sur l’une des options.

    ![Répondre à une question](real-time-web-applications-with-signalr/_static/image6.png "Répondre à une question")

    *Répondre à une question*
8. Une fois que vous avez cliqué sur l’un des boutons, la réponse doit s’afficher.

    ![Question à réponse correcte](real-time-web-applications-with-signalr/_static/image7.png "Question à réponse correcte")

    *Réponse à la question correctement*
9. Notez que les informations fournies dans la page des statistiques sont obsolètes. Actualisez la page pour afficher les résultats mis à jour.

    ![Page des statistiques](real-time-web-applications-with-signalr/_static/image8.png "Page des statistiques")

    *Page des statistiques*
10. Revenez à Visual Studio et arrêtez le débogage.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Tâche 2 : ajout de Signalr au quiz de l’inversion pour afficher les graphiques en ligne

Au cours de cette tâche, vous allez ajouter Signalr à la solution et envoyer automatiquement des mises à jour aux clients lorsqu’une nouvelle réponse est envoyée au serveur.

1. Dans le menu **Outils** de Visual Studio, sélectionnez **Gestionnaire de package NuGet**, puis cliquez sur **console du gestionnaire de package**.
2. Dans la fenêtre **console du gestionnaire de package** , exécutez la commande suivante :

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Installation du package signalr](real-time-web-applications-with-signalr/_static/image9.png "Installation du package signalr")

    *Installation du package signalr*

   > [!NOTE]
   > Lors de l’installation des packages NuGet **signalr** version 2.0.2 à partir d’une nouvelle application MVC 5, vous devez mettre à jour manuellement les packages **OWIN** vers la version 2.0.1 (ou une version ultérieure) avant d’installer signalr. Pour ce faire, vous pouvez exécuter le script suivant dans la **console du gestionnaire de package**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > Dans une version ultérieure de Signalr, les dépendances OWIN seront automatiquement mises à jour.
3. Dans **Explorateur de solutions**, développez le dossier **scripts** et notez que les fichiers *js* signalr ont été ajoutés à la solution.

    ![Références JavaScript signalr](real-time-web-applications-with-signalr/_static/image10.png "Références JavaScript signalr")

    *Références JavaScript signalr*
4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **GeekQuiz** , sélectionnez **Ajouter** | **nouveau dossier**, puis nommez-le **hubs**.
5. Cliquez avec le bouton droit sur le dossier **hubs** , puis sélectionnez **Ajouter | Nouvel élément**.

    ![Ajouter un nouvel élément](real-time-web-applications-with-signalr/_static/image11.png "Ajouter un nouvel élément")

    *Ajouter un nouvel élément*
6. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez l’élément  **C# visuel | Web | Nœud signalr** dans le volet gauche, sélectionnez la **classe de concentrateur signalr (v2)** dans le volet central, nommez le fichier **StatisticsHub.cs** , puis cliquez sur **Ajouter**.

    ![Boîte de dialogue Ajouter un nouvel élément](real-time-web-applications-with-signalr/_static/image12.png "Boîte de dialogue Ajouter un nouvel élément")

    *Boîte de dialogue Ajouter un nouvel élément*
7. Remplacez le code de la classe **StatisticsHub** par le code suivant.

    (Extrait de code- *RealTimeSignalR-EX1-StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Ouvrez **Startup.cs** et ajoutez la ligne suivante à la fin de la méthode de **configuration** .

    (Extrait de code- *RealTimeSignalR-EX1-MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Ouvrez la page **StatisticsService.cs** dans le dossier **services** et ajoutez les directives using suivantes.

    (Extrait de code- *RealTimeSignalR-EX1-UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Pour informer les clients connectés des mises à jour, vous devez d’abord récupérer un objet de **contexte** pour la connexion actuelle. L’objet **Hub** contient des méthodes pour envoyer des messages à un seul client ou à une diffusion à tous les clients connectés. Ajoutez la méthode suivante à la classe **StatisticsService** pour diffuser les données de statistiques.

    (Extrait de code- *RealTimeSignalR-EX1-NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > Dans le code ci-dessus, vous utilisez un nom de méthode arbitraire pour appeler une fonction sur le client (c.-à-d. : *UpdateStatistics*). Le nom de la méthode que vous spécifiez est interprété comme un objet dynamique, ce qui signifie qu’il n’y a pas de validation IntelliSense ou de compilation pour celui-ci. L’expression est évaluée au moment de l’exécution. Lorsque l’appel de méthode s’exécute, Signalr envoie le nom de la méthode et les valeurs de paramètre au client. Si le client a une méthode qui correspond au nom, cette méthode est appelée et les valeurs de paramètre lui sont passées. Si aucune méthode correspondante n’est trouvée sur le client, aucune erreur n’est générée. Pour plus d’informations, reportez-vous au Guide de l' [API ASP.net signalr hubs](../guide-to-the-api/hubs-api-guide-server.md).
11. Ouvrez la page **TriviaController.cs** dans le dossier **Controllers** et ajoutez les directives using suivantes.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Ajoutez le code en surbrillance suivant à la méthode d’action de **publication** .

    (Extrait de code- *RealTimeSignalR-EX1-NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Ouvrez la page **Statistics. cshtml** à l’intérieur des **vues |** Dossier de démarrage. Recherchez la section **scripts** et ajoutez les références de script suivantes au début de la section.

    (Extrait de code- *RealTimeSignalR-EX1-SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Quand vous ajoutez Signalr et d’autres bibliothèques de scripts à votre projet Visual Studio, le gestionnaire de package peut installer une version du fichier de script Signalr qui est plus récente que la version indiquée dans cette rubrique. Assurez-vous que la référence de script dans votre code correspond à la version de la bibliothèque de scripts installée dans votre projet.
14. Ajoutez le code en surbrillance suivant pour connecter le client au hub Signalr et mettre à jour les données de statistiques lors de la réception d’un nouveau message à partir du concentrateur.

    (Extrait de code- *RealTimeSignalR-EX1-SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    Dans ce code, vous créez un proxy Hub et vous inscrivez un gestionnaire d’événements pour écouter les messages envoyés par le serveur. Dans ce cas, vous écoutez les messages envoyés par le biais de la méthode *UpdateStatistics* .

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tâche 3 : exécution de la solution

Dans cette tâche, vous allez exécuter la solution pour vérifier que l’affichage des statistiques est mis à jour automatiquement à l’aide de Signalr après avoir répondu à une nouvelle question.

1. Appuyez sur **F5** pour exécuter la solution.

    > [!NOTE]
    > Si vous n’êtes pas déjà connecté à l’application, connectez-vous avec l’utilisateur que vous avez créé dans la tâche 1.
2. Ouvrez la page des **statistiques** dans une nouvelle fenêtre et placez la page d' **hébergement** et la page de **statistiques** côte à côte comme vous l’avez fait dans la tâche 1.
3. Dans la page d' **hébergement** , répondez à la question en cliquant sur l’une des options.

    ![Réponse à une autre question](real-time-web-applications-with-signalr/_static/image13.png "Réponse à une autre question")

    *Réponse à une autre question*
4. Une fois que vous avez cliqué sur l’un des boutons, la réponse doit s’afficher. Notez que les informations sur les statistiques de la page sont mises à jour automatiquement après avoir répondu à la question avec les informations mises à jour sans qu’il soit nécessaire d’actualiser la page entière.

    ![Page de statistiques actualisée après la réponse](real-time-web-applications-with-signalr/_static/image14.png "Page de statistiques actualisée après la réponse")

    *Page de statistiques actualisée après la réponse*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Exercice 2 : montée en charge à l’aide de SQL Server

Lors de la mise à l’échelle d’une application Web, vous pouvez généralement choisir entre *les* options de montée en puissance et de *montée* en puissance. La *montée* en puissance signifie l’utilisation d’un serveur de plus grande taille, avec plus de ressources (processeur, RAM, etc.), tandis que l’augmentation de la *taille des* instances implique l’ajout de serveurs supplémentaires pour gérer la charge. Le problème avec ce dernier est que les clients peuvent être acheminés vers différents serveurs. Un client connecté à un serveur ne reçoit pas les messages envoyés à partir d’un autre serveur.

Vous pouvez résoudre ces problèmes en utilisant un composant appelé *backplane*, pour transférer les messages entre les serveurs. Quand un fond de panier est activé, chaque instance d’application envoie des messages au fond de panier et le fond de panier les transfère aux autres instances de l’application.

Il existe actuellement trois types de plans pour Signalr :

- **Azure Service bus Windows**. Service Bus est une infrastructure de messagerie qui permet aux composants d’envoyer des messages faiblement couplés.
- **SQL Server**. Le fond de panier SQL Server écrit des messages dans des tables SQL. Le fond de panier utilise Service Broker pour une messagerie efficace. Toutefois, il fonctionne également si Service Broker n’est pas activé.
- **ReDim**. Redims est un magasin clé-valeur en mémoire. Redims prend en charge un modèle de publication/abonnement (« Pub/Sub ») pour l’envoi de messages.

Chaque message est envoyé via un bus de messages. Un bus de messages implémente l’interface [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , qui fournit une abstraction de publication/abonnement. Ces plans fonctionnent en remplaçant le **IMessageBus** par défaut par un bus conçu pour ce backplane.

Chaque instance de serveur se connecte au fond de panier via le bus. Lorsqu’un message est envoyé, il passe au fond de panier et le fond de panier l’envoie à tous les serveurs. Lorsqu’un serveur reçoit un message du fond de panier, il stocke le message dans son cache local. Le serveur remet ensuite les messages aux clients à partir de son cache local.

Pour plus d’informations sur le fonctionnement du fond de panier Signalr, lisez cet [article](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Dans certains cas, un fond de panier peut devenir un goulot d’étranglement. Voici quelques scénarios typiques de Signalr :
> 
> - [Diffusion](tutorial-server-broadcast-with-signalr.md) sur le serveur (par exemple, cotation boursière) : les plans sont adaptés à ce scénario, car le serveur contrôle la vitesse à laquelle les messages sont envoyés.
> - [Client à client](tutorial-getting-started-with-signalr.md) (par exemple, conversation) : dans ce scénario, le fond de panier peut être un goulot d’étranglement si le nombre de messages est mis à l’échelle avec le nombre de clients ; autrement dit, si le taux de messages augmente proportionnellement au fur et à mesure que d’autres clients se joignent.
> - [Haute fréquence en temps réel](tutorial-high-frequency-realtime-with-signalr.md) (par exemple, jeux en temps réel) : un backplane n’est pas recommandé pour ce scénario.

Dans cet exercice, vous allez utiliser **SQL Server** pour distribuer des messages dans l’application du **quiz** de l’inversion. Vous exécuterez ces tâches sur un seul ordinateur de test pour apprendre à configurer la configuration, mais pour tirer pleinement parti, vous devrez déployer l’application Signalr sur deux serveurs ou plus. Vous devez également installer SQL Server sur l’un des serveurs, ou sur un serveur dédié distinct.

![Scale Out à l’aide du diagramme de SQL Server](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Tâche 1 : comprendre le scénario

Au cours de cette tâche, vous allez exécuter 2 instances du quiz de l’un des **passionnés** simulant plusieurs instances IIS sur votre ordinateur local. Dans ce scénario, lorsque vous répondez à des questions sur une application, la mise à jour n’est pas notifiée sur la page des statistiques de la seconde instance. Cette simulation ressemble à un environnement dans lequel votre application est déployée sur plusieurs instances et utilise un équilibreur de charge pour communiquer avec eux.

1. Ouvrez la solution **Begin. sln** située dans le dossier **source/EX2-ScalingOutWithSQLServer/Begin** . Une fois chargé, vous **reExplorateur de serveurs** marquerez que la solution a deux projets avec des structures identiques, mais des noms différents. Cela permet de simuler l’exécution de deux instances de la même application sur votre ordinateur local.

    ![Commencer la solution simuler 2 instances du quiz du passionné](real-time-web-applications-with-signalr/_static/image16.png "Commencer la solution simuler 2 instances du quiz du passionné")

    *Commencer la solution simuler 2 instances du quiz du passionné*
2. Ouvrez la page Propriétés de la solution en cliquant avec le bouton droit sur le nœud de la solution et en sélectionnant **Propriétés**. Sous **projet de démarrage**, sélectionnez **plusieurs projets de démarrage** et modifiez la valeur **action** pour les deux projets sur *Démarrer*.

    ![Démarrage de plusieurs projets](real-time-web-applications-with-signalr/_static/image17.png "Démarrage de plusieurs projets")

    *Démarrage de plusieurs projets*
3. Appuyez sur **F5** pour exécuter la solution. L’application lance deux instances de **quiz** sur les différents ports, en simulant plusieurs instances de la même application. Épinglez l’un des navigateurs à gauche et l’autre à droite de votre écran. Connectez-vous avec vos informations d’identification ou enregistrez un nouvel utilisateur. Une fois connecté, conservez la page des anecdotes à gauche et accédez à la page des **statistiques** dans le navigateur à droite.

    ![Quiz de l’un des passionnés côte à côte](real-time-web-applications-with-signalr/_static/image18.png)

    *Quiz de l’un des passionnés côte à côte*

    ![Quiz de l’un des passionnés dans différents ports](real-time-web-applications-with-signalr/_static/image19.png)

    *Quiz de l’un des passionnés dans différents ports*
4. Commencez à répondre aux questions dans le navigateur de gauche. vous remarquerez que la page des **statistiques** dans le bon navigateur n’est pas mise à jour. Cela est dû au fait que **signalr** utilise un cache local pour distribuer les messages entre les clients et que ce scénario simule plusieurs instances. par conséquent, le cache n’est pas partagé entre eux. Vous pouvez vérifier que **signalr** fonctionne en testant les mêmes étapes, mais en utilisant une seule application. Dans les tâches suivantes, vous allez configurer un fond de panier pour répliquer les messages entre les instances.
5. Revenez à Visual Studio et arrêtez le débogage.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Tâche 2 : création de la SQL Server fond de panier

Au cours de cette tâche, vous allez créer une base de données qui servira de fond de panier pour l’application du **quiz** de l’inversion. Vous utiliserez **Explorateur d’objets SQL Server** pour parcourir votre serveur et initialiser la base de données. En outre, vous allez activer l' **Service Broker**.

1. Dans **Visual Studio**, ouvrez la **vue** de menu et sélectionnez **Explorateur d’objets SQL Server**.
2. Connectez-vous à votre instance de base de données locale en cliquant avec le bouton droit sur le nœud **SQL Server** et en sélectionnant l’option **ajouter SQL Server..** ..

    ![Ajout d’une instance de SQL Server](real-time-web-applications-with-signalr/_static/image20.png "Ajout d’une instance de SQL Server")

    *Ajout d’une instance de SQL Server à Explorateur d’objets SQL Server*
3. Définissez le **nom du serveur** sur *(\v11.0)* et laissez **authentification Windows** comme mode d’authentification. Cliquez sur **Connect** pour continuer.

    ![Connexion à la base de données locale](real-time-web-applications-with-signalr/_static/image21.png "Connexion à la base de données locale")

    *Connexion à la base de données locale*
4. Maintenant que vous êtes connecté à votre instance de base de données locale, vous devez créer une base de données qui représente le SQL Server fond de panier pour Signalr. Pour ce faire, cliquez avec le bouton droit sur le nœud **bases de données** , puis sélectionnez **Ajouter une nouvelle base de données**.

    ![Ajout d’une nouvelle base de données](real-time-web-applications-with-signalr/_static/image22.png "Ajout d’une nouvelle base de données")

    *Ajout d’une nouvelle base de données*
5. Définissez le nom de la base de données sur *signalr* et cliquez sur **OK** pour le créer.

    ![Création de la base de données Signalr](real-time-web-applications-with-signalr/_static/image23.png "Création de la base de données Signalr")

    *Création de la base de données Signalr*

    > [!NOTE]
    > Vous pouvez choisir n’importe quel nom pour la base de données.
6. Pour recevoir des mises à jour plus efficacement à partir du backplane, il est recommandé d’activer les Service Broker pour la base de données. Service Broker fournit une prise en charge native de la messagerie et de la mise en file d’attente dans SQL Server. Le fond de panier fonctionne également sans Service Broker. Ouvrez une nouvelle requête en cliquant avec le bouton droit sur la base de données et sélectionnez **nouvelle requête**.

    ![Ouverture d’une nouvelle requête](real-time-web-applications-with-signalr/_static/image24.png "Ouverture d’une nouvelle requête")

    *Ouverture d’une nouvelle requête*
7. Pour vérifier si la Service Broker est activée, interrogez la colonne **is\_Broker\_activée** de l’affichage catalogue **sys. databases** . Exécutez le script suivant dans la fenêtre de requête ouverte récemment.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Interrogation de l’état de l’Service Broker](real-time-web-applications-with-signalr/_static/image25.png "Interrogation de l’état de l’Service Broker")

    *Interrogation de l’état de l’Service Broker*
8. Si la valeur de la colonne **est\_broker\_activée** dans votre base de données est &quot;0&quot;, utilisez la commande suivante pour l’activer. Remplacez **&lt;votre&gt;de base de données** par le nom que vous avez défini lors de la création de la base de données (par exemple : signalr).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Activation de Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Activation de Service Broker")

    *Activation de Service Broker*

    > [!NOTE]
    > Si cette requête semble se bloquer, assurez-vous qu’aucune application n’est connectée à la base de la base de la.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Tâche 3 : configuration de l’application Signalr

Au cours de cette tâche, vous allez configurer le **quiz** de l’initiateur pour la connexion au SQL Server fond de panier. Vous allez d’abord ajouter le package NuGet **signalr. SqlServer** et définir la chaîne de connexion à votre base de données de fond de panier.

1. Ouvrez la **console du gestionnaire de package** dans **Outils** > **Gestionnaire de package NuGet**. Assurez-vous que **GeekQuiz** Project est sélectionné dans la liste déroulante **projet par défaut** . Tapez la commande suivante pour installer le package NuGet **Microsoft. Aspnet. signalr. SqlServer** .

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Répétez l’étape précédente, mais cette fois pour le projet **GeekQuiz2**.
3. Pour configurer la SQL Server fond de panier, ouvrez le fichier **Startup.cs** du projet **GeekQuiz** et ajoutez le code suivant à la méthode **configure** . Remplacez **&lt;votre&gt;de base de données** par le nom de votre base de données que vous avez utilisé lors de la création du fond de panier SQL Server. Répétez cette étape pour le projet **GeekQuiz2** .

    (Extrait de code- *RealTimeSignalR-EX2-StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Maintenant que les deux projets sont configurés pour utiliser le SQL Server fond de panier, appuyez sur **F5** pour les exécuter simultanément.
5. Là encore, **Visual Studio** lance deux instances du quiz de l’un des **passionnés** dans différents ports. Épinglez l’un des navigateurs sur la gauche et l’autre à droite de votre écran et connectez-vous avec vos informations d’identification. Conservez la page des anecdotes à gauche et accédez à la page des **statistiques** dans le bon navigateur.
6. Commencez à répondre aux questions dans le navigateur de gauche. Cette fois-ci, la page de **statistiques** est mise à jour grâce au fond de panier. Basculer entre les applications (les**statistiques** sont désormais sur la gauche, et les **anecdotes** sur la droite) et répéter le test pour vérifier qu’il fonctionne pour les deux instances. Le fond de panier fait office de *cache partagé* de messages pour chaque serveur connecté, et chaque serveur stocke les messages dans leur propre cache local pour les distribuer aux clients connectés.
7. Revenez à Visual Studio et arrêtez le débogage.
8. Le composant SQL Server backplane génère automatiquement les tables nécessaires sur la base de données spécifiée. Dans le panneau **Explorateur d’objets SQL Server** , ouvrez la base de données que vous avez créée pour le fond de panier (p. ex. : signalr) et développez ses tables. Les tableaux suivants doivent s’afficher :

    ![Tables générées par le fond de panier](real-time-web-applications-with-signalr/_static/image27.png)

    *Tables générées par le fond de panier*
9. Cliquez avec le bouton droit sur la table **signalr. Messages\_0** , puis sélectionnez **afficher les données**.

    ![Afficher la table des messages de fond de panier Signalr](real-time-web-applications-with-signalr/_static/image28.png)

    *Afficher la table des messages de fond de panier Signalr*
10. Vous pouvez voir les différents messages envoyés au **Hub** lorsque vous répondez aux questions des questionnaires. Le fond de panier distribue ces messages à toute instance connectée.

    ![Table des messages de fond de panier](real-time-web-applications-with-signalr/_static/image29.png)

    *Table des messages de fond de panier*

---

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Dans ce laboratoire pratique, vous avez appris à ajouter **signalr** à votre application et à envoyer des notifications du serveur à vos clients connectés à l’aide de **hubs**. En outre, vous avez appris à mettre à l’échelle votre application à l’aide d’un composant de *fond de panier* lorsque votre application est déployée dans plusieurs instances IIS.
