---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Surveillance et télémétrie (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f61810ea7b486b2fa0bbb234edea7541eedde835
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583145"
---
# <a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Surveillance et télémétrie (génération d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Un grand nombre de personnes s’appuient sur les clients pour leur faire savoir quand leur application est défaillante. Ce n’est pas vraiment une pratique recommandée, et surtout pas dans le Cloud. Il n’existe aucune garantie de notification rapide et, lorsque vous êtes averti, vous recevez souvent des données minimales ou trompeuses sur ce qui s’est passé. Avec de bons systèmes de télémétrie et de journalisation, vous pouvez être conscient de ce qui se passe avec votre application, et en cas de problème, vous êtes immédiatement informé et vous avez des informations de dépannage utiles à utiliser.

## <a name="buy-or-rent-a-telemetry-solution"></a>Acheter ou louer une solution de télémétrie

> [!NOTE]
> Cet article a été écrit avant la sortie de [application Insights](/azure/application-insights/app-insights-overview) . Application Insights est l’approche préférée pour les solutions de télémétrie sur Azure. Pour plus d’informations, consultez [configurer application Insights pour votre site web ASP.net](/azure/application-insights/app-insights-asp-net) .

L’un des aspects importants de l’environnement Cloud est qu’il est vraiment facile d’acheter ou de louer votre façon de vous faire une victoire. La télémétrie est un exemple. Sans trop d’efforts, vous pouvez obtenir un système de télémétrie vraiment efficace, très rentable. De nombreux partenaires s’intègrent à Azure et certains ont des niveaux gratuits, ce qui vous permet d’obtenir des données de télémétrie de base pour rien. Voici quelques-uns des éléments actuellement disponibles sur Azure :

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [DynaTrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) comprend également des fonctionnalités d’analyse.

Nous allons rapidement passer en revue la configuration de nouvelles Relic pour montrer à quel degré il est facile d’utiliser un système de télémétrie.

Dans le portail de gestion Azure, inscrivez-vous au service. Cliquez sur **nouveau**, puis sur **Store**. La boîte de dialogue **choisir un module complémentaire** s’affiche. Faites défiler la liste et cliquez sur **New Relic**.

![Choisir un module complémentaire](monitoring-and-telemetry/_static/image1.png)

Cliquez sur la flèche droite et choisissez le niveau de service souhaité. Pour cette démonstration, nous allons utiliser le niveau gratuit.

![Personnaliser le module complémentaire](monitoring-and-telemetry/_static/image2.png)

Cliquez sur la flèche droite, confirmez « Purchase », et New Relic s’affiche maintenant comme module complémentaire dans le portail.

![Passer en revue l’achat](monitoring-and-telemetry/_static/image3.png)

![Nouveau module complémentaire Relic dans le portail de gestion](monitoring-and-telemetry/_static/image4.png)

Cliquez sur **informations de connexion**, puis copiez la clé de licence.

![Informations de connexion](monitoring-and-telemetry/_static/image5.png)

Accédez à l’onglet **configurer** de votre application Web dans le portail, définissez **analyse des performances** sur **complémentaire**, puis définissez la liste déroulante **choisir un module** complémentaire sur **nouveau Relic**. Ensuite, cliquez sur **Enregistrer**.

![Nouveau Relic dans l’onglet configurer](monitoring-and-telemetry/_static/image6.png)

Dans Visual Studio, installez le nouveau package NuGet Relic dans votre application.

![Analyse des développeurs dans l’onglet configurer](monitoring-and-telemetry/_static/image7.png)

Déployez l’application sur Azure et commencez à l’utiliser. Créez quelques tâches Fix it pour fournir une activité pour les nouveaux Relic à surveiller.

Revenez ensuite à la page **nouveau Relic** sous l’onglet **modules complémentaires** du portail, puis cliquez sur **gérer**. Le portail vous envoie le nouveau portail de gestion Relic à l’aide de l’authentification unique pour l’authentification, ce qui vous évite de devoir entrer à nouveau vos informations d’identification. La page vue d’ensemble présente diverses statistiques de performances. (Cliquez sur l’image pour afficher la taille complète de la page de vue d’ensemble.)

[![nouvel onglet d’analyse Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Voici quelques-unes des statistiques que vous pouvez voir :

- Temps de réponse moyen à différents moments de la journée.

    ![Temps de réponse](monitoring-and-telemetry/_static/image10.png)
- Taux de débit (en demandes par minute) à différents moments de la journée.

    ![Débit](monitoring-and-telemetry/_static/image11.png)
- Temps processeur du serveur passé à gérer différentes requêtes HTTP.

    ![Temps de transaction Web](monitoring-and-telemetry/_static/image12.png)
- Temps processeur passé dans les différentes parties du code d’application :

    ![Détails du suivi](monitoring-and-telemetry/_static/image13.png)
- Statistiques de performances historiques.

    ![Performances historiques](monitoring-and-telemetry/_static/image14.png)
- Appels à des services externes tels que le service BLOB et statistiques sur la fiabilité et la réactivité du service.

    ![Services externes](monitoring-and-telemetry/_static/image15.png)

    ![Services externes](monitoring-and-telemetry/_static/image16.png)

    ![Service externe](monitoring-and-telemetry/_static/image17.png)
- Des informations sur l’origine du trafic de l’application Web américaine.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

Vous pouvez également configurer des rapports et des événements. Par exemple, vous pouvez indiquer chaque fois que vous commencez à voir des erreurs, envoyer un e-mail au personnel de support technique d’alerte pour le problème.

![Rapports](monitoring-and-telemetry/_static/image19.png)

New Relic n’est qu’un exemple de système de télémétrie ; vous pouvez également vous en procurer tous à partir d’autres services. La beauté du Cloud est que, sans avoir à écrire de code, et pour des dépenses minimes ou inexistantes, vous pouvez soudain obtenir beaucoup plus d’informations sur la façon dont votre application est utilisée et sur ce que vos clients rencontrent réellement.

<a id="log"></a>
## <a name="log-for-insight"></a>Journal pour Insight

Un package de télémétrie est une bonne première étape, mais vous devez tout de même instrumenter votre propre code. Le service de télémétrie vous indique à quel moment il y a un problème et vous indique ce que les clients rencontrent, mais il peut ne pas vous donner une idée importante de ce qui se passe dans votre code.

Vous ne souhaitez pas avoir à accéder à distance à un serveur de production pour voir ce que fait votre application. Cela peut être pratique lorsque vous avez un serveur, mais qu’en est-il lorsque vous avez mis à l’échelle des centaines de serveurs et que vous ne savez pas à quoi vous devez vous mettre à distance ? Votre journalisation doit fournir suffisamment d’informations pour que vous n’ayez jamais à vous connecter à distance à des serveurs de production pour analyser et déboguer des problèmes. Vous devez consigner suffisamment d’informations pour pouvoir isoler les problèmes uniquement par le biais des journaux.

### <a name="log-in-production"></a>Journalisation en production

De nombreuses personnes activent le suivi en production uniquement lorsqu’il y a un problème et qu’ils veulent déboguer. Cela peut entraîner un délai considérable entre le moment où vous avez pris connaissance d’un problème et le moment où vous recevez des informations de dépannage utiles à son sujet. Et les informations que vous recevez peuvent ne pas être utiles pour les erreurs intermittentes.

Ce que nous recommandons dans l’environnement Cloud où le stockage est peu coûteux, c’est que vous laissez toujours la journalisation en production. Ainsi, lorsque des erreurs se produisent, vous les avez déjà journalisées et vous avez des données historiques qui peuvent vous aider à analyser les problèmes qui se développent au fil du temps ou régulièrement à des moments différents. Vous pouvez automatiser un processus de vidage pour supprimer les anciens journaux, mais il est possible que la configuration d’un tel processus soit plus coûteuse que la conservation des journaux.

Les dépenses supplémentaires de journalisation sont très simples par rapport à la durée de résolution des problèmes et à l’argent que vous pouvez économiser en faisant en sorte que toutes les informations dont vous avez besoin soient déjà disponibles en cas de problème. Ensuite, lorsqu’une personne vous indique qu’elle a rencontré une erreur aléatoire à environ 8:00 la dernière nuit, mais qu’elle ne se souvient pas de l’erreur, vous pouvez facilement identifier le problème.

Pendant moins de $4 mois, vous pouvez conserver 50 gigaoctets de journaux, et l’impact sur les performances de la journalisation est trivial tant que vous gardez une chose à l’esprit : afin d’éviter les goulots d’étranglement des performances, assurez-vous que votre bibliothèque de journalisation est asynchrone.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Différencier les journaux qui informent des journaux qui nécessitent une action

Les journaux sont censés vous informer (je souhaite que vous sachiez quoi que ce soit) ou agir (je veux faire une opération). Veillez à écrire uniquement les journaux ACT pour les problèmes qui requièrent véritablement une personne ou un processus automatisé pour agir. Un trop grand nombre de journaux ACT créeront du bruit, nécessitant trop de travail pour les examiner tous afin de trouver des problèmes authentiques. Et si vos journaux ACT déclenchent automatiquement certaines actions, telles que l’envoi d’e-mails au personnel de support, évitez de laisser des milliers de telles actions être déclenchées par un seul problème.

Dans le suivi System. Diagnostics .NET, les journaux peuvent recevoir des niveaux d’erreur, d’avertissement, d’informations et de débogage/détaillé. Vous pouvez différencier les journaux d’information en réservant le niveau d’erreur pour les journaux ACT et en utilisant les niveaux inférieurs pour informer les journaux.

![Niveaux de journalisation](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurer les niveaux de journalisation au moment de l’exécution

Bien qu’il soit intéressant d’avoir la journalisation Always on en production, une autre meilleure pratique consiste à implémenter une infrastructure de journalisation qui vous permet d’ajuster au moment de l’exécution le niveau de détail que vous journalisez, sans redéployer ou redémarrer votre application. Par exemple, lorsque vous utilisez la fonctionnalité de suivi dans `System.Diagnostics` vous pouvez créer des journaux d’erreur, d’avertissement, d’informations et de débogage/commentaires. Nous vous recommandons de toujours enregistrer les journaux des erreurs, des avertissements et des informations en production, et vous pouvez ajouter dynamiquement la journalisation de débogage/commentaires pour la résolution des problèmes au cas par cas.

Web Apps dans Azure App Service disposez d’une prise en charge intégrée pour écrire des journaux `System.Diagnostics` dans le système de fichiers, le stockage table ou le stockage BLOB. Vous pouvez sélectionner différents niveaux de journalisation pour chaque destination de stockage, et vous pouvez modifier le niveau de journalisation à la volée sans redémarrer votre application. La prise en charge du stockage d’objets BLOB facilite l’exécution de travaux d’analyse [HDInsight](https://docs.microsoft.com/azure/hdinsight/) sur vos journaux d’application, car HDInsight sait comment travailler directement avec le stockage d’objets BLOB.

### <a name="log-exceptions"></a>journaliser des exceptions

Ne vous contentez pas de placer une *exception. ToString ()* dans votre code de journalisation. Qui laisse des informations contextuelles. Dans le cas d’erreurs SQL, il ignore le numéro d’erreur SQL. Pour toutes les exceptions, incluez les informations de contexte, l’exception elle-même et les exceptions internes pour vous assurer que vous fournissez tout ce qui sera nécessaire pour le dépannage. Par exemple, les informations de contexte peuvent inclure le nom du serveur, un identificateur de transaction et un nom d’utilisateur (mais pas le mot de passe ou un secret).

Si vous vous fiez à chaque développeur pour effectuer la bonne chose avec la journalisation des exceptions, certaines d’entre elles ne le sont pas. Pour vous assurer qu’il est correctement effectué à chaque fois, générez la gestion des exceptions directement dans votre interface d’enregistreur d’événements : transmettez l’objet exception lui-même à la classe Logger et Journalisez les données d’exception correctement dans la classe Logger.

### <a name="log-calls-to-services"></a>Consigner les appels aux services

Nous vous recommandons vivement d’écrire un journal chaque fois que votre application appelle un service, qu’il s’agisse d’une base de données ou d’une API REST ou d’un service externe. Incluez dans vos journaux non seulement une indication de réussite ou d’échec, mais aussi la durée de chaque demande. Dans l’environnement Cloud, vous rencontrerez souvent des problèmes liés à des ralentissements et non à des interruptions complètes. Une opération qui prend normalement 10 millisecondes peut commencer soudainement à prendre une seconde. Quand quelqu’un vous indique que votre application est lente, vous souhaitez être en mesure de regarder de nouveaux Relic ou de tout service de télémétrie dont vous disposez et de valider leur expérience, et vous souhaitez que vous puissiez regarder vos propres journaux pour vous plonger dans les détails de la raison pour laquelle il est lent.

### <a name="use-an-ilogger-interface"></a>Utiliser une interface ILogger

Ce que nous vous recommandons de faire lorsque vous créez une application de production consiste à créer une interface *ILogger* simple et à coller certaines méthodes dans celle-ci. Cela facilite la modification ultérieure de l’implémentation de journalisation et n’a pas besoin de parcourir l’ensemble de votre code pour le faire. Nous pourrions utiliser la classe `System.Diagnostics.Trace` tout au long de l’application Fix it, mais à la place, nous l’utilisons en coulisses dans une classe de journalisation qui implémente *ILogger*, et nous effectuons des appels de méthode *ILogger* dans toute l’application.

De cette façon, si vous souhaitez que votre journalisation soit plus riche, vous pouvez remplacer [`System.Diagnostics.Trace`](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) par le mécanisme de journalisation de votre choix. Par exemple, à mesure que votre application se développe, vous pouvez décider que vous souhaitez utiliser un package de journalisation plus complet, tel que [nlog](http://nlog-project.org/) ou le [bloc d’application de journalisation Enterprise Library](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4net](http://logging.apache.org/log4net/) est un autre Framework de journalisation courant, mais il n’effectue pas de journalisation asynchrone.)

L’une des raisons possibles de l’utilisation d’une infrastructure telle que NLog consiste à faciliter la Division de la sortie de journalisation en plusieurs magasins de données volumineux et de grande valeur. Cela vous permet de stocker efficacement des volumes importants de données informant que vous n’avez pas besoin d’exécuter des requêtes rapides sur, tout en conservant un accès rapide aux données ACT.

### <a name="semantic-logging"></a>Journalisation sémantique

Pour une façon relativement nouvelle d’effectuer une journalisation qui peut produire des informations de diagnostic plus utiles, consultez [bloc d’application de journalisation sémantique d’Enterprise Library (dalle)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). La dalle utilise la prise en charge des [suivi d’v nements pour Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) et [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) dans .net 4,5 pour vous permettre de créer des journaux plus structurés et interrogeables. Vous définissez une méthode différente pour chaque type d’événement que vous journalisez, ce qui vous permet de personnaliser les informations que vous écrivez. Par exemple, pour consigner une erreur SQL Database vous pouvez appeler une méthode `LogSQLDatabaseError`. Pour ce type d’exception, vous savez qu’une information clé est le numéro de l’erreur. vous pouvez donc inclure un paramètre de numéro d’erreur dans la signature de la méthode et enregistrer le numéro d’erreur sous la forme d’un champ distinct dans l’enregistrement du journal que vous écrivez. Étant donné que le nombre se trouve dans un champ séparé, vous pouvez obtenir plus facilement et de manière fiable des rapports basés sur des numéros d’erreur SQL que vous ne pouviez pas si vous concaténez simplement le numéro d’erreur dans une chaîne de message.

## <a name="logging-in-the-fix-it-app"></a>Journalisation dans l’application Fix it

### <a name="the-ilogger-interface"></a>Interface ILogger

Voici l’interface *ILogger* dans l’application Fix it.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Ces méthodes vous permettent d’écrire des journaux aux quatre mêmes niveaux pris en charge par *System. Diagnostics*. Les méthodes TraceApi sont destinées à la journalisation des appels de service externes avec des informations sur la latence. Vous pouvez également ajouter un ensemble de méthodes pour le niveau de débogage/détail.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Implémentation de l’enregistreur d’événements de l’interface ILogger

L’implémentation de l’interface est très simple. En fait, il appelle simplement les méthodes *System. Diagnostics* standard. L’extrait de code suivant montre les trois méthodes d’informations et l’autre les unes des autres.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Appel des méthodes ILogger

Chaque fois que le code de l’application Fix it intercepte une exception, il appelle une méthode *ILogger* pour consigner les détails de l’exception. Et chaque fois qu’il effectue un appel vers la base de données, le service BLOB ou une API REST, il démarre un chronomètre avant l’appel, arrête le chronomètre lorsque le service retourne et enregistre le temps écoulé, ainsi que les informations sur la réussite ou l’échec.

Notez que le message du journal comprend le nom de la classe et le nom de la méthode. Il est recommandé de s’assurer que les messages du journal identifient la partie du code d’application qui les a écrits.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Donc maintenant, pour chaque fois que l’application Fix it a effectué un appel à SQL Database, vous pouvez voir l’appel, la méthode qui l’a appelé et exactement le temps qu’il a pris.

![SQL Database requête dans les journaux](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Si vous parcourez les journaux, vous pouvez voir que la durée des appels de base de données prend est variable. Ces informations peuvent être utiles : étant donné que l’application journalise tous ces éléments, vous pouvez analyser les tendances historiques en ce qui concerne le fonctionnement du service de base de données au fil du temps. Par exemple, un service peut être le plus souvent rapide, mais les demandes peuvent échouer ou les réponses peuvent ralentir à certaines heures de la journée.

Vous pouvez effectuer la même opération pour le service BLOB : chaque fois que l’application charge un nouveau fichier, il y a un journal, et vous pouvez voir exactement combien de temps il a fallu pour charger chaque fichier.

![Journal de chargement d’objets BLOB](monitoring-and-telemetry/_static/image23.png)

Il s’agit simplement de quelques lignes de code supplémentaires à écrire chaque fois que vous appelez un service. à présent, chaque fois que quelqu’un dit qu’il a rencontré un problème, vous savez exactement ce qu’est le problème, s’il s’agit d’une erreur, ou même s’il s’est exécuté lentement. Vous pouvez identifier la source du problème sans avoir à vous connecter à distance à un serveur ou à activer la journalisation une fois que l’erreur se produit et à l’espoir de la recréer.

## <a name="dependency-injection-in-the-fix-it-app"></a>Injection de dépendances dans l’application Fix it

Vous vous demandez peut-être comment le constructeur du référentiel dans l’exemple ci-dessus obtient l’implémentation de l’interface de l’enregistreur d’événements :

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Pour associer l’interface à l’implémentation, l’application utilise l' [injection de dépendances](http://en.wikipedia.org/wiki/Dependency_injection)(di) avec [AutoFac](http://autofac.org/). L’injection de code vous permet d’utiliser un objet basé sur une interface à de nombreux emplacements de votre code, et vous devez simplement spécifier à un emplacement l’implémentation utilisée lorsque l’interface est instanciée. Cela facilite la modification de l’implémentation : par exemple, vous souhaiterez peut-être remplacer l’enregistreur d’événements System. Diagnostics par un enregistreur d’événements NLog. Ou pour les tests automatisés, vous pouvez substituer une version factice de l’enregistreur d’événements.

L’application Fix It utilise DI dans tous les référentiels et tous les contrôleurs. Les constructeurs des classes de contrôleur obtiennent une interface *ITaskRepository* de la même façon que le référentiel obtient une interface de l’enregistreur d’événements :

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

L’application utilise la bibliothèque AutoFac DI pour fournir automatiquement des instances *TaskRepository* et *logger* pour ces constructeurs.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Ce code indique en fait que partout où un constructeur a besoin d’une interface *ILogger* , passe une instance de la classe *logger* et, chaque fois qu’il a besoin d’une interface *IFixItTaskRepository* , passe une instance de la classe *FixItTaskRepository* .

[AutoFac](http://autofac.org/) est l’une des nombreuses infrastructures d’injection de dépendances que vous pouvez utiliser. [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), qui est recommandé et pris en charge par Microsoft Patterns and Practices, est un autre couramment utilisé.

## <a name="built-in-logging-support-in-azure"></a>Prise en charge intégrée de la journalisation dans Azure

Azure prend en charge les types de [journalisation suivants pour Web Apps dans Azure App service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System. Diagnostics Tracing (vous pouvez activer et désactiver et définir des niveaux à la volée sans redémarrer le site).
- Événements Windows.
- Journaux IIS (HTTP/FREB).

Azure prend en charge les types suivants de [journalisation dans les services Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Suivi System. Diagnostics.
- Compteurs de performance.
- Événements Windows.
- Journaux IIS (HTTP/FREB).
- Analyse de répertoire personnalisée.

L’application Fix It utilise le suivi System. Diagnostics. Pour activer la journalisation de System. Diagnostics dans une application Web, il vous suffit de basculer un commutateur dans le portail ou d’appeler l’API REST. Dans le portail, cliquez sur l’onglet **configuration** de votre site, puis faites défiler la page pour voir la section **diagnostic d’application** . Vous pouvez activer ou désactiver la journalisation et sélectionner le niveau de journalisation de votre choix. Vous pouvez demander à Azure d’écrire les journaux dans le système de fichiers ou dans un compte de stockage.

![Diagnostics d’application et diagnostics de site dans l’onglet configurer](monitoring-and-telemetry/_static/image24.png)

Après avoir activé la journalisation dans Azure, vous pouvez voir les journaux dans la fenêtre sortie de Visual Studio au fur et à mesure de leur création.

![Menu journaux de diffusion en continu](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu journaux de diffusion en continu](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Vous pouvez également avoir des journaux écrits dans votre compte de stockage et les afficher avec n’importe quel outil pouvant accéder au service de table de stockage Azure, par exemple **Explorateur de serveurs** dans Visual Studio ou [Explorateur stockage Azure](https://azure.microsoft.com/features/storage-explorer/).

![Journaux dans Explorateur de serveurs](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Récapitulatif

Il est très simple d’implémenter un système de télémétrie prêt à l’emploi, d’enregistrer la journalisation dans votre propre code et de configurer la journalisation dans Azure. Et lorsque vous rencontrez des problèmes de production, la combinaison d’un système de télémétrie et de journaux personnalisés vous aidera à résoudre rapidement les problèmes avant qu’ils ne deviennent des problèmes majeurs pour vos clients.

Dans le [chapitre suivant](transient-fault-handling.md) , nous allons examiner comment gérer les erreurs temporaires afin qu’elles ne deviennent pas des problèmes de production que vous devez examiner.

## <a name="resources"></a>Ressources

Pour plus d'informations, voir les ressources ci-dessous.

Documentation principalement sur la télémétrie :

- [Modèles et pratiques Microsoft-conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez Guide d’instrumentation et de télémétrie, conseils sur le contrôle de service, modèle de surveillance de point de terminaison d’intégrité et modèle de reconfiguration du Runtime.
- [Un pincement minime dans le Cloud : activation de la surveillance des performances de nouvelle Relic sur les sites Web Azure](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Meilleures pratiques pour la conception de services à grande échelle sur les services Cloud Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Livre blanc en Mark SIMM et Michael thomassy. Consultez la section télémétrie et Diagnostics.
- [Développement de nouvelle génération avec application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Article de MSDN Magazine.

Documentation principalement sur la journalisation :

- [Bloc d’application de journalisation sémantique (dalle)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie présente le cas de la journalisation sémantique avec la dalle.
- [Création de journaux structurés et significatifs avec la journalisation sémantique](https://channel9.msdn.com/Events/Build/2013/3-336). Vidéosurveillance Julien Dominguez présente le cas de la journalisation sémantique avec la dalle.
- [EF6 journalisation SQL – Partie 1 : journalisation simple](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers montre comment enregistrer des requêtes exécutées par Entity Framework dans EF 6.
- [Résilience des connexions et interception des commandes avec l’Entity Framework dans une application MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quatrième dans une série de didacticiels en neuf parties, montre comment utiliser la fonctionnalité d’interception de commande EF 6 pour consigner les commandes SQL envoyées à la base de données par Entity Framework.
- [Améliorez la C# journalisation à l’aide des attributs d’informations de l’appelant 5,0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Comment consigner facilement le nom de la méthode d’appel sans le coder en dur dans des littéraux ou utiliser la réflexion pour l’extraire manuellement.

Documentation principalement sur la résolution des problèmes :

- [Blog sur le dépannage d’Azure &amp; le débogage](https://blogs.msdn.com/b/kwill/).
- [AzureTools : l’utilitaire de diagnostic utilisé par l’équipe de Developer support Azure](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Présente et fournit un lien de téléchargement pour un outil qui peut être utilisé sur une machine virtuelle Azure pour télécharger et exécuter un large éventail d’outils de diagnostic et de surveillance. Utile lorsque vous devez diagnostiquer un problème sur une machine virtuelle particulière.
- [Dépanner une application Web dans Azure App service à l’aide de Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Didacticiel pas à pas pour bien démarrer avec le suivi System. Diagnostics et le débogage à distance.

Vidéos :

- [Failsafe : création de services Cloud évolutifs et résilients](https://channel9.msdn.com/Series/FailSafe). Série en neuf parties de Ulrich Homann, Marc Mercuri et Mark SIMM. Présente des concepts de haut niveau et des principes architecturaux de manière très accessible et intéressante, avec des histoires tirées de l’expérience de l’équipe de conseil clientèle de Microsoft avec les clients réels. Les épisodes 4 et 9 concernent la surveillance et la télémétrie. L’épisode 9 comprend une vue d’ensemble des services de surveillance disponibles : metricshub, AppDynamics, New Relic et PagerDuty.
- [Création de Big : leçons apprises par les clients Azure-partie II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark SIMM parle de la conception en cas de défaillance et de l’instrumentation de tout. Semblable à la série Failsafe, mais présente des détails supplémentaires.

Exemple de code :

- [Notions de base du service Cloud dans Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application créé par l’équipe de conseil clientèle Microsoft Azure. Illustre les pratiques de télémétrie et de journalisation, comme expliqué dans les articles suivants. L’exemple implémente la journalisation des applications à l’aide de [nlog](http://nlog-project.org/). Pour obtenir une documentation connexe, consultez la [série de quatre articles wiki TechNet sur la télémétrie et la journalisation](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Précédent](design-to-survive-failures.md)
> [Suivant](transient-fault-handling.md)
