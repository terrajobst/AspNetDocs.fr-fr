---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Création d’une batterie de serveurs avec l’infrastructure de batterie de serveurs Web | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment utiliser l’infrastructure de batterie de serveurs Web (WFF) 2,0 pour créer et configurer une batterie de serveurs Web à partir d’une collection de serveurs.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 204996514bed336e60ab77f184a923f04e7e2bba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637248"
---
# <a name="creating-a-server-farm-with-the-web-farm-framework"></a>Création d’une batterie de serveurs avec le framework de batterie de serveurs web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment utiliser l’infrastructure de batterie de serveurs Web (WFF) 2,0 pour créer et configurer une batterie de serveurs Web à partir d’une collection de serveurs.

WFF vous permet de synchroniser les produits et composants de plateforme Web, les applications Web, les sites Web et les paramètres de configuration sur plusieurs serveurs Web à charge équilibrée. Dans les scénarios où vous avez besoin de plusieurs serveurs Web, comme des environnements intermédiaires et de production, cela peut considérablement simplifier votre processus de déploiement et de configuration. Vous pouvez déployer une application Web sur un seul serveur&#x2014;. le *serveur*&#x2014;principal et WFF répliquent automatiquement cette application Web sur tous les autres serveurs Web de la batterie de serveurs.

## <a name="understanding-the-web-farm-framework"></a>Fonctionnement de l’infrastructure de batterie de serveurs Web

Vous pouvez utiliser WFF 2,0 pour approvisionner, gérer et déployer du contenu vers un groupe de serveurs Web. Un déploiement WFF se compose de trois rôles de serveur clés :

- *Serveur contrôleur*. Vous utilisez ce serveur pour créer et configurer des batteries de serveurs WFF. Le serveur contrôleur gère la synchronisation des composants Web Platform, des paramètres de configuration et des applications entre les serveurs Web dans une batterie de serveurs. Vous installez WFF 2,0 sur le serveur contrôleur, et le serveur de contrôleur installe à son tour l’agent WFF sur chacun des serveurs d’une batterie de serveurs. Le serveur contrôleur n’appartient pas de manière conceptuelle à une batterie de serveurs WFF, et un seul serveur contrôleur peut gérer plusieurs batteries de serveurs. Dans ce scénario, vous utilisez un seul serveur de contrôleur WFF pour créer et gérer la batterie de serveurs de test et la batterie de serveurs de production.
- *Serveur principal*. Chaque batterie de serveurs WFF comprend un seul serveur principal. Lorsque vous installez des composants de plateforme Web ou que vous déployez des applications sur le serveur principal, WFF synchronise vos modifications sur tous les autres serveurs de la batterie de serveurs.
- *Serveur secondaire*. Chaque batterie de serveurs WFF comprend un ou plusieurs serveurs secondaires. Toutes les modifications que vous apportez au serveur principal sont répliquées sur chaque serveur secondaire de la batterie de serveurs.

Cela montre comment ces rôles de serveur sont liés aux environnements de préproduction et de production de Fabrikam, Inc. :

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

Dans ce scénario, l’environnement intermédiaire et l’environnement de production sont tous deux configurés en tant que batteries de serveurs WFF. Un seul serveur de contrôleur WFF gère les deux batteries de serveurs. Au sein de chaque batterie de serveurs, toute modification apportée au serveur principal est répliquée sur chaque serveur secondaire.

Avant de commencer à configurer vos environnements intermédiaires et de production, nous vous recommandons de lire les articles suivants pour vous familiariser avec les concepts clés de WFF 2,0 :

- [Vue d’ensemble de l’infrastructure de batterie de serveurs Web 2,0 pour IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Configuration d’une batterie de serveurs avec l’infrastructure de batterie de serveurs Web 2,0 pour IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Configuration requise pour le système et la plateforme pour Web Farm Framework 2,0 pour IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Vue d’ensemble des tâches

Pour effectuer les tâches et procédures pas à pas de cette rubrique, vous devez disposer d’au&#x2014;moins trois serveurs, un contrôleur WFF, un serveur Web principal pour la batterie de serveurs et un ou plusieurs serveurs Web secondaires pour la batterie de serveurs. Vous pouvez ajouter d’autres serveurs secondaires à une batterie de serveurs WFF à tout moment. À un niveau élevé, pour créer et configurer une batterie de serveurs WFF pour votre environnement intermédiaire ou de production, vous devez :

- Créez un serveur contrôleur en installant Internet Information Services (IIS) 7,5 et WFF 2,0.
- Préparez les serveurs principaux et secondaires en créant un compte d’administrateur commun et en configurant les exceptions de pare-feu.
- Configurez la batterie de serveurs à l’aide du gestionnaire des services Internet sur le serveur contrôleur.
- Configurez l’équilibrage de charge à l’aide d’IIS Application Request Routing (ARR) ou d’une autre technologie d’équilibrage de charge.

Les tâches et procédures pas à pas de cette rubrique partent du principe que vous commencez avec les builds de serveur propre exécutant Windows Server 2008 R2. Avant de commencer, pour chaque serveur, assurez-vous que :

- Windows Server 2008 R2 Service Pack 1 et toutes les mises à jour disponibles sont installées.
- Le serveur est joint à un domaine.
- Le serveur possède une adresse IP statique.

> [!NOTE]
> Pour plus d’informations sur la jonction d’ordinateurs à un domaine, consultez [jonction d’ordinateurs au domaine et ouverture de session](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Pour plus d’informations sur la configuration d’adresses IP statiques, consultez [configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="create-the-wff-controller-server"></a>Créer le serveur de contrôleur WFF

Pour créer un serveur de contrôleur WFF, vous devez installer IIS 7 ou une version ultérieure et WFF 2,0 ou une version ultérieure. En coulisses, WFF utilise l’outil de déploiement Web IIS (Web Deploy) 2. x pour synchroniser les serveurs de votre batterie de serveurs. Si vous utilisez la Web Platform Installer pour installer WFF, le programme d’installation télécharge et installe automatiquement Web Deploy pour vous.

**Pour créer le serveur de contrôleur WFF**

1. Téléchargez et installez le [Web Platform Installer](https://go.microsoft.com/?linkid=9739157).
2. En haut de la fenêtre **Web Platform Installer 3,0** , cliquez sur **produits**.
3. Sur le côté gauche de la fenêtre, dans le volet de navigation, cliquez sur **serveur**.
4. Dans la ligne **Configuration recommandée pour IIS 7** , cliquez sur **Ajouter**.
5. Dans l' <strong>infrastructure de batterie de serveurs Web 2.</strong> <em>x</em> ligne, cliquez sur <strong>Ajouter</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Cliquez sur **Suivant**. Notez que le Web Platform Installer a ajouté l’outil de déploiement Web, ainsi que diverses autres dépendances, à la liste d’installation.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Passez en revue les termes du contrat de licence et, si vous en acceptez les termes, cliquez sur **J’accepte**.
8. Une fois l’installation terminée, cliquez sur **Terminer**, puis fermez la fenêtre **Web Platform Installer 3,0** .

## <a name="configure-the-primary-and-secondary-servers"></a>Configurer les serveurs principaux et secondaires

Avant de créer une batterie de serveurs WFF, vous devez effectuer certaines tâches de préparation sur les serveurs Web qui composent la batterie :

- Ajoutez des exceptions de pare-feu pour permettre aux fonctionnalités de **réseau de base**, d' **administration à distance**et de **partage de fichiers et d’imprimantes** de communiquer avec le serveur de contrôleur WFF.
- Créez un compte de domaine (par exemple, **FABRIKAM\stagingfarm**) dans Active Directory et ajoutez-le au groupe Administrateurs local sur chaque serveur. Vous utiliserez ce compte en tant que compte d’administrateur de batterie de serveurs lors de la création de la batterie de serveurs.

Pour plus d’informations sur la configuration de ces exceptions de pare-feu dans le pare-feu Windows, consultez [Configuration requise pour le système et la plateforme pour l’infrastructure de batterie de serveurs Web 2,0 pour IIS 7](https://go.microsoft.com/?linkid=9805128). Pour les autres systèmes de pare-feu, consultez la documentation de votre produit.

Vous pouvez utiliser la procédure suivante pour ajouter un compte de domaine au groupe Administrateurs local dans Windows Server 2008 R2. Vous devez effectuer cette procédure sur chaque serveur que vous souhaitez ajouter à la batterie&#x2014;de serveurs en d’autres termes, ajouter le même compte de domaine au groupe Administrateurs local sur le serveur principal et sur chaque serveur secondaire.

**Pour ajouter un compte de domaine au groupe Administrateurs local**

1. Dans le menu **Démarrer** , pointez sur **Outils d’administration**, puis cliquez sur **Gestionnaire de serveur**.
2. Dans la fenêtre **Gestionnaire de serveur** , dans le volet de l’arborescence, développez **configuration**, développez **utilisateurs et groupes locaux**, puis cliquez sur **groupes**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. Dans le volet **groupes** , double-cliquez sur **administrateurs**.
4. Dans la boîte de dialogue **Propriétés des administrateurs** , cliquez sur **Ajouter**.
5. Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes** , tapez (ou accédez à) votre compte de domaine (par exemple, **FABRIKAM\stagingfarm**), puis cliquez sur **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. Dans la boîte de dialogue **Propriétés des administrateurs** , cliquez sur **OK**.

Vos serveurs sont maintenant prêts à être ajoutés à une batterie de serveurs. Dans le cas du serveur principal, vous pouvez configurer le serveur pour qu’il réponde aux besoins de votre application avant ou après la&#x2014;création de la batterie de serveurs dans les deux cas, WFF synchronise les serveurs en déployant les mêmes produits, composants ou configurations sur vos serveurs secondaires. Par souci de simplicité, ce didacticiel suppose que vous configurez le serveur principal lorsque vous avez terminé la création de la batterie de serveurs.

## <a name="create-the-wff-server-farm"></a>Créer la batterie de serveurs WFF

À ce stade, tous vos serveurs sont prêts à être ajoutés à une batterie de serveurs WFF :

- Vous avez installé WFF sur le serveur contrôleur.
- Vous avez configuré des exceptions de pare-feu sur vos serveurs Web principaux et secondaires.
- Vous avez ajouté un compte de domaine au groupe Administrateurs local sur vos serveurs Web principaux et secondaires.

L’étape suivante consiste à créer la batterie de serveurs dans WFF. Vous pouvez le faire à partir du gestionnaire des services Internet sur le serveur du contrôleur WFF.

**Pour créer une batterie de serveurs WFF**

1. Sur le serveur de contrôleur WFF, dans le menu **Démarrer** , pointez sur **Outils d’administration**, puis cliquez sur **Gestionnaire de Internet Information Services (IIS)** .
2. Dans le volet **connexions** , développez le nœud du serveur local, cliquez avec le bouton droit sur **batteries de serveurs**, puis cliquez sur créer une **batterie**de serveurs.
3. Dans la boîte de dialogue **créer une batterie** de serveurs, tapez un nom explicite pour la batterie de serveurs (par exemple, batterie de serveurs de **test**), puis sélectionnez **configurer une batterie**de serveurs.
4. Tapez le nom d’utilisateur et le mot de passe du compte de domaine que vous avez ajouté au groupe Administrateurs local sur chaque serveur.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Cliquez sur **Next**.
6. Dans la page **Ajouter des serveurs** , tapez le nom de domaine complet (FQDN) du serveur principal, sélectionnez **serveur principal**, puis cliquez sur **Ajouter**.
7. À ce stade, WFF tente de contacter le serveur principal à l’aide des informations d’identification que vous avez fournies. Si la connexion est établie, le serveur principal est ajouté à la table dans la page **Ajouter des serveurs** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Vous avez peut-être remarqué que le **serveur est disponible pour l’équilibrage de charge** est sélectionné par défaut. WFF utilise le module IIS ARR pour implémenter l’équilibrage de charge et ainsi distribuer les demandes entre les serveurs Web de votre batterie de serveurs. Dans la plupart des scénarios, vous ne désactivez pas l’option le **serveur est disponible pour l’équilibrage de charge** si vous souhaitiez utiliser à la place une solution d’équilibrage de charge tierce.
8. Dans la page **Ajouter des serveurs** , tapez le nom de domaine complet de votre premier serveur secondaire, puis cliquez sur **Ajouter**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Répétez l’étape 7 pour tous les serveurs secondaires supplémentaires de votre batterie, puis cliquez sur **Terminer**.

Votre batterie de serveurs WFF est maintenant en cours d’exécution. Tous les produits ou composants de plateforme Web que vous installez sur le serveur principal, ainsi que les applications Web ou le contenu que vous déployez sur le serveur principal, seront automatiquement approvisionnés sur tous vos serveurs secondaires.

WFF est un sujet large et complexe, et vous pouvez en savoir plus à ce sujet sur le site Web de [Microsoft Web Farm Framework 2,0 pour IIS 7](https://go.microsoft.com/?linkid=9805129) . Toutefois, pour l’instant, il existe deux domaines de fonctionnalités que vous devez connaître :

- L' *approvisionnement d’applications* est le processus qui réplique le contenu à partir du serveur principal, comme les applications Web et les paramètres de configuration, sur tous les serveurs secondaires de la batterie de serveurs. Par exemple, si vous déployez l’exemple de solution du gestionnaire de contacts sur votre serveur intermédiaire principal, le processus de provisionnement d’application WFF déploie cette solution sur tous vos serveurs de mise en lots secondaires. Par défaut, le processus de configuration d’application s’exécute toutes les 30 secondes.
- L’approvisionnement de la *plateforme* est le processus qui synchronise les produits et composants de plateforme Web à partir du serveur principal sur tous les serveurs secondaires de la batterie de serveurs. Par exemple, si vous installez ASP.NET MVC 3 sur votre serveur intermédiaire principal, le processus de configuration de la plateforme utilise le Web Platform Installer pour installer ASP.NET MVC 3 sur tous vos serveurs de mise en lots secondaires. Par défaut, le processus de configuration de la plateforme s’exécute toutes les cinq minutes.

Vous pouvez gérer les paramètres de configuration de l’application et de la plateforme de base à partir du gestionnaire des services Internet sur votre serveur de contrôleur WFF.

**Explorez les paramètres de configuration de l’application et de la plateforme**

1. Dans le gestionnaire des services Internet, dans le volet **connexions** , sélectionnez votre batterie de serveurs.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. Dans le volet **batterie de serveurs** , double-cliquez sur approvisionnement de l' **application**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Comme vous pouvez le voir, la batterie de serveurs est actuellement configurée pour synchroniser le contenu Web et les paramètres de configuration entre le serveur principal et les serveurs secondaires toutes les 30 secondes.
4. Cliquez sur **précédent**, puis double-cliquez sur Configuration de la **plateforme**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Comme vous pouvez le voir, la batterie de serveurs est actuellement configurée pour synchroniser les composants et les produits de plateforme Web entre le serveur principal et les serveurs secondaires toutes les cinq minutes.
6. Cliquez sur **Précédent**.
7. Pour forcer la batterie de serveurs à synchroniser les produits de plateforme Web immédiatement, dans le volet **actions** , cliquez sur **configurer la plateforme**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > L’approvisionnement de la plateforme peut prendre un certain temps. Le processus d’installation s’exécute en arrière-plan sur les serveurs secondaires de votre batterie de serveurs.
8. Une fois que vous avez accordé suffisamment de temps pour terminer le processus d’approvisionnement, vous pouvez vérifier que les produits et composants que vous avez ajoutés au serveur principal ont été répliqués sur les serveurs secondaires. Par exemple, vous pouvez ouvrir une session sur un serveur secondaire et utiliser la fenêtre **Gestionnaire de serveur** pour vérifier que le rôle de serveur Web a été installé.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Vous pouvez également consulter la liste programmes installés pour vérifier que différents composants de la plateforme Web ont été ajoutés.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurer l’équilibrage de charge

Lorsque vous créez une batterie de serveurs Web, vous devez configurer une certaine forme d’équilibrage de charge pour distribuer les requêtes HTTP entre vos serveurs Web. Il peut s’agir de l’équilibrage de charge réseau Windows Server 2008, d’IIS ARR ou d’une solution d’équilibrage de charge basée sur un matériel ou un logiciel tiers.

WFF est conçu pour s’intégrer étroitement avec IIS ARR. Pour tirer parti de cette intégration, vous devez installer le module ARR sur le serveur du contrôleur WFF. Vous dirigez ensuite tout votre trafic Web vers le serveur de contrôleur, en général en configurant les enregistrements DNS (Domain Name System). Le serveur contrôleur répartit ensuite les demandes entrantes entre les serveurs de votre batterie, en fonction de la disponibilité du serveur et de divers autres critères.

> [!NOTE]
> Vous n’avez pas besoin d’utiliser ARR avec WFF ; vous pouvez configurer WFF pour qu’il fonctionne avec des solutions d’équilibrage de charge tierces. Pour plus d’informations, consultez [vue d’ensemble de l’infrastructure de batterie de serveurs Web 2,0 pour IIS 7](https://go.microsoft.com/?linkid=9805126).

L’équilibrage de charge à l’aide de ARR est un sujet complexe, dont la plupart dépasse le cadre de ce didacticiel. Toutefois, vous pouvez utiliser la procédure suivante pour installer le module ARR et prendre en main l’équilibrage de charge.

**Pour configurer l’équilibrage de charge sur le serveur de contrôleur WFF**

1. Sur le serveur de contrôleur WFF, lancez le Web Platform Installer.
2. En haut de la fenêtre **Web Platform Installer 3,0** , cliquez sur **produits**.
3. Sur le côté gauche de la fenêtre, dans le volet de navigation, cliquez sur **serveur**.
4. Dans la ligne **Application Request Routing 2,5** , cliquez sur **Ajouter**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Cliquez sur **installer**, puis suivez les instructions de la fenêtre d’installation de la **plateforme Web** .
6. Une fois l’installation terminée, lancez le gestionnaire des services Internet, puis dans le volet **connexions** , cliquez sur le nœud de votre batterie de serveurs. Notez que plusieurs nouvelles icônes ont été ajoutées au volet **batterie de serveurs** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. Dans le volet **batterie de serveurs** , double-cliquez sur équilibrage de la **charge**.
8. Dans le volet équilibrage de **charge** , sélectionnez un algorithme d’équilibrage de charge (par exemple, la **demande la moins courante**).

    > [!NOTE]
    > Pour plus d’informations sur les algorithmes d’équilibrage de charge et d’autres paramètres de configuration, consultez [application Request Routing module](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. Dans le volet **actions** , cliquez sur **appliquer**.

Vous avez maintenant configuré l’équilibrage de charge de base pour les serveurs de votre batterie de serveurs. Si vous dirigez tout le trafic de votre batterie de serveurs Web vers le serveur contrôleur, les demandes sont distribuées entre les serveurs de votre batterie de serveurs en fonction de la disponibilité et de l’algorithme d’équilibrage de charge que vous avez sélectionné.

Pour plus d’informations sur la configuration de l’équilibrage de charge avec ARR, consultez [application Request Routing module](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Surveiller la batterie de serveurs

Vous pouvez surveiller l’intégrité de votre batterie de serveurs à tout moment via le gestionnaire des services Internet sur le serveur contrôleur. Dans le volet **connexions** , développez votre batterie de serveurs, puis cliquez sur **serveurs**. Le volet central affiche un résumé de chaque serveur de la batterie de serveurs, ainsi qu’un journal des traces de l’activité récente.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusion

Votre batterie de serveurs WFF doit maintenant être en cours d’exécution. Vous pouvez configurer le serveur principal pour qu’il prenne en charge&#x2014;l’approche de déploiement que vous préférez&#x2014;. pour plus d’informations, consultez la section relative à la lecture et votre configuration sera répliquée sur chaque serveur secondaire de la batterie de serveurs.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur tous les aspects de la configuration et de l’utilisation de WFF, consultez le site Web [de Microsoft Web Farm Framework 2,0 pour IIS 7](https://go.microsoft.com/?linkid=9805129) .

> [!div class="step-by-step"]
> [Précédent](configuring-a-database-server-for-web-deploy-publishing.md)
> [Suivant](configuring-deployment-properties-for-a-target-environment.md)
