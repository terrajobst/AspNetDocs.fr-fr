---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Modèle de travail centré sur la file d’attente (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: c73b070f11366e781bcea70ffc84fd49a47d469a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582775"
---
# <a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Modèle de travail centré sur la file d’attente (génération d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Précédemment, nous avons vu que l’utilisation de plusieurs services peut aboutir à un contrat SLA « composite », où le contrat SLA effectif de l’application est le *produit* des contrats SLA individuels. Par exemple, l’application Fix It utilise les sites Web, le stockage et les SQL Database. Si l’un de ces services échoue, l’application renvoie une erreur à l’utilisateur.

La mise en cache est un bon moyen de gérer les défaillances temporaires pour le contenu en lecture seule. Mais que se passe-t-il si votre application a besoin de travailler ? Par exemple, lorsque l’utilisateur soumet une nouvelle tâche Fix it, l’application ne peut pas simplement placer la tâche dans le cache. L’application doit écrire la tâche Fix it dans un magasin de données persistant, afin qu’elle puisse être traitée.

C’est là qu’intervient le modèle de travail centré sur la file d’attente. Ce modèle permet un couplage lâche entre un niveau Web et un service principal.

Voici comment fonctionne le modèle. Lorsque l’application reçoit une demande, elle place un élément de travail dans une file d’attente et retourne immédiatement la réponse. Ensuite, un processus principal distinct extrait des éléments de travail à partir de la file d’attente et effectue le travail.

Le modèle de travail centré sur la file d’attente est utile pour :

- Travail qui prend du temps (latence élevée).
- Travail nécessitant un service externe qui peut ne pas toujours être disponible.
- Travail nécessitant beaucoup de ressources (UC élevée).
- Travail qui tirerait parti du niveau de tarif (soumis à des pics de charge soudains).

## <a name="reduced-latency"></a>Latence réduite

Les files d’attente sont utiles chaque fois que vous effectuez des tâches fastidieuses. Si une tâche prend quelques secondes ou plus, au lieu de bloquer l’utilisateur final, placez l’élément de travail dans une file d’attente. Dites à l’utilisateur « nous travaillons dessus », puis utilisez un écouteur de file d’attente pour traiter la tâche en arrière-plan.

Par exemple, lorsque vous achetez un produit auprès d’un détaillant en ligne, le site Web confirme votre commande immédiatement. Mais cela ne signifie pas que vos éléments se trouvent déjà dans un camion en cours de livraison. Ils placent une tâche dans une file d’attente, et en arrière-plan, elle procède à la vérification de solvabilité, prépare vos articles pour l’expédition, etc.

Pour les scénarios avec une latence réduite, le temps total de bout en bout peut être plus long avec une file d’attente, par rapport à la tâche de façon synchrone. Mais même les autres avantages peuvent l’emporter sur ce désavantage.

## <a name="increased-reliability"></a>Fiabilité accrue

Dans la version de Fix it que nous avons examiné jusqu’à présent, le serveur Web frontal est étroitement couplé avec le serveur principal SQL Database. Si le service de base de données SQL n’est pas disponible, l’utilisateur reçoit une erreur. Si les nouvelles tentatives ne fonctionnent pas (autrement dit, si l’échec est plus que temporaire), la seule chose que vous pouvez faire est d’afficher une erreur et de demander à l’utilisateur de réessayer ultérieurement.

![Diagramme montrant l’échec du serveur Web frontal en cas d’échec de SQL Database serveur principal](queue-centric-work-pattern/_static/image1.png)

À l’aide de files d’attente, lorsqu’un utilisateur soumet une tâche Fix it, l’application écrit un message dans la file d’attente. La charge utile de message est une représentation [JSON](http://json.org/) de la tâche. Dès que le message est écrit dans la file d’attente, l’application retourne et affiche immédiatement un message de réussite à l’utilisateur.

Si l’un des services principaux (par exemple, la base de données SQL ou l’écouteur de file d’attente) passe en mode hors connexion, les utilisateurs peuvent toujours soumettre de nouvelles tâches de réparation. Les messages sont simplement mis en file d’attente jusqu’à ce que les services principaux soient à nouveau disponibles. À ce stade, les services principaux vont rattraper le Backlog.

![Diagramme montrant que le serveur Web frontal continue à fonctionner en cas d’erreur SQL Database](queue-centric-work-pattern/_static/image2.png)

En outre, vous pouvez désormais ajouter davantage de logique backend sans vous soucier de la résilience du serveur frontal. Par exemple, vous souhaiterez peut-être envoyer un message électronique ou SMS au propriétaire chaque fois qu’un nouveau correctif est attribué. Si l’e-mail ou le service SMS n’est plus disponible, vous pouvez traiter tout le reste, puis placer un message dans une file d’attente distincte pour l’envoi de messages électroniques/SMS.

Auparavant, notre contrat SLA effectif était Web Apps &times; de stockage &times; SQL Database = 99,7%. (Voir [conception pour survivre aux échecs](design-to-survive-failures.md).)

Lorsque nous modifions l’application pour utiliser une file d’attente, le serveur Web frontal dépend uniquement de Web Apps et de stockage, pour un contrat SLA composite de 99,8%. (Notez que les files d’attente font partie du service de stockage Azure, elles sont donc incluses dans le même contrat SLA que le stockage d’objets BLOB.)

Si vous avez besoin de plus de 99,8%, vous pouvez créer deux files d’attente dans deux régions différentes. Désignez-en un comme principal et l’autre comme secondaire. Dans votre application, basculez vers la file d’attente secondaire si la file d’attente principale n’est pas disponible. Le risque d’être indisponible en même temps est très faible.

## <a name="rate-leveling-and-independent-scaling"></a>Nivellement des taux et mise à l’échelle indépendante

Les files d’attente sont également utiles pour un événement appelé audit des *taux* ou nivellement de la *charge*.

Les applications Web sont souvent sujettes à des pics soudains dans le trafic. Bien que vous puissiez utiliser la mise à l’échelle automatique pour ajouter automatiquement des serveurs Web pour gérer l’augmentation du trafic Web, la mise à l’échelle automatique risque de ne pas pouvoir réagir suffisamment rapidement pour gérer les pics brusques de charge. Si les serveurs Web peuvent décharger une partie du travail qu’ils doivent effectuer en écrivant un message dans une file d’attente, ils peuvent gérer plus de trafic. Un service principal peut ensuite lire les messages de la file d’attente et les traiter. La profondeur de la file d’attente augmente ou diminue à mesure que la charge entrante varie.

Avec une grande partie de son travail long à décharger sur un service principal, la couche Web peut répondre plus facilement aux pics soudains du trafic. Et vous économisez de l’argent, car une quantité de trafic donnée peut être gérée par moins de serveurs Web.

Vous pouvez mettre à l’échelle la couche Web et le service principal indépendamment. Par exemple, vous pouvez avoir besoin de trois serveurs Web, mais un seul serveur qui traite les messages de la file d’attente. Ou, si vous exécutez une tâche nécessitant beaucoup de ressources en arrière-plan, vous aurez peut-être besoin de davantage de serveurs principaux.

![](queue-centric-work-pattern/_static/image3.png)

La mise à l’échelle automatique fonctionne avec les services principaux et le niveau Web. Vous pouvez augmenter ou réduire le nombre de machines virtuelles qui traitent les tâches dans la file d’attente, en fonction de l’utilisation du processeur des machines virtuelles principales. Ou bien, vous pouvez mettre à l’échelle automatiquement en fonction du nombre d’éléments qui se trouvent dans une file d’attente. Par exemple, vous pouvez demander à la mise à l’échelle automatique d’essayer de ne pas conserver plus de 10 éléments dans la file d’attente. Si la file d’attente contient plus de 10 éléments, la mise à l’échelle automatique ajoutera des machines virtuelles. Lors de la rattrapage, la mise à l’échelle automatique supprimera les machines virtuelles supplémentaires.

## <a name="adding-queues-to-the-fix-it-application"></a>Ajout de files d’attente à l’application Fix it

Pour implémenter le modèle de file d’attente, nous devons apporter deux modifications à l’application Fix it.

- Lorsqu’un utilisateur soumet une nouvelle tâche Fix it, placez la tâche dans la file d’attente, au lieu de l’écrire dans la base de données.
- Créez un service principal qui traite les messages dans la file d’attente.

Pour la file d’attente, nous allons utiliser le [service de stockage de files d’attente Azure](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Une autre option consiste à utiliser [Azure Service bus](https://docs.microsoft.com/azure/service-bus/).

Pour déterminer le service de file d’attente à utiliser, réfléchissez à la manière dont votre application doit envoyer et recevoir les messages dans la file d’attente :

- Si vous avez des producteurs coopérants et des consommateurs concurrents, envisagez d’utiliser le service de stockage de files d’attente Azure. « Producteurs coopérants » signifie que plusieurs processus ajoutent des messages à une file d’attente. « Consommateurs concurrents » signifie que plusieurs processus extraient des messages de la file d’attente pour les traiter, mais qu’un message donné ne peut être traité que par un « consommateur ». Si vous avez besoin d’un débit supérieur à celui que vous pouvez obtenir avec une seule file d’attente, utilisez des files d’attente supplémentaires et/ou des comptes de stockage supplémentaires.
- Si vous avez besoin d’un [modèle de publication/abonnement](http://en.wikipedia.org/wiki/Publish/subscribe), envisagez d’utiliser Azure Service bus files d’attente.

L’application Fix it correspond aux producteurs coopérants et aux consommateurs concurrents.

Une autre considération concerne la disponibilité des applications. Le service de stockage de file d’attente fait partie du même service que nous utilisons pour le stockage d’objets BLOB, donc son utilisation n’a aucun effet sur notre contrat SLA. Azure Service Bus est un service distinct avec son propre contrat SLA. Si nous avions utilisé Service Bus files d’attente, nous devrions tenir compte d’un pourcentage SLA supplémentaire et notre contrat SLA composite serait plus faible. Lorsque vous choisissez un service de file d’attente, veillez à bien comprendre l’impact de votre choix sur la disponibilité des applications. Pour plus d’informations, consultez la section [ressources](#resources) .

## <a name="creating-queue-messages"></a>Création de messages de file d’attente

Pour placer une tâche Fix it sur la file d’attente, le serveur Web frontal effectue les étapes suivantes :

1. Créez une instance [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) . L’instance de `CloudQueueClient` est utilisée pour exécuter des demandes sur le service de file d’attente.
2. Créez la file d’attente, si elle n’existe pas encore.
3. Sérialisez la tâche Fix it.
4. Appelez [CloudQueue. AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) pour placer le message dans la file d’attente.

Nous allons effectuer ce travail dans le constructeur et la méthode `SendMessageAsync` d’une nouvelle classe `FixItQueueManager`.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Ici, nous utilisons la bibliothèque [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) pour sérialiser le correctif au format JSON. Vous pouvez utiliser l’approche de sérialisation que vous préférez. JSON présente l’avantage de pouvoir être explicite, tout en étant moins détaillé que XML.

Le code de qualité de production ajoute une logique de gestion des erreurs, interrompt si la base de données n’est plus disponible, gère la récupération de manière plus propre, crée la file d’attente sur le démarrage de l’application et gère les[messages « incohérents »](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Un message incohérent est un message qui ne peut pas être traité pour une raison quelconque. Vous ne souhaitez pas que les messages incohérents se trouvent dans la file d’attente, où le rôle de travail essaiera continuellement de les traiter, d’échouer, de réessayer, d’échouer, etc.).

Dans l’application MVC frontale, nous devons mettre à jour le code qui crée une nouvelle tâche. Au lieu de placer la tâche dans le référentiel, appelez la méthode `SendMessageAsync` présentée ci-dessus.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Traitement des messages de la file d’attente

Pour traiter les messages dans la file d’attente, nous allons créer un service principal. Le service principal exécutera une boucle infinie qui effectue les étapes suivantes :

1. Obtient le message suivant de la file d’attente.
2. Désérialisez le message vers une tâche Fix it.
3. Écrivez la tâche Fix it dans la base de données.

Pour héberger le service principal, nous allons créer un service Cloud Azure qui contient un *rôle de travail*. Un rôle de travail se compose d’une ou plusieurs machines virtuelles qui peuvent effectuer le traitement principal. Le code qui s’exécute sur ces machines virtuelles extrait les messages de la file d’attente dès qu’ils sont disponibles. Pour chaque message, nous allons désérialiser la charge utile JSON et écrire une instance de l’entité Fix it Task dans la base de données, en utilisant le même référentiel que celui utilisé précédemment dans la couche Web.

Les étapes suivantes montrent comment ajouter un projet de rôle de travail à une solution qui a un projet Web standard. Ces étapes ont déjà été effectuées dans le projet Fix it que vous pouvez télécharger.

Tout d’abord, ajoutez un projet de service Cloud à la solution Visual Studio. Cliquez avec le bouton droit sur la solution et sélectionnez **Ajouter**, puis **nouveau projet**. Dans le volet gauche, développez **visuel C#**  et sélectionnez **Cloud**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

Dans la boîte de dialogue **nouveau service Cloud Azure** , développez le nœud **visuel C#**  dans le volet gauche. Sélectionnez **rôle de travail** , puis cliquez sur l’icône de flèche droite.

![](queue-centric-work-pattern/_static/image6.png)

(Notez que vous pouvez également ajouter un *rôle Web*. Nous pourrions exécuter le correctif frontal Fix it dans le même service Cloud au lieu de l’exécuter sur un site Web Azure. Cela présente des avantages pour faciliter la coordination des connexions entre les serveurs frontaux et principaux. Toutefois, pour simplifier cette démonstration, nous conservons le frontal dans une application Web Azure App Service et n’exécutons que le serveur principal dans un service Cloud.)

Un nom par défaut est affecté au rôle de travail. Pour modifier le nom, pointez la souris sur le rôle de travail dans le volet droit, puis cliquez sur l’icône en forme de crayon.

![](queue-centric-work-pattern/_static/image7.png)

Cliquez sur **OK** pour terminer la boîte de dialogue. Cela ajoute deux projets à la solution Visual Studio.

- projet Azure qui définit le service Cloud, y compris les informations de configuration.
- Projet de rôle de travail qui définit le rôle de travail.

![](queue-centric-work-pattern/_static/image8.png)

Pour plus d’informations, consultez [création d’un projet Azure avec Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Dans le rôle de travail, nous interrogeons les messages en appelant la méthode `ProcessMessageAsync` de la classe `FixItQueueManager` que nous avons vu précédemment.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

La méthode `ProcessMessagesAsync` vérifie s’il y a un message en attente. S’il en existe un, il désérialise le message dans une entité `FixItTask` et enregistre l’entité dans la base de données. Il effectue une boucle jusqu’à ce que la file d’attente soit vide.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

L’interrogation des messages de la file d’attente entraîne des frais de transaction réduits. par conséquent, lorsqu’il n’y a aucun message en attente de traitement, la méthode `RunAsync` du rôle de travail attend une seconde avant d’interroger à nouveau en appelant `Task.Delay(1000)`.

Dans un projet Web, l’ajout de code asynchrone peut automatiquement améliorer les performances, car IIS gère un pool de threads limité. Ce n’est pas le cas dans un projet de rôle de travail. Pour améliorer l’extensibilité du rôle de travail, vous pouvez écrire du code multithread ou utiliser du code asynchrone pour implémenter la [programmation parallèle](https://msdn.microsoft.com/library/ff963553.aspx). L’exemple n’implémente pas la programmation parallèle, mais montre comment rendre le code asynchrone afin de pouvoir implémenter la programmation parallèle.

## <a name="summary"></a>Récapitulatif

Dans ce chapitre, vous avez vu comment améliorer la réactivité, la fiabilité et l’évolutivité des applications en implémentant le modèle de travail centré sur la file d’attente.

Il s’agit du dernier des 13 modèles couverts dans ce livre électronique, mais il existe bien sûr beaucoup d’autres modèles et pratiques qui peuvent vous aider à créer des applications Cloud efficaces. Le [chapitre final](more-patterns-and-guidance.md) fournit des liens vers des ressources pour les rubriques qui n’ont pas été couvertes dans ces 13 modèles.

<a id="resources"></a>
## <a name="resources"></a>Ressources

Pour plus d’informations sur les files d’attente, consultez les ressources suivantes.

Documentation :

- [Stockage Microsoft Azure files d’attente, partie 1 : prise en main](https://www.red-gate.com/simple-talk/cloud/platform-as-a-service/microsoft-azure-storage-queues-part-1-getting-started/). Article par chiffre romain Schacherl.
- [Exécution de tâches en arrière-plan](https://msdn.microsoft.com/library/ff803365.aspx), chapitre 5 de [déplacement d’applications vers le Cloud, 3ème édition](https://msdn.microsoft.com/library/ff728592.aspx) à partir de Microsoft Patterns and Practices. (En particulier, la section [« utilisation des files d’attente stockage Azure »](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Meilleures pratiques pour optimiser l’évolutivité et la rentabilité des solutions de messagerie basée sur les files d’attente sur Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Livre blanc de Valery Mizonov.
- [Comparaison entre les files d’attente Azure et les files d’attente Service bus](https://msdn.microsoft.com/magazine/jj159884.aspx). L’article MSDN Magazine fournit des informations supplémentaires qui peuvent vous aider à choisir le service de file d’attente à utiliser. L’article indique que Service Bus dépend des services ACS pour l’authentification, ce qui signifie que vos files d’attente SB seraient indisponibles quand ACS n’est pas disponible. Toutefois, depuis l’écriture de l’article, SB a été modifié pour vous permettre d’utiliser des [jetons](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) SAP comme alternative à ACS.
- [Modèles et pratiques Microsoft-conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez initiation à la messagerie asynchrone, modèle de canaux et de filtres, modèle de transaction de compensation, modèle de consommateurs concurrents, modèle CQRS.
- [Passage CQRS](https://msdn.microsoft.com/library/jj554200). Livre électronique sur CQRS par Microsoft Patterns and Practices.

Vidéo :

- [Failsafe : création de services Cloud évolutifs et résilients](https://channel9.msdn.com/Series/FailSafe). Série de vidéos en neuf parties par Ulrich Homann, Marc Mercuri et Mark SIMM. Présente des concepts de haut niveau et des principes architecturaux de manière très accessible et intéressante, avec des histoires tirées de l’expérience de l’équipe de conseil clientèle de Microsoft avec les clients réels. Pour obtenir une présentation du service et des files d’attente Azure Storage, consultez épisode 5 à partir de 35:13.

> [!div class="step-by-step"]
> [Précédent](distributed-caching.md)
> [Suivant](more-patterns-and-guidance.md)
