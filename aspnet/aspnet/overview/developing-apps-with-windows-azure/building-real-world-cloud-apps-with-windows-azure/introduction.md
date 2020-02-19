---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Création d’applications Cloud réalistes avec Azure | Microsoft Docs
author: MikeWasson
description: Ce livre électronique vous guide à travers une approche basée sur des modèles pour créer des solutions de Cloud réalistes. Les modèles s’appliquent au processus de développement ainsi qu’à un...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: b88573b3702b755b155e8da35f5f8a67931bafc6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457113"
---
# <a name="building-real-world-cloud-apps-with-azure"></a>Création d’applications Cloud réalistes avec Azure

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Ce livre électronique vous guide à travers une approche basée sur des modèles pour créer des solutions de Cloud réalistes. Les modèles s’appliquent au processus de développement ainsi qu’aux pratiques d’architecture et de codage.
> 
> Le contenu est basé sur une présentation développée par Scott Guthrie et livrée par lui à la Conférence des développeurs norvégiens (norvégiens) en juin de 2013 (partie[1](http://vimeo.com/68215538) [, partie 2)](http://vimeo.com/68215602)et à Microsoft Tech Ed Australie en septembre 2013 ([partie 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [partie 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). De [nombreuses autres](more-patterns-and-guidance.md#acknowledgments) ont mis à jour et augmenté le contenu lors de sa transition de la vidéo au format écrit.

## <a name="intended-audience"></a>Public visé

Les développeurs qui s’intéressent au développement pour le Cloud, qui envisagent de migrer vers le Cloud ou qui sont nouveaux dans le développement Cloud, trouveront ici une vue d’ensemble concise des concepts et pratiques les plus importants qu’ils doivent connaître. Les concepts sont illustrés par des exemples concrets et chaque chapitre est lié à d’autres ressources pour obtenir des informations plus approfondies. Les exemples et les liens vers des ressources supplémentaires sont pour les infrastructures et les services Microsoft, mais les principes illustrés s’appliquent également à d’autres infrastructures de développement Web et à d’autres environnements Cloud.

Les développeurs qui développent déjà pour le Cloud peuvent trouver des idées qui vous aideront à les rendre plus efficaces. Chaque chapitre de la série peut être lu indépendamment, ce qui vous permet de choisir et de sélectionner les rubriques qui vous intéressent.

Toute personne qui a regardé Scott Guthrie *à créer des applications Cloud réelles avec présentation Azure* et qui souhaite obtenir plus de détails et des informations mises à jour trouvera cela ici.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Modèles de développement Cloud

Ce livre électronique explique les treize modèles recommandés pour le développement en nuage. « Pattern » est utilisé ici à un sens large pour signifier qu’il est recommandé d’effectuer les opérations suivantes : comment développer, concevoir et coder des applications Cloud. Il s’agit de modèles clés qui vous aideront à « passer à l’pit de la réussite » si vous les suivez.

- [Automatisez tout](automate-everything.md).

    - Utilisez des scripts pour optimiser l’efficacité et réduire les erreurs dans les processus répétitifs.
    - Démonstration : scripts de gestion Azure.
- [Contrôle de code source](source-control.md). 

    - Configurez la structure de branchement dans le contrôle de code source pour faciliter le workflow DevOps.
    - Demo : ajouter des scripts au contrôle de code source.
    - Démonstration : conserver les données sensibles en dehors du contrôle de code source.
    - Demo : utilisez git dans Visual Studio.
- [Intégration et remise continues](continuous-integration-and-continuous-delivery.md). 

    - Automatisez la génération et le déploiement avec chaque archivage de contrôle de code source.
- [Meilleures pratiques pour le développement Web](web-development-best-practices.md). 

    - Conservez l’état du niveau Web.
    - Démonstration : mise à l’échelle et mise à l’échelle automatique dans Web Apps dans Azure App Service.
    - Évitez l’état de la session.
    - Utilisez un CDN avec un secours lorsque le CDN n’est pas disponible.
    - Utilisez le modèle de programmation asynchrone.
    - Démonstration : Async dans ASP.NET MVC et Entity Framework.
- [Authentification unique](single-sign-on.md). 

    - Présentation de Azure Active Directory.
    - Démo : créer une application ASP.NET qui utilise Azure Active Directory.
- [Options de stockage de données](data-storage-options.md) : 

    - Types de magasins de données.
    - Comment choisir le magasin de données approprié.
    - Démo : Azure SQL Database.
- [Stratégies de partitionnement des données](data-partitioning-strategies.md). 

    - Partitionnez les données verticalement, horizontalement ou les deux pour faciliter la mise à l’échelle d’une base de données relationnelle.
- [Stockage d’objets BLOB non structuré](unstructured-blob-storage.md). 

    - Stockez les fichiers dans le Cloud à l’aide du service BLOB.
    - Démonstration : utilisation du stockage d’objets BLOB dans l’application Fix it.
- [Conception pour survivre aux échecs](design-to-survive-failures.md). 

    - Types d’échecs.
    - Étendue de l’échec.
    - Compréhension des contrats SLA.
- [Surveillance et télémétrie](monitoring-and-telemetry.md). 

    - Pourquoi vous devez acheter une application de télémétrie et écrire votre propre code pour instrumenter votre application.
    - Démo : nouvelle Relic pour Azure
    - Démonstration : écriture de code dans l’application Fix it.
    - Démonstration : injection de dépendances dans l’application Fix it.
    - Démonstration : prise en charge intégrée de la journalisation dans Azure.
- [Gestion des erreurs temporaires](transient-fault-handling.md). 

    - Utilisez une logique de nouvelle tentative intelligente/d’interruption pour atténuer l’effet des échecs temporaires.
    - Démo : réessayer/annuler en Entity Framework 6.
- [Mise en cache distribuée](distributed-caching.md). 

    - Améliorez l’évolutivité et réduisez les coûts de transaction de base de données en utilisant la mise en cache distribuée.
- [Modèle de travail centré sur la file d’attente](queue-centric-work-pattern.md). 

    - Activez la haute disponibilité et améliorez l’évolutivité en couplage faiblement les niveaux Web et Worker.
    - Démonstration : files d’attente Azure Storage dans l’application Fix it.
- [Autres modèles d’application Cloud et conseils](more-patterns-and-guidance.md).
- [Annexe : L’exemple d’application Fix It](the-fix-it-sample-application.md)

    - Problèmes connus
    - Pratiques recommandées
    - Comment télécharger, générer, exécuter et déployer.

Ces modèles s’appliquent à tous les environnements Cloud, mais nous allons les illustrer à l’aide d’exemples basés sur des technologies et des services Microsoft, tels que Visual Studio, Team Foundation Service, ASP.NET et Azure.

Le reste de ce chapitre présente l’exemple d’application Fix it et le Web Apps dans Azure App Service environnement Cloud dans lequel l’application Fix It s’exécute.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Exemple d’application Fix it

La plupart des captures d’écran et des exemples de code présentés dans ce livre électronique sont basés sur l’application de réparation développée initialement par [Scott Guthrie](https://weblogs.asp.net/scottgu/) pour illustrer les pratiques et les modèles de développement d’applications Cloud recommandés.

![Page d’hébergement de l’application Fix it](introduction/_static/image1.png)

L’exemple d’application est un système de gestion de tickets d’élément de travail simple. Lorsque vous avez besoin de corriger un problème, vous créez un ticket et l’attribuez à quelqu’un, et d’autres personnes peuvent se connecter et voir les tickets qui leur sont affectés et marquer les tickets comme terminés une fois le travail terminé.

Il s’agit d’un projet Web Visual Studio standard. Il repose sur ASP.NET MVC et utilise une base de données SQL Server. Il peut s’exécuter localement dans IIS Express et peut être déployé sur un site Web Azure pour s’exécuter dans le Cloud. Vous pouvez vous connecter à l’aide de l’authentification par formulaire et d’une base de données locale ou à l’aide d’un fournisseur de réseaux sociaux tel que Google. (Par la suite, nous allons voir comment se connecter avec un compte d’organisation Active Directory.)

![Page de connexion](introduction/_static/image2.png)

Une fois que vous êtes connecté, vous pouvez créer un ticket, l’affecter à une personne et télécharger une image de ce que vous souhaitez corriger.

![Créer une tâche Fix it](introduction/_static/image3.png)

![Tâche Fix it créée](introduction/_static/image4.png)

Vous pouvez suivre la progression des éléments de travail que vous avez créés, voir les tickets qui vous sont assignés, afficher les détails du ticket et marquer les éléments comme étant terminés.

Il s’agit d’une application très simple du point de vue des fonctionnalités, mais vous verrez comment la créer afin qu’elle puisse s’adapter à des millions d’utilisateurs et résiste à des choses telles que des défaillances de bases de données et des arrêts de connexion. Vous verrez également comment créer un flux de travail de développement automatisé et agile, ce qui vous permet de démarrer facilement et de rendre l’application plus efficace et mieux en effectuant une itération rapide et rapide du cycle de développement.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Web Apps dans Azure App Service

L’environnement Cloud utilisé pour l’application Fix it est un service Azure que nous appelons sites Web. Ce service vous permet d’héberger votre propre application Web dans Azure sans avoir à créer des machines virtuelles et à les mettre à jour, à installer et à configurer IIS, etc. Nous hébergeons votre site sur nos machines virtuelles et fournissons automatiquement la sauvegarde et la récupération, ainsi que d’autres services pour vous. Le service sites Web fonctionne avec ASP.NET, node. js, PHP et Python. Elle vous permet de déployer très rapidement à l’aide de Visual Studio, Web Deploy, FTP, git ou TFS. Il s’agit généralement de quelques secondes entre le moment où vous démarrez un déploiement et le moment où votre mise à jour est disponible sur Internet. Il est tout à fait gratuit, et vous pouvez monter en puissance à mesure que votre trafic augmente.

En arrière-plan, Web Apps dans Azure App Service fournit de nombreux composants et fonctionnalités architecturaux que vous auriez besoin de créer vous-même si vous envisagez d’héberger un site Web à l’aide d’IIS sur vos propres machines virtuelles. Un composant est un point de terminaison de déploiement qui configure automatiquement IIS et installe votre application sur le nombre de machines virtuelles sur lesquelles vous souhaitez exécuter votre site.

![Service de déploiement](introduction/_static/image5.png)

Lorsqu’un utilisateur accède au site Web, il n’atteint pas les machines virtuelles IIS directement, il passe par [application Request Routing (arr)](https://www.iis.net/downloads/microsoft/application-request-routing) équilibrages de charge. Vous pouvez les utiliser avec vos propres serveurs, mais l’avantage est qu’ils sont configurés automatiquement pour vous. Ils utilisent une méthode heuristique intelligente qui prend en compte des facteurs tels que l’affinité de session, la profondeur des files d’attente dans IIS et l’utilisation du processeur sur chaque ordinateur pour diriger le trafic vers les machines virtuelles qui hébergent votre site Web.

![Équilibreur de charge ARR](introduction/_static/image6.png)

Si une machine tombe en panne, Azure la récupère automatiquement à partir de la rotation, fait tourner une nouvelle instance de machine virtuelle et commence à diriger le trafic vers la nouvelle instance, tout cela sans temps d’indisponibilité pour votre application.

![Récupération automatique à partir d’une défaillance de la machine](introduction/_static/image7.png)

Tout cela a lieu automatiquement. Tout ce que vous avez à faire, c’est créer un site Web et y déployer votre application, à l’aide de Windows PowerShell, de Visual Studio ou du portail de gestion Azure.

Pour obtenir un didacticiel pas à pas rapide et facile qui montre comment créer une application Web dans Visual Studio et la déployer sur un site Web Azure, consultez prise en [main d’Azure et de ASP.net](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Résumé

Cette introduction a fourni une liste de rubriques abordées par le livre, des captures d’écran de l’exemple d’application et une brève présentation de l’Web Apps dans Azure App Service environnement Cloud. L’un des grands avantages du développement d’applications dans et pour le Cloud est qu’il est facile d’automatiser des tâches de développement répétitives, telles que la création d’un environnement de test et le déploiement de votre code vers celui-ci. La procédure à suivre est l’objet du [chapitre suivant](automate-everything.md).

## <a name="resources"></a>Ressources

Pour plus d’informations sur les sujets abordés dans ce chapitre, consultez les ressources suivantes.

Documentation :

- [Web Apps dans Azure App service](https://azure.microsoft.com/services/app-service/web/). Page du portail pour la documentation Azure sur Web Apps.
- [Web Apps, services Cloud et machines virtuelles : quand les utiliser ?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS comme indiqué dans ce chapitre ne constitue qu’une des trois façons d’exécuter des applications Web dans Azure. Cet article explique les différences entre les trois façons et fournit des conseils sur la façon de choisir celui qui convient à votre scénario. Comme les sites Web, cloud services est une fonctionnalité PaaS d’Azure. Les machines virtuelles sont une fonctionnalité IaaS. Pour une explication de PaaS et IaaS, consultez le chapitre [options de données](data-storage-options.md#paasiaas) .

Vidéos :

- [Scott Guthrie commence à l’étape 0-qu’est-ce qu’Azure système d’exploitation cloud ?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architecture de sites Web-avec Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Les sites Web Azure sont internes avec NIR Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Next](automate-everything.md)
