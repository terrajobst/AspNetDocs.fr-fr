---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Déploiement Web dans l’entreprise | Microsoft Docs
author: jrjlee
description: Ce didacticiel explique comment répondre à un grand nombre de défis que vous pouvez rencontrer lorsque vous gérez le déploiement d’applications Web à l’échelle de l’entreprise vers devel...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: bc7bb676a71af4ec3451aa3adf3c03ce3b5200d5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628169"
---
# <a name="web-deployment-in-the-enterprise"></a>Déploiement web dans l’entreprise

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ce didacticiel explique comment répondre à un grand nombre de défis que vous pouvez rencontrer lorsque vous gérez le déploiement d’applications Web à l’échelle de l’entreprise vers des environnements de développement, de test, intermédiaires et de production. Ce didacticiel comprend une solution de référence, ainsi qu’un mélange de contenu conceptuel et axé sur les tâches, pour vous guider dans diverses tâches et procédures courantes.
> 
> Pour une traduction italienne de ces didacticiels, visitez [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="enterprise-deployment-challenges"></a>Défis du déploiement d’entreprise

Les organisations rencontrent souvent ces difficultés lorsqu’elles cherchent à gérer le déploiement de solutions complexes à l’échelle de l’entreprise :

- Vous devez être en mesure de déployer des projets dans plusieurs environnements, tels que des environnements de développement ou de test, des plateformes intermédiaires et des serveurs de production. La solution doit être déployée avec des paramètres de configuration différents pour chaque environnement.
- Vous devez déployer plusieurs projets dépendants simultanément dans le cadre d’un processus de génération et de déploiement automatisé à étape unique.
- Vous devez être en mesure de conduire le déploiement à partir d’un processus automatisé. Par exemple, vous souhaitez utiliser un processus d’intégration continue (CI) pour déployer des applications Web dans un environnement de test lors de l’archivage d’un nouveau code.
- Vous devez être en mesure de contrôler le processus de déploiement et de définir des variables de déploiement en dehors de Visual Studio, car il est peu probable que les développeurs disposent des paramètres de configuration corrects ou des informations d’identification nécessaires pour chaque environnement cible.
- Vous devez déployer des projets de base de données basés sur un schéma et conserver les données existantes lors des déploiements suivants.
- Vous devez déployer les bases de données d’appartenance sur une base ad hoc sans déployer les données de compte d’utilisateur. Vous devrez peut-être également mettre à jour le schéma des bases de données d’appartenance déployées sans perdre les données de compte d’utilisateur existantes.
- Vous devez exclure certains fichiers ou dossiers lorsque vous déployez du contenu dans différents environnements cibles.

## <a name="overview-of-approach"></a>Vue d’ensemble de l’approche

Ce didacticiel, ainsi que les autres didacticiels de cette série, utilisent cette approche de haut niveau pour répondre aux défis décrits ci-dessus.

- **Utilisez des fichiers de projet Microsoft Build Engine (MSBuild) personnalisés pour contrôler le processus de génération et de déploiement global.**
- Cela vous permet de créer et de déployer chaque projet de la solution dans le cadre d’une opération unique et scriptable.
- Les paramètres spécifiques à l’environnement sont configurés à l’aide de fichiers de projet simples spécifiques à l’environnement. Contrairement à l’approche centrée sur Visual Studio qui consiste à utiliser des configurations de solution et des profils de publication pour configurer des déploiements pour différents environnements, cette approche vous permet de configurer et de gérer le processus de déploiement en dehors de Visual Studio. Cela signifie que les développeurs n’ont pas besoin d’avoir une connaissance approfondie des chaînes de connexion, des points de terminaison de service, des informations d’identification du serveur et d’autres variables de déploiement pour les environnements de destination.
- Les fichiers projet personnalisés peuvent être appelés par Team Build dans le cadre d’un flux de travail Team Foundation Server (TFS). Cela vous permet de configurer un déploiement automatisé pour les scénarios d’intégration continue.

**Utilisez l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS) pour empaqueter et déployer des projets d’application Web.**

- Web Deploy fournit une infrastructure qui vous permet d’empaqueter et de déployer le contenu de votre application Web sur un serveur Web IIS de destination, ainsi que des dépendances, des paramètres de configuration, des paramètres de sécurité et d’autres exigences.
- Vous pouvez contrôler l’ensemble du processus de déploiement et d’empaquetage à partir de vos fichiers de projet MSBuild personnalisés. Vous pouvez également manipuler les paramètres de configuration qui accompagnent votre package de déploiement Web, tels que les chaînes de connexion, les points de terminaison de service et les détails de destination IIS.
- Web Deploy, ainsi que le pipeline de publication Web, offre un grand nombre de points d’extensibilité qui vous permettent de personnaliser vos déploiements. Par exemple, il est facile d’exclure les fichiers et dossiers indésirables de vos packages de déploiement Web.

**Utilisez l’utilitaire VSDBCMD. exe pour déployer et mettre à jour des schémas de base de données.**

- VSDBCMD vous permet de déployer des bases de données à partir d’un fichier de schéma de base de données (. dbschema), qui est généré quand vous générez un projet de base de données Visual Studio. En revanche, la fonctionnalité de déploiement de base de données incluse dans Web Deploy est plus adaptée au déploiement de bases de données existantes à partir d’une instance de SQL Server locale.
- Contrairement aux fonctionnalités de Visual Studio pour le déploiement de projets de base de données, VSDBCMD vous permet de déployer des mises à jour différentielles vers une base de données cible existante. Cela vous permet de conserver toutes les données existantes pendant la mise à niveau du schéma de base de données.
- Vous pouvez exécuter des commandes VSDBCMD à partir de vos fichiers de projet MSBuild personnalisés.

## <a name="content-map"></a>Plan du contenu

Ce didacticiel comprend des rubriques qui se trouvent dans quatre domaines principaux.

Ces rubriques présentent la solution&#x2014;de référence, la solution&#x2014;de gestionnaire de contacts, et décrivent comment la télécharger et la configurer sur votre ordinateur local :

- [La solution Gestionnaire de contacts](the-contact-manager-solution.md)
- [Configuration de la solution Gestionnaire de contacts](setting-up-the-contact-manager-solution.md)

Ces rubriques présentent les fichiers projet MSBuild, décrivent comment vous pouvez créer et utiliser des fichiers projet personnalisés, et vous guident tout au long du processus de déploiement pour la solution de gestionnaire de contacts :

- [Présentation du fichier projet](understanding-the-project-file.md)
- [Présentation du processus de génération](understanding-the-build-process.md)

Ces rubriques décrivent le déploiement d’applications Web, notamment la façon dont le processus de génération et d’empaquetage fonctionne, comment le processus de génération s’intègre au pipeline de publication Web, comment modifier les paramètres de déploiement et comment déployer des packages Web vers la destination environnements

- [Génération et empaquetage des projets d’application web](building-and-packaging-web-application-projects.md)
- [Configuration des paramètres pour le déploiement de package web](configuring-parameters-for-web-package-deployment.md)
- [Déploiement de packages web](deploying-web-packages.md)

- [Déploiement de projets de base de données](deploying-database-projects.md) décrit les différentes techniques que vous pouvez utiliser pour déployer des projets de base de données Visual Studio, ainsi que les avantages et inconvénients de chaque approche. [Création et exécution d’un fichier de commandes de déploiement](creating-and-running-a-deployment-command-file.md) décrit comment créer un fichier de commandes simple qui encapsule votre logique de déploiement et vous permet de déployer des solutions complexes en tant que processus en une seule étape.
- Enfin, [l’installation manuelle des packages Web](manually-installing-web-packages.md) conclut le didacticiel en vous expliquant comment importer des packages Web dans IIS.

## <a name="key-technologies"></a>Technologies clés

Les rubriques de ce didacticiel utilisent principalement ces technologies pour gérer la génération et le déploiement :

- Visual Studio 2010
- MSBuild
- IIS 7,5
- Web Deploy 2.0
- Utilitaire de déploiement de base de données VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Autres didacticiels de cette série

Ce formulaire fait partie d’une série de cinq didacticiels sur le déploiement Web à l’échelle de l’entreprise. Voici les autres didacticiels de la série :

- [Déploiement d’applications Web dans des scénarios d’entreprise](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ce contenu de présentation fournit l’arrière-plan contextuel de la série de didacticiels. Il décrit le scénario du didacticiel et illustre comment les tâches et les procédures pas à pas décrites dans la série s’intègrent à un processus plus large Application Lifecycle Management (ALM).
- [Configuration des environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ce didacticiel explique comment configurer des serveurs Windows pour prendre en charge différents scénarios de déploiement, y compris le déploiement de packages Web à distance à l’aide du service de Deployment Agent Web (l’agent distant) ou du gestionnaire de Web Deploy et du déploiement de base de données distante. Il fournit des conseils sur le choix de la méthode de déploiement appropriée pour votre propre environnement et décrit comment utiliser l’infrastructure de batterie de serveurs Web (WFF) pour répliquer des applications Web déployées sur tous les serveurs Web d’une batterie de serveurs.
- [Configuration de Team Foundation Server pour le déploiement Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ce didacticiel explique comment configurer TFS pour prendre en charge différents scénarios de déploiement, y compris le déploiement automatisé dans le cadre d’un processus d’intégration continue et déclencher manuellement des déploiements de builds spécifiques.
- [Déploiement Web d’entreprise avancé](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ce didacticiel explique comment effectuer diverses tâches de déploiement plus avancées, telles que la personnalisation des déploiements de bases de données pour plusieurs environnements, à l’exclusion de fichiers et de dossiers du déploiement, et la mise hors connexion des applications Web pendant le processus de déploiement. .

> [!div class="step-by-step"]
> [Next](the-contact-manager-solution.md)
