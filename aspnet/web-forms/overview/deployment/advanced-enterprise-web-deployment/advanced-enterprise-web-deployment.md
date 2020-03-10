---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Déploiement Web d’entreprise avancé | Microsoft Docs
author: jrjlee
description: Ce didacticiel vous montre comment effectuer diverses tâches requises ou souhaitables dans de nombreux scénarios de déploiement d’entreprise. Pour une traduction italienne...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: bf4c7d021763017592483df35cb717295c4924aa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587604"
---
# <a name="advanced-enterprise-web-deployment"></a>Déploiement web d’entreprise avancé

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ce didacticiel vous montre comment effectuer diverses tâches requises ou souhaitables dans de nombreux scénarios de déploiement d’entreprise.
> 
> Pour une traduction italienne de ces didacticiels, visitez [http://www.lucamorelli.it](http://www.lucamorelli.it).

Ce formulaire fait partie d’une série de didacticiels basés sur les exigences de déploiement d’entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de&#x2014;gestion des [contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md), dans lequel le processus de génération est&#x2014;contrôlé par deux fichiers projet contenant des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="scenario-overview"></a>Vue d’ensemble du scénario

Le scénario de haut niveau pour ces didacticiels est décrit dans [déploiement Web d’entreprise : vue d’ensemble du scénario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Nous vous recommandons de consulter cette rubrique avant de commencer à utiliser ce didacticiel.

## <a name="how-to-use-this-tutorial"></a>Comment utiliser ce didacticiel

- Chacune des rubriques de ce didacticiel est autonome et résout un problème ou un problème particulier qui se produit dans les scénarios de déploiement d’entreprise. Vous n’avez pas besoin d’utiliser ces rubriques dans un ordre particulier. Toutefois, ce didacticiel couvre certaines tâches avancées. Par conséquent, vous devez vous familiariser avec les concepts et les techniques abordés par le [déploiement Web dans le](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) didacticiel d’entreprise afin de tirer le meilleur parti de ce contenu.
- Ce didacticiel comprend les rubriques suivantes :
- [Exécution d’un déploiement « What If »](performing-a-what-if-deployment.md). Dans de nombreux scénarios, vous voudrez déterminer l’impact d’un déploiement proposé sur un environnement cible ou d’un contenu existant avant d’apporter des modifications. Cette rubrique décrit comment vous pouvez exécuter un déploiement « What If » pour générer des fichiers journaux et des scripts de mise à jour de base de données comme si vous aviez déployé du contenu vers un environnement cible, sans apporter de modifications. L’analyse de ces ressources peut vous aider à identifier les problèmes potentiels à l’avance d’un déploiement en direct.
- [Personnalisation des déploiements de bases de données pour plusieurs environnements](customizing-database-deployments-for-multiple-environments.md). Lorsque vous déployez un projet de base de données vers plusieurs destinations, vous souhaiterez souvent personnaliser les propriétés de déploiement pour chaque environnement cible. Par exemple, dans les environnements de test, vous devez généralement recréer la base de données sur chaque déploiement, alors que dans les environnements intermédiaires ou de production, vous seriez beaucoup plus susceptible de créer des mises à jour incrémentielles pour conserver vos données. Cette rubrique décrit comment vous pouvez incorporer ces modifications de propriété dans votre logique de déploiement en créant un fichier de configuration de déploiement (. sqldeployment) spécifique à l’environnement pour chaque environnement cible.
- Déploiement des appartenances de rôle de base de données dans des environnements de test. Lorsque vous recréez une base de données&#x2014;pour chaque déploiement, par exemple, dans le cadre d’une build d’intégration continue (ci)&#x2014;et que vous la déployez dans un environnement de test, vous devez généralement configurer des appartenances de rôle de base de données à chaque fois. Par exemple, vous devez généralement accorder des autorisations à l’identité du pool d’applications associé à votre application Web. Cette rubrique explique comment automatiser ce processus en ajoutant un script SQL de suivi de déploiement à votre logique de déploiement.
- [Déploiement de bases de données d’appartenance dans des environnements d’entreprise](deploying-membership-databases-to-enterprise-environments.md). Les bases de données d’appartenance ASP.NET ont différentes caractéristiques qui peuvent compliquer le processus de déploiement. Par exemple, un déploiement de schéma uniquement laisse la base de données dans un État non opérationnel. Dans la plupart des scénarios, il est préférable de créer une base de données d’appartenance directement dans chaque environnement de destination. Toutefois, si vous devez déployer une base de données d’appartenance, cette rubrique décrit certaines des approches que vous pouvez utiliser pour répondre aux défis inhérents.
- [Exclusion de fichiers et de dossiers du déploiement](excluding-files-and-folders-from-deployment.md). Dans certains scénarios, vous souhaiterez adapter le contenu de votre package Web à des environnements de destination spécifiques. Par exemple, vous pouvez inclure des versions complètes de bibliothèques JavaScript lorsque vous déployez dans un environnement de test, pour prendre en charge le débogage côté client, mais utilisez des versions minimisés des bibliothèques lorsque vous déployez dans un environnement intermédiaire ou de production. Cette rubrique décrit comment vous pouvez exclure des fichiers et des dossiers spécifiques du processus de création du package.
- Mise [hors connexion des applications Web avec Web Deploy](taking-web-applications-offline-with-web-deploy.md). Lorsque vous déployez des solutions dans un environnement intermédiaire ou de production, vous souhaiterez souvent mettre vos applications Web hors connexion pendant toute la durée du processus de déploiement. Cette rubrique décrit comment ajouter une *application\_fichier. htm hors connexion* à votre application Web au démarrage du processus de déploiement et la supprimer à la fin. Lorsque l’application *\_fichier. htm hors connexion* est en place, tous les utilisateurs qui accèdent à l’application Web sont automatiquement redirigés vers l’application *\_fichier. htm hors connexion* .
- [Exécution de scripts Windows PowerShell à partir de MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). De nombreux scénarios de déploiement requièrent des actions postérieures au déploiement, telles que l’ajout de sources d’événements personnalisées au registre ou la configuration de la réplication entre des instances de SQL Server. Ces actions sont souvent accomplies par le biais de scripts Windows PowerShell. Cette rubrique explique comment exécuter des scripts Windows PowerShell à partir d’un fichier projet Microsoft Build Engine (MSBuild) dans le cadre du processus de génération et de déploiement.
- [Résolution des problèmes liés au processus d’empaquetage](troubleshooting-the-packaging-process.md). Le pipeline de publication Web (WPP) définit une propriété MSBuild nommée **EnablePackageProcessLoggingAndAssert** que vous pouvez utiliser pour générer des informations détaillées sur le processus d’empaquetage pour les projets d’application Web. Cette rubrique décrit ce que fait la propriété et comment l’utiliser.

## <a name="key-technologies"></a>Technologies clés

Ce didacticiel se concentre sur l’utilisation de ces produits et technologies pour prendre en charge le déploiement automatisé de la génération et du Web :

- Visual Studio 2010 et Team Foundation Server (TFS) 2010
- MSBuild et TFS Team Build
- Internet Information Services (IIS) 7,5
- Outil de déploiement Web IIS (Web Deploy) 2,1
- Utilitaire de déploiement de base de données VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Autres didacticiels de cette série

Ce formulaire fait partie d’une série de cinq didacticiels sur le déploiement Web à l’échelle de l’entreprise. Voici d’autres didacticiels de la série :

- [Déploiement d’applications Web dans des scénarios d’entreprise](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ce contenu de présentation fournit l’arrière-plan contextuel de la série de didacticiels. Il décrit le scénario du didacticiel et illustre comment les tâches et les procédures pas à pas décrites dans la série s’intègrent à un processus plus large Application Lifecycle Management (ALM).
- [Déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ce didacticiel fournit une présentation conceptuelle des fichiers projet MSBuild, du WPP, du Web Deploy et d’autres technologies connexes. Il explique comment vous pouvez utiliser ces outils ensemble pour gérer des processus de déploiement complexes.
- [Configuration des environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ce didacticiel explique comment configurer des serveurs Windows pour prendre en charge différents scénarios de déploiement, y compris le déploiement de packages Web à distance à l’aide du service de Deployment Agent Web (l’agent distant) ou du gestionnaire de Web Deploy et du déploiement de base de données distante. Il fournit des conseils sur le choix de la méthode de déploiement appropriée pour votre propre environnement et décrit comment utiliser l’infrastructure de batterie de serveurs Web (WFF) pour répliquer des applications Web déployées sur tous les serveurs Web d’une batterie de serveurs.
- [Configuration de Team Foundation Server pour le déploiement Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ce didacticiel explique comment configurer TFS pour prendre en charge différents scénarios de déploiement, y compris le déploiement automatisé dans le cadre d’un processus d’intégration continue et déclencher manuellement des déploiements de builds spécifiques.

> [!div class="step-by-step"]
> [Next](performing-a-what-if-deployment.md)
