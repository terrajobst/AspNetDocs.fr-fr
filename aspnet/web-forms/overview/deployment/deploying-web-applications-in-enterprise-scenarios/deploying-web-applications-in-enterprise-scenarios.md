---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Déploiement d’applications Web dans des scénarios d’entreprise à l’aide de Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: Cet ensemble de didacticiels décrit les outils et techniques que vous pouvez utiliser pour déployer des applications Web dans différents scénarios d’entreprise. Il explique comment optimiser l’utilisation...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: eb7b82d16079d4d086af1919d092fb28db60d8b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574157"
---
# <a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Déployer des applications web dans des scénarios d’entreprise avec Visual Studio 2010

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cet ensemble de didacticiels décrit les outils et techniques que vous pouvez utiliser pour déployer des applications Web dans différents scénarios d’entreprise. Il explique comment tirer le meilleur parti des technologies telles que Visual Studio 2010, le Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7,5, l’outil de déploiement Web IIS (Web Deploy), l’infrastructure de batterie de serveurs Web (WFF) et des utilitaires comme VSDBCMD. exe pour simplifier et gérer le processus de déploiement. Il comprend des présentations conceptuelles et des conseils orientés tâche qui vous aideront à :
> 
> - Examinez et établissez les exigences de déploiement pour une application Web à l’échelle de l’entreprise.
> - Configurez les environnements de serveur Web de test, intermédiaire et de production pour prendre en charge le déploiement Web.
> - Configurez les processus d’intégration continue (CI) Team Foundation Server (TFS) pour prendre en charge le déploiement Web automatisé.
> - Déployez des applications Web à l’échelle de l’entreprise dans différents environnements serveur avec différentes exigences et restrictions.
> - Déployez les modifications apportées aux applications Web qui s’exécutent dans différents environnements de serveurs.
> 
> > [!NOTE]
> > Bien que ces didacticiels décrivent l’utilisation de TFS en tant que serveur d’intégration continue, l’aide est facilement adaptée à n’importe quel serveur CI. Vous n’avez pas besoin d’une connaissance détaillée de TFS pour comprendre et tirer parti des didacticiels.
> 
> 
> Pour une traduction italienne de ces didacticiels, visitez [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="about-the-authors"></a>À propos des auteurs

Jason Lee est un technicien principal de [content Master](http://www.contentmaster.com/) , où il travaille avec des produits et technologies Microsoft, en particulier SharePoint et ASP.net, depuis plusieurs années. Jason détient un doctorat en informatique et est actuellement certifié MCPD et MCTS. Vous pouvez lire le blog technique de Jason à l’adresse [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin est un technicien principal de la [maîtrise du contenu](http://www.contentmaster.com/) qui a écrit des livres blancs, de la documentation du kit de développement logiciel, des présentations PowerPoint et des cours de formation en ligne et animés par un instructeur pendant sa carrière. Membre d’origine de l’équipe de documentation ASP.NET, il a travaillé avec les technologies Web de Microsoft depuis plus de dix ans.

## <a name="target-audience"></a>Public visé

Cet ensemble de didacticiels est destiné aux développeurs d’applications Web ASP.NET et aux architectes de solutions qui utilisent Visual Studio 2010 pour créer des applications Web à l’échelle de l’entreprise. Pour tirer le meilleur parti du contenu, vous devez être familiarisé avec l’utilisation de Visual Studio 2010 et avoir une connaissance de base de TFS, ainsi qu’une connaissance des technologies de plateforme Web Microsoft comme ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Projets de base de données de serveur et Visual Studio. Toutefois, vous n’avez pas besoin de vous familiariser avec les outils et les technologies de déploiement ou vous devez savoir comment configurer les systèmes CI.

## <a name="requirements"></a>Configuration requise

Pour suivre les procédures pas à pas et effectuer les tâches décrites dans ces didacticiels, vous devez installer ce logiciel sur votre ordinateur de développement :

- Visual Studio 2010 édition Premium ou Ultimate avec Service Pack 1
- .NET Framework 4.0
- .NET Framework 3,5 avec Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Pour effectuer les étapes de déploiement décrites dans ces procédures pas à pas, vous devez avoir accès aux exemples d’environnements de déploiement d’applications Web. Pour de meilleurs résultats, ces environnements doivent refléter le modèle de déploiement d’entreprise de votre organisation. Vous pouvez ensuite modifier les procédures pas à pas fournies dans cette documentation pour refléter les environnements de déploiement et les exigences de votre organisation.

## <a name="series-contents"></a>Contenu de la série

Cette section d’introduction se compose de deux rubriques supplémentaires. Ils sont conçus pour fournir un contexte plus large pour les didacticiels suivants :

- [Déploiement Web d’entreprise : vue d’ensemble du scénario](enterprise-web-deployment-scenario-overview.md). Cette rubrique décrit le scénario qui sous-tend chacun des didacticiels de cette série. Ce scénario se concentre sur les exigences de la Application Lifecycle Management (ALM) d’une société fictive nommée Fabrikam, Inc. au fur et à mesure qu’elle développe une application Web à l’échelle de l’entreprise.
- [Application Lifecycle Management : du développement à la production](application-lifecycle-management-from-development-to-production.md). Cette rubrique fournit une vue d’ensemble détaillée d’un processus de déploiement de bout en bout. Il illustre comment Fabrikam, Inc. déplace une application Web ASP.NET à l’échelle de l’entreprise via des environnements de test, intermédiaire et de production dans le cadre d’un processus de développement continu.

La série comprend quatre ensembles de didacticiels. Chaque porte sur différents aspects du déploiement Web :

- [Déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ce didacticiel fournit une introduction conceptuelle sur les fichiers projet MSBuild, le pipeline de publication Web, Web Deploy et d’autres technologies associées. Il explique comment vous pouvez utiliser ces outils ensemble pour gérer des processus de déploiement complexes.
- [Configuration des environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ce didacticiel explique comment configurer des serveurs Windows pour prendre en charge différents scénarios de déploiement, y compris le déploiement de packages Web à distance à l’aide du service de Deployment Agent Web (l’agent distant) ou du gestionnaire de Web Deploy et du déploiement de base de données distante. Il fournit des conseils sur le choix de la méthode de déploiement appropriée pour votre propre environnement et décrit comment utiliser WFF pour répliquer des applications Web déployées sur tous les serveurs Web d’une batterie de serveurs.
- [Configuration de Team Foundation Server pour le déploiement Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ce didacticiel explique comment configurer TFS pour prendre en charge différents scénarios de déploiement, y compris le déploiement automatisé dans le cadre d’un processus d’intégration continue et déclencher manuellement des déploiements de builds spécifiques.
- [Déploiement Web d’entreprise avancé](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ce didacticiel explique comment effectuer diverses tâches de déploiement plus avancées, telles que la personnalisation des déploiements de bases de données pour plusieurs environnements, à l’exclusion de fichiers et de dossiers du déploiement, et la mise hors connexion des applications Web pendant le processus de déploiement. .

## <a name="where-to-start"></a>Emplacement de départ

Cet ensemble de didacticiels utilise un exemple de solution avec un niveau de complexité réaliste, ainsi qu’un scénario de déploiement d’entreprise fictif, pour fournir une implémentation de référence et pour donner un contexte commun aux tâches et aux procédures. La rubrique suivante, [déploiement Web d’entreprise : vue d’ensemble du scénario](enterprise-web-deployment-scenario-overview.md), présente le scénario et l’exemple de solution. À partir de là, vous pouvez utiliser les didacticiels et les rubriques qui correspondent le mieux à vos besoins.

> [!div class="step-by-step"]
> [Next](enterprise-web-deployment-scenario-overview.md)
