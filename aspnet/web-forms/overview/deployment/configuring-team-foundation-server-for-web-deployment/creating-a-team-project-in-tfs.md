---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Création d’un projet d’équipe dans TFS | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment créer un nouveau projet d’équipe dans Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639656"
---
# <a name="creating-a-team-project-in-tfs"></a>Créer un projet d’équipe dans TFS

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment créer un nouveau projet d’équipe dans Team Foundation Server (TFS) 2010.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Pour approvisionner et utiliser un nouveau projet d’équipe dans TFS, vous devez effectuer ces étapes de haut niveau :

- Accordez des autorisations à l’utilisateur qui créera le nouveau projet d’équipe.
- Créez le projet d’équipe.
- Accordez des autorisations aux membres de l’équipe qui vont travailler sur le projet.
- Archiver du contenu.

Cette rubrique vous montre comment effectuer ces procédures et identifie les utilisateurs et les rôles de travail qui sont susceptibles d’être responsables de chaque procédure. Sachez que, selon la structure de votre organisation, chacune de ces tâches peut être la responsabilité d’une autre personne.

Les tâches et procédures pas à pas de cette rubrique partent du principe que vous avez installé et configuré TFS, et que vous avez créé une collection de projets d’équipe dans le cadre du processus de configuration. Pour plus d’informations sur ces hypothèses et pour obtenir des informations générales plus générales sur le scénario, consultez [configurer un serveur de builds TFS pour le déploiement Web](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Accorder des autorisations au créateur du projet d’équipe

Pour créer un projet d’équipe, vous devez disposer des autorisations suivantes :

- Vous devez disposer de l’autorisation de **création de projets** sur la couche application TFS. Vous accordez généralement cette autorisation en ajoutant des utilisateurs au groupe TFS administrateurs de la **collection de projets** . Le groupe global **Team Foundation Administrators** comprend également cette autorisation.
- Vous devez avoir l’autorisation de créer des sites d’équipe dans la collection de sites SharePoint qui correspond à la collection de projets d’équipe TFS. Vous accordez généralement cette autorisation en ajoutant l’utilisateur à un groupe SharePoint avec des droits de **contrôle total** sur la collection de sites SharePoint.
- Si vous utilisez SQL Server Reporting Services fonctionnalités, vous devez être membre du rôle gestionnaire de **contenu Team Foundation** dans Reporting Services.

### <a name="who-performs-these-procedures"></a>Qui effectue ces procédures ?

En règle générale, la personne ou le groupe qui administre le déploiement TFS effectue également ces procédures.

Étant donné qu’il s’agit d’un jeu d’autorisations à privilèges élevés, les nouveaux projets d’équipe sont généralement créés par un petit sous-ensemble d’utilisateurs responsable de l’administration d’un déploiement TFS. Les développeurs ne bénéficient généralement pas des autorisations requises pour créer de nouveaux projets d’équipe.

### <a name="grant-permissions-in-tfs"></a>Accorder des autorisations dans TFS

Si vous souhaitez permettre à un utilisateur de créer des projets d’équipe, la première tâche de haut niveau consiste à ajouter l’utilisateur au groupe administrateurs de la **collection de projets** pour la collection de projets d’équipe.

**Pour ajouter un utilisateur au groupe administrateurs de la collection de projets**

1. Sur le serveur TFS, dans le menu **Démarrer** , pointez sur **tous les programmes**, cliquez sur **Microsoft Team Foundation Server 2010**, puis sur **console Administration Team Foundation**.
2. Dans l’arborescence de navigation, développez **couche application**, puis cliquez sur **collections de projets d’équipe**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. Dans le volet **collections de projets d’équipe** , sélectionnez la collection de projets d’équipe que vous souhaitez gérer.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Sous l’onglet **général** , cliquez sur **appartenance au groupe**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. Dans la boîte de dialogue **groupes globaux** , sélectionnez le groupe administrateurs de la **collection de projets** , puis cliquez sur **Propriétés**.
6. Dans la boîte de dialogue **Propriétés du groupe de Team Foundation Server** , sélectionnez **utilisateur ou groupe Windows**, puis cliquez sur **Ajouter**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs ou les groupes** , tapez le nom de l’utilisateur pour lequel vous souhaitez pouvoir créer des projets d’équipe, cliquez sur **vérifier les noms**, puis sur **OK**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. Dans la boîte de dialogue **Propriétés du groupe de Team Foundation Server** , cliquez sur **OK**.
9. Dans la boîte de dialogue **groupes globaux** , cliquez sur **Fermer**.

### <a name="grant-permissions-in-sharepoint-services"></a>Accorder des autorisations dans SharePoint Services

Ensuite, vous devez accorder à l’utilisateur l’autorisation de créer des sites d’équipe dans la collection de sites SharePoint qui correspond à votre collection de projets d’équipe TFS.

**Pour accorder des autorisations contrôle total sur la collection de sites SharePoint**

1. Dans la Console Administration Team Foundation Server, sur la page **collections de projets d’équipe** , sélectionnez la collection de projets d’équipe que vous souhaitez gérer.
2. Dans l’onglet **site SharePoint** , notez la valeur de l’URL d' **emplacement de site par défaut actuelle** .

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Ouvrez Internet Explorer, puis accédez à l’URL que vous avez notée à l’étape 2.

    > [!NOTE]
    > Si vous n’êtes pas connecté à Windows en tant qu’utilisateur qui a créé la collection de projets d’équipe, vous devez vous connecter à SharePoint en tant qu’utilisateur pour pouvoir continuer.
4. Dans le menu **Actions de site** , cliquez sur **Paramètres du site**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Sur la page **paramètres du site** , sous **utilisateurs et autorisations**, cliquez sur **personnes et groupes**.
6. Dans le volet de navigation de gauche, cliquez sur **groupes**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Dans la page **personnes et groupes : tous les groupes** , cliquez sur **configurer des groupes pour ce site**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Vous pouvez recevoir une erreur <strong>http 404 introuvable</strong> en raison d’un double bogue d’encodage http. Si cela se produit, remplacez l’URL par le suivant :   
   > `[site_collection_URL]/_layouts/permsetup.aspx` Par exemple :  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Dans la page **configurer les groupes pour ce site** , ajoutez l’utilisateur qui créera des projets d’équipe au groupe **propriétaires** , puis cliquez sur **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Pour plus d’informations sur l’activation des utilisateurs pour créer des projets d’équipe dans une collection de projets d’équipe, consultez [définir des autorisations d’administrateur pour les collections de projets d’équipe](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Créer un projet d’équipe et ajouter des utilisateurs

Une fois que vous disposez des autorisations nécessaires, vous pouvez utiliser la fenêtre **Team Explorer** dans Visual Studio 2010 pour créer un nouveau projet d’équipe. Cette approche fournit un assistant qui collecte toutes les informations requises et effectue les tâches nécessaires dans TFS, SharePoint et SQL Server Reporting Services. Vous devez également accorder des autorisations sur le nouveau projet d’équipe aux membres de l’équipe de développement, afin de leur permettre d’ajouter et de modifier du contenu.

### <a name="who-performs-these-procedures"></a>Qui effectue ces procédures ?

En général, un administrateur TFS ou un responsable de l’équipe de développement effectue ces procédures.

### <a name="create-a-new-team-project"></a>Créer un nouveau projet d’équipe

La procédure suivante décrit comment créer un nouveau projet d’équipe dans TFS 2010.

**Pour créer un nouveau projet d’équipe**

1. Dans le **menu Démarrer** , pointez **sur tous les programmes**, cliquez sur **Microsoft Visual Studio 2010**, cliquez avec le bouton droit sur **Microsoft Visual Studio 2010**, puis cliquez sur **exécuter en tant qu’administrateur**.

    > [!NOTE]
    > Si vous n’exécutez pas Visual Studio 2010 en tant qu’administrateur, l’Assistant Nouveau projet d’équipe échouera à la dernière étape.
2. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.
3. Dans Visual Studio, dans le menu **équipe** , cliquez sur **se connecter à Team Foundation Server**.

    > [!NOTE]
    > Si vous avez déjà configuré une connexion à un serveur TFS, vous pouvez omettre les étapes 4-7.
4. Dans la boîte de dialogue **connexion au projet d’équipe** , cliquez sur **serveurs**.
5. Dans la boîte de dialogue **Ajouter/supprimer des Team Foundation Server** , cliquez sur **Ajouter**.
6. Dans la boîte de dialogue **Ajouter un Team Foundation Server** , fournissez les détails de votre instance TFS, puis cliquez sur **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. Dans la boîte de dialogue **Ajouter/supprimer des Team Foundation Server** , cliquez sur **Fermer**.
8. Dans la boîte de dialogue **se connecter au projet d’équipe** , sélectionnez l’instance TFS à laquelle vous souhaitez vous connecter, sélectionnez la collection de projets d’équipe à laquelle vous souhaitez ajouter, puis cliquez sur **se connecter**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. Dans la fenêtre **Team Explorer** , cliquez avec le bouton droit sur la collection de projets d’équipe, puis cliquez sur **nouveau projet d’équipe**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. Dans la boîte de dialogue **nouveau projet d’équipe** , fournissez un nom et une description pour le projet d’équipe, puis cliquez sur **suivant**.

    > [!NOTE]
    > Si votre projet d’équipe comprend des espaces, vous risquez de rencontrer des problèmes lorsque vous utilisez l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy) pour déployer des packages à partir du chemin de sortie. Les espaces dans le chemin d’accès peuvent compliquer considérablement l’exécution des commandes Web Deploy.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Dans la page **Sélectionner un modèle de processus** , sélectionnez le modèle de processus que vous souhaitez utiliser pour gérer le processus de développement, puis cliquez sur **suivant**.

    > [!NOTE]
    > Pour plus d’informations sur les modèles de processus pour TFS, consultez [modèles de processus et outils](https://msdn.microsoft.com/vstudio/aa718795).
12. Sur la page **paramètres du site d’équipe** , laissez les paramètres par défaut inchangés, puis cliquez sur **suivant**.
13. Ce paramètre crée, ou identifie, un site d’équipe SharePoint associé au projet d’équipe TFS. Votre équipe de développement peut utiliser ce site pour gérer la documentation, participer à des thèmes de discussion, créer des pages wiki et effectuer diverses autres tâches qui ne sont pas liées au code. Pour plus d’informations, consultez [interactions entre les produits SharePoint et Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Dans la page **spécifier les paramètres du contrôle de code source** , laissez les paramètres par défaut inchangés, puis cliquez sur **suivant**.
15. Ce paramètre identifie ou crée l’emplacement dans la hiérarchie des dossiers TFS qui fera office de dossier racine pour votre contenu.
16. Sur la page **confirmer les paramètres du projet d’équipe** , cliquez sur **Terminer**.
17. Lorsque le nouveau projet d’équipe a été créé, dans la page **projet d’équipe créé** , cliquez sur **Fermer**.

### <a name="add-users-to-a-team-project"></a>Ajouter des utilisateurs à un projet d’équipe

Maintenant que vous avez créé le nouveau projet d’équipe, vous pouvez accorder des autorisations aux utilisateurs pour leur permettre de commencer à ajouter et à collaborer sur du contenu.

**Pour ajouter des utilisateurs à un projet d’équipe**

1. Dans Visual Studio 2010, dans la fenêtre **Team Explorer** , cliquez avec le bouton droit sur le projet d’équipe, pointez sur **paramètres du projet d’équipe**, puis cliquez sur appartenance au **groupe**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Pour permettre à un utilisateur d’ajouter, de modifier et de supprimer du code sous contrôle de code source, vous devez l’ajouter au groupe **Contributors** .
3. Dans la boîte de dialogue **groupes de projets** , sélectionnez le groupe **contributeurs** , puis cliquez sur **Propriétés**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. Dans la boîte de dialogue **Propriétés du groupe de Team Foundation Server** , sélectionnez **utilisateur ou groupe Windows**, puis cliquez sur **Ajouter**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs ou les groupes** , tapez le nom d’utilisateur de l’utilisateur que vous souhaitez ajouter au projet d’équipe, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. Dans la boîte de dialogue **Propriétés du groupe de Team Foundation Server** , cliquez sur **OK**.
7. Dans la boîte de dialogue **groupes de projets** , cliquez sur **Fermer**.

## <a name="conclusion"></a>Conclusion

À ce stade, votre nouveau projet d’équipe est prêt à être utilisé, et votre équipe de développeurs peut commencer à ajouter du contenu et à collaborer sur le processus de développement.

La rubrique suivante, [Ajout de contenu au contrôle de code source](adding-content-to-source-control.md), explique comment ajouter du contenu au contrôle de code source.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils plus larges sur la création de projets d’équipe dans TFS, consultez [créer un projet d’équipe](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Pour plus d’informations sur l’activation des utilisateurs pour créer des projets d’équipe dans une collection de projets d’équipe, consultez [définir des autorisations d’administrateur pour les collections de projets d’équipe](https://msdn.microsoft.com/library/dd547204.aspx). Pour plus d’informations sur l’ajout d’utilisateurs aux projets d’équipe, consultez [Ajouter des utilisateurs aux projets d’équipe](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Précédent](configuring-team-foundation-server-for-web-deployment.md)
> [Suivant](adding-content-to-source-control.md)
