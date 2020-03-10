---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configuration des environnements de serveur pour le déploiement Web | Microsoft Docs
author: jrjlee
description: Ce didacticiel vous montre comment configurer des environnements de serveur pour prendre en charge le déploiement et la publication de sites Web en un seul clic, ou les publier dans différents SCEN...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 073161ce4faa3b7ba6749d7dfbaa5309eeca4f74
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634469"
---
# <a name="configuring-server-environments-for-web-deployment"></a>Configuration d’environnements serveur pour le déploiement web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ce didacticiel vous montre comment configurer des environnements de serveur pour prendre en charge le déploiement et la publication de sites Web en un clic, ou les publier dans différents scénarios. Ce didacticiel contient des rubriques qui vous guident dans l’exécution de différentes tâches, telles que la configuration d’un serveur Web pour prendre en charge des approches spécifiques du déploiement et de la configuration d’une batterie de serveurs Web Farm Framework (WFF), ainsi que des vues d’ensemble basées sur des scénarios qui fournissent conseils de bout en bout de niveau supérieur.
> 
> Le didacticiel utilise le scénario de déploiement Fabrikam, Inc. décrit dans [déploiement Web d’entreprise : vue d’ensemble du scénario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) comme point de référence pour les exemples et l’infrastructure réseau.
> 
> Pour une traduction italienne de ces didacticiels, visitez [http://www.lucamorelli.it](http://www.lucamorelli.it).

Ce didacticiel comprend les rubriques suivantes :

- [Choix de la bonne approche pour le déploiement web](choosing-the-right-approach-to-web-deployment.md)
- [Scénario : configuration d’un environnement de test pour le déploiement web](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Scénario : configuration d’un environnement de préproduction pour le déploiement web](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Scénario : configuration d’un environnement de production pour le déploiement web](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Configuration d’un serveur web pour la publication Web Deploy (Agent distant)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Configuration d’un serveur web pour la publication Web Deploy (Gestionnaire Web Deploy)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Configuration d’un serveur web pour la publication Web Deploy (Déploiement hors connexion)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Configuration d’un serveur de base de données pour la publication Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md)
- [Création d’une batterie de serveurs avec le framework de batterie de serveurs web](creating-a-server-farm-with-the-web-farm-framework.md)
- [Configuration des propriétés de déploiement pour un environnement cible](configuring-deployment-properties-for-a-target-environment.md)

La première rubrique, le [choix de l’approche la plus appropriée pour le déploiement Web](choosing-the-right-approach-to-web-deployment.md), décrit les principales approches que vous pouvez utiliser pour publier des applications Web à l’aide de l’outil de déploiement web (Web Deploy) Internet Information Services (IIS) 2,0. Il identifie également les scénarios qui correspondent à chaque approche. À partir de là, chaque rubrique de scénario fournit une vue d’ensemble détaillée des tâches à effectuer et identifie les rubriques que vous devez utiliser pour effectuer ces tâches.

Si vous utilisez l’approche fractionner le fichier de projet décrite dans [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md) pour générer et déployer votre solution, la dernière rubrique, [Configuration des propriétés de déploiement pour un environnement cible](configuring-deployment-properties-for-a-target-environment.md), explique comment configurer des fichiers projet spécifiques à l’environnement pour le déploiement dans différents environnements de destination.

## <a name="key-technologies"></a>Technologies clés

Ce didacticiel se concentre sur l’utilisation de ces produits et technologies pour prendre en charge le déploiement Web :

- IIS 7,5
- Web Deploy 2. x
- WFF 2. x
- Service de gestion Web IIS (WMSvc)

Ce didacticiel aborde également l’utilisation de Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4,0 et ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Autres didacticiels de cette série

Ce formulaire fait partie d’une série de cinq didacticiels sur le déploiement Web à l’échelle de l’entreprise. Voici les autres didacticiels de la série :

- [Déploiement d’applications Web dans des scénarios d’entreprise](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ce contenu de présentation fournit l’arrière-plan contextuel de la série de didacticiels. Il décrit le scénario du didacticiel et illustre comment les tâches et les procédures pas à pas décrites dans la série s’intègrent à un processus plus large Application Lifecycle Management (ALM).
- [Déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ce didacticiel fournit une introduction conceptuelle aux fichiers de projet Microsoft Build Engine (MSBuild), au pipeline de publication Web, aux Web Deploy et à d’autres technologies associées. Il explique comment vous pouvez utiliser ces outils ensemble pour gérer des processus de déploiement complexes.
- [Configuration de Team Foundation Server pour le déploiement Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ce didacticiel explique comment configurer Team Foundation Server (TFS) pour prendre en charge différents scénarios de déploiement, y compris le déploiement automatisé dans le cadre d’un processus d’intégration continue (CI) et déclencher manuellement des déploiements de builds spécifiques.
- [Déploiement Web d’entreprise avancé](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ce didacticiel explique comment effectuer diverses tâches de déploiement plus avancées, telles que la personnalisation des déploiements de bases de données pour plusieurs environnements, à l’exclusion de fichiers et de dossiers du déploiement, et la mise hors connexion des applications Web pendant le processus de déploiement. .

> [!div class="step-by-step"]
> [Next](choosing-the-right-approach-to-web-deployment.md)
