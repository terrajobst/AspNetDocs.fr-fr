---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Installation manuelle des packages Web | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment importer manuellement un package de déploiement Web dans Internet Information Services (IIS). La rubrique Building and Packaging Web WC...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: f778549d3e26989a2e71ef21171adec521842729
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634203"
---
# <a name="manually-installing-web-packages"></a>Installation manuelle de packages web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment importer manuellement un package de déploiement Web dans Internet Information Services (IIS).
> 
> La rubrique [génération et empaquetage des projets d’application Web](building-and-packaging-web-application-projects.md) décrit comment l’outil de déploiement Web IIS (Web Deploy), conjointement avec le Microsoft Build Engine (MSBuild) et le pipeline de publication Web (WPP), vous permet d’empaqueter vos projets d’application Web dans un seul fichier zip. Ce fichier, communément appelé package de déploiement Web (ou simplement un package de déploiement), contient toutes les informations de configuration et de contenu nécessaires à IIS afin de recréer votre application Web sur un serveur Web.
> 
> Une fois que vous avez créé un package de déploiement Web, vous pouvez le publier sur un serveur IIS de différentes façons. Dans de nombreux scénarios, vous souhaiterez tirer parti des points d’intégration entre MSBuild, WPP et Web Deploy pour créer et installer des packages Web à distance dans le cadre d’un processus de génération et de déploiement automatisé ou en une seule étape. Ce processus est décrit dans [déploiement de packages Web](deploying-web-packages.md). Toutefois, cela n’est pas toujours possible. Supposons que vous souhaitiez déployer une application Web dans un environnement de production accessible sur Internet. Pour des raisons de sécurité, un tel environnement de production est tout au moins susceptible de se trouver derrière un pare-feu sur un sous-réseau distinct du serveur de builds, dans un réseau de périmètre (également appelé DMZ, zone démilitarisée et sous-réseau filtré). Dans de nombreux cas, l’environnement de production se trouve sur un domaine distinct ou sur un réseau isolé physiquement.
> 
> Dans ces scénarios, votre seule option consiste à porter le package Web sur le serveur de destination et à l’importer manuellement dans IIS. Bien que cette approche exclue le déploiement automatisé, il s’agit toujours d’une technique très efficace pour&#x2014;la publication d’une application Web. vous copiez simplement un fichier zip unique sur votre serveur Web et vous utilisez un Assistant pour vous guider tout au long du processus d’importation.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Vous devez effectuer ces tâches de haut niveau pour importer un package de déploiement Web dans IIS :

- Créez un package de déploiement Web à l’aide de la ligne de commande MSBuild, Team Build ou Visual Studio 2010.
- Copiez le package Web sur le serveur Web de destination.
- Utilisez l’Assistant importation de package d’application dans le gestionnaire des services Internet pour installer le package Web et fournir des valeurs pour les variables telles que les chaînes de connexion et les points de terminaison de service.

Cette rubrique vous montre comment effectuer ces procédures. Les tâches et procédures pas à pas de cette rubrique supposent que vous êtes déjà familiarisé avec les concepts qui sous-tendent les packages Web, les Web Deploy et le WPP. Pour plus d’informations, consultez [génération et empaquetage de projets d’application Web](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Cette rubrique est mieux utilisée conjointement avec [configurer un serveur Web pour la publication de Web Deploy (déploiement hors connexion)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), qui explique comment installer les composants requis et préparer un site Web IIS pour l’importation de package.

## <a name="create-a-web-deployment-package"></a>Créer un package de déploiement Web

La première tâche consiste à créer un package de déploiement Web pour le projet d’application Web que vous souhaitez déployer. Vous pouvez créer des packages Web de plusieurs façons.

**Approche 1 : créer un package dans le cadre du processus de génération avec Visual Studio**

Vous pouvez configurer votre projet d’application Web pour créer un package de déploiement Web après chaque génération via l’onglet **Package/Publication Web** sur les pages de propriétés du projet. Ce processus est décrit dans [génération et empaquetage de projets d’application Web](building-and-packaging-web-application-projects.md).

**Approche 2 : créer un package dans le cadre du processus de génération avec MSBuild**

Si vous générez votre projet d’application Web à l’aide de MSBuild directement, soit via un fichier projet MSBuild personnalisé, soit à partir de la ligne de commande, vous pouvez créer un package de déploiement Web dans le cadre du processus de génération en incluant les propriétés **DeployOnBuild = true** et **DeployTarget = package** dans votre commande. Ce processus est décrit dans [fonctionnement du processus de génération](understanding-the-build-process.md).

**Approche 3 : créer un package à la demande dans Visual Studio**

Vous pouvez créer un package de déploiement Web pour un projet d’application Web à tout moment dans Visual Studio 2010. Pour ce faire, dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur votre projet d’application Web, puis cliquez sur **générer un package de déploiement**.

![](manually-installing-web-packages/_static/image1.png)

**Approche 4 : créer un package à la demande à partir de la ligne de commande**

Vous pouvez créer un package de déploiement Web à partir de la ligne de commande en appelant la cible du **package** sur votre projet d’application Web à l’aide de MSBuild. La commande doit ressembler à ceci :

[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]

Quelle que soit l’approche que vous utilisez, le résultat final est le même. Le WPP crée un package de déploiement Web sous forme de fichier zip, ainsi que diverses ressources de prise en charge, dans le dossier de sortie de votre projet d’application Web.

![](manually-installing-web-packages/_static/image2.png)

Lorsque vous envisagez d’importer le package Web manuellement, vous n’avez besoin que du fichier zip. Copiez ce fichier sur votre serveur Web cible. vous pouvez commencer le processus d’importation.

## <a name="import-a-web-package-into-iis"></a>Importer un package Web dans IIS

Vous pouvez utiliser la procédure suivante pour importer un package de déploiement Web à partir du système de fichiers local vers un site Web IIS. Avant d’effectuer cette procédure, assurez-vous que vous disposez des éléments suivants :

- Le package de déploiement Web a été copié sur le serveur Web.
- Configuration d’un serveur Web IIS pour héberger votre application.

Pour plus d’informations sur la configuration d’un serveur Web IIS pour la prise en charge des packages de déploiement Web, consultez [configurer un serveur Web pour la publication de Web Deploy (déploiement hors connexion)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Pour importer un package de déploiement Web à l’aide du gestionnaire des services Internet**

1. Dans le gestionnaire des services Internet, dans le volet **connexions** , cliquez avec le bouton droit sur votre site Web IIS, pointez sur **déployer**, puis cliquez sur **importer l’application**.

    ![](manually-installing-web-packages/_static/image3.png)
2. Dans l’Assistant Importer un package d’application, dans la page **Sélectionner le package** , accédez à l’emplacement de votre package de déploiement Web, puis cliquez sur **suivant**.
3. Dans la page **Sélectionner le contenu du package** , effacez le contenu dont vous n’avez pas besoin, puis cliquez sur **suivant**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > Dans de nombreux cas, il se peut que vous ne souhaitiez pas importer tout ce qui est fourni avec un package de déploiement Web. Par exemple, il se peut que vous ne souhaitiez pas autoriser Web Deploy à remplacer la base de données associée.  
    > Les entrées d' **autorisations Grant** définissent des autorisations sur le système de fichiers de destination pour garantir que l’identité du pool d’applications peut accéder au dossier physique qui stocke le contenu du site Web. En outre, l’utilisateur de l’authentification anonyme dispose d’une autorisation de lecture sur le dossier pour permettre à l’application de fournir des fichiers de type MIME (Multipurpose Internet Mail Extensions). Si vous préférez, vous pouvez supprimer ces entrées et configurer les autorisations manuellement.
4. Sur la page entrer les informations sur le **package d’application** , fournissez les informations demandées.

    ![](manually-installing-web-packages/_static/image5.png)
5. Lorsque vous créez un package Web, le WPP analyse le fichier de configuration de votre application et détecte toutes les variables, telles que les chaînes de connexion et les points de terminaison de service. Dans ce cas :

    1. Le **chemin d’accès** de l’application est le chemin d’accès IIS dans lequel vous souhaitez installer votre application. Ce paramètre est commun à tous les packages de déploiement créés par le WPP.
    2. L' **adresse du point de terminaison du service ContactService** est l’adresse que l’application doit utiliser pour communiquer avec le service WCF déployé. Ce paramètre correspond à une entrée dans le fichier *Web. config* .
    3. Le premier paramètre de **chaîne de connexion** est la chaîne de connexion que Web Deploy devez utiliser pour déployer la base de données associée à l’application (dans ce cas, une base de données d’appartenance ASP.net). Ce paramètre correspond au paramètre de l’onglet **Package/Publication SQL** dans Visual Studio.
    4. Le deuxième paramètre de **chaîne de connexion** est la chaîne de connexion que votre application utilisera pour communiquer avec la base de données lorsqu’elle est en cours d’exécution. Cela correspond à une entrée de chaîne de connexion dans le fichier *Web. config* .

        > [!NOTE]
        > Pour plus d’informations sur l’origine de ces paramètres, consultez [Configuration des paramètres pour le déploiement de packages Web](configuring-parameters-for-web-package-deployment.md).
6. Cliquez sur **Next**.
7. Si ce n’est pas la première fois que vous déployez l’application sur ce site Web, vous êtes invité à spécifier si vous souhaitez supprimer tout le contenu existant avant l’installation. Choisissez l’option adaptée à vos besoins, puis cliquez sur **suivant**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Une fois l’installation du package terminée, cliquez sur **Terminer**.

    ![](manually-installing-web-packages/_static/image7.png)

À ce stade, vous avez correctement publié votre application Web sur IIS.

## <a name="conclusion"></a>Conclusion

Cette rubrique explique comment importer un package de déploiement Web dans un site Web IIS à l’aide du gestionnaire des services Internet. Cette approche de la publication d’applications Web est appropriée lorsque les contraintes de sécurité ou d’infrastructure rendent le déploiement à distance impossible ou indésirable.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la configuration d’un serveur Web IIS pour prendre en charge l’importation manuelle d’un package Web, consultez [configurer un serveur Web pour la publication de Web Deploy (déploiement hors connexion)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Pour plus d’informations générales sur le déploiement de packages Web, consultez [procédure pas à pas : déploiement d’un projet d’application Web à l’aide d’un package de déploiement Web (partie 1 sur 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Précédent](creating-and-running-a-deployment-command-file.md)
