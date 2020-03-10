---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scénario : configuration d’un environnement de test pour le déploiement Web | Microsoft Docs'
author: jrjlee
description: Cette rubrique décrit un scénario de déploiement Web classique pour les environnements de développement ou de test, et décrit les tâches à effectuer pour configurer une si...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: d580e550f2461837f0e8a4e477273348b49cb53e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637815"
---
# <a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scénario : configuration d’un environnement de test pour le déploiement Web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit un scénario de déploiement Web classique pour les environnements de développement ou de test, et décrit les tâches que vous devez effectuer pour configurer un environnement similaire.

Lorsque les développeurs travaillent sur des applications Web, ils ont souvent accès à un environnement serveur qu’ils peuvent utiliser pour tester les modifications apportées à leurs applications dans un paramètre réaliste. Ce type d’environnement de développement ou de test présente généralement les caractéristiques suivantes :

- L’environnement se compose d’un serveur Web unique et d’un serveur de base de données unique.
- Les développeurs disposent généralement de privilèges d’administrateur sur les serveurs pour leur permettre de configurer l’environnement en fonction des besoins de leurs applications.
- Les modifications apportées aux applications sont régulièrement déployées, de sorte que l’environnement doit prendre en charge un déploiement à étape unique ou automatisé.

Par exemple, dans notre [scénario de didacticiel](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink est développeur chez Fabrikam, Inc. Matt travaille sur la solution du gestionnaire de contacts et doit régulièrement déployer des modifications dans un environnement de test. Matt est un administrateur sur le serveur Web de test et le serveur de base de données de test. Au départ, Matt doit pouvoir déployer la solution directement dans l’environnement de test.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

À mesure que le travail progresse et que plus de développeurs rejoignent l’équipe, la solution du gestionnaire de contacts est configurée pour l’intégration continue (CI) dans Team Foundation Server (TFS). Chaque fois qu’un développeur Archive du contenu, Team Build doit générer la solution, exécuter des tests unitaires et déployer automatiquement la solution dans l’environnement de test.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Vue d'ensemble de la solution

L’environnement de test doit prendre en charge un déploiement à étape unique ou automatisé à partir d’un ordinateur distant, ce qui vous permet de choisir deux approches principales. Vous pouvez :

- Configurez le serveur Web de test pour prendre en charge le déploiement à l’aide du service de Deployment Agent Web (l’agent distant).
- Configurez le serveur Web de test pour prendre en charge le déploiement à l’aide du gestionnaire de Web Deploy.

> [!NOTE]
> Vous pouvez également utiliser [Web Deploy à la demande](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (l’agent temporaire). Cela est similaire à l’approche de l’agent distant en termes d’exigences et de contraintes.

Dans ce cas, les développeurs disposent de privilèges d’administrateur sur les serveurs de destination, et l’environnement de test n’est pas soumis à des contraintes de sécurité strictes. par conséquent, le choix logique consiste à configurer le serveur Web de test pour prendre en charge le déploiement à l’aide de l’agent distant. Cela est moins complexe et nécessite moins de configuration initiale que l’approche du gestionnaire de Web Deploy. Vous devez également configurer votre serveur de base de données pour prendre en charge l’accès et le déploiement à distance.

Ces rubriques fournissent toutes les informations dont vous avez besoin pour effectuer les tâches suivantes :

- [Configurez un serveur Web pour la publication Web Deploy (agent distant)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Cette rubrique explique comment créer un serveur Web qui prend en charge Web Deploy la publication, à l’aide de l’approche de l’agent distant, à partir d’une version propre à Windows Server 2008 R2.
- [Configurez un serveur de base de données pour la publication Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). Cette rubrique explique comment configurer un serveur de base de données pour prendre en charge l’accès et le déploiement à distance, à partir d’une installation par défaut de SQL Server 2008 R2.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la configuration d’un environnement intermédiaire standard, consultez [scénario : configuration d’un environnement intermédiaire pour le déploiement Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Pour obtenir des conseils sur la configuration d’un environnement de production classique, consultez [scénario : configuration d’un environnement de production pour le déploiement Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Précédent](choosing-the-right-approach-to-web-deployment.md)
> [Suivant](scenario-configuring-a-staging-environment-for-web-deployment.md)
