---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Ajout de contenu au contrôle de code source | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment ajouter du contenu au contrôle de code source dans Team Foundation Server (TFS) 2010. Il décrit comment ajouter des solutions et des projets à une équipe...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634462"
---
# <a name="adding-content-to-source-control"></a>Ajout de contenu au contrôle de code source

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment ajouter du contenu au contrôle de code source dans Team Foundation Server (TFS) 2010. Il décrit comment ajouter des solutions et des projets à un projet d’équipe dans TFS, et explique comment ajouter des dépendances externes, telles que des frameworks ou des assemblys, au contrôle de code source.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Dans la plupart des cas, chaque membre de l’équipe de développement doit être en mesure d’ajouter du contenu au contrôle de code source. Pour ajouter une solution au contrôle de code source dans TFS, vous devez effectuer ces étapes de haut niveau :

- Connectez-vous à un projet d’équipe.
- Mappez la structure de dossiers du projet d’équipe sur le serveur à une structure de dossiers sur votre ordinateur local.
- Ajoutez la solution et son contenu au contrôle de code source.
- Ajoutez toutes les dépendances externes au contrôle de code source.

Cette rubrique vous montre comment effectuer ces procédures.

Les tâches et procédures pas à pas de cette rubrique partent du principe que vous avez déjà créé un nouveau projet d’équipe TFS pour gérer votre contenu. Pour plus d’informations sur la création d’un projet d’équipe, consultez [création d’un projet d’équipe dans TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Qui effectue ces procédures ?

Dans la plupart des cas, chaque membre de l’équipe de développement doit être en mesure d’ajouter et de modifier du contenu dans des projets d’équipe spécifiques.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Se connecter à un projet d’équipe et créer un mappage de dossier

Avant d’ajouter du contenu au contrôle de code source, vous devez vous connecter à un projet d’équipe et créer un mappage entre la structure de dossiers sur le serveur et le système de fichiers sur votre ordinateur local.

**Pour se connecter à un projet d’équipe et mapper un chemin d’accès local**

1. Sur votre station de travail de développement, ouvrez Visual Studio 2010.
2. Dans Visual Studio, dans le menu **équipe** , cliquez sur **se connecter à Team Foundation Server**.

    > [!NOTE]
    > Si vous avez déjà configuré une connexion à un serveur TFS, vous pouvez omettre les étapes 3-6.
3. Dans la boîte de dialogue **connexion au projet d’équipe** , cliquez sur **serveurs**.
4. Dans la boîte de dialogue **Ajouter/supprimer des Team Foundation Server** , cliquez sur **Ajouter**.
5. Dans la boîte de dialogue **Ajouter un Team Foundation Server** , fournissez les détails de votre instance TFS, puis cliquez sur **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. Dans la boîte de dialogue **Ajouter/supprimer des Team Foundation Server** , cliquez sur **Fermer**.
7. Dans la boîte de dialogue **se connecter au projet d’équipe** , sélectionnez l’instance TFS à laquelle vous souhaitez vous connecter, sélectionnez la collection de projets d’équipe, sélectionnez le projet d’équipe auquel vous souhaitez ajouter, puis cliquez sur **se connecter**.

    ![](adding-content-to-source-control/_static/image2.png)
8. Dans la fenêtre **Team Explorer** , développez votre projet d’équipe, puis double-cliquez sur **contrôle de code source**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Sous l’onglet **Explorateur du contrôle de code source** , cliquez sur **non mappé**.

    ![](adding-content-to-source-control/_static/image4.png)
10. Dans la boîte de dialogue **mapper** , dans la zone **dossier local** , accédez à (ou créez) un dossier local pour agir en tant que dossier racine du projet d’équipe, puis cliquez sur **mapper**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Lorsque vous êtes invité à télécharger les fichiers sources, cliquez sur **Oui**.

    ![](adding-content-to-source-control/_static/image6.png)

À ce stade, vous avez mappé le dossier côté serveur du projet d’équipe vers un dossier local sur votre station de travail de développeur. Vous avez également téléchargé tout le contenu existant à partir du projet d’équipe dans la structure de vos dossiers locaux. Vous pouvez maintenant commencer à ajouter votre propre contenu au contrôle de code source.

## <a name="add-projects-and-solutions-to-source-control"></a>Ajouter des projets et des solutions au contrôle de code source

Pour ajouter des projets et des solutions au contrôle de code source, vous devez d’abord les déplacer vers le dossier mappé du projet d’équipe sur votre ordinateur local. Vous pouvez ensuite archiver le contenu pour synchroniser vos ajouts avec le serveur.

**Pour ajouter des projets au contrôle de code source**

1. Sur votre station de travail de développement, déplacez vos projets et solutions vers un emplacement approprié dans la structure de dossiers mappés pour le projet d’équipe.

    > [!NOTE]
    > De nombreuses organisations auront une approche privilégiée de l’Organisation des projets et des solutions dans le contrôle de code source. Pour obtenir des conseils sur la structure des dossiers, consultez [Comment : structurer vos dossiers de contrôle de code source dans Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Ouvrez la solution dans Visual Studio 2010.
3. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur la solution, puis cliquez sur **Ajouter la solution au contrôle de code source**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > Dans certains cas, selon la façon dont votre organisation aime le contenu de la structure dans TFS, vous devrez peut-être ajouter des projets au contrôle de code source individuellement pour mieux contrôler la façon dont votre code source est organisé.
4. Vérifiez que l’onglet **Explorateur du contrôle de code source** affiche le contenu que vous avez ajouté dans la structure de dossiers du serveur pour le projet d’équipe.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > L’onglet **Explorateur du contrôle de code source** affiche votre contenu sans invite supplémentaire, car vous avez ajouté votre solution à un dossier mappé sur le système de fichiers local. Si votre solution se trouvait dans un emplacement non mappé, vous serez invité à spécifier des emplacements de dossiers dans TFS et dans votre système de fichiers local.
5. Sous l’onglet **Explorateur du contrôle de code source** , dans le volet **dossiers** , cliquez avec le bouton droit sur le projet d’équipe (par exemple, **ContactManager**), puis cliquez sur **archiver les modifications en attente**.
6. Dans la boîte de dialogue **Archiver – fichiers sources** , tapez un commentaire, puis cliquez sur **Archiver**.

    ![](adding-content-to-source-control/_static/image9.png)

À ce stade, vous avez ajouté votre solution au contrôle de code source dans TFS.

## <a name="add-external-dependencies-to-source-control"></a>Ajouter des dépendances externes au contrôle de code source

Lorsque vous ajoutez un projet ou une solution au contrôle de code source, tous les fichiers et dossiers de votre projet ou de votre solution sont également ajoutés. Toutefois, dans de nombreux cas, les projets et les solutions s’appuient également sur des dépendances externes, comme les assemblys locaux, pour fonctionner correctement. Vous devez ajouter des ressources de ce type au contrôle de code source pour permettre à la fois à Team Build et à d’autres membres de l’équipe de développement de générer votre code avec succès.

Par exemple, la structure de dossiers de l’exemple de solution du gestionnaire de contacts comprend un dossier nommé packages. Contient l’assembly et diverses ressources de prise en charge pour le ADO.NET Entity Framework 4,1. Le dossier Packages ne fait pas partie de la solution gestionnaire de contacts, mais la solution ne sera pas correctement générée sans celui-ci. Pour permettre à Team Build de générer la solution, vous devez ajouter le dossier Packages au contrôle de code source.

> [!NOTE]
> L’inclusion d’un dossier Packages est typique de ce qui se passe lorsque vous ajoutez le Entity Framework, ou des ressources similaires, à votre solution à l’aide de l’extension NuGet pour Visual Studio 2010.

**Pour ajouter du contenu non-projet au contrôle de code source**

1. Assurez-vous que les éléments que vous souhaitez ajouter (par exemple, le dossier Packages) se trouvent à un emplacement approprié au sein d’un dossier mappé sur votre système de fichiers local.
2. Dans Visual Studio 2010, dans la fenêtre **Team Explorer** , développez votre projet d’équipe, puis double-cliquez sur **contrôle de code source**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Sous l’onglet **Explorateur du contrôle de code source** , dans le volet **dossiers** , sélectionnez le dossier qui contient l’élément ou les éléments que vous souhaitez ajouter.
4. Cliquez sur le bouton **Ajouter des éléments au dossier** .

    ![](adding-content-to-source-control/_static/image11.png)
5. Dans la boîte de dialogue **Ajouter au contrôle de code source** , sélectionnez le ou les éléments que vous souhaitez ajouter, puis cliquez sur **suivant**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Dans l’onglet **éléments exclus** , sélectionnez tous les éléments requis qui ont été automatiquement exclus (par exemple, les assemblys), puis cliquez sur **inclure**le ou les éléments.

    ![](adding-content-to-source-control/_static/image13.png)
7. Dans l’onglet **éléments à ajouter** , vérifiez que tous les fichiers que vous souhaitez inclure sont répertoriés, puis cliquez sur **Terminer**.

    ![](adding-content-to-source-control/_static/image14.png)
8. Dans la fenêtre **Explorateur du contrôle de code source** , cliquez sur le bouton **Archiver** .

    ![](adding-content-to-source-control/_static/image15.png)
9. Dans la boîte de dialogue **Archiver – fichiers sources** , tapez un commentaire, puis cliquez sur **Archiver**.

À ce stade, vous avez ajouté les dépendances externes de votre solution au contrôle de code source.

## <a name="conclusion"></a>Conclusion

Cette rubrique décrit comment se connecter à un projet d’équipe, mapper une structure de dossiers et ajouter du contenu au contrôle de code source. Pour plus d’informations sur l’utilisation des éléments sous contrôle de code source, consultez [utilisation du contrôle de version](https://msdn.microsoft.com/library/ms181368.aspx).

La rubrique suivante, [configuration d’un serveur de builds TFS pour le déploiement Web](configuring-a-tfs-build-server-for-web-deployment.md), explique comment préparer un serveur TFS Team Build pour la génération et le déploiement de votre solution.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des informations plus complètes sur l’utilisation du contrôle de code source dans TFS, consultez [utilisation du contrôle de version](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Précédent](creating-a-team-project-in-tfs.md)
> [Suivant](configuring-a-tfs-build-server-for-web-deployment.md)
