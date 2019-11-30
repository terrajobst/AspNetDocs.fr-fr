---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Conception pour survivre aux défaillances (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 9bf9acb8b4f8521d03c053c124c5fc4a07d6cb9a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585650"
---
# <a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Conception pour survivre aux défaillances (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

L’un des points à prendre en compte lors de la création de n’importe quel type d’application, mais surtout qui s’exécutera dans le Cloud où de nombreuses personnes l’utiliseront, est la façon de concevoir l’application afin qu’elle puisse gérer correctement les défaillances et continuer à fournir une valeur aussi grande que possibilité. En raison de suffisamment de temps, les problèmes se posent dans un environnement ou un système logiciel. La façon dont votre application gère ces situations détermine la manière dont les clients obtiennent et le temps que vous devez consacrer à l’analyse et à la résolution des problèmes.

## <a name="types-of-failures"></a>Types d’échecs

Il existe deux catégories de base de défaillances que vous souhaitez gérer différemment :

- Des échecs temporaires et de réparation automatique, tels que des problèmes de connectivité réseau intermittente.
- Les échecs qui nécessitent une intervention.

Pour les échecs temporaires, vous pouvez implémenter une stratégie de nouvelle tentative pour vous assurer que la majeure partie du temps que l’application récupère rapidement et automatiquement. Vos clients peuvent remarquer un temps de réponse légèrement plus long, mais ils ne seront pas affectés. Nous présenterons des moyens de gérer ces erreurs dans le [chapitre relatif à la gestion des erreurs temporaires](transient-fault-handling.md).

Pour les échecs de connexion, vous pouvez implémenter une fonctionnalité de surveillance et de journalisation qui vous avertit rapidement lorsque des problèmes surviennent et qui facilitent l’analyse de la cause racine. Nous allons vous montrer quelques méthodes pour vous aider à rester au-dessus de ces types d’erreurs dans le chapitre sur la [surveillance et la télémétrie](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Étendue de l’échec

Vous devez également réfléchir à l’étendue de l’échec, qu’il s’agisse d’un seul ordinateur affecté, d’un service complet, tel qu’un SQL Database ou un stockage, ou d’une région entière.

![Étendue de l’échec](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Défaillances de l’ordinateur

Dans Azure, un serveur défaillant est automatiquement remplacé par un nouveau serveur, et une application Cloud bien conçue permet une récupération automatique et rapide de ce type d’échec. Nous avons précédemment souligné les avantages de l’évolutivité d’un niveau Web sans État, et la facilité de récupération d’un serveur défaillant est un autre avantage de abandon. La facilité de récupération est également l’un des avantages des fonctionnalités PaaS (Platform-as-a-service), telles que SQL Database et Azure App Service Web Apps. Les défaillances matérielles sont rares, mais lorsqu’elles se produisent, ces services les gèrent automatiquement. vous n’avez même pas besoin d’écrire du code pour gérer les défaillances de l’ordinateur lorsque vous utilisez l’un de ces services.

### <a name="service-failures"></a>Échecs de service

Les applications Cloud utilisent généralement plusieurs services. Par exemple, l’application Fix It utilise le service de SQL Database, le service de stockage, et l’application Web est déployée sur Azure App Service. Que fait votre application si l’un des services dont vous dépendez échoue ? Pour certains échecs de service, un message « Désolé, réessayez plus tard » peut être le plus approprié. Mais dans de nombreux scénarios, vous pouvez faire mieux. Par exemple, lorsque votre magasin de données principal est arrêté, vous pouvez accepter les entrées de l’utilisateur, afficher « votre demande a été reçue » et stocker l’entrée dans un endroit temporaire. Ensuite, lorsque le service dont vous avez besoin est de nouveau opérationnel, vous pouvez récupérer l’entrée et la traiter.

Le chapitre [modèle de travail centré sur la file d’attente](queue-centric-work-pattern.md) montre un moyen de gérer ce scénario. L’application Fix it stocke les tâches dans SQL Database, mais il n’est pas nécessaire de quitter le travail lorsque SQL Database est arrêté. Dans ce chapitre, nous allons voir comment stocker les entrées utilisateur d’une tâche dans une file d’attente et comment utiliser un processus de travail pour lire la file d’attente et mettre à jour la tâche. Si SQL est en panne, la possibilité de créer des tâches de réparation n’est pas affectée. le processus de travail peut attendre et traiter de nouvelles tâches lorsque SQL Database est disponible.

### <a name="region-failures"></a>Échecs de région

Les régions entières peuvent échouer. Une catastrophe naturelle peut détruire un centre de données, elle peut être aplatie par un Meteor, la ligne de tronc dans le centre de données peut être coupée par un agriculteur qui enfoui une vache avec un backhoe, etc. Si votre application est hébergée dans le centre de Stricken, que faites-vous ? Il est possible de configurer votre application dans Azure de manière à ce qu’elle s’exécute dans plusieurs régions simultanément afin que, en cas de sinistre en un, vous continuiez à fonctionner dans une autre région. De tels échecs sont extrêmement rares, et la plupart des applications ne passent pas en revue les cerceaus nécessaires pour garantir un service ininterrompu par le biais de défaillances de ce type. Consultez la section ressources à la fin du chapitre pour plus d’informations sur la façon de maintenir votre application disponible même en cas de défaillance d’une région.

L’objectif d’Azure est de faciliter considérablement la gestion de tous ces types de défaillances, et vous verrez des exemples de la façon dont nous faisons cela dans les chapitres suivants.

## <a name="slas"></a>SLA

Les gens sont souvent informés des contrats de niveau de service (SLA) dans l’environnement Cloud. Fondamentalement, ceux-ci sont des promesses que les entreprises font de la fiabilité de leur service. Un contrat SLA de 99,9% signifie que vous devez vous attendre à ce que le service fonctionne correctement 99,9% du temps. Il s’agit d’une valeur assez courante pour un contrat SLA et elle ressemble à un nombre très élevé, mais vous risquez de ne pas vous rendre compte du temps d’inactivité de 1%. Voici un tableau qui indique le temps d’indisponibilité, en pourcentage, de plusieurs contrats de niveau de service par rapport à une année, un mois et une semaine.

![Table SLA](design-to-survive-failures/_static/image2.png)

Par conséquent, un contrat SLA de 99,9% signifie que votre service peut être en baisse 8,76 heures par an ou 43,2 minutes par mois. C’est plus de temps que la plupart des gens se rendent compte. Ainsi, en tant que développeur, vous souhaitez savoir qu’un certain temps d’arrêt est possible et le gérer de manière appropriée. À un moment donné, quelqu’un va utiliser votre application, et un service va être en cours et vous souhaitez réduire l’impact négatif de cela sur le client.

Une chose à savoir sur un contrat SLA est le laps de temps auquel il fait référence : l’horloge est-elle réinitialisée chaque semaine, chaque mois ou chaque année ? Dans Azure, nous avons réinitialisé l’horloge chaque mois, ce qui est mieux pour vous qu’un contrat SLA annuel, car un contrat SLA annuel peut masquer des mois incorrects en les décalant avec une série de bons mois.

Bien sûr, nous cherchons toujours à faire mieux que le contrat SLA. en règle générale, vous serez trop loin. La promesse est que, si nous sommes toujours en baisse pendant une période plus longue que la durée maximale, vous pouvez demander de l’argent. La quantité d’argent que vous recevez ne compensera probablement pas complètement l’impact sur l’activité de l’excédent, mais cet aspect du contrat SLA agit comme une stratégie de mise en application et vous informe que nous le faisons très sérieusement.

### <a name="composite-slas"></a>SLA composites

Un élément important à prendre en compte lorsque vous examinez les contrats SLA est l’impact de l’utilisation de plusieurs services dans une application, chaque service ayant un contrat SLA distinct. Par exemple, l’application Fix It utilise Azure App Service Web Apps, stockage Azure et SQL Database. Voici leurs numéros de contrat SLA à la date de rédaction de ce livre électronique en décembre 2013 :

![Site Web du SLA, stockage, SQL Database](design-to-survive-failures/_static/image3.png)

Quelle est la durée d’indisponibilité maximale attendue pour l’application en fonction de ces contrats SLA de service ? Vous pensez peut-être que votre temps d’arrêt est égal au taux de contrat de service (SLA) le plus défavorable, ou à 99,9% dans ce cas. Cela est vrai si les trois services ont toujours échoué en même temps, mais ce n’est pas nécessairement ce qui se passe réellement. Chaque service peut échouer indépendamment à des moments différents. vous devez donc calculer le contrat SLA composite en multipliant les numéros de contrats SLA individuels.

![SLA composite](design-to-survive-failures/_static/image4.png)

Par conséquent, votre application peut être arrêtée non seulement 43,2 minutes par mois, mais également 3 fois cette quantité, 108 minutes par mois, et toujours dans les limites du contrat SLA Azure.

Ce problème n’est pas propre à Azure. Nous fournissons en fait les meilleurs contrats SLA du Cloud de n’importe quel service Cloud disponible et vous aurez des problèmes similaires à traiter si vous utilisez les services Cloud de votre fournisseur. Ce qu’il faut dire, c’est l’importance de réfléchir à la façon dont vous pouvez concevoir votre application pour gérer les échecs de service inévitables, car ils peuvent se produire suffisamment souvent pour avoir un impact sur vos clients ou utilisateurs.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Contrats SLA Cloud comparés à l’expérience de l’entreprise

Les gens disent parfois « dans mon application d’entreprise, je n’ai jamais ces problèmes ». Si vous demandez le temps d’indisponibilité d’un mois, ils disent généralement qu’ils se produisent occasionnellement. Et si vous demandez à quelle fréquence il faut savoir que « parfois, nous avons besoin de sauvegarder ou d’installer un nouveau serveur ou de mettre à jour un logiciel ». Bien entendu, cela compte comme un temps d’indisponibilité. La plupart des applications d’entreprise, à moins qu’elles ne soient particulièrement stratégiques, sont en fait inférieures à la durée autorisée par nos contrats SLA de service. Mais lorsqu’il s’agit de votre serveur et de votre infrastructure et que vous êtes responsable de l’informatique et du contrôle de celle-ci, vous avez tendance à vous sentir moins Angst des temps d’indisponibilité. Dans un environnement Cloud, vous êtes dépendant d’une autre personne et vous ne savez pas ce qui se passe, vous pouvez donc avoir tendance à vous en soucier.

Lorsqu’une entreprise atteint un pourcentage de temps d’attente plus élevé que celui obtenu à partir d’un contrat SLA Cloud, elle le fait en consacrant beaucoup plus d’argent au matériel. Un service Cloud peut faire cela, mais il devrait en être facturé davantage pour ses services. Au lieu de cela, vous tirez parti d’un service économique et concevez vos logiciels afin que les échecs inévitables entraînent une interruption minimale de vos clients. Votre travail en tant que concepteur d’applications Cloud n’est pas tellement grand d’éviter les défaillances, car cela évite les désastres, et vous pouvez le faire en vous concentrant sur les logiciels, et non sur le matériel. Tandis que les applications d’entreprise cherchent à maximiser le temps moyen entre les défaillances, les applications Cloud cherchent à réduire le temps moyen de récupération.

### <a name="not-all-cloud-services-have-slas"></a>Tous les services Cloud n’ont pas de contrat SLA

Sachez également que tous les services Cloud n’ont même pas de contrat SLA. Si votre application dépend d’un service sans aucune garantie de mise en route, vous risquez d’être beaucoup plus long que vous pourriez l’imaginer. Par exemple, si vous activez la connexion à votre site à l’aide d’un fournisseur de réseaux sociaux tel que Facebook ou Twitter, contactez le fournisseur de services pour déterminer s’il existe un contrat SLA et si vous n’en trouvez pas un. Toutefois, si le service d’authentification tombe en panne ou n’est pas en mesure de prendre en charge le volume de requêtes que vous levez, vos clients sont verrouillés de votre application. Vous pouvez être en baisse pendant plusieurs jours ou plus. Les créateurs d’une nouvelle application attendaient des centaines de millions de téléchargements et prenaient une dépendance à l’authentification Facebook, mais n’ont pas parlé à Facebook avant d’être en ligne et découverts trop tard, car il n’existait pas de contrat SLA pour ce service.

### <a name="not-all-downtime-counts-toward-slas"></a>Tous les temps d’arrêt ne sont pas comptabilisés dans les contrats SLA

Certains services de Cloud Computing peuvent délibérément refuser le service si votre application les utilise. C’est ce que l’on appelle la *limitation*. Si un service a un contrat SLA, il doit indiquer les conditions dans lesquelles vous pouvez être limité, et la conception de votre application doit éviter ces conditions et réagir de manière appropriée à la limitation si elle se produit. Par exemple, si les demandes à un service commencent à échouer lorsque vous dépassez un certain nombre par seconde, vous souhaitez vous assurer que les nouvelles tentatives automatiques ne se produisent pas si rapidement qu’elles provoquent la poursuite de la limitation. Nous aurons plus d’informations sur la limitation dans le [chapitre relatif à la gestion des erreurs temporaires](transient-fault-handling.md).

## <a name="summary"></a>Récapitulatif

Ce chapitre a essayé de vous aider à comprendre pourquoi une application Cloud réelle doit être conçue pour survivre aux défaillances de manière appropriée. À compter du [chapitre suivant](monitoring-and-telemetry.md), les autres modèles de cette série décrivent plus en détail certaines stratégies que vous pouvez utiliser pour effectuer cette opération :

- Avoir une bonne [surveillance et une télémétrie](monitoring-and-telemetry.md), afin que vous trouviez rapidement les défaillances nécessitant une intervention, et que vous disposiez d’informations suffisantes pour les résoudre.
- [Gérer les erreurs temporaires](transient-fault-handling.md) en implémentant une logique de nouvelle tentative intelligente, afin que votre application soit automatiquement récupérée lorsqu’elle peut et revient à la logique de [disjoncteur](transient-fault-handling.md#circuitbreakers) lorsqu’elle ne l’est pas.
- Utilisez [la mise en cache distribuée](distributed-caching.md) afin de réduire le débit, la latence et les problèmes de connexion avec l’accès aux bases de données.
- Implémentez un couplage faible via le [modèle de travail centré sur la file d’attente](queue-centric-work-pattern.md), afin que le serveur frontal de votre application puisse continuer à fonctionner lorsque le back end est arrêté.

## <a name="resources"></a>Ressources

Pour plus d’informations, consultez les chapitres ultérieurs dans ce livre électronique et les ressources suivantes.

Documentation :

- [Failsafe : aide sur les architectures résilientes du Cloud](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Livre blanc de Marc Mercuri, Ulrich Homann et Andrew Townhill. Version de page Web de la série de vidéos de secours.
- [Meilleures pratiques pour la conception de services à grande échelle sur les services Cloud Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Livre blanc en Mark SIMM et Michael thomassy.
- [Guide technique de la continuité des activités Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Livre blanc de Patrick Wickline et Jason Roth.
- [Récupération d’urgence et haute disponibilité pour les applications Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Livre blanc de Michael McKeown, Hanu Kommalapati et Jason Roth.
- [Modèles et pratiques Microsoft-conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez Guide de déploiement de plusieurs centres de données, modèle disjoncteur.
- [Support Azure-contrats de niveau de service](https://azure.microsoft.com/support/legal/sla/).
- [Continuité des activités dans Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Documentation sur SQL Database fonctionnalités de haute disponibilité et de récupération d’urgence.
- [Haute disponibilité et récupération d’urgence pour SQL Server dans les machines virtuelles Azure](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Vidéos :

- [Failsafe : création de services Cloud évolutifs et résilients](https://channel9.msdn.com/Series/FailSafe). Série en neuf parties de Ulrich Homann, Marc Mercuri et Mark SIMM. Présente des concepts de haut niveau et des principes architecturaux de manière très accessible et intéressante, avec des histoires tirées de l’expérience de l’équipe de conseil clientèle de Microsoft avec les clients réels. Les épisodes 1 et 8 vont de plus en plus sur les raisons de concevoir des applications Cloud pour survivre aux défaillances. Consultez également la discussion de suivi de la limitation dans l’épisode 2 à partir de 49:57, la discussion des points d’échec et des modes d’échec dans l’épisode 2 à partir de 56:05, et la discussion des disjoncteurs dans l’épisode 3 à partir de 40:55.
- [Création de Big : leçons apprises par les clients Azure-partie II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark SIMM parle de la conception en cas de défaillance et de l’instrumentation de tout. Semblable à la série Failsafe, mais présente des détails supplémentaires.

> [!div class="step-by-step"]
> [Précédent](unstructured-blob-storage.md)
> [Suivant](monitoring-and-telemetry.md)
