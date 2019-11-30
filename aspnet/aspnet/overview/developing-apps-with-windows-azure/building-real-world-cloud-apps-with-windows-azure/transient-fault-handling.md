---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Gestion des erreurs temporaires (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: fc281e3d8f7c9edd4d98b029a67e58113132a8b3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583657"
---
# <a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Gestion des erreurs temporaires (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Quand vous concevez une application Cloud réelle, l’un des points à prendre en compte est la gestion des interruptions de service temporaires. Ce problème est particulièrement important dans les applications Cloud, car vous êtes dépendant des connexions réseau et des services externes. Vous pouvez souvent obtenir de rares problèmes qui sont généralement à la réparation spontanée, et si vous n’êtes pas prêt à les gérer intelligemment, ils entraîneront une mauvaise expérience de vos clients.

## <a name="causes-of-transient-failures"></a>Causes des échecs temporaires

Dans l’environnement Cloud, vous constaterez que des connexions de base de données ont échoué et ont lieu périodiquement. Cela est en partie dû au fait que vous allez passer par un plus grand nombre d’équilibreurs de charge par rapport à l’environnement local dans lequel votre serveur Web et votre serveur de base de données disposent d’une connexion physique directe. Par ailleurs, parfois, lorsque vous êtes dépendant d’un service mutualisée, vous verrez que les appels au service sont plus lents ou expirent, car une autre personne qui utilise le service le fait de manière intensive. Dans d’autres cas, vous êtes peut-être l’utilisateur qui atteint le service trop fréquemment et le service vous limite délibérément : refuse les connexions, afin de vous empêcher d’affecter négativement d’autres locataires du service.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Utiliser une logique de nouvelle tentative intelligente/d’interruption pour atténuer l’effet des échecs temporaires

Au lieu de lever une exception et d’afficher une page non disponible ou d’erreur à votre client, vous pouvez identifier les erreurs qui sont généralement temporaires et retenter automatiquement l’opération qui a provoqué l’erreur, dans l’espoir, avant que vous ne puissiez réussir. La plupart du temps, l’opération réussira à la deuxième tentative, et vous récupérerez de l’erreur sans que le client ait été conscient qu’un problème est survenu.

Il existe plusieurs façons d’implémenter une logique de nouvelle tentative intelligente.

- Le groupe Microsoft Patterns &amp; Practices a un [bloc d’application de gestion des erreurs temporaires](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) qui effectue tout pour vous si vous utilisez ADO.net pour l’accès SQL Database (et non par le biais de Entity Framework). Il vous suffit de définir une stratégie pour les nouvelles tentatives : le nombre de nouvelles tentatives d’exécution d’une requête ou d’une commande et la durée d’attente entre les tentatives, et encapsuler votre code SQL dans un bloc *using* .

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH prend également en charge [Azure in-Role cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) et [service bus](https://azure.microsoft.com/services/service-bus/).
- Lorsque vous utilisez la Entity Framework vous ne travaillez pas directement directement avec les connexions SQL, vous ne pouvez pas utiliser ce package de modèles et de pratiques, mais Entity Framework 6 génère ce type de logique de nouvelle tentative dans l’infrastructure. De la même façon, vous spécifiez la stratégie de nouvelle tentative, puis EF utilise cette stratégie chaque fois qu’il accède à la base de données.

    Pour utiliser cette fonctionnalité dans l’application Fix it, il vous suffit d’ajouter une classe qui dérive de *DbConfiguration* et d’activer la logique de nouvelle tentative.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Pour SQL Database exceptions que le Framework identifie en général comme des erreurs temporaires, le code indiqué indique à EF de retenter l’opération jusqu’à 3 fois, avec un délai d’attente exponentiel entre les tentatives et un délai maximal de 5 secondes. L’interruption exponentielle signifie qu’après chaque nouvelle tentative d’exécution, elle attend pendant une période plus longue avant de réessayer. Si trois tentatives dans une ligne échouent, une exception est levée. La section suivante sur les disjoncteurs explique pourquoi vous souhaitez une interruption exponentielle et un nombre limité de nouvelles tentatives.

    Vous pouvez rencontrer des problèmes similaires lorsque vous utilisez le service de stockage Azure, comme le fait l’application Fix it pour les objets BLOB, et l’API cliente de stockage .NET implémente déjà le même type de logique. Vous spécifiez simplement la stratégie de nouvelle tentative, ou vous n’avez même pas à le faire si vous êtes satisfait des paramètres par défaut.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Disjoncteurs

Il existe plusieurs raisons pour lesquelles vous ne souhaitez pas effectuer un trop grand nombre de tentatives sur une période donnée :

- Un trop grand nombre d’utilisateurs qui réessayent de manière permanente les demandes ayant échoué peuvent nuire à l’expérience des autres utilisateurs. Si des millions de personnes effectuent toutes des nouvelles tentatives répétées, vous pouvez lier les files d’attente de dispatch IIS et empêcher votre application de traiter les demandes qu’elle pourrait gérer avec succès.
- Si tout le monde tente une nouvelle tentative en raison d’une défaillance du service, il peut y avoir un grand nombre de requêtes en attente pour que le service soit submergé au démarrage de la récupération.
- Si l’erreur est due à une limitation et qu’il y a une fenêtre de temps utilisée par le service pour la limitation, les nouvelles tentatives peuvent déplacer cette fenêtre et provoquer la poursuite de la limitation.
- Vous pouvez avoir un utilisateur en attente d’une page Web à afficher. Faire en sorte que les gens attendent trop longtemps peut être plus ennuyeux, qui les conseille relativement rapidement pour réessayer plus tard.

L’interruption exponentielle résout une partie de ces problèmes en limitant la fréquence des nouvelles tentatives qu’un service peut obtenir de votre application. Toutefois, vous devez également avoir des *disjoncteurs*: cela signifie qu’à un certain seuil de nouvelles tentatives, votre application cesse de réessayer et entreprend une autre action, telle que l’un des éléments suivants :

- De secours personnalisé. Si vous ne parvenez pas à obtenir un cours d’action de Reuters, vous pouvez peut-être le faire à partir de Bloomberg ; Si vous ne pouvez pas récupérer de données à partir de la base de données, vous pouvez peut-être les récupérer à partir du cache.
- Échec du silence. Si vous avez besoin d’un service qui n’est pas tout ou rien pour votre application, il vous suffit de retourner la valeur NULL lorsque vous ne pouvez pas obtenir les données. Si vous affichez une tâche Fix it et que le service BLOB ne répond pas, vous pouvez afficher les détails de la tâche sans l’image.
- Faire échouer rapidement. Erreur de l’utilisateur pour éviter de saturer le service avec des demandes de nouvelle tentative qui pourraient entraîner une interruption du service pour d’autres utilisateurs ou étendre une fenêtre de limitation. Vous pouvez afficher un message « réessayer plus tard ».

Il n’y a pas de stratégie de nouvelle tentative adaptée à une seule taille. Vous pouvez réessayer plus de temps et attendre plus longtemps dans un processus de travail en arrière-plan asynchrone que dans une application Web synchrone où un utilisateur attend une réponse. Vous pouvez attendre plus longtemps entre les nouvelles tentatives pour un service de base de données relationnelle que pour un service de cache. Voici quelques exemples de stratégies de nouvelle tentative recommandées qui vous donnent une idée de la façon dont les nombres peuvent varier. (« Fast First » signifie aucun délai avant la première nouvelle tentative.

![Exemples de stratégies de nouvelle tentative](transient-fault-handling/_static/image1.png)

Pour SQL Database de l’aide sur les stratégies de nouvelle tentative, consultez [résoudre les erreurs temporaires et les erreurs de connexion à SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Récapitulatif

Une stratégie de nouvelle tentative/interruption peut aider à rendre les erreurs temporaires invisibles au client la plupart du temps, et Microsoft fournit des infrastructures que vous pouvez utiliser pour réduire votre travail à l’implémentation d’une stratégie, que vous utilisiez ADO.NET, Entity Framework ou le service de stockage Azure.

Dans le [chapitre suivant](distributed-caching.md), nous allons examiner comment améliorer les performances et la fiabilité à l’aide de la mise en cache distribuée.

## <a name="resources"></a>Ressources

Pour plus d'informations, voir les ressources suivantes :

Documentation

- [Meilleures pratiques pour la conception de services à grande échelle sur les services Cloud Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Livre blanc en Mark SIMM et Michael thomassy. Semblable à la série Failsafe, mais présente des détails supplémentaires. Consultez la section télémétrie et Diagnostics.
- [Failsafe : aide sur les architectures résilientes du Cloud](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Livre blanc de Marc Mercuri, Ulrich Homann et Andrew Townhill. Version de page Web de la série de vidéos de secours.
- [Modèles et pratiques Microsoft-conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez modèle de nouvelle tentative, modèle de superviseur de l’agent du planificateur.
- [Tolérance aux pannes dans Azure SQL Database](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Billet de blog de Tony Petrossian.
- [Entity Framework-résilience de connexion/logique de nouvelle tentative](https://msdn.microsoft.com/data/dn456835). Comment utiliser et personnaliser la fonctionnalité de gestion des erreurs temporaires de Entity Framework 6.
- [Résilience des connexions et interception des commandes avec l’Entity Framework dans une application MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quatrième dans une série de didacticiels en neuf parties, montre comment configurer la fonctionnalité de résilience de connexion EF 6 pour SQL Database.

Vidéos

- [Failsafe : création de services Cloud évolutifs et résilients](https://channel9.msdn.com/Series/FailSafe). Série en neuf parties de Ulrich Homann, Marc Mercuri et Mark SIMM. Présente des concepts de haut niveau et des principes architecturaux de manière très accessible et intéressante, avec des histoires tirées de l’expérience de l’équipe de conseil clientèle de Microsoft avec les clients réels. Reportez-vous à la discussion des disjoncteurs dans l’épisode 3 à partir de 40:55.
- [Création de Big : leçons apprises par les clients Azure-partie II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark SIMM parle de la conception en cas d’échec, de la gestion des erreurs temporaires et de l’instrumentation de tout.

Exemple de code

- [Notions de base du service Cloud dans Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application créé par le Microsoft Azure équipe de Conseil client qui montre comment utiliser le [bloc de gestion des erreurs temporaires](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH) d’Enterprise Library. Pour plus d’informations, consultez [couche d’accès aux données des notions de base du service Cloud-gestion des erreurs temporaires](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH est recommandé pour l’accès aux bases de données à l’aide de ADO.NET directement (sans utiliser Entity Framework).

> [!div class="step-by-step"]
> [Précédent](monitoring-and-telemetry.md)
> [Suivant](distributed-caching.md)
