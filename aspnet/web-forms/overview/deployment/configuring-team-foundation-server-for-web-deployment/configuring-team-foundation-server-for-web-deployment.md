---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Configuration de Team Foundation Server pour le déploiement Web | Microsoft Docs
author: jrjlee
description: Ce didacticiel vous montre comment configurer Team Foundation Server (TFS) 2010 pour générer des solutions et déployer du contenu Web dans différents environnements cibles. ...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 638d696abbc5f05957c0ed2eb7ebb65fce7813ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528391"
---
# <a name="configuring-team-foundation-server-for-web-deployment"></a>Configuration de Team Foundation Server pour le déploiement web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ce didacticiel vous montre comment configurer Team Foundation Server (TFS) 2010 pour générer des solutions et déployer du contenu Web dans différents environnements cibles. Cela comprend les scénarios d’intégration continue (CI), où vous déployez automatiquement le contenu chaque fois qu’un développeur apporte une modification. Il peut également inclure des scénarios de déclencheurs manuels, dans lesquels un administrateur peut déclencher le déploiement d’une build spécifique dans un environnement intermédiaire une fois que la Build a été vérifiée et validée dans l’environnement de test. Les rubriques de ce didacticiel vous guideront tout au long du processus de configuration, notamment :
> 
> - Comment créer un nouveau projet d’équipe dans TFS.
> - Comment ajouter du contenu au contrôle de code source.
> - Comment configurer un serveur de builds pour prendre en charge l’intégration continue et le déploiement.
> - Comment créer une définition de build incluant une logique de déploiement.
> - Comment configurer des autorisations pour un déploiement automatisé.
> 
> Pour une traduction italienne de ces didacticiels, visitez [http://www.lucamorelli.it](http://www.lucamorelli.it).

Ce didacticiel part du principe que vous avez installé TFS 2010 et créé une collection de projets d’équipe dans le cadre du processus de configuration initial. Le [Guide d’installation de Team Foundation pour Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) fournit des conseils complets sur ces tâches.

## <a name="context"></a>Contexte

Cette section fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de&#x2014;gestion des [contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md), dans lequel le processus de génération est&#x2014;contrôlé par deux fichiers projet contenant des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="scenario-overview"></a>Vue d’ensemble du scénario

Le scénario de haut niveau pour ces didacticiels est décrit dans [déploiement Web d’entreprise : vue d’ensemble du scénario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Nous vous recommandons de consulter cette rubrique avant de commencer à utiliser ce didacticiel.

## <a name="how-to-use-this-tutorial"></a>Comment utiliser ce didacticiel

S’il s’agit de la première fois que vous effectuez les tâches décrites dans ce didacticiel, ou si vous souhaitez suivre les exemples à l’aide de l’exemple de solution, vous devez parcourir les rubriques du didacticiel dans l’ordre. Vous pouvez également utiliser des rubriques individuelles comme aide pour des tâches spécifiques. Ce didacticiel comprend les rubriques suivantes :

- [Création d’un projet d’équipe dans TFS](creating-a-team-project-in-tfs.md). Un projet d’équipe est l’unité principale du contrôle de code source, de la gestion des processus et de la build dans TFS. Vous devez créer un projet d’équipe avant de pouvoir ajouter du contenu au contrôle de code source ou créer des définitions de Build.
- [Ajout de contenu au contrôle de code source](adding-content-to-source-control.md). Une fois que vous avez créé un projet d’équipe, vous pouvez commencer à ajouter du contenu au contrôle de code source. Vous devez ajouter vos projets et solutions, ainsi que toutes les dépendances externes, avant de pouvoir commencer à configurer des builds.
- [Configuration d’un serveur de builds TFS pour le déploiement Web](configuring-a-tfs-build-server-for-web-deployment.md). Si vous souhaitez générer le contenu de votre projet d’équipe, vous devez configurer un serveur de builds. Dans la plupart des cas, il doit s’agir d’un ordinateur distinct de votre installation TFS. Pour configurer un serveur de builds, vous devez installer et configurer le service de build TFS, installer Visual Studio 2010, créer des contrôleurs de build et des agents de build, installer tous les produits ou composants dont votre code a besoin pour générer correctement et installer le Outil de déploiement Web Internet Information Services (IIS) (Web Deploy).
- [Création d’une définition de build qui prend en charge le déploiement](creating-a-build-definition-that-supports-deployment.md). Avant de commencer à mettre en file d’attente ou de déclencher des builds dans TFS, vous devez créer au moins une définition de build pour votre projet d’équipe. La définition de build définit tous les aspects de la génération, y compris les éléments qui doivent être inclus dans la build, ce qui doit déclencher la génération, et où Team Build doit envoyer les sorties de la génération. Vous pouvez configurer une définition de build pour exécuter des fichiers projet Microsoft Build Engine (MSBuild) personnalisés, ce qui vous permet d’inclure la logique de déploiement dans vos builds automatisées.
- [Déploiement d’une build spécifique](deploying-a-specific-build.md). Dans de nombreux scénarios, vous souhaiterez déployer une build spécifique, plutôt que la version la plus récente, dans un environnement cible. Dans ce cas, vous pouvez configurer une définition de build qui déploie le contenu à partir d’un dossier de dépôt spécifique.
- [Configuration des autorisations pour le déploiement Team Build](configuring-permissions-for-team-build-deployment.md). Si le service de build doit déployer du contenu dans le cadre d’un processus de génération automatisé, vous devez accorder diverses autorisations au compte de service de build sur tous les serveurs Web et serveurs de base de données de destination.

## <a name="key-technologies"></a>Technologies clés

Ce didacticiel se concentre sur l’utilisation de ces produits et technologies pour prendre en charge le déploiement automatisé de la génération et du Web :

- Visual Studio Team Foundation Server 2010
- Team Build et MSBuild
- Web Deploy

Ce didacticiel aborde également l’utilisation de Windows Server 2008 R2, IIS 7,5, SQL Server 2008 R2, ASP.NET 4,0 et ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Autres didacticiels de cette série

Ce formulaire fait partie d’une série de cinq didacticiels sur le déploiement Web à l’échelle de l’entreprise. Voici les autres didacticiels de la série :

- [Déploiement d’applications Web dans des scénarios d’entreprise](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ce contenu de présentation fournit l’arrière-plan contextuel de la série de didacticiels. Il décrit le scénario du didacticiel et illustre comment les tâches et les procédures pas à pas décrites dans la série s’intègrent à un processus plus large Application Lifecycle Management (ALM).
- [Déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ce didacticiel fournit une introduction conceptuelle sur les fichiers projet MSBuild, le pipeline de publication Web (WPP), Web Deploy et d’autres technologies associées. Il explique comment vous pouvez utiliser ces outils ensemble pour gérer des processus de déploiement complexes.
- [Configuration des environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ce didacticiel explique comment configurer des serveurs Windows pour prendre en charge différents scénarios de déploiement, y compris le déploiement de packages Web à distance à l’aide du service de Deployment Agent Web (l’agent distant) ou du gestionnaire de Web Deploy et du déploiement de base de données distante. Il fournit des conseils sur le choix de la méthode de déploiement appropriée pour votre propre environnement et décrit comment utiliser l’infrastructure de batterie de serveurs Web (WFF) pour répliquer des applications Web déployées sur tous les serveurs Web d’une batterie de serveurs.
- [Déploiement Web d’entreprise avancé](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ce didacticiel explique comment effectuer diverses tâches de déploiement plus avancées, telles que la personnalisation des déploiements de bases de données pour plusieurs environnements, à l’exclusion de fichiers et de dossiers du déploiement, et la mise hors connexion des applications Web pendant le processus de déploiement. .

> [!div class="step-by-step"]
> [Next](creating-a-team-project-in-tfs.md)
