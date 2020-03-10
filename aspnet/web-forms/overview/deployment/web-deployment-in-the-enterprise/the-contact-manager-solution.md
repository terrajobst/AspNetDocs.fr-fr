---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: La solution du gestionnaire de contacts | Microsoft Docs
author: jrjlee
description: Cette série de didacticiels utilise un exemple&#x2014;de solution de gestion&#x2014;de contact pour représenter une application à l’échelle de l’entreprise avec un feuillet R1 réaliste...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 12ed7827f7392e559e04121386f7cd045de8462b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586267"
---
# <a name="the-contact-manager-solution"></a>La solution Gestionnaire de contacts

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette [série de didacticiels](web-deployment-in-the-enterprise.md) utilise un exemple&#x2014;de solution la solution&#x2014;de gestionnaire de contacts pour représenter une application à l’échelle de l’entreprise avec un niveau de complexité réaliste. Cette rubrique présente la solution gestionnaire de contacts, décrit les principaux composants de la solution et identifie les défis liés au déploiement de ce type d’application sur différentes plateformes de destination dans un environnement d’entreprise.
> 
> Au fur et à mesure que vous parcourez les rubriques de ces didacticiels, vous pouvez utiliser la solution gestionnaire de contacts comme implémentation de référence qui montre comment vous pouvez répondre à des défis spécifiques dans les scénarios de déploiement d’entreprise. La rubrique suivante, [configuration de la solution du gestionnaire de contacts](setting-up-the-contact-manager-solution.md), explique comment télécharger et exécuter la solution sur votre station de travail de développeur.

## <a name="solution-overview"></a>Vue d'ensemble de la solution

La solution gestionnaire de contacts se compose de quatre projets individuels :

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager. Mvc**. Il s’agit d’un projet d’application Web ASP.NET MVC 3 qui représente le point d’entrée de la solution. Il offre des fonctionnalités d’application Web de base, telles que la possibilité pour les utilisateurs de créer et d’afficher les détails du contact. L’application s’appuie sur un service Windows Communication Foundation (WCF) pour gérer les contacts et une base de données de services d’application ASP.NET pour gérer l’authentification et l’autorisation.
- **ContactManager. base de données**. Il s’agit d’un projet de base de données Visual Studio. Le projet définit le schéma d’une base de données qui stocke les détails du contact.
- **ContactManager. service**. Il s’agit d’un projet de service Web WCF. Le service WCF expose un point de terminaison qui permet aux appelants d’effectuer des opérations de création, d’extraction, de mise à jour et de suppression (CRUD) sur la base de données **ContactManager** . Le service s’appuie sur la base de données **ContactManager** et l’assembly **ContactManager. Common. dll** .
- **ContactManager. Common**. Il s’agit d’un projet de bibliothèque de classes. Le service WCF s’appuie sur les types définis dans cet assembly.

La solution comprend également un dossier de solution nommé Publish. Il contient différents fichiers de projet et fichiers de commandes personnalisés qui montrent comment vous pouvez contrôler et manipuler le processus de génération et de déploiement. Ceux-ci sont traités plus en détail plus loin dans ce didacticiel.

À un niveau conceptuel, les composants de la solution s’intègrent comme suit :

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Tandis que l’application Web ASP.NET MVC 3 utilise le fournisseur d’appartenances ASP.NET, toutes les pages de l’application Web autorisent l’accès anonyme. Il ne s’agit pas d’une configuration réaliste. Toutefois, la solution est configurée de cette façon pour faciliter le déploiement et le test de la solution sans configurer les comptes d’utilisateur et les rôles.

## <a name="deployment-challenges"></a>Défis du déploiement

La solution gestionnaire de contacts illustre plusieurs défis de déploiement qui sont communs à un grand nombre de scénarios de déploiement d’entreprise :

- La solution est constituée de plusieurs projets dépendants. Vous devez déployer ces projets simultanément.
- Les chaînes de connexion et les points de terminaison de service doivent être mis à jour pour chaque environnement, et dans de nombreux cas, ces informations ne sont pas disponibles pour le développeur.
- Lorsque vous déployez la base de données **ContactManager** dans des environnements intermédiaires et de production, vous devez conserver les données existantes lors des déploiements suivants.
- Lorsque vous déployez la base de données des services d’application ASP.NET, vous devez déployer certaines données de configuration, mais omettre les données de compte d’utilisateur.
- Les projets incluent certains fichiers et dossiers qui ne doivent pas être déployés. Vous devez exclure ces fichiers et dossiers du processus de déploiement.
- La solution doit prendre en charge le déploiement automatisé à partir d’un serveur de builds Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusion

Cette rubrique a fourni une vue d’ensemble de haut niveau de la solution de gestion des contacts et a identifié certains des défis de déploiement inhérents qui sont communs à un grand nombre de scénarios de déploiement d’entreprise. Les autres rubriques de ce didacticiel décrivent certaines des techniques que vous pouvez utiliser pour répondre à ces défis.

La rubrique suivante, [configuration de la solution du gestionnaire de contacts](setting-up-the-contact-manager-solution.md), explique comment télécharger et exécuter la solution sur votre station de travail de développeur.

> [!div class="step-by-step"]
> [Précédent](web-deployment-in-the-enterprise.md)
> [Suivant](setting-up-the-contact-manager-solution.md)
