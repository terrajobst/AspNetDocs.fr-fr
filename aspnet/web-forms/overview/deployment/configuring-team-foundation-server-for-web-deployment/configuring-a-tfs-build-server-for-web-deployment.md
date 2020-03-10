---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Configuration d’un serveur de builds TFS pour le déploiement Web | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment préparer un serveur de builds Team Foundation Server (TFS) pour générer et déployer vos solutions à l’aide de Team Build et d’Internet informat...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b3aaf7234706d149a3c784347528923f662c3511
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631095"
---
# <a name="configuring-a-tfs-build-server-for-web-deployment"></a>Configuration d’un serveur de builds TFS pour le déploiement web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment préparer un serveur de builds Team Foundation Server (TFS) pour générer et déployer vos solutions à l’aide de Team Build et de l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS).

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Pour préparer un serveur de builds à la création et au déploiement de vos solutions, vous devez :

- Installez et configurez le service de build TFS.
- Installez Visual Studio 2010.
- Installez tous les produits ou composants requis pour générer votre solution, tels que les versions du .NET Framework ou ASP.NET MVC.
- Installez Web Deploy 2,0 ou une version ultérieure.

Cette rubrique vous montre comment effectuer ces procédures ou pointer vers d’autres ressources où elles existent. Les tâches et procédures pas à pas de cette rubrique supposent que :

- Vous commencez avec une nouvelle génération de serveur exécutant Windows Server 2008 R2 Service Pack 1.
- Le serveur est joint à un domaine avec une adresse IP statique.
- Vous avez installé la couche application TFS sur un serveur distinct, comme décrit dans [déploiement Web d’entreprise : vue d’ensemble du scénario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Qui effectue ces procédures ?

Dans la plupart des cas, un administrateur TFS est responsable de la configuration des serveurs de builds. Dans certains cas, l’équipe de développement peut prendre possession de serveurs de build spécifiques.

## <a name="install-and-configure-the-tfs-build-service"></a>Installer et configurer le service de build TFS

Quand vous configurez un serveur de builds, votre première tâche consiste à installer et à configurer le service de build TFS. Dans le cadre de ce processus, vous devez :

- Installez le service de build TFS et configurez un compte de service. Toutes les tâches de génération, y compris le déploiement, sont exécutées à l’aide de l’identité du compte de service de Build.
- Créez un *contrôleur de build* et un ou plusieurs *agents de build*. Chaque contrôleur de build gère un ensemble d’agents de Build. Quand vous mettrez en file d’attente une build, le contrôleur de build assigne la tâche de génération à un agent de build disponible. Chaque collection de projets d’équipe dans TFS est mappée à un contrôleur de build unique.
- Configurez un dossier de dépôt pour vos sorties de génération. Il s’agit d’un partage réseau. Toutes les sorties de génération, comme les packages de déploiement Web, sont envoyées au dossier de dépôt.

Le chapitre [administration de Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) sur MSDN contient toutes les ressources dont vous avez besoin pour effectuer les tâches suivantes :

- Pour obtenir une vue d’ensemble conceptuelle de Team Foundation Build, notamment le service de build, les contrôleurs de build et les agents de build, consultez [Présentation du système Team Foundation Build](https://msdn.microsoft.com/library/dd793166.aspx).
- Pour plus d’informations sur l’installation et la configuration du service de build, consultez [configurer un ordinateur de build](https://msdn.microsoft.com/library/ms181712.aspx).
- Pour plus d’informations sur la création de contrôleurs de build, consultez [créer et utiliser un contrôleur de build](https://msdn.microsoft.com/library/ee330987.aspx).
- Pour plus d’informations sur la création d’agents de build, consultez [créer et utiliser des agents de build](https://msdn.microsoft.com/library/bb399135.aspx).
- Pour plus d’informations sur la création et la configuration de dossiers de dépôt, consultez [configurer des dossiers de dépôt](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Installer les produits et composants requis

Pour permettre au serveur de builds de générer vos solutions, vous devez installer les produits, les composants ou les assemblys requis par votre solution. Avant d’installer des composants de plateforme Web, vous devez installer Visual Studio 2010 (n’importe quelle version) sur le serveur de builds. Cela permet de s’assurer que les fichiers cibles de Microsoft Build Engine base (MSBuild) et les fichiers cibles du pipeline de publication Web (WPP) sont disponibles pour le service de Build. Le programme d’installation de Visual Studio doit également installer Web Deploy, dont vous aurez besoin si vous envisagez de déployer des packages Web dans le cadre de votre processus de génération.

La meilleure façon d’installer des composants de plateforme Web courants consiste à utiliser la [Web Platform Installer](https://go.microsoft.com/?linkid=9805118). Cela vous permet de vous assurer que vous installez la dernière version de chaque produit, et qu’il détecte et installe automatiquement les composants requis pour chaque produit. Dans le cas de la solution [Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , vous devez utiliser le Web Platform Installer pour installer ces produits et composants :

- **.NET Framework 4,0**. Cela est nécessaire pour exécuter des applications qui ont été générées sur cette version du .NET Framework.
- **Outil de déploiement Web 2,1 ou version ultérieure**. Cela installe Web Deploy (et son exécutable sous-jacent, MSDeploy. exe) sur votre serveur. Dans le cadre de ce processus, il installe et démarre le service Web Deployment Agent. Ce service vous permet de déployer des packages Web à partir d’un ordinateur distant.
- **ASP.NET MVC 3**. Cela installe les assemblys dont vous avez besoin pour exécuter les applications ASP.NET MVC 3.

**Pour installer les produits et composants requis**

1. Installez Visual Studio 2010. Lorsque vous êtes invité à sélectionner les fonctionnalités à installer, vous devez inclure les éléments suivants :

    1. Tous les langages de programmation que vous devez compiler.
    2. Visual Web Developer. Cela permet de s’assurer que les cibles WPP sont ajoutées à votre serveur de builds.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Une fois l’installation de Visual Studio 2010 terminée, téléchargez et installez [Visual studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (s’il n’est pas déjà inclus dans votre support d’installation).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 résout un bogue qui peut empêcher MSBuild de localiser l’exécutable MSDeploy.
3. Téléchargez et lancez le [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
4. En haut de la fenêtre **Web Platform Installer 3,0** , cliquez sur **produits**.
5. Sur le côté gauche de la fenêtre, dans le volet de navigation, cliquez sur **frameworks**.
6. Dans la ligne **Microsoft .NET Framework 4** , si le .NET Framework n’est pas déjà installé, cliquez sur **Ajouter**.

    > [!NOTE]
    > Vous avez peut-être déjà installé le .NET Framework 4,0 à Windows Update. Si un produit ou un composant est déjà installé, le Web Platform Installer le signale en remplaçant le bouton **Ajouter** par le texte **installé**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. Dans la ligne **ASP.NET MVC 3 (Visual Studio 2010)** , cliquez sur **Ajouter**.
8. Dans le volet de navigation, cliquez sur **serveur**.
9. Dans la ligne de l' **outil de déploiement Web 2,1** , cliquez sur **Ajouter**.
10. Cliquez sur **Suivant**. Le Web Platform Installer affiche une liste des produits&#x2014;avec les dépendances&#x2014;associées à installer et vous invite à accepter les termes du contrat de licence.
11. Passez en revue les termes du contrat de licence et, si vous en acceptez les termes, cliquez sur **J’accepte**.
12. Une fois l’installation terminée, cliquez sur **Terminer**, puis fermez la fenêtre **Web Platform Installer 3,0** .

> [!NOTE]
> Si votre processus de déploiement comprend l’utilisation d’outils comme VSDBCMD. exe ou SQLCMD. exe, vous devez vous assurer qu’ils sont installés sur votre serveur de builds. VSDBCMD. exe est un outil Visual Studio qui est généralement ajouté au serveur quand vous installez Team Foundation Build. SQLCMD. exe est un outil SQL Server. Vous pouvez télécharger une version autonome de SQLCMD. exe à partir de la page [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) .

## <a name="conclusion"></a>Conclusion

À ce stade, votre serveur de builds est prêt à commencer à créer et à déployer vos projets d’application Web. La rubrique suivante, [création d’une définition de build qui prend en charge le déploiement](creating-a-build-definition-that-supports-deployment.md), décrit comment créer et configurer une définition de build pour contrôler quand et comment vos projets sont générés et déployés.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations générales sur l’utilisation de Team Build, consultez [administration de Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Précédent](adding-content-to-source-control.md)
> [Suivant](creating-a-build-definition-that-supports-deployment.md)
