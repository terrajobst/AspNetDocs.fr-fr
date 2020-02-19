---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Meilleures pratiques pour le développement Web (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: dfd8a3ac2328d3f17dfbe36e68b37d181177b0f4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457087"
---
# <a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Meilleures pratiques pour le développement Web (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Les trois premiers modèles s’intéressent à la configuration d’un processus de développement Agile. les autres sont à propos de l’architecture et du code. Celui-ci est une collection de meilleures pratiques pour le développement Web :

- [Serveurs Web sans état](#stateless) derrière un équilibreur de charge intelligent.
- [Évitez l’état de session](#sessionstate) (ou si vous ne pouvez pas l’éviter, utilisez le cache distribué plutôt qu’une base de données).
- [Utilisez un CDN pour mettre](#cdn) en cache les ressources de fichiers statiques (images, scripts).
- [Utilisez la prise en charge asynchrone de .net 4.5](#async) pour éviter les appels de blocage.

Ces pratiques sont valides pour tout développement Web, pas seulement pour les applications Cloud, mais elles sont particulièrement importantes pour les applications Cloud. Ils fonctionnent ensemble pour vous aider à optimiser l’utilisation de la mise à l’échelle hautement flexible offerte par l’environnement Cloud. Si vous ne suivez pas ces pratiques, vous rencontrerez des limitations lorsque vous essaierez de mettre à l’échelle votre application.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Niveau Web sans état derrière un équilibreur de charge intelligent

Le *niveau Web sans état* signifie que vous ne stockez pas de données d’application dans la mémoire ou le système de fichiers du serveur Web. Le maintien de l’état de votre niveau Web vous permet de bénéficier d’une meilleure expérience client et d’économiser de l’argent :

- Si le niveau Web est sans État et qu’il se trouve derrière un équilibreur de charge, vous pouvez rapidement répondre aux modifications apportées au trafic d’application en ajoutant ou en supprimant dynamiquement des serveurs. Dans l’environnement Cloud dans lequel vous payez uniquement les ressources serveur tant que vous les utilisez réellement, cette capacité à répondre aux changements de demande peut se traduire par des économies considérables.
- Un niveau Web sans état est très simple à mettre à l’échelle de l’application. Cela vous permet également de répondre plus rapidement aux besoins de mise à l’échelle et de consacrer moins d’argent au développement et aux tests dans le processus.
- Les serveurs de Cloud Computing, tels que les serveurs locaux, doivent être corrigés et redémarrés occasionnellement. et si la couche Web est sans État, réacheminez le trafic lorsqu’un serveur tombe temporairement en panne, ne provoquera pas d’erreurs ou un comportement inattendu.

La plupart des applications réelles doivent stocker l’état d’une session Web ; le point principal ici n’est pas de le stocker sur le serveur Web. Vous pouvez stocker l’état d’une autre façon, par exemple sur le client dans les cookies ou hors du serveur de processus, dans l’état de session ASP.NET à l’aide d’un fournisseur de cache. Vous pouvez stocker des fichiers dans le [stockage d’objets BLOB Windows Azure](unstructured-blob-storage.md) plutôt que dans le système de fichiers local.

À titre d’exemple, la facilité de mise à l’échelle d’une application dans les sites Web Windows Azure si votre niveau Web est sans État, consultez l’onglet mettre à l' **échelle** d’un site Web Windows Azure dans le portail de gestion :

![Onglet Mettre à l’échelle](web-development-best-practices/_static/image1.png)

Si vous souhaitez ajouter des serveurs Web, il vous suffit de faire glisser le curseur nombre d’instances vers la droite. Affectez-lui la valeur 5, puis cliquez sur **Enregistrer**. en quelques secondes, vous avez 5 serveurs Web dans Windows Azure qui gèrent le trafic de votre site Web.

![Cinq instances](web-development-best-practices/_static/image2.png)

Vous pouvez tout aussi facilement définir le nombre d’instances sur 3 ou sur 1. Lorsque vous effectuez une mise à l’échelle, vous commencez immédiatement à économiser de l’argent, car Windows Azure facture à la minute, et non à l’heure.

Vous pouvez également indiquer à Windows Azure d’augmenter ou de diminuer automatiquement le nombre de serveurs Web en fonction de l’utilisation du processeur. Dans l’exemple suivant, lorsque l’utilisation du processeur passe au-dessous de 60%, le nombre de serveurs Web passe à un minimum de 2, et si l’utilisation du processeur est supérieure à 80%, le nombre de serveurs Web sera augmenté jusqu’à un maximum de 4.

![Mettre à l’échelle par utilisation de l’UC](web-development-best-practices/_static/image3.png)

Ou que se passe-t-il si vous savez que votre site sera occupé uniquement pendant les heures de travail ? Vous pouvez indiquer à Windows Azure d’exécuter plusieurs serveurs au cours de la journée et de réduire les nuits à un seul serveur, soir, nuits et week-ends. Les séries de captures d’écran suivantes montrent comment configurer le site Web pour qu’il exécute un serveur en dehors des heures de travail de 8 h 00 à 17 h 00.

![Mise à l’échelle par planification](web-development-best-practices/_static/image4.png)

![Définir les heures de planification](web-development-best-practices/_static/image5.png)

![Planification diurne](web-development-best-practices/_static/image6.png)

![Planification weeknight](web-development-best-practices/_static/image7.png)

![Planification du week-end](web-development-best-practices/_static/image8.png)

Et bien évidemment, tout cela peut être fait dans des scripts et dans le portail.

La capacité de votre application à être montée en charge est presque illimitée dans Windows Azure, tant que vous évitez les obstacles à l’ajout ou à la suppression dynamique de machines virtuelles serveur, en conservant l’état du niveau Web.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Éviter l’état de la session

Dans une application cloud réelle, il n’est souvent pas pratique d’éviter de stocker une forme d’état de session utilisateur, mais certaines approches ont davantage d’incidence que d’autres sur les performances et l'extensibilité. Si vous devez stocker un état, la meilleure solution consiste à veiller à ce qu’il reste de petite taille et à le stocker dans des cookies. Si cela n’est pas possible, la meilleure solution consiste à utiliser l’état de session ASP.NET avec un fournisseur pour le [cache en mémoire distribué](distributed-caching.md#sessionstate). La pire solution du point de vue des performances et de l’extensibilité consiste à utiliser un fournisseur d’état de session s'appuyant sur une base de données.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Utiliser un CDN pour mettre en cache les ressources de fichiers statiques

CDN est un acronyme de Content Delivery Network. Vous fournissez des ressources de fichiers statiques, telles que des images et des fichiers de script, à un fournisseur CDN, et le fournisseur met en cache ces fichiers dans les centres de données du monde entier afin que les utilisateurs accèdent à votre application, et qu’ils obtiennent une réponse relativement rapide et une faible latence pour le cache. ressources. Cela permet d’accélérer le temps de chargement global du site et de réduire la charge sur vos serveurs Web. Les CDN sont particulièrement importants si vous atteignez un public largement distribué géographiquement.

Windows Azure dispose d’un CDN et vous pouvez utiliser d’autres CDN dans une application qui s’exécute dans Windows Azure ou dans un environnement d’hébergement Web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Utilisez la prise en charge asynchrone de .NET 4.5 pour éviter les appels de blocage

.NET 4,5 a amélioré C# les langages de programmation et vb afin de simplifier grandement la gestion des tâches de façon asynchrone. L’avantage de la programmation asynchrone n’est pas seulement pour les situations de traitement parallèle, par exemple lorsque vous souhaitez lancer simultanément plusieurs appels de service Web. Cela permet également à votre serveur Web de fonctionner plus efficacement et plus fiable dans des conditions de charge élevée. Un serveur Web n’a qu’un nombre limité de threads disponibles et, dans des conditions de charge élevée, lorsque tous les threads sont en cours d’utilisation, les demandes entrantes doivent attendre que les threads soient libérés. Si votre code d’application ne gère pas les tâches comme les requêtes de base de données et les appels de service Web de façon asynchrone, de nombreux threads sont inutilement liés pendant que le serveur attend une réponse d’e/s. Cela limite la quantité de trafic que le serveur peut gérer dans des conditions de charge élevée. Avec la programmation asynchrone, les threads en attente d’un service Web ou d’une base de données pour retourner des données sont libérés pour traiter les nouvelles demandes jusqu’à ce que les données soient reçues. Dans un serveur Web occupé, des centaines voire des milliers de demandes peuvent ensuite être traitées rapidement, ce qui devrait normalement attendre que les threads soient libérés.

Comme vous l’avez vu précédemment, il est aussi facile de réduire le nombre de serveurs Web qui traitent votre site Web que de les augmenter. Par conséquent, si un serveur peut atteindre un débit supérieur, vous n’en avez pas besoin et vous pouvez réduire vos coûts, car vous avez besoin de moins de serveurs pour un volume de trafic donné que vous ne le feriez autrement.

La prise en charge du modèle de programmation asynchrone .NET 4,5 est incluse dans ASP.NET 4,5 pour Web Forms, MVC et l’API Web. dans Entity Framework 6 et l’API de [stockage Windows Azure](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Prise en charge asynchrone dans ASP.NET 4,5

Dans ASP.NET 4,5, la prise en charge de la programmation asynchrone a été ajoutée non seulement au langage, mais également aux infrastructures MVC, Web Forms et d’API Web. Par exemple, une méthode d’action du contrôleur MVC ASP.NET reçoit les données d’une requête Web et les transmet à une vue qui crée ensuite le code HTML à envoyer au navigateur. La méthode d’action doit souvent obtenir des données d’une base de données ou d’un service Web afin de les afficher dans une page Web ou d’enregistrer des données entrées dans une page Web. Dans ces scénarios, il est facile de rendre la méthode d’action asynchrone : au lieu de retourner un objet *ActionResult* , vous retournez la *tâche&lt;ActionResult&gt;* et marquez la méthode avec le mot clé *Async* . À l’intérieur de la méthode, lorsqu’une ligne de code lance une opération qui implique un temps d’attente, vous la Marquez avec le mot clé await.

Voici une méthode d’action simple qui appelle une méthode de dépôt pour une requête de base de données :

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Et voici la même méthode qui gère l’appel de base de données de façon asynchrone :

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

En coulisses, le compilateur génère le code asynchrone approprié. Lorsque l’application effectue l’appel à `FindTaskByIdAsync`, ASP.NET effectue la demande de `FindTask`, puis déroule le thread de travail et le rend disponible pour traiter une autre requête. Lorsque la demande de `FindTask` est effectuée, un thread est redémarré pour poursuivre le traitement du code qui vient après cet appel. Pendant l’intervalle de temps entre le moment où la demande de `FindTask` est initiée et le moment où les données sont retournées, vous disposez d’un thread disponible pour effectuer des tâches utiles qui, sinon, seraient bloquées en attente de la réponse.

Il existe une surcharge pour le code asynchrone, mais dans des conditions de faible charge, cette surcharge est négligeable, tandis que dans les conditions de charge élevée, vous êtes en mesure de traiter les demandes qui, sinon, sont retenues en attente de threads disponibles.

Il a été possible de réaliser ce type de programmation asynchrone depuis ASP.NET 1,1, mais il était difficile d’écrire, de sujette aux erreurs et de déboguer. Maintenant que nous avons simplifié le codage pour IT dans ASP.NET 4,5, il n’y a aucune raison de ne pas le faire.

### <a name="async-support-in-entity-framework-6"></a>Prise en charge d’Async dans Entity Framework 6

Dans le cadre de la prise en charge de Async dans 4,5, nous avons fourni la prise en charge asynchrone des appels de service Web, des sockets et des e/s de système de fichiers, mais le modèle le plus courant pour les applications Web est de toucher une base de données, et nos bibliothèques de données ne prenaient pas en charge Async. À présent Entity Framework 6 ajoute la prise en charge asynchrone de l’accès aux bases de données.

Dans Entity Framework 6, toutes les méthodes qui provoquent l’envoi d’une requête ou d’une commande à la base de données ont des versions Async. L’exemple ci-dessous montre la version asynchrone de la méthode *Find* .

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Et cette prise en charge asynchrone n’est pas seulement pour les insertions, les suppressions, les mises à jour et les recherches simples. elle fonctionne également avec les requêtes LINQ :

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Il existe une version `Async` de la méthode `ToList`, car, dans ce code, il s’agit de la méthode qui entraîne l’envoi d’une requête à la base de données. Les méthodes `Where` et `OrderByDescending` configurent uniquement la requête, tandis que la méthode `ToListAsync` exécute la requête et stocke la réponse dans la variable `result`.

## <a name="summary"></a>Résumé

Vous pouvez implémenter les meilleures pratiques de développement Web décrites ici dans n’importe quelle infrastructure de programmation Web et n’importe quel environnement Cloud, mais nous avons des outils dans ASP.NET et Windows Azure pour faciliter la tâche. Si vous suivez ces modèles, vous pouvez facilement augmenter la capacité de votre niveau Web et réduire vos dépenses, car chaque serveur sera en mesure de gérer davantage de trafic.

Le [chapitre suivant](single-sign-on.md) présente la manière dont le Cloud active les scénarios d’authentification unique.

## <a name="resources"></a>Ressources

Pour plus d’informations, consultez les ressources suivantes.

Serveurs Web sans État :

- [Microsoft Patterns and Practices-Guide de mise à l’échelle](https://msdn.microsoft.com/library/dn589774.aspx)automatique.
- [Désactivation de l’affinité d’instance ARR dans les sites Web Windows Azure](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Le billet de blog de Erez benari, explique l’affinité de session dans les sites Web Windows Azure.

CDN

- [Failsafe : création de services Cloud évolutifs et résilients](https://channel9.msdn.com/Series/FailSafe). Série de vidéos en neuf parties par Ulrich Homann, Marc Mercuri et Mark SIMM. Consultez la discussion sur le CDN dans l’épisode 3 à partir de 1:34:00.
- [Modèle d’hébergement de contenu statique Microsoft Patterns and Practices](https://msdn.microsoft.com/library/dn589776.aspx)
- [Revues de CDN](http://www.cdnreviews.com/). Vue d’ensemble de nombreux CDN.

Programmation asynchrone :

- [Utilisation de méthodes asynchrones dans ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Didacticiel de Rick Anderson.
- [Programmation asynchrone avec Async et await (C# et Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). Livre blanc MSDN qui explique le raisonnement pour la programmation asynchrone, comment il fonctionne dans ASP.NET 4,5 et comment écrire du code pour l’implémenter.
- [Entity Framework requête asynchrone et enregistrer](https://msdn.microsoft.com/data/jj819165)
- [Comment créer des applications Web ASP.net à l’aide de Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Présentation vidéo de Rowan Miller. Contient une démonstration graphique de la façon dont la programmation asynchrone peut faciliter des augmentations spectaculaires du débit du serveur Web dans des conditions de charge élevée.
- [Failsafe : création de services Cloud évolutifs et résilients](https://channel9.msdn.com/Series/FailSafe). Série de vidéos en neuf parties par Ulrich Homann, Marc Mercuri et Mark SIMM. Pour connaître les discussions sur l’impact de la programmation asynchrone sur l’évolutivité, consultez épisode 4 et épisode 8.
- [La magie de l’utilisation de méthodes asynchrones dans ASP.NET 4,5 plus un piège important](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Billet de blog de Scott Hanselman, principalement sur l’utilisation d’Async dans des applications ASP.NET Web Forms.

Pour plus d’informations sur les meilleures pratiques en matière de développement Web, consultez les ressources suivantes :

- [Exemple d’application Fix it-meilleures pratiques](the-fix-it-sample-application.md#bestpractices). L’annexe de ce livre électronique répertorie un certain nombre de meilleures pratiques qui ont été implémentées dans l’application Fix it.
- [Liste de vérification des développeurs Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Précédent](continuous-integration-and-continuous-delivery.md)
> [Suivant](single-sign-on.md)
