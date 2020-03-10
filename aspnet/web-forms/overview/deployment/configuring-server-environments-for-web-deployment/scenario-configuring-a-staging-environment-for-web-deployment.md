---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scénario : configuration d’un environnement intermédiaire pour le déploiement Web | Microsoft Docs'
author: jrjlee
description: Cette rubrique décrit un scénario de déploiement Web classique pour un environnement intermédiaire et explique les tâches à effectuer pour configurer un env similaire...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: eaa61ca850817f8dd98955b59e94be93389bf256
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637843"
---
# <a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scénario : configuration d’un environnement intermédiaire pour le déploiement Web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit un scénario de déploiement Web classique pour un environnement intermédiaire et explique les tâches à effectuer pour configurer un environnement similaire.

Un grand nombre d’organisations utilisent des environnements intermédiaires pour afficher un aperçu des mises à jour des applications ou sites Web. Cela permet aux utilisateurs au sein de l’organisation d’explorer et de passer en revue les nouvelles fonctionnalités ou le contenu avant le déploiement du site « en direct » ou en d’autres termes dans un environnement de production. L’environnement intermédiaire est conçu pour répliquer l’environnement de production le plus fidèlement possible, afin de fournir une version préliminaire réaliste. Ce type d’environnement intermédiaire présente généralement les caractéristiques suivantes :

- L’environnement se compose de plusieurs serveurs Web avec équilibrage de charge et d’un ou plusieurs serveurs de base de données, souvent avec le clustering de basculement et la mise en miroir de bases de données.
- Les applications peuvent être déployées manuellement par une équipe de développement ou automatiquement par un serveur Team Build.
- Il est peu probable que les comptes d’utilisateurs ou de processus qui déploient des applications disposent de privilèges d’administrateur sur les serveurs de test.
- Les modifications apportées aux applications sont régulièrement déployées, de sorte que l’environnement doit prendre en charge un déploiement à étape unique ou automatisé.

> [!NOTE]
> La montée en charge d’un déploiement de base de données sur plusieurs serveurs dépasse le cadre de ce didacticiel. Pour plus d’informations sur ce sujet, consultez [documentation en ligne de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).

Par exemple, dans notre [scénario de didacticiel](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) gère la solution du gestionnaire de contacts. L’administrateur TFS, Rob Walters, a créé une définition de build qui permet aux développeurs de déclencher un déploiement vers l’environnement intermédiaire en fonction des besoins.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Notez que dans la plupart des cas, vous ne souhaiterez pas nécessairement déployer la version la plus récente dans l’environnement intermédiaire. Au lieu de cela, vous êtes beaucoup plus susceptible de vouloir déployer une build spécifique qui a déjà été validée et vérifiée dans l’environnement de test.

## <a name="solution-overview"></a>Vue d'ensemble de la solution

Dans ce scénario, vous pouvez déduire ces faits à partir d’une analyse des spécifications du déploiement :

- Le compte d’utilisateur ou de processus qui effectue le déploiement ne dispose pas de privilèges d’administrateur sur les serveurs de test, donc les serveurs Web intermédiaires doivent prendre en charge le déploiement non-administrateur. Par conséquent, vous devez configurer les serveurs Web intermédiaires pour qu’ils utilisent le gestionnaire de Web Deploy plutôt que l’agent distant.
- L’environnement intermédiaire comprend plusieurs serveurs Web, mais il doit prendre en charge un déploiement en un seul clic ou automatisé. vous devez donc utiliser l’infrastructure de batterie de serveurs Web (WFF) pour créer une batterie de serveurs. À l’aide de cette approche, vous pouvez déployer une application sur un serveur Web (le serveur principal) et WFF répliquera le déploiement sur tous les autres serveurs Web de l’environnement intermédiaire.
- Le compte d’utilisateur ou de processus qui effectue le déploiement doit avoir les autorisations nécessaires pour créer des bases de données. Par conséquent, vous devez ajouter le compte au rôle de serveur **dbcreator** sur le serveur de base de données, en plus de configurer le serveur de base de données pour prendre en charge l’accès et le déploiement à distance.

Ces rubriques fournissent toutes les informations dont vous avez besoin pour effectuer les tâches suivantes :

- [Créer une batterie de serveurs avec l’infrastructure de batterie de serveurs Web](creating-a-server-farm-with-the-web-farm-framework.md). Cette rubrique explique comment créer et configurer une batterie de serveurs à l’aide de WFF, afin que les composants et produits de la plateforme Web, les paramètres de configuration, les sites Web et les applications soient répliqués sur plusieurs serveurs Web à charge équilibrée.
- [Configurez un serveur Web pour la publication Web Deploy (gestionnaire de Web Deploy)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Cette rubrique explique comment créer un serveur Web qui prend en charge Web Deploy la publication, à l’aide de l’approche de l’agent distant, à partir d’une version propre à Windows Server 2008 R2.
- [Configurez un serveur de base de données pour la publication Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). Cette rubrique explique comment configurer un serveur de base de données pour prendre en charge l’accès et le déploiement à distance, à partir d’une installation par défaut de SQL Server 2008 R2.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la configuration d’un environnement de test de développeur standard, consultez [scénario : configuration d’un environnement de test pour le déploiement Web](scenario-configuring-a-test-environment-for-web-deployment.md). Pour obtenir des conseils sur la configuration d’un environnement de production classique, consultez [scénario : configuration d’un environnement de production pour le déploiement Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Précédent](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Suivant](scenario-configuring-a-production-environment-for-web-deployment.md)
