---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Application Lifecycle Management : du développement à la production | Microsoft Docs'
author: jrjlee
description: Cette rubrique montre comment une société fictive gère le déploiement d’une application Web ASP.NET via des environnements de test, intermédiaire et de production, comme par...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 230cf4393db0ee19cfc42ed54359d61e7926a49d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640664"
---
# <a name="application-lifecycle-management-from-development-to-production"></a>Application Lifecycle Management : du développement à la production

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique montre comment une société fictive gère le déploiement d’une application Web ASP.NET via des environnements de test, intermédiaire et de production dans le cadre d’un processus de développement continu. Tout au long de cette rubrique, vous trouverez des liens vers des informations supplémentaires et des procédures pas à pas sur la façon d’effectuer des tâches spécifiques.
> 
> Cette rubrique est conçue pour fournir une vue d’ensemble de haut niveau pour une [série de didacticiels](deploying-web-applications-in-enterprise-scenarios.md) sur le déploiement Web dans l’entreprise. Ne vous inquiétez pas si vous n’êtes pas familiarisé avec certains des&#x2014;concepts décrits ici, les didacticiels suivants fournissent des informations détaillées sur toutes ces tâches et techniques.
> 
> > [!NOTE]
> > Par souci de simplicité, cette rubrique n’aborde pas la mise à jour des bases de données dans le cadre du processus de déploiement. Toutefois, effectuer des mises à jour incrémentielles des fonctionnalités des bases de données est une exigence de nombreux scénarios de déploiement d’entreprise, et vous trouverez des conseils sur la façon de procéder plus loin dans cette série de didacticiels. Pour plus d’informations, consultez [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md).

## <a name="overview"></a>Présentation

Le processus de déploiement illustré ici est basé sur le scénario de déploiement de Fabrikam, Inc. décrit dans [déploiement Web d’entreprise : vue d’ensemble du scénario](enterprise-web-deployment-scenario-overview.md). Vous devez lire la vue d’ensemble du scénario avant d’étudier cette rubrique. Fondamentalement, le scénario examine la manière dont une organisation gère le déploiement d’une application Web raisonnablement complexe, la [solution de gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), par le biais de différentes phases dans un environnement d’entreprise classique.

À un niveau élevé, la solution gestionnaire de contacts passe par ces étapes dans le cadre du processus de développement et de déploiement :

1. Un développeur examine du code dans Team Foundation Server (TFS) 2010.
2. TFS génère le code et exécute tous les tests unitaires associés au projet d’équipe.
3. TFS déploie la solution dans l’environnement de test.
4. L’équipe de développement vérifie et valide la solution dans l’environnement de test.
5. L’administrateur de l’environnement intermédiaire effectue un déploiement « en cas de problème » dans l’environnement intermédiaire, afin d’établir si le déploiement entraîne des problèmes.
6. L’administrateur de l’environnement intermédiaire effectue un déploiement en direct vers l’environnement intermédiaire.
7. La solution subit des tests d’acceptation utilisateur dans l’environnement intermédiaire.
8. Les packages de déploiement Web sont importés manuellement dans l’environnement de production.

Ces étapes font partie d’un cycle de développement continu.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

Dans la pratique, le processus est légèrement plus compliqué, comme vous le verrez lorsque nous examinerons chaque étape plus en détail. Fabrikam, Inc. utilise une approche différente du déploiement pour chaque environnement cible.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Le reste de cette rubrique examine les étapes clés de ce cycle de vie du déploiement :

- **Conditions préalables**: comment vous devez configurer votre infrastructure de serveur avant de mettre en place votre logique de déploiement.
- **Développement et déploiement initiaux**: ce que vous devez faire avant de déployer votre solution pour la première fois.
- **Déploiement à tester**: Comment empaqueter et déployer le contenu dans un environnement de test automatiquement lorsqu’un développeur Archive un nouveau code.
- **Déploiement dans**un environnement intermédiaire : comment déployer des builds spécifiques dans un environnement intermédiaire et comment effectuer des déploiements de type « que si » pour s’assurer qu’un déploiement ne provoque pas de problèmes.
- **Déploiement en production**: Comment importer des packages Web dans un environnement de production lorsque l’infrastructure réseau empêche le déploiement à distance.

## <a name="prerequisites"></a>Conditions préalables requises

La première tâche dans un scénario de déploiement consiste à s’assurer que votre infrastructure de serveur répond aux exigences de vos outils et techniques de déploiement. Dans ce cas, Fabrikam, Inc. a configuré son infrastructure de serveur comme suit :

- TFS est configuré pour inclure une collection de projets d’équipe, des contrôleurs de build et des agents de Build. Pour plus d’informations, consultez [configuration d’Team Foundation Server pour le déploiement Web automatisé](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) .
- L’environnement de test est configuré pour accepter les déploiements distants à l’aide du service Web Deployment Agent (l’agent distant), comme décrit dans [scénario : configuration d’un environnement de test pour le déploiement Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) et [configuration d’un serveur Web pour la publication Web Deploy (agent distant)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- L’environnement intermédiaire est configuré pour accepter les déploiements distants à l’aide du point de terminaison du gestionnaire de Web Deploy, comme décrit dans [scénario : configuration d’un environnement intermédiaire pour le déploiement Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) et [configuration d’un serveur Web pour la publication Web Deploy (gestionnaire de Web Deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- L’environnement de production est configuré pour permettre à un administrateur d’importer manuellement des packages de déploiement Web dans Internet Information Services (IIS), comme décrit dans [scénario : configuration d’un environnement de production pour le déploiement Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) et [configuration d’un serveur Web pour la publication Web Deploy (déploiement hors connexion)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Développement et déploiement initiaux

Pour que l’équipe de développement Fabrikam, Inc. puisse déployer la solution du gestionnaire de contacts pour la première fois, elle doit effectuer les tâches suivantes :

- Créez un nouveau projet d’équipe dans TFS.
- Créez les fichiers de projet Microsoft Build Engine (MSBuild) qui contiennent la logique de déploiement.
- Créez les définitions de build TFS qui déclenchent les processus de déploiement.

### <a name="create-a-new-team-project"></a>Créer un nouveau projet d’équipe

- L’administrateur TFS, Rob Walters, crée un nouveau projet d’équipe pour l’application, comme décrit dans [création d’un projet d’équipe dans TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Ensuite, le développeur principal, Matt Hink, crée une solution squelette. Il Archive ses fichiers dans le nouveau projet d’équipe dans TFS, comme décrit dans [Ajout de contenu au contrôle de code source](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Créer la logique de déploiement

Matt Hink crée différents fichiers projet MSBuild personnalisés, à l’aide de l’approche fractionner le fichier projet décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt crée :

- Un fichier projet nommé *Publish. proj* qui exécute le processus de déploiement. Ce fichier contient des cibles MSBuild qui génèrent les projets de la solution, créent des packages Web et déploient les packages dans un environnement de serveur de destination.
- Fichiers projet spécifiques à l’environnement nommés *env-dev. proj* et *env-stage. proj*. Celles-ci contiennent des paramètres spécifiques à l’environnement de test et à l’environnement intermédiaire, respectivement, comme les chaînes de connexion, les points de terminaison de service et les détails du service distant qui recevra le package Web. Pour obtenir des conseils sur le choix des paramètres appropriés pour des environnements de destination spécifiques, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Pour exécuter le déploiement, un utilisateur exécute le fichier *Publish. proj* à l’aide de MSBuild ou Team Build et spécifie l’emplacement du fichier projet spécifique à l’environnement approprié (*env-dev. proj* ou *env-stage. proj*) comme argument de ligne de commande. Le fichier *Publish. proj* importe ensuite le fichier projet spécifique à l’environnement pour créer un jeu complet d’instructions de publication pour chaque environnement cible.

> [!NOTE]
> La façon dont ces fichiers de projet personnalisé fonctionnent est indépendante du mécanisme que vous utilisez pour appeler MSBuild. Par exemple, vous pouvez utiliser directement la ligne de commande MSBuild, comme décrit dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Vous pouvez exécuter les fichiers projet à partir d’un fichier de commandes, comme décrit dans [créer et exécuter un fichier de commandes de déploiement](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Vous pouvez également exécuter les fichiers projet à partir d’une définition de build dans TFS, comme décrit dans [création d’une définition de build qui prend en charge le déploiement](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> Dans chaque cas, le résultat final est le&#x2014;même MSBuild exécute le fichier projet fusionné et déploie votre solution dans l’environnement cible. Vous bénéficiez ainsi d’une grande flexibilité dans la façon dont vous déclenchez votre processus de publication.

Une fois que vous avez créé les fichiers de projet personnalisés, Matt les ajoute à un dossier de solution et les vérifie dans le contrôle de code source.

### <a name="create-build-definitions"></a>Créer les définitions de build

En tant que tâche de préparation finale, Matt et Rob fonctionnent ensemble pour créer trois définitions de build pour le nouveau projet d’équipe :

- **DeployToTest**. Cela génère la solution du gestionnaire de contacts et le déploie dans l’environnement de test chaque fois qu’un archivage se produit.
- **DeployToStaging**. Cela permet de déployer des ressources à partir d’une build précédente spécifiée dans l’environnement intermédiaire lorsqu’un développeur met en file d’attente la Build.
- **DeployToStaging-WhatIf**. Cela effectue un déploiement « What If » dans l’environnement intermédiaire lorsqu’un développeur met en file d’attente la Build.

Les sections suivantes fournissent plus de détails sur chacune de ces définitions de Build.

## <a name="deployment-to-test"></a>Déploiement à tester

L’équipe de développement de Fabrikam, Inc. entretient des environnements de test pour effectuer diverses activités de test logiciel, telles que la vérification et la validation, le test de la convivialité, les tests de compatibilité et les tests ponctuels ou exploratoires.

L’équipe de développement a créé une définition de build dans TFS nommée **DeployToTest**. Cette définition de build utilise un déclencheur d’intégration continue, ce qui signifie que le processus de génération s’exécute chaque fois qu’un membre de l’équipe de développement Fabrikam, Inc. effectue un archivage. Quand une build est déclenchée, la définition de build :

- Générez la solution ContactManager. sln. Cela génère à son tour chaque projet dans la solution.
- Exécutez des tests unitaires dans la structure de dossiers de la solution (si la solution est générée avec succès).
- Exécutez les fichiers projet personnalisés qui contrôlent le processus de déploiement (si la solution est générée avec succès et passe tous les tests unitaires).

Le résultat final est que si la solution est générée avec succès et passe des tests unitaires, les packages Web et toutes les autres ressources de déploiement sont déployés dans l’environnement de test.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Comment le processus de déploiement fonctionne-t-il ?

La définition de build **DeployToTest** fournit ces arguments à MSBuild :

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]

Les propriétés de package **DeployOnBuild = true** et **DeployTarget =** sont utilisées quand Team Build génère les projets dans la solution. Quand le projet est un projet d’application Web, ces propriétés indiquent à MSBuild de créer un package de déploiement Web pour le projet. La propriété **TargetEnvPropsFile** indique au fichier *Publish. proj* où trouver le fichier projet spécifique à l’environnement à importer.

> [!NOTE]
> Pour obtenir une procédure pas à pas détaillée de la création d’une définition de build comme celle-ci, consultez [création d’une définition de build qui prend en charge le déploiement](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

Le fichier *Publish. proj* contient des cibles qui génèrent chaque projet de la solution. Toutefois, il comprend également la logique conditionnelle qui ignore ces cibles de génération si vous exécutez le fichier dans Team Build. Cela vous permet de tirer parti des fonctionnalités de génération supplémentaires offertes par Team Build, comme la possibilité d’exécuter des tests unitaires. Si la génération de la solution ou les tests unitaires échouent, le fichier *Publish. proj* n’est pas exécuté et l’application n’est pas déployée.

La logique conditionnelle est obtenue en évaluant la propriété **BuildingInTeamBuild** . Il s’agit d’une propriété MSBuild qui prend automatiquement la valeur **true** lorsque vous utilisez Team Build pour générer vos projets.

## <a name="deployment-to-staging"></a>Déploiement dans un environnement intermédiaire

Quand une build répond à l’ensemble des exigences de l’équipe de développement dans l’environnement de test, l’équipe peut vouloir déployer la même build dans un environnement intermédiaire. Les environnements intermédiaires sont généralement configurés pour correspondre le plus fidèlement possible aux caractéristiques de l’environnement de production ou « en direct », par exemple, en termes de spécifications de serveur, de systèmes d’exploitation, de logiciels et de configuration de réseau. Les environnements intermédiaires sont souvent utilisés pour les tests de charge, les tests d’acceptation des utilisateurs et les révisions internes plus larges. Les builds sont déployées dans l’environnement intermédiaire directement à partir du serveur de builds.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Les définitions de build utilisées pour déployer la solution dans l’environnement intermédiaire, **DeployToStaging-WhatIf** et **DeployToStaging**, partagent les caractéristiques suivantes :

- Ils ne génèrent en fait rien. Lorsque Rob déploie la solution dans l’environnement intermédiaire, il souhaite déployer une build existante spécifique qui a déjà été vérifiée et validée dans l’environnement de test. Les définitions de build doivent simplement exécuter les fichiers projet personnalisés qui contrôlent le processus de déploiement.
- Quand Rob déclenche une build, il utilise les paramètres de build pour spécifier la build qui contient les ressources qu’il souhaite déployer à partir du serveur de builds.
- Les définitions de build ne sont pas déclenchées automatiquement. Rob met en file d’attente manuellement une build lorsqu’il souhaite déployer la solution dans l’environnement intermédiaire.

Il s’agit du processus de haut niveau pour un déploiement vers l’environnement intermédiaire :

1. L’administrateur de l’environnement intermédiaire, Rob Walters, met en file d’attente une build à l’aide de la définition de build **DeployToStaging-WhatIf** . Rob utilise les paramètres de définition de build pour spécifier la build qu’il souhaite déployer.
2. La définition de build **DeployToStaging-WhatIf** exécute les fichiers projet personnalisés en mode « quoi si ». Cela génère des fichiers journaux comme si Rob effectuait un déploiement actif, mais il n’apporte aucune modification à l’environnement de destination.
3. Rob passe en revue les fichiers journaux pour déterminer les effets du déploiement sur l’environnement intermédiaire. En particulier, Rob souhaite vérifier ce qui sera ajouté, ce qui sera mis à jour et ce qui sera supprimé.
4. Si Rob est convaincu que le déploiement n’apporte aucune modification indésirable aux ressources ou aux données existantes, il met en file d’attente une build à l’aide de la définition de build **DeployToStaging** .
5. La définition de build **DeployToStaging** exécute les fichiers projet personnalisés. Celles-ci publient les ressources de déploiement sur le serveur Web principal dans l’environnement intermédiaire.
6. Le contrôleur de l’infrastructure de batterie de serveurs Web (WFF) synchronise les serveurs Web dans l’environnement intermédiaire. L’application est alors disponible sur tous les serveurs Web de la batterie de serveurs.

### <a name="how-does-the-deployment-process-work"></a>Comment le processus de déploiement fonctionne-t-il ?

La définition de build **DeployToStaging** fournit ces arguments à MSBuild :

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]

La propriété **TargetEnvPropsFile** indique au fichier *Publish. proj* où trouver le fichier projet spécifique à l’environnement à importer. La propriété **OutputRoot** remplace la valeur intégrée et indique l’emplacement du dossier de build qui contient les ressources que vous souhaitez déployer. Lorsque Rob met en file d’attente la build, il utilise l’onglet **paramètres** pour fournir une valeur mise à jour pour la propriété **OutputRoot** .

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Pour plus d’informations sur la création d’une définition de build comme celle-ci, consultez [déployer une build spécifique](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).

La définition de build **DeployToStaging-WhatIf** contient la même logique de déploiement que la définition de build **DeployToStaging** . Toutefois, il comprend l’argument supplémentaire **WhatIf = true**:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]

Dans le fichier *Publish. proj* , la propriété **WhatIf** indique que toutes les ressources de déploiement doivent être publiées en mode « quoi si ». En d’autres termes, les fichiers journaux sont générés comme si le déploiement avait eu son avance, mais rien n’est réellement modifié dans l’environnement de destination. Cela vous permet d’évaluer l’impact d’un déploiement&#x2014;proposé en particulier, ce qui sera ajouté, ce qui sera mis à jour et ce qui&#x2014;sera supprimé avant d’apporter des modifications.

> [!NOTE]
> Pour plus d’informations sur la configuration des déploiements de type « quels sont les cas », consultez [exécution d’un déploiement « What If »](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).

Une fois que vous avez déployé votre application sur le serveur Web principal dans l’environnement intermédiaire, WFF synchronise automatiquement l’application sur tous les serveurs de la batterie de serveurs.

> [!NOTE]
> Pour plus d’informations sur la configuration de WFF pour synchroniser des serveurs Web, consultez [créer une batterie de serveurs avec l’infrastructure de batterie](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md)de serveurs Web.

## <a name="deployment-to-production"></a>Déploiement en production

Quand une build a été approuvée dans l’environnement intermédiaire, l’équipe Fabrikam, Inc. peut publier l’application dans l’environnement de production. L’environnement de production est l’endroit où l’application devient « active » et atteint son public cible d’utilisateurs finaux.

L’environnement de production se trouve dans un réseau de périmètre accessible sur Internet. Il est isolé du réseau interne qui contient le serveur de builds. L’administrateur de l’environnement de production, Lisa Andrews, doit copier manuellement les packages de déploiement Web à partir du serveur de builds et les importer dans IIS sur le serveur Web de production principal.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Il s’agit du processus de haut niveau pour un déploiement vers l’environnement de production :

1. L’équipe des développeurs conseille à Lisa qu’une build est prête pour le déploiement en production. L’équipe conseille à Lisa l’emplacement des packages de déploiement Web dans le dossier de dépôt sur le serveur de builds.
2. Lisa collecte les packages Web à partir du serveur de builds et les copie sur le serveur Web principal dans l’environnement de production.
3. Lisa utilise le gestionnaire des services Internet pour importer et publier les packages Web sur le serveur Web principal.
4. Le contrôleur WFF synchronise les serveurs Web dans l’environnement de production. L’application est alors disponible sur tous les serveurs Web de la batterie de serveurs.

### <a name="how-does-the-deployment-process-work"></a>Comment le processus de déploiement fonctionne-t-il ?

Le gestionnaire des services Internet comprend un Assistant importation de package d’application qui facilite la publication de packages Web sur un site Web IIS. Pour obtenir une procédure pas à pas sur la façon d’effectuer cette procédure, consultez [installation manuelle de packages Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusion

Cette rubrique a fourni une illustration du cycle de vie du déploiement pour une application Web classique à l’échelle de l’entreprise.

Cette rubrique fait partie d’une série de didacticiels qui fournissent des conseils sur différents aspects du déploiement d’applications Web. Dans la pratique, il existe un grand nombre de tâches et de considérations supplémentaires à chaque étape du processus de déploiement, et il n’est pas possible de les couvrir dans une seule procédure pas à pas. Pour plus d’informations, consultez les didacticiels suivants :

- [Déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ce didacticiel fournit une introduction complète aux techniques de déploiement Web à l’aide de MSBuild et de l’outil de déploiement Web IIS (Web Deploy).
- [Configuration des environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ce didacticiel fournit des conseils sur la façon de configurer des environnements Windows Server pour prendre en charge différents scénarios de déploiement.
- [Configuration de Team Foundation Server pour le déploiement Web automatisé](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ce didacticiel fournit des conseils sur la façon d’intégrer la logique de déploiement dans les processus de build TFS.
- [Déploiement Web d’entreprise avancé](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ce didacticiel fournit des conseils sur la façon de répondre aux défis de déploiement les plus complexes auxquels font face les entreprises.

> [!div class="step-by-step"]
> [Précédent](enterprise-web-deployment-scenario-overview.md)
