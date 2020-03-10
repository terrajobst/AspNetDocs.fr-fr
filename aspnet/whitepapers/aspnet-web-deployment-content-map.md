---
uid: whitepapers/aspnet-web-deployment-content-map
title: Déploiement Web ASP.NET-ressources recommandées | Microsoft Docs
author: rick-anderson
description: Cette rubrique fournit des liens vers des ressources de documentation sur la manière de déployer (publier) des applications Web ASP.NET sur IIS à l’aide de Visual Studio 2010, Visual Web de...
ms.author: riande
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 96873c8f2b0ad2415f371aceb651400c801a3338
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636989"
---
# <a name="aspnet-web-deployment---recommended-resources"></a>Déploiement web ASP.NET - Ressources recommandées

> Cette rubrique fournit des liens vers des ressources de documentation sur la manière de déployer (publier) des applications Web ASP.NET sur IIS à l’aide de Visual Studio 2010, Visual Web Developer 2010 et versions ultérieures.
> 
> Si vous connaissez un billet de blog, un [StackOverflow](http://stackoverflow.com) thread ou tout autre lien utile, [envoyez-nous un e-mail](mailto:aspnetue@microsoft.com?subject=Deployment%20Content%20Map) avec le lien.
> 
> > [!NOTE] 
> > 
> > La plupart de ces ressources décrivent les fonctionnalités de déploiement disponibles uniquement si vous installez une version récente de la [mise à jour de publication Web de Visual Studio](https://go.microsoft.com/fwlink/?LinkID=208120). Certaines fonctionnalités sont disponibles uniquement dans Visual Studio 2012 ou Visual Studio 2013.

Cette rubrique contient les sections suivantes :

- [Comprendre les options de déploiement pour les projets Web](#understanding)
- [Recherche de fournisseurs d’hébergement pour une application ASP.NET](#findinghosting)
- [Déploiement d’une application Web à partir de Visual Studio](#fromvs)
- [Déploiement d’une application Web en créant et en installant un package de déploiement Web](#package)
- [Déploiement d’une application Web à l’aide d’un processus d’intégration continue (CI)](#ci)
- [Utilisation des transformations Web. config pour modifier les paramètres dans le fichier Web. config ou le fichier app. config de destination au cours du déploiement](#transforms)
- [Utilisation de paramètres de Web Deploy pour modifier les paramètres de l’application Web de destination lors du déploiement](#webdeployparms)
- [Vérification de la mise hors ligne d’une application pendant le déploiement](#appoffline)
- [Déploiement d’une base de données ou modifications d’une base de données dans le cadre du déploiement d’applications Web](#databasewithweb)
- [Déploiement d’une base de données séparément du déploiement d’applications Web](#databaseseparate)
- [Déploiement d’une application Web qui utilise des services d’application ASP.NET tels que l’appartenance et le profilage](#aspnetmembership)
- [Précompilation pour le déploiement](#precompiling)
- [Déploiement d’une application Web intranet](#intranet)
- [Automatisation des tâches de déploiement courantes qui ne sont pas automatisées dès l’emploi](#automating)
- [Configuration de serveurs Web pour permettre aux développeurs de déployer des applications Web à l’aide de Web Deploy](#configuringservers)
- [Configuration des serveurs pour un fournisseur d’hébergement](#hostingprovider)
- [Résolution des problèmes de déploiement](#troubleshooting)
- [Obtention d’aide sur une question de déploiement spécifique](#gettinghelp)
- [Ressources supplémentaires pour MSBuild](#additional)

<a id="understanding"></a>

## <a name="understanding-deployment-options-for-web-projects"></a>Comprendre les options de déploiement pour les projets Web

- [Présentation du déploiement Web pour Visual Studio et ASP.net](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Comment déployer un site Web Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Décrit les options et les liens vers les ressources pour le déploiement de projets Web sur les sites Web Windows Azure, y compris la [livraison continue](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatisée à partir du [contrôle de code source](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) et l’utilisation de Visual Studio.
- [Améliorations de la publication Web dans Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (vidéo de Scott Hanselman).
- [Vue d’ensemble du déploiement Web dans Visual studio 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog de Vishal Joshi). Un billet de blog plus ancien, mais certaines des ressources Visual Studio 2010 auxquelles il est lié ont des informations qui restent pertinentes pour Visual Studio 2012.

<a id="findinghosting"></a>

## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Recherche de fournisseurs d’hébergement pour une application ASP.NET

- [Hébergement ASP.NET](https://asp.net/hosting)

<a id="fromvs"></a>

## <a name="deploying-a-web-application-from-visual-studio"></a>Déploiement d’une application Web à partir de Visual Studio

- [Comment déployer un site Web Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explique les options et fournit des liens vers des ressources pour le déploiement de projets Web sur des sites Web Windows Azure. Contient une section relative au déploiement à partir de Visual Studio.
- [Déploiement web ASP.NET en utilisant Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). série de didacticiels en 12 parties, montre comment déployer des applications Web avec des bases de données SQL Serveres. Pour le déploiement de base de données utilise à la fois le fournisseur dbDacFx et Migrations Entity Framework Code First. Contient également des informations sur les [transformations de fichier Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), le [déploiement de fichiers individuels](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), le [déploiement de ligne de commande](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)et [la personnalisation du pipeline de publication de Visual Studio Web en modifiant les fichiers. pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). S’applique à tous les projets Web ASP.NET, y compris Web Forms, MVC et l’API Web.)
- [Comment : déployer un projet Web à l’aide de la publication en un clic dans Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (informations de référence sur l’Assistant Publication de sites Web Visual Studio.)
- [Déploiement d’une application Web ASP.net avec SQL Server Compact à l’aide de Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Il s’agit d’une version antérieure du **déploiement Web ASP.net à l’aide de Visual Studio** répertorié en haut de cette section. Principalement utile pour plus d’informations sur le déploiement de bases de données SQL Server Compact et sur la migration de SQL Server Compact vers une édition complète de SQL Server.
- [Application multiniveau .net utilisant des tables de stockage, des files d’attente et des objets BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (site Microsoft Azure). série de didacticiels en 5 parties, montre comment créer un projet MVC et le déployer dans un service Cloud Windows Azure.

<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Déploiement d’une application Web en créant et en installant un package de déploiement Web

- [Comment : créer un package de déploiement Web dans Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Comment : installer un package de déploiement à l’aide du fichier Deploy. cmd créé par Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Utilisation d’un package Web Deploy pour le déploiement sur IIS dans la zone dev et sur un ordinateur hôte tiers](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog de Sayed Hashimi). Comment utiliser le gestionnaire des services Internet pour installer un package de déploiement dans IIS sur l’ordinateur local et dans une société d’hébergement qui prend en charge le gestionnaire des services Internet pour l’administration à distance.
- [Génération d’un Package Web Deploy à partir de Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (site Web IIS.net). Contient des instructions pour la création et l’installation d’un package à partir de la ligne de commande.
- [Package une fois publié](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog de Sayed Hashimi). Introduit un package NuGet qui automatise le processus de transformation du fichier Web. config pour plusieurs environnements de destination, afin que vous puissiez déployer un package sur plusieurs serveurs. Consultez également la [vidéo PackageWeb](https://www.youtube.com/watch?v=-LvUJFI8CzM) par Sayed Hashimi.

Consultez également la section suivante.

<a id="ci"></a>

## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Déploiement d’une application Web à l’aide d’un processus d’intégration continue (CI)

- [Intégration continue et livraison continue (création d’applications Cloud réalistes avec Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Chapitre de livre électronique qui présente l’intégration continue et la livraison continue.
- [Comment déployer un site Web Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Décrit les options et les liens vers les ressources pour le déploiement de projets Web sur les sites Web Windows Azure. Contient une section sur l’automatisation du déploiement à partir du contrôle de code source.
- [Déploiement d’applications Web dans des scénarios d’entreprise](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). une série de didacticiels de 40, montre comment automatiser le déploiement dans un processus d’intégration à l’aide de Visual Studio 2010 et Team Foundation Server 2010.
- À l' [intérieur du Microsoft Build Engine : utilisation de MSBuild et Team Foundation Build, par Sayed Hashimi et William Bartholomew](http://msbuildbook.com). Il s’agit d’un livre, et non d’une ressource Web, mais il s’agit d’un guide essentiel pour apprendre à configurer MSBuild pour des scénarios d’intégration continue.
- [Pack d’extension MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Comprend des tâches de déploiement.
- [Guide de personnalisation de Team Foundation Build](https://aka.ms/vsarsolutions). La documentation de ALM Rangers sur la configuration de Team Foundation Server couvre le déploiement Web et inclut des didacticiels et des vidéos.
- [SLOWCHEETAH XML transforme à partir d’un serveur ci](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog de Sayed Hashimi). Explique comment utiliser SlowCheetah, un complément Visual Studio pour transformer App. config et d’autres fichiers XML.

Voir aussi [s’assurer qu’une application est hors ligne pendant le déploiement](aspnet-web-deployment-content-map.md#appoffline) , plus loin dans cette page.

<a id="transforms"></a>

## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Utilisation des transformations Web. config pour modifier les paramètres dans le fichier Web. config ou le fichier app. config de destination au cours du déploiement

- [Transformations de fichier Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Syntaxe de transformation Web. config pour le déploiement d’un projet Web à l’aide de Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Outils web 2012,2-transformations Web. config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (vidéo YouTube par Sayed Hashimi). Montre comment configurer et afficher un aperçu des transformations Web. config.
- [Comment faire désactiver la transformation Web. config ?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Quand dois-je utiliser Web Deploy paramètres au lieu des transformations Web. config ?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [Xdt (XML document transformation) publié sur CodePlex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog sur les outils et le développement Web .net). Annonce la disponibilité du code source pour le moteur de transformation de fichier Web. config et répertorie certains outils qui l’utilisent.
- [Sites Web Windows Azure : fonctionnement des chaînes d’application et des chaînes de connexion](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Une alternative aux transformations Web. config si votre environnement de destination est un site Web Windows Azure et que vous souhaitez transformer des `appSettings` ou des `connectionStrings`.

<a id="webdeployparms"></a>

## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Utilisation de paramètres de Web Deploy pour modifier les paramètres de l’application Web de destination lors du déploiement

- [Comment : utiliser des paramètres de Web Deploy dans un package de déploiement Web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy : comment mettre à jour les paramètres de l’application lors de la publication basée sur le profil de publication](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog de Sayed Hashimi). Montre comment intégrer des paramètres Web Deploy dans des profils de publication Visual Studio.
- [Paramétrage du Web Deploy](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (site Web IIS.net).
- [Web Deploy le paramétrage en action](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog de Vishal Joshi).
- [Web Deploy paramétrage et transformation Web. config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog de Vishal Joshi).
- [Sites Web Windows Azure : fonctionnement des chaînes d’application et des chaînes de connexion](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Une alternative aux paramètres Web Deploy si votre environnement de destination est un site Web Windows Azure et que vous souhaitez paramétrer `appSettings` ou `connectionStrings`.

<a id="appoffline"></a>

## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Vérification de la mise hors ligne d’une application pendant le déploiement

- [Déploiement Web ASP.net à l’aide de Visual Studio : déploiement d’une mise à jour de code](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Consultez la section **mettre l’application hors connexion pendant le déploiement.**
- Mise [hors connexion d’une application avant publication](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (site IIS.net). Décrit une fonctionnalité intégrée à Web Deploy 3,0 qui automatise la gestion d’une application\_fichier. htm hors connexion. Cette fonctionnalité ne fonctionne pas avec une application personnalisée\_les fichiers. htm hors connexion.
- [Comment mettre votre application Web hors connexion pendant la publication](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog de Sayed Hashimi). Comment automatiser le processus d’utilisation d’une application personnalisée\_fichier. htm hors connexion.
- [Mises à jour de publication Web pour l’application hors connexion et useCheckSum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog de développement Web Microsoft). Une autre option pour automatiser l’utilisation de l’application\_fichier. htm hors connexion.
- [Web Deploy 3,5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (site IIS.net). Nouvelle fonctionnalité de Web Deploy 3,5 pour l’application personnalisée\_les fichiers. htm hors connexion.

<a id="databasewithweb"></a>

## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Déploiement d’une base de données ou modifications d’une base de données dans le cadre du déploiement d’applications Web

- [Configuration du déploiement de base de données dans Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Vue d’ensemble des options de déploiement d’une base de données avec un projet Web.
- [Déploiement web ASP.NET en utilisant Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). une série de didacticiels en 12 parties, illustre le déploiement de base de données à l’aide du fournisseur dbDacFx et Migrations Entity Framework Code First.
- [Comment : déployer un projet Web à l’aide de la publication en un clic dans Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Déployez une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et SQL Database à un site Web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Didacticiel long qui crée et déploie une application qui utilise une base de données SQL Server unique pour l’appartenance et les données d’application.
- [Déploiement d’une application Web ASP.net avec SQL Server Compact à l’aide de Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). une série de didacticiels en 12 parties, montre comment déployer des bases de données SQL Server Compact et comment migrer de SQL Server Compact vers une édition complète de SQL Server.

Voir aussi déploiement d’une application Web en créant et en installant un package de déploiement Web et en déployant une application Web à l’aide d’un processus d’intégration continue (CI), plus haut dans cette page.

<a id="databaseseparate"></a>

## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Déploiement d’une base de données séparément du déploiement d’applications Web

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Inclusion de données dans un projet de base de données SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blog de l’équipe SQL Server Data Tools). Comment déployer à la fois le schéma et les données lors du déploiement d’une base de données.
- [Comment déployer une base de données sur Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (site Microsoft Azure)
- [Migration de bases de données vers Windows Azure SQL Database (anciennement SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migration d’une base de données vers SQL Azure à l’aide de SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blog de l’équipe SQL Server Data Tools).
- [Migration d’applications centrées sur les données vers Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migration de bases de données SQL Server vers Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).

<a id="aspnetmembership"></a>

## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Déploiement d’une application Web qui utilise des services d’application ASP.NET tels que l’appartenance et le profilage

- [Déployez une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et SQL Database à un site Web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Didacticiel long qui crée et déploie une application qui utilise une base de données SQL Server unique pour l’appartenance et les données d’application.
- [ASP.net Identity](https://asp.net/identity/). Ressources pour ASP.NET Identity.
- [Déploiement web ASP.NET en utilisant Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). série de didacticiels en 12 parties, montre comment déployer une base de données d’appartenance ASP.NET.
- [Configuration d’un site Web qui utilise services d’application](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Pour les projets de site Web, mais s’applique également aux projets d’application Web.
- [Utilisateurs et rôles sur le site Web de production](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Pour les projets de site Web, mais s’applique également aux projets d’application Web.

<a id="precompiling"></a>

## <a name="precompiling-for-deployment"></a>Précompilation pour le déploiement

- [Vue d’ensemble de la précompilation d’un projet d’application Web ASP.net](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Onglet Package/Publication Web, propriétés du projet](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Paramètres avancés de précompilation, boîte de dialogue](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).

<a id="intranet"></a>

## <a name="deploying-an-intranet-web-application"></a>Déploiement d’une application Web intranet

- [Utilisez l’option d’authentification organisationnelle locale (ADFS) avec ASP.net dans Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (blog de Vittorio Bertocci.).
- [Comment créer un site intranet à l’aide de ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Les anciennes procédures pas à pas écrites pour Visual Studio 2010 ne reflètent pas les modifications majeures apportées aux modèles de projet intranet introduits dans Visual Studio 2013.

<a id="automating"></a>

## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatisation des tâches de déploiement courantes qui ne sont pas automatisées dès l’emploi

- [Déploiement Web ASP.net à l’aide de Visual Studio : déploiement de fichiers supplémentaires](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Définition des autorisations de dossier sur publication Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog de Sayed Hashimi).
- [Comment étendre le fichier de cibles pour inclure des paramètres de Registre pour un package de projet Web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog des outils de développement Web).
- [Extension de la transformation XML (Web. config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog de Sayed Hashimi). Montre comment créer des transformations XDT personnalisées.
- Le [fournisseur personnalisé de l’outil de déploiement Web (MSDeploy) prend 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog de Sayed Hashimi). Montre comment créer un Web Deploy fournisseur personnalisé.
- [Comment empaqueter et déployer des composants com](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog des outils de développement Web).
- [Comment empaqueter des assemblys .net](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog des outils de développement Web). Comment déployer des assemblys dans le GAC.
- [Script Out tout : initialisez votre machine virtuelle Windows Azure pour votre serveur Web avec IIS, Web Deploy et d’autres choses](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog de Tugberk Ugurlu).

<a id="configuringservers"></a>

## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Configuration de serveurs Web pour permettre aux développeurs de déployer des applications Web à l’aide de Web Deploy

- [Installation et configuration de Web Deploy pour les déploiements administrateur et non-administrateur](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (site IIS.net).

<a id="hostingprovider"></a>

## <a name="configuring-servers-for-a-hosting-provider"></a>Configuration des serveurs pour un fournisseur d’hébergement

- [Guide de déploiement de l’hébergement Microsoft ASP.net 4](https://go.microsoft.com/fwlink/?LinkId=191365) (Centre de téléchargement Microsoft).
- [Générez un fichier XML de profil](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (site IIS.net).

<a id="troubleshooting"></a>

## <a name="troubleshooting-deployment-problems"></a>Résolution des problèmes de déploiement

- [Dépannage des sites Web Windows Azure dans Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (site Microsoft Azure).
- [Déploiement Web ASP.net à l’aide de Visual Studio : Dépannage](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Résolution des problèmes courants liés à Web Deploy](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Web Deploy codes d’erreur](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (site IIS.net).
- [FAQ sur le déploiement Web pour Visual Studio et ASP.net](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Principales différences entre IIS et le serveur de développement ASP.net](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Différences de configuration courantes entre le développement et la production](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hébergement des Applications ASP.net dans un niveau de confiance moyen](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 spécialistes du site Rolla).

<a id="gettinghelp"></a>

## <a name="getting-help-with-a-specific-deployment-question"></a>Obtention d’aide sur une question de déploiement spécifique

- [Forum sur la configuration et le déploiement de ASP.net](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).

<a id="additional"></a>

## <a name="additional-resources"></a>Ressources supplémentaires

Cette section fournit des liens vers des ressources supplémentaires qui sont utiles pour en savoir plus sur l’utilisation de Visual Studio et des outils de déploiement IIS.

Les blogs suivants contiennent souvent des informations sur le déploiement Web de Visual Studio :

- [Outils de développement Web sur le blog Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog de Sayed Hashimi](http://www.sedodream.com/).

Les ressources suivantes fournissent de la documentation sur Web Deploy, l’infrastructure IIS que Visual Studio utilise pour effectuer des tâches de déploiement de projet d’application Web. Vous pouvez poser des questions sur Web Deploy dans le Forum de l' [outil de déploiement Web](https://go.microsoft.com/fwlink/?LinkId=149411) sur le site Web IIS.net.

- [Présentation de Web Deploy](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Installation et configuration de Web Deploy](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Scripts PowerShell pour l’automatisation de l’installation de Web Deploy](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Outils de déploiement Web](https://go.microsoft.com/fwlink/?LinkId=151481). Nœud de la table des matières de niveau supérieur pour Web Deploy documentation sur le site TechNet. Contient des informations de référence utiles, mais la plupart des pages TechNet n’ont pas été mises à jour depuis des années.
- [Espace de noms Microsoft. Web. Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). La documentation de l’API n’a pas été mise à jour depuis la version 1,0.
- [Blog de l’équipe de déploiement Web Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Onglet publier sur le site web IIS.net](https://www.iis.net/learn/publish).
