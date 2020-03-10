---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Déploiement Web d’entreprise : vue d’ensemble du scénario | Microsoft Docs'
author: jrjlee
description: Cet ensemble de didacticiels utilise un exemple de solution avec un niveau de complexité réaliste, ainsi qu’un scénario de déploiement d’entreprise fictif, pour fournir une référence...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 9786879844da13c21e6a953b1ab24b29ca8121e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574129"
---
# <a name="enterprise-web-deployment-scenario-overview"></a>Déploiement Web d’entreprise : vue d’ensemble du scénario

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cet ensemble de didacticiels utilise un exemple de solution avec un niveau de complexité réaliste, ainsi qu’un scénario de déploiement d’entreprise fictif, pour fournir une implémentation de référence et pour donner un contexte commun aux tâches et aux procédures. Cette rubrique décrit le scénario de didacticiel et présente l’exemple de solution.

## <a name="scenario-description"></a>Description du scénario

Fabrikam, Inc., société fictive, crée une solution qui permet aux équipes de vente à distance de stocker et de récupérer les informations de contact à partir d’une interface Web.

Les processus Application Lifecycle Management (ALM) de Fabrikam, Inc. exigent que la solution soit déployée sur trois environnements serveur à différentes étapes du processus de développement logiciel :

- Un test développeur ou un environnement « bac à sable ».
- Environnement intermédiaire basé sur l’intranet.
- Un environnement de production accessible sur Internet.

Chacun de ces environnements présente des exigences différentes en matière de configuration et de sécurité, et chacun d’entre eux pose des problèmes de déploiement uniques.

### <a name="the-fabrikam-inc-server-infrastructure"></a>L’infrastructure de serveur de Fabrikam, Inc.

Il s’agit de l’infrastructure de développement et de déploiement de haut niveau de Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Les stations de travail de développement, l’infrastructure de contrôle de code source, l’environnement de test des développeurs et l’environnement intermédiaire résident tous sur le réseau intranet au sein du domaine Fabrikam.net. L’environnement de production réside sur un réseau de périmètre (également appelé DMZ, zone démilitarisée et sous-réseau filtré), qui est isolé du réseau intranet par un pare-feu. Il s’agit d’un scénario de déploiement courant : vous isolez généralement vos serveurs Web accessibles sur Internet de votre infrastructure de serveur interne via l’utilisation de pare-feu ou de serveurs de passerelle.

Dans cet exemple :

- Un serveur Team Foundation Server (TFS) 2010 avec un serveur de builds distinct fournit le contrôle de code source et la fonctionnalité d’intégration continue (CI).
- L’environnement de test des développeurs comprend un serveur Web Internet Information Services (IIS) 7,5 et un serveur de base de données SQL Server 2008 R2.
- L’environnement de production comprend plusieurs serveurs Web IIS 7,5 synchronisés par un serveur de contrôleur WFF (Web Farm Framework), ainsi qu’un serveur de base de données SQL Server 2008 R2. Dans la pratique, le serveur de base de données peut utiliser le clustering ou la mise en miroir pour améliorer l’évolutivité et la disponibilité.
- L’environnement intermédiaire est conçu pour répliquer la configuration de l’environnement de production le plus fidèlement possible.
- Les stratégies d’isolement réseau et de pare-feu n’autorisent pas le déploiement direct et automatisé à partir de l’intranet vers le réseau de périmètre.

La configuration de chacun de ces environnements est décrite plus en détail dans le deuxième didacticiel, [Configuration des environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Rôles d’équipe pour ALM

Ces utilisateurs sont impliqués dans la création, la gestion, la création et la publication de la solution du gestionnaire de contacts :

- Matt Hink est développeur d’applications Web chez Fabrikam, Inc. Il fait partie de l’équipe qui a développé la solution gestionnaire de contacts à l’aide de Visual Studio 2010. Matt dispose de droits d’administrateur complets sur les serveurs de l’environnement de test des développeurs, ce qui lui permet de configurer l’environnement en fonction de ses besoins. Il dispose également d’un accès utilisateur à l’instance de Visual Studio 2010 TFS où il stocke le code source de la solution du gestionnaire de contacts.
- Rob Walters est un administrateur de serveur pour l’équipe de développement Fabrikam, Inc. Rob dispose d’un accès administratif sur le serveur TFS pour pouvoir configurer tous les aspects de TFS et Team Build. Rob dispose également d’un accès administratif aux serveurs Web de test et de mise en lots et agit en tant qu’administrateur de base de données pour les serveurs de base de données dans les environnements de test et intermédiaires. Rob a configuré Team Build sur le serveur TFS pour effectuer les tâches suivantes :

    - Générez et exécutez des tests unitaires sur l’application chaque fois qu’un utilisateur archive un fichier sur TFS. C’est ce que l’on appelle CI.
    - Déployez l’application du gestionnaire de contacts dans l’environnement de test automatiquement une fois que l’application a réussi les tests unitaires. Cela comprend la publication de la base de données sur les serveurs de test lors du déploiement initial et des mises à jour de la base de données après le déploiement initial.
    - Déployez l’application du gestionnaire de contacts dans l’environnement intermédiaire au cours d’un processus en une seule étape.
    - Créez un package Web qu’un administrateur de serveur Web et un administrateur de bases de serveurs peuvent utiliser pour publier l’application dans l’environnement de production.
- Lisa Andrews est un administrateur de serveur chargé de déployer des applications sur les serveurs de production Fabrikam, Inc. Elle dispose d’un accès en lecture au partage dans lequel TFS Team Build stocke le package de déploiement Web une fois qu’il a créé l’application du gestionnaire de contacts. Elle dispose également d’un accès administratif aux serveurs Web de production afin de pouvoir déployer l’application en production. En outre, elle agit en tant qu’administrateur de base de données qui déploie les bases de données et les mises à jour de base de données sur le serveur de base de données dans l’environnement de production.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>La solution Gestionnaire de contacts

La solution gestionnaire de contacts est conçue pour permettre aux utilisateurs enregistrés et connectés d’ajouter et de modifier les informations de contact via une interface Web. La solution gestionnaire de contacts se compose de quatre projets individuels :

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager. Mvc**. Il s’agit d’un projet d’application Web ASP.NET MVC3 qui représente le point d’entrée de la solution. Il offre des fonctionnalités d’application Web de base, telles que la possibilité pour les utilisateurs de créer et d’afficher les détails du contact. L’application s’appuie sur un service Windows Communication Foundation (WCF) pour gérer les contacts et une base de données de services d’application ASP.NET pour gérer l’authentification et l’autorisation.
- **ContactManager. base de données**. Il s’agit d’un projet de base de données Visual Studio 2010. Le projet définit le schéma d’une base de données qui stocke les détails du contact.
- **ContactManager. service**. Il s’agit d’un projet de service Web WCF. WCF expose un point de terminaison qui permet aux appelants d’effectuer des opérations de création, d’extraction, de mise à jour et de suppression (CRUD) sur la base de données du gestionnaire de contacts. Le service s’appuie sur la base de données du gestionnaire de contacts et l’assembly ContactManager. Common. dll.
- **ContactManager. Common**. Il s’agit d’un projet de bibliothèque de classes. Le service WCF s’appuie sur les types définis dans cet assembly.

Une révision complète de la solution et de ses exigences de déploiement est fournie dans le premier didacticiel de cette série, [déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Tâches de déploiement

Il existe plusieurs tâches distinctes impliquées dans le déploiement d’applications dans différents environnements d’une grande organisation. Voici les principales tâches que les didacticiels couvrent :

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Voici une liste de chaque étape du processus de déploiement du point de vue des utilisateurs décrits précédemment dans ce document :

1. Tous les membres de l’équipe examinent la solution du gestionnaire de contacts dans Visual Studio 2010 pour déterminer les exigences et problèmes de déploiement clés.
2. Matt Hink peut déployer la solution du gestionnaire de contacts directement depuis la station de travail de développement vers l’environnement de test des développeurs, pour effectuer un test initial de la logique de déploiement.
3. Matt Hink ajoute l’application au contrôle de code source dans TFS.
4. Rob Walters crée différentes définitions de build pour la solution gestionnaire de contacts dans Team Build. Une définition de build utilise CI pour déployer la solution dans l’environnement de test de développeur chaque fois qu’un utilisateur archive un nouveau code. Une autre définition de build permet aux utilisateurs de déclencher des déploiements dans l’environnement intermédiaire en fonction des besoins.
5. Chaque fois qu’un utilisateur archive un nouveau code, Team Build génère automatiquement les composants de la solution, exécute des tests unitaires et déploie la solution dans l’environnement de test du développeur si la génération a réussi et que les tests unitaires réussissent.
6. Lorsqu’un utilisateur déclenche un déploiement sur l’environnement intermédiaire, la solution est empaquetée et déployée dans un processus en une seule étape. Ce processus génère également un package pour un déploiement manuel vers l’environnement de production.
7. Lisa Andrews déploie l’application dans l’environnement de production en important manuellement le package Web créé à l’étape 6.

### <a name="key-deployment-issues"></a>Problèmes de déploiement clés

La solution du gestionnaire de contacts et le scénario Fabrikam, Inc. mettent en évidence les problèmes courants et les défis que vous pouvez rencontrer lorsque vous déployez des solutions complexes à l’échelle de l’entreprise. Exemple :

- Vous devez être en mesure de déployer des projets dans plusieurs environnements, tels que des environnements de développement ou de test, des plateformes intermédiaires et des serveurs de production. La solution doit être déployée avec des paramètres de configuration différents pour chaque environnement.
- Vous devez déployer plusieurs projets dépendants simultanément dans le cadre d’un processus de génération et de déploiement automatisé à étape unique.
- Vous devez être en mesure de conduire le déploiement à partir d’un processus automatisé. Par exemple, vous souhaitez utiliser un processus d’intégration continue pour déployer des applications Web dans un environnement intermédiaire lors de l’archivage d’un nouveau code.
- Vous devez être en mesure de contrôler le processus de déploiement et de définir des variables de déploiement en dehors de Visual Studio, car il est peu probable que les développeurs disposent des paramètres de configuration corrects ou des informations d’identification nécessaires pour chaque environnement cible.
- Vous devez déployer des projets de base de données basés sur un schéma et conserver les données existantes lors des déploiements suivants.
- Vous devez déployer les bases de données d’appartenance sur une base ad hoc sans déployer les données de compte d’utilisateur. Vous devrez peut-être également mettre à jour le schéma des bases de données d’appartenance déployées sans perdre les données de compte d’utilisateur existantes.
- Vous devez exclure certains fichiers ou dossiers lorsque vous déployez du contenu dans différents environnements cibles.

En outre, la gestion du déploiement lorsque les mises à jour sont fréquentes et incrémentielles lève quelques défis supplémentaires. Exemple :

- Vous exécutez des tests unitaires chaque fois qu’un développeur Archive un nouveau code. Vous souhaitez déployer la solution uniquement si le code réussit les tests unitaires.
- Lorsque vous déployez une application Web dans un environnement intermédiaire ou de production, vous souhaitez rediriger les utilisateurs vers une *application\_fichier. htm hors connexion* pendant la durée du processus de déploiement.
- Vous souhaitez consigner les activités de déploiement. Le processus de déploiement doit envoyer des notifications par courrier électronique de déploiements réussis ou ayant échoué à des destinataires désignés.
- Si un déploiement automatisé échoue, le processus de déploiement doit retenter le déploiement actuel ou déployer le package Web précédent à la place.

> [!div class="step-by-step"]
> [Précédent](deploying-web-applications-in-enterprise-scenarios.md)
> [Suivant](application-lifecycle-management-from-development-to-production.md)
