---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Création et exécution d’un fichier de commandes de déploiement | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment générer un fichier de commandes qui vous permet d’exécuter un déploiement à l’aide de fichiers de projet Microsoft Build Engine (MSBuild) comme une seule étape...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: f1477ff423e4898385066a35b42503f3c70dcc68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634308"
---
# <a name="creating-and-running-a-deployment-command-file"></a>Création et exécution d’un fichier de commandes de déploiement

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment générer un fichier de commandes qui vous permet d’exécuter un déploiement à l’aide de fichiers de projet Microsoft Build Engine (MSBuild) en tant que processus reproductible à étape unique.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de&#x2014;gestion des [contacts](the-contact-manager-solution.md) pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du processus de génération](understanding-the-build-process.md), dans lequel le processus de génération est&#x2014;contrôlé par deux fichiers projet contenant des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="process-overview"></a>Vue d’ensemble du processus

Dans cette rubrique, vous allez apprendre à créer et exécuter un fichier de commandes qui utilise ces fichiers projet pour effectuer un déploiement reproductible dans votre environnement cible. Fondamentalement, le fichier de commandes doit simplement contenir une commande MSBuild qui :

- Indique à MSBuild d’exécuter le fichier *Publish. proj* agnostique à l’environnement.
- Indique au fichier *Publish. proj* quel fichier contient les paramètres de projet spécifiques à l’environnement et où le trouver.

## <a name="create-an-msbuild-command"></a>Créer une commande MSBuild

Comme décrit dans [la rubrique comprendre le processus de génération](understanding-the-build-process.md), le fichier&#x2014;projet spécifique à l’environnement, par exemple, *env-dev. proj*&#x2014;est conçu pour être importé dans le fichier *Publish. proj* non agnostique de l’environnement au moment de la génération. Ensemble, ces deux fichiers fournissent un ensemble complet d’instructions qui indiquent à MSBuild comment créer et déployer votre solution.

Le fichier *Publish. proj* utilise un élément **Import** pour importer le fichier projet spécifique à l’environnement.

[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]

Par conséquent, lorsque vous utilisez MSBuild. exe pour générer et déployer la solution du gestionnaire de contacts, vous devez :

- Exécutez MSBuild. exe sur le fichier *Publish. proj* .
- Spécifiez l’emplacement du fichier projet spécifique à l’environnement en fournissant un paramètre de ligne de commande nommé **TargetEnvPropsFile**.

Pour ce faire, votre commande MSBuild doit ressembler à ceci :

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]

À ce stade, il s’agit d’une étape simple pour passer à un déploiement reproductible à une seule étape. Il vous suffit d’ajouter votre commande MSBuild dans un fichier. cmd. Dans la solution du gestionnaire de contacts, le dossier de publication comprend un fichier nommé *Publish-dev. cmd* qui fait exactement cela.

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]

> [!NOTE]
> Le commutateur **/FL** indique à MSBuild de créer un fichier journal nommé *MSBuild. log* dans le répertoire de travail dans lequel MSBuild. exe a été appelé.

Pour déployer ou redéployer la solution du gestionnaire de contacts, il vous suffit d’exécuter le fichier *Publish-dev. cmd* . Quand vous exécutez le fichier, MSBuild :

- Générez tous les projets de la solution.
- Générez des packages Web pouvant être déployés pour les projets d’application Web.
- Générez des fichiers. dbschema et. DeployManifest pour les projets de base de données.
- Déployez les packages Web sur le serveur Web.
- Déployez la base de données sur le serveur de base de données.

## <a name="run-the-deployment"></a>Exécuter le déploiement

Une fois que vous avez créé un fichier de commandes pour votre environnement cible, vous devez être en mesure d’effectuer l’intégralité du déploiement en exécutant simplement le fichier.

**Pour déployer la solution gestionnaire de contacts dans votre environnement de test**

1. Sur votre station de travail de développeur, ouvrez l’Explorateur Windows, puis accédez à l’emplacement du fichier *Publish-dev. cmd* .
2. Double-cliquez sur le fichier pour l’exécuter.
3. Si une boîte de dialogue **ouvrir un fichier-Avertissement de sécurité** s’affiche, cliquez sur **exécuter**.
4. Si vos paramètres de configuration et vos serveurs de test sont correctement configurés, la fenêtre d’invite de commandes affiche un message **Build Succeeded** lorsque MSBuild a terminé le traitement des fichiers projet.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. S’il s’agit de la première fois que vous déployez la solution dans cet environnement, vous devez ajouter le compte d’ordinateur du serveur Web de test aux rôles de base de données **\_DataWriter** et **DB\_DataReader** sur la base de données **ContactManager** . Cette procédure est décrite dans [configurer un serveur de base de données pour la publication de Web Deploy](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Vous devez uniquement attribuer ces autorisations lorsque vous créez la base de données. Par défaut, le processus de génération ne recrée pas la base de données&#x2014;sur chaque déploiement à la place, il compare la base de données existante au schéma le plus récent et ne fait que les modifications requises. Par conséquent, vous devez uniquement mapper ces rôles de base de données la première fois que vous déployez la solution.
6. Ouvrez Internet Explorer et accédez à l’URL de l’application du gestionnaire de contacts (par exemple, `http://testweb1:85/ContactManager/`).
7. Vérifiez que l’application fonctionne comme prévu et que vous êtes en mesure d’ajouter des contacts.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusion

La création d’un fichier de commandes contenant vos instructions MSBuild vous offre un moyen simple et rapide de créer et de déployer une solution à projets multiples dans un environnement de destination spécifique. Si vous devez déployer à plusieurs reprises votre solution dans plusieurs environnements de destination, vous pouvez créer plusieurs fichiers de commandes. Dans chaque fichier de commande, la commande MSBuild génère le même fichier de projet universel, mais elle spécifie un autre fichier projet spécifique à l’environnement. Par exemple, un fichier de commandes à publier dans un environnement de développement ou de test peut contenir cette commande MSBuild :

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]

Un fichier de commandes à publier dans un environnement intermédiaire peut contenir cette commande MSBuild :

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]

> [!NOTE]
> Pour obtenir des conseils sur la personnalisation des fichiers projet spécifiques à l’environnement pour vos propres environnements de serveur, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Vous pouvez également personnaliser le processus de génération pour chaque environnement en remplaçant les propriétés ou en définissant différents commutateurs dans votre commande MSBuild. Pour plus d’informations, consultez Référence de la [ligne de commande MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Précédent](deploying-database-projects.md)
> [Suivant](manually-installing-web-packages.md)
