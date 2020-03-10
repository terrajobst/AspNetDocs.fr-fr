---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scénario : configuration d’un environnement de production pour le déploiement Web | Microsoft Docs'
author: jrjlee
description: Cette rubrique décrit un scénario de déploiement Web classique pour un environnement de production et explique les tâches que vous devez effectuer pour configurer un...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d76e715cdbf6ec484fa0ff98b3b3d1d8dfd3961
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635421"
---
# <a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scénario : configuration d’un environnement de production pour le déploiement Web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit un scénario de déploiement Web classique pour un environnement de production et explique les tâches à effectuer pour configurer un environnement similaire.

L’environnement de production est la destination finale d’une application Web ou d’un site Web. À ce stade, votre application a été testée, a été déployée dans un environnement intermédiaire et est prête à « devenir en ligne ». Les caractéristiques d’un environnement de production peuvent varier considérablement en fonction de la nature et de l’objectif de votre contenu Web, de la taille de votre organisation, de votre public cible et de nombreux autres facteurs. Dans un scénario à l’échelle de l’entreprise, l’environnement de production peut présenter les caractéristiques suivantes :

- L’environnement se compose de plusieurs serveurs Web avec équilibrage de charge et d’un ou plusieurs serveurs de base de données, souvent avec le clustering de basculement et la mise en miroir de bases de données.
- Si l’environnement est accessible sur Internet, il est probable qu’il soit séparé de votre réseau interne. Il peut se trouver sur un autre sous-réseau dans un réseau de périmètre, il peut se trouver sur un autre domaine et se trouver sur une infrastructure réseau entièrement différente.
- Il est très peu probable que les développeurs et les comptes de processus serveur de build disposent de privilèges d’administrateur sur les serveurs de production.
- Les modifications apportées aux applications sont déployées sur une base moins fréquente que les déploiements de test ou de mise en lots.

> [!NOTE]
> La montée en charge d’un déploiement de base de données sur plusieurs serveurs dépasse le cadre de ce didacticiel. Pour plus d’informations sur ce sujet, consultez [documentation en ligne de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).

Par exemple, dans notre [scénario de didacticiel](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), un serveur Team Build comprend des définitions de build qui permettent aux utilisateurs de générer la solution du gestionnaire de contacts et de la déployer dans un environnement intermédiaire en une seule étape. Lorsque l’application est prête à être déployée en production, en raison des contraintes imposées par les exigences de sécurité et de l’infrastructure réseau, l’administrateur de l’environnement de production doit copier manuellement le package Web sur un serveur Web de production et l’importer. dans le gestionnaire de Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Vue d'ensemble de la solution

Dans ce scénario, vous pouvez déduire ces faits à partir d’une analyse des spécifications du déploiement :

- En raison des restrictions de sécurité et de la configuration du réseau, vous ne pouvez pas configurer l’environnement de production pour prendre en charge un déploiement en un seul clic ou automatisé. Le déploiement hors connexion est la seule approche viable dans ce scénario.
- L’environnement de production comprend plusieurs serveurs Web, ce qui vous permet d’utiliser l’infrastructure de batterie de serveurs Web (WFF) pour créer une batterie de serveurs. À l’aide de cette approche, l’administrateur doit uniquement importer l’application sur un serveur Web (le serveur principal) et WFF répliquera le déploiement sur tous les autres serveurs Web de l’environnement de production.

Ces rubriques fournissent toutes les informations dont vous avez besoin pour effectuer les tâches suivantes :

- [Créer une batterie de serveurs avec l’infrastructure de batterie de serveurs Web](configuring-a-database-server-for-web-deploy-publishing.md). Cette rubrique explique comment créer et configurer une batterie de serveurs à l’aide de WFF, afin que les composants et produits de la plateforme Web, les paramètres de configuration, les sites Web et les applications soient répliqués sur plusieurs serveurs Web à charge équilibrée.
- [Configurez un serveur Web pour la publication de Web Deploy (déploiement hors connexion)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Cette rubrique explique comment créer un serveur Web qui permet aux administrateurs d’importer et de déployer des packages Web manuellement, en commençant par une version propre à Windows Server 2008 R2.
- [Configurez un serveur de base de données pour la publication Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). Cette rubrique explique comment configurer un serveur de base de données pour prendre en charge l’accès et le déploiement à distance, à partir d’une installation par défaut de SQL Server 2008 R2.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la configuration d’un environnement de test de développeur standard, consultez [scénario : configuration d’un environnement de test pour le déploiement Web](scenario-configuring-a-test-environment-for-web-deployment.md). Pour obtenir des conseils sur la configuration d’un environnement intermédiaire standard, consultez [scénario : configuration d’un environnement intermédiaire pour le déploiement Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Précédent](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Suivant](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
