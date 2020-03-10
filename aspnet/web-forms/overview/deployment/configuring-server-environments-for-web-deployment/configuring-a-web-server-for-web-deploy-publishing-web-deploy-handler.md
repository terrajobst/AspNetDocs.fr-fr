---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Configuration d’un serveur Web pour la publication Web Deploy (gestionnaire de Web Deploy) | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment configurer un serveur Web Internet Information Services (IIS) pour prendre en charge la publication et le déploiement Web à l’aide d’IIS Web Deploy Han...
ms.author: riande
ms.date: 01/29/2017
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: baaebd32f08d3c6b861572c5c5a16ec0fb70aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568564"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Configuration d’un serveur web pour la publication Web Deploy (Gestionnaire Web Deploy)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment configurer un serveur Web Internet Information Services (IIS) pour prendre en charge la publication et le déploiement Web à l’aide du gestionnaire de Web Deploy IIS.
> 
> Lorsque vous travaillez avec Web Deploy 2,0 ou une version ultérieure, vous pouvez utiliser trois approches principales pour obtenir vos applications ou sites sur un serveur Web. Vous pouvez :
> 
> - Utilisez le *service de l’agent distant Web Deploy*. Cette approche requiert moins de configuration du serveur Web, mais vous devez fournir les informations d’identification d’un administrateur de serveur local pour pouvoir déployer n’importe quoi sur le serveur.
> - Utilisez le *Gestionnaire de Web Deploy*. Cette approche est beaucoup plus complexe et nécessite plus de travail initial pour configurer le serveur Web. Toutefois, lorsque vous utilisez cette approche, vous pouvez configurer IIS pour permettre aux utilisateurs non-administrateurs d’effectuer le déploiement. Le gestionnaire de Web Deploy est disponible uniquement dans IIS version 7 ou ultérieure.
> - Utilisez le *déploiement en mode hors connexion*. Cette approche nécessite la configuration minimale du serveur Web, mais un administrateur de serveur doit copier manuellement le package Web sur le serveur et l’importer par le biais du gestionnaire des services Internet.
> 
> Pour plus d’informations sur les fonctionnalités principales, les avantages et les inconvénients de ces approches, consultez [choix de l’approche appropriée pour le déploiement Web](choosing-the-right-approach-to-web-deployment.md).

Oui, si vous souhaitez permettre aux utilisateurs non-administrateurs de déployer du contenu vers des sites Web IIS spécifiques. Cette approche est souvent souhaitable dans ces types de scénarios :

- Environnements intermédiaires ou de production, où le compte de personne ou de service qui déclenche le déploiement à distance est peu susceptible d’avoir accès aux informations d’identification d’un administrateur de serveur.
- Les environnements hébergés, où vous souhaitez donner aux utilisateurs distants la possibilité de mettre à jour leurs sites Web sans leur donner le contrôle total de vos serveurs Web (ou l’accès aux sites Web de toute autre personne).

Dans les scénarios de développement ou de test, ou dans les organisations de petite taille, le déploiement de contenu à l’aide des informations d’identification d’administrateur du serveur est souvent moins contentieuse. Dans ces scénarios, la configuration de vos serveurs Web pour prendre en charge le déploiement à l’aide de l' [Web Deploy service de l’agent distant](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) offre une approche plus simple.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Pour configurer le serveur Web de façon à accepter et à déployer des packages Web à partir d’un ordinateur distant à l’aide de l’approche du gestionnaire de Web Deploy, vous devez :

- Créez ou choisissez un compte d’utilisateur de domaine (l’utilisateur non-administrateur) dont vous allez utiliser les informations d’identification pour effectuer des déploiements.
- Installez IIS 7,5, y compris le service de gestion Web et le module d’authentification de base.
- Installez Web Deploy 2,1 ou une version ultérieure.
- Configurez le service de gestion Web pour autoriser les connexions à distance, puis démarrez le service.
- Créez un site Web IIS pour héberger le contenu déployé.
- Accordez des autorisations d’utilisateur non-administrateur sur votre site Web dans le gestionnaire des services Internet.
- Vérifiez que les règles de délégation du service de gestion Web permettent au service d’ajouter et de modifier du contenu de site Web à l’aide de votre compte d’utilisateur non-administrateur.
- Configurez tous les pare-feu pour autoriser les connexions entrantes sur le port 8172.

Pour héberger l’exemple de solution ContactManager, vous devez également :

- Installez le .NET Framework 4,0.
- Installez ASP.NET MVC 3.

Cette rubrique vous indique comment effectuer chacune de ces procédures. Les tâches et procédures pas à pas de cette rubrique partent du principe que vous commencez avec une version propre du serveur exécutant Windows Server 2016. Avant de continuer, assurez-vous que :

- Windows Server 2016
- Le serveur est joint à un domaine.
- Le serveur possède une adresse IP statique.

> [!NOTE]
> Pour plus d’informations sur la jonction d’ordinateurs à un domaine, consultez [jonction d’ordinateurs au domaine et ouverture de session](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Pour plus d’informations sur la configuration d’adresses IP statiques, consultez [configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Installer les produits et les composants

Cette section vous guide tout au long de l’installation des produits et composants requis sur le serveur Web. Avant de commencer, il est recommandé d’exécuter Windows Update pour vous assurer que votre serveur est entièrement à jour.

Dans ce cas, vous devez installer les éléments suivants :

- **Configuration recommandée pour IIS 7**. Cela active le rôle de **serveur Web (IIS)** sur votre serveur Web et installe l’ensemble des modules et composants IIS dont vous avez besoin pour héberger une application ASP.net.
- **IIS : service de gestion**. Cela installe le service de gestion Web (WMSvc) dans IIS. Ce service permet la gestion à distance des sites Web IIS et expose le point de terminaison du gestionnaire de Web Deploy aux clients.
- **IIS : authentification de base**. Cette installation installe le module d’authentification de base IIS. Cela permet au service de gestion Web (WMSvc) d’authentifier les informations d’identification que vous fournissez.
- **Outil de déploiement Web 2,1 ou version ultérieure**. Cela installe Web Deploy (et son exécutable sous-jacent, MSDeploy. exe) sur votre serveur. Dans le cadre de ce processus, il installe le gestionnaire de Web Deploy et l’intègre au service de gestion Web.
- **.NET Framework 4,0**. Cela est nécessaire pour exécuter des applications qui ont été générées sur cette version du .NET Framework.
- **ASP.NET MVC 3**. Cela installe les assemblys dont vous avez besoin pour exécuter les applications MVC 3.

> [!NOTE]
> Cette procédure pas à pas décrit l’utilisation de la Web Platform Installer pour installer et configurer différents composants. Bien qu’il ne soit pas nécessaire d’utiliser la Web Platform Installer, elle simplifie le processus d’installation en détectant automatiquement les dépendances et en garantissant que vous recevez toujours les dernières versions du produit. Pour plus d’informations, consultez [Microsoft Web Platform Installer](https://go.microsoft.com/?linkid=9805118).

**Pour installer les produits et composants requis**

1. Téléchargez et installez le [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
2. Une fois l’installation terminée, l’Web Platform Installer est lancée automatiquement.

    > [!NOTE]
    > Vous pouvez à présent lancer le Web Platform Installer à tout moment à partir du menu **Démarrer** . Pour ce faire, dans le menu **Démarrer** , cliquez sur **tous les programmes**, puis sur **Microsoft Web Platform Installer**.
3. En haut de la fenêtre **Web Platform Installer**, cliquez sur **Produits**.
4. Sur le côté gauche de la fenêtre, dans le volet de navigation, cliquez sur **frameworks**.
5. Dans la ligne **Microsoft .NET Framework 4** , si le .NET Framework n’est pas déjà installé, cliquez sur **Ajouter**.

    > [!NOTE]
    > Vous avez peut-être déjà installé le .NET Framework 4,0 à Windows Update. Si un produit ou un composant est déjà installé, le Web Platform Installer le signale en remplaçant le bouton **Ajouter** par le texte **installé**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. Dans la ligne **ASP.NET MVC 3 (Visual Studio 2010)** , cliquez sur **Ajouter**.
7. Dans le volet de navigation, cliquez sur **serveur**.
8. Dans la ligne **Configuration recommandée pour IIS 7** , cliquez sur **Ajouter**.
9. Dans la ligne de l' **outil de déploiement Web 2,1** , cliquez sur **Ajouter**.
10. Dans la ligne **IIS : authentification de base** , cliquez sur **Ajouter**.
11. Dans la ligne **IIS : Management Service** , cliquez sur **Ajouter**.
12. Cliquez sur **Suivant**. Le Web Platform Installer affiche une liste des produits&#x2014;avec les dépendances&#x2014;associées à installer et vous invite à accepter les termes du contrat de licence.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Passez en revue les termes du contrat de licence et, si vous en acceptez les termes, cliquez sur **J’accepte**.
14. Une fois l’installation terminée, cliquez sur **Terminer**, puis fermez la fenêtre de **Web Platform Installer** .

Si vous avez installé le .NET Framework 4,0 avant d’installer IIS, vous devez exécuter l' [outil d’inscription ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) pour inscrire la dernière version de ASP.net auprès d’IIS. Si vous ne le faites pas, vous constaterez que le service IIS traite le contenu statique (comme les fichiers HTML) sans aucun problème, mais renvoie l' **erreur HTTP 404,0 – introuvable** lorsque vous tentez d’accéder au contenu ASP.net. Vous pouvez utiliser la procédure suivante pour vous assurer que ASP.NET 4,0 est inscrit.

**Pour inscrire ASP.NET 4,0 avec IIS**

1. Cliquez sur **Démarrer**, puis tapez **invite de commandes**.
2. Dans les résultats de la recherche, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.
3. Dans la fenêtre d’invite de commandes, accédez au répertoire **%windir%\Microsoft.NET\Framework\v4.0.30319**
4. Tapez cette commande et appuyez sur entrée :

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Si vous envisagez d’héberger des applications Web 64 bits à tout moment, vous devez également enregistrer la version 64 bits de ASP.NET avec IIS. Pour ce faire, dans la fenêtre d’invite de commandes, accédez au répertoire **%windir%\Microsoft.NET\Framework64\v4.0.30319**
6. Tapez cette commande et appuyez sur entrée :

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Nous vous recommandons d’utiliser à nouveau Windows Update à ce stade pour télécharger et installer les mises à jour disponibles pour les nouveaux produits et composants que vous avez installés.

## <a name="configure-the-web-management-service"></a>Configurer le service de gestion Web

Maintenant que vous avez installé tout ce dont vous avez besoin, l’étape suivante consiste à configurer le service de gestion Web dans IIS. À un niveau élevé, vous devez effectuer les tâches suivantes :

- Activez l’authentification de base au niveau du serveur.
- Configurez le service de gestion Web pour accepter les connexions à distance.
- Démarrez le service de gestion Web.
- Vérifiez que les règles de délégation du service de gestion Web requises sont en place.

**Pour configurer le service de gestion Web**

1. Dans le menu **Démarrer** , pointez sur **Outils d’administration**, puis cliquez sur gestionnaire de **Internet Information Services (IIS)** .
2. Dans le gestionnaire des services Internet, dans le volet **connexions** , cliquez sur le nœud du serveur (par exemple, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. Dans le volet central, sous **IIS**, double-cliquez sur **authentification**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image20.png)
4. Cliquez avec le bouton droit sur **authentification de base**, puis cliquez sur **activer**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. Dans le volet **connexions** , cliquez à nouveau sur le nœud serveur pour revenir aux paramètres de niveau supérieur.
6. Dans le volet central, sous **gestion**, double-cliquez sur **service de gestion**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. Dans le volet central, sélectionnez **activer les connexions à distance**.

    > [!NOTE]
    > Si le service de gestion Web est déjà en cours d’exécution, vous devez d’abord l’arrêter.
8. Dans le volet **actions** , cliquez sur **Démarrer** pour démarrer le service de gestion Web.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Si vous êtes invité à enregistrer vos paramètres, cliquez sur **Oui**.

    > [!NOTE]
    > Vous pouvez également configurer le service pour qu’il démarre automatiquement. Pour ce faire, ouvrez la console Services, cliquez avec le bouton droit sur **service de gestion Web**, puis cliquez sur **Propriétés**. Dans la liste déroulante **type de démarrage** , sélectionnez **automatique**, puis cliquez sur **OK**.
10. Dans le volet **connexions** , cliquez à nouveau sur le nœud serveur pour revenir aux paramètres de niveau supérieur.
11. Dans le volet central, sous **gestion**, double-cliquez sur **délégation du service de gestion**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Vérifiez que le volet central contient un ensemble de règles.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Ces règles permettent aux utilisateurs du service de gestion Web autorisé d’utiliser différents fournisseurs de Web Deploy. Par exemple, pour déployer des applications et du contenu Web sur IIS par le biais du gestionnaire de Web Deploy, il doit exister une règle de délégation qui permet à tous les utilisateurs du service de gestion Web authentifiés d’utiliser les fournisseurs **contentPath** et **iisapp** (dernière règle que vous pouvez voir dans la capture d’écran).

    Si vous avez installé des produits et des composants dans l’ordre décrit dans cette rubrique, la dernière version de Web Deploy doit ajouter automatiquement toutes les règles de délégation requises au service de gestion Web. Si la page délégation du service de gestion n’affiche aucune règle, vous devez les créer vous-même. Pour obtenir des instructions sur la procédure à suivre, consultez [configurer le gestionnaire de déploiement Web](https://go.microsoft.com/?linkid=9805124).
13. Dans le volet **connexions** , cliquez à nouveau sur le nœud serveur pour revenir aux paramètres de niveau supérieur.

## <a name="create-and-configure-an-iis-website"></a>Créer et configurer un site Web IIS

Avant de pouvoir déployer du contenu Web sur votre serveur, vous devez créer et configurer un site Web IIS pour héberger le contenu. Web Deploy pouvez déployer des packages Web uniquement sur un site Web IIS existant ; il ne peut pas créer le site Web pour vous. Vous devez également effectuer une configuration supplémentaire pour autoriser votre compte non-administrateur à déployer du contenu à distance. À un niveau élevé, vous devez effectuer les tâches suivantes :

- Créez un dossier sur le système de fichiers pour héberger votre contenu.
- Créez un site Web IIS pour servir le contenu, puis associez-le au dossier local.
- Accordez des autorisations de lecture à l’identité du pool d’applications sur le dossier local.
- Accordez les autorisations IIS nécessaires au compte de domaine qui déploiera votre application Web.

Bien que rien ne vous empêche de déployer du contenu vers le site Web par défaut dans IIS, cette approche n’est pas recommandée pour les scénarios de test ou de démonstration. Pour simuler un environnement de production, vous devez créer un nouveau site Web IIS avec des paramètres spécifiques à la configuration requise de votre application.

**Pour créer un site Web IIS**

1. Sur le système de fichiers local, créez un dossier pour stocker votre contenu (par exemple, **C:\DemoSite**).
2. Dans le menu **Démarrer** , pointez sur **Outils d’administration**, puis cliquez sur gestionnaire de **Internet Information Services (IIS)** .
3. Dans le gestionnaire des services Internet, dans le volet **connexions** , développez le nœud du serveur (par exemple, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Cliquez avec le bouton droit sur le nœud **sites** , puis cliquez sur **Ajouter un site Web**.
5. Dans la zone **nom du site** , tapez un nom pour le site Web IIS (par exemple, **DemoSite**).
6. Dans la zone **chemin d’accès physique** , tapez (ou accédez à) le chemin d’accès à votre dossier local (par exemple, **C:\DemoSite**).
7. Dans la zone **port** , tapez le numéro de port sur lequel vous souhaitez héberger le site Web (par exemple, **85**).

    > [!NOTE]
    > Les numéros de port standard sont 80 pour HTTP et 443 pour HTTPs. Toutefois, si vous hébergez ce site Web sur le port 80, vous devez arrêter le site Web par défaut pour pouvoir accéder à votre site.
8. Laissez la zone **nom d’hôte** vide, à moins que vous ne souhaitiez configurer un enregistrement DNS (Domain Name System) pour le site Web, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > Dans un environnement de production, vous souhaiterez probablement héberger votre site Web sur le port 80 et configurer un en-tête d’hôte, ainsi que les enregistrements DNS correspondants. Pour plus d’informations sur la configuration des en-têtes d’hôte dans IIS 7, consultez [configurer un en-tête d’hôte pour un site Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Pour plus d’informations sur le rôle de serveur DNS dans Windows Server, consultez [vue d’ensemble du serveur DNS](https://technet.microsoft.com/library/cc770392.aspx) et [serveur DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Dans le volet **Actions** , sous **Modifier le Site**, cliquez sur **Liaisons**.
10. Dans la boîte de dialogue **Liaisons de site Web**, cliquez sur **Ajouter**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. Dans la boîte de dialogue **Ajouter la liaison de site** , définissez l' **adresse IP** et le **port** correspondant à la configuration de votre site existant.
12. Dans la zone **nom d’hôte** , tapez le nom de votre serveur Web (par exemple, **STAGEWEB1**), puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > La première liaison de site vous permet d’accéder au site localement à l’aide de l’adresse IP et du port ou `http://localhost:85`. La deuxième liaison de site vous permet d’accéder au site à partir d’autres ordinateurs sur le domaine à l’aide du nom de l’ordinateur (par exemple, http://stageweb1:85).
13. Dans la boîte de dialogue **Liaisons de site Web**, cliquez sur **Fermer**.
14. Dans le volet **connexions** , cliquez sur **pools d’applications**.
15. Dans le volet **pools d’applications** , cliquez avec le bouton droit sur le nom de votre pool d’applications, puis cliquez sur **paramètres de base**. Par défaut, le nom de votre pool d’applications correspond au nom de votre site Web (par exemple, **DemoSite**).
16. Dans la liste **version du CLR .net** , sélectionnez **.NET CLR v 4.0.30319**, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image21.png)

    > [!NOTE]
    > L’exemple de solution requiert .NET Framework 4,0. Cela n’est pas obligatoire pour les Web Deploy en général.

Pour que votre site Web puisse traiter du contenu, l’identité du pool d’applications doit avoir des autorisations de lecture sur le dossier local qui stocke le contenu. Dans IIS 7,5, les pools d’applications s’exécutent avec une identité de pool d’applications unique par défaut (contrairement aux versions antérieures d’IIS, où les pools d’applications s’exécutent généralement en utilisant le compte de service réseau). L’identité du pool d’applications n’est pas un compte d’utilisateur réel et n’apparaît pas sur les listes d'&#x2014;utilisateurs ou de groupes à la place, elle est créée dynamiquement au démarrage du pool d’applications. Chaque identité du pool d’applications est ajoutée au groupe de sécurité **IIS\_IUSRS** local en tant qu’élément masqué.

Pour accorder des autorisations à l’identité d’un pool d’applications sur un fichier ou un dossier, deux options s’offrent à vous :

- Affectez directement des autorisations à l’identité du pool d’applications, à l’aide du format <strong>\<d’IIS AppPool/strong ><em>[nom du pool d’applications]</em>(par exemple, <strong>IIS AppPool\DemoSite</strong>).
- Attribuez des autorisations au groupe **IUSRS IIS\_** .

L’approche la plus courante consiste à assigner des autorisations au groupe local **IIS\_IUSRS** , car cette approche vous permet de modifier les pools d’applications sans reconfigurer les autorisations du système de fichiers. La procédure suivante utilise cette approche basée sur les groupes.

> [!NOTE]
> Pour plus d’informations sur les identités du pool d’applications dans IIS 7,5, consultez [identités du pool d’applications](https://go.microsoft.com/?linkid=9805123).

**Pour configurer les autorisations de dossier pour un site Web IIS**

1. Dans l’Explorateur Windows, accédez à l’emplacement de votre dossier local.
2. Cliquez avec le bouton droit sur le dossier, puis cliquez sur **Propriétés**.
3. Sous l'onglet **Security**, cliquez sur **Edit**, puis sur **Add**.
4. Cliquez sur **Emplacements**. Dans la boîte de dialogue **emplacements** , sélectionnez le serveur local, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , tapez **IIS\_IUSRS**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.
6. Dans la boîte de dialogue <strong>autorisations pour</strong><em>[nom du dossier]</em> , Notez que les autorisations <strong>lecture &amp; exécuter</strong>, <strong>afficher le contenu du dossier</strong>et <strong>lecture</strong> sont affectées par défaut au nouveau groupe. Laissez ce n’est pas modifié, puis cliquez sur <strong>OK</strong>.
7. Cliquez sur <strong>OK</strong> pour fermer la boîte de dialogue<strong>Propriétés</strong> de <em>[nom du dossier]</em>.

En guise de dernière tâche, vous devez accorder les autorisations appropriées à l’utilisateur non-administrateur dont vous allez utiliser les informations d’identification pour déployer du contenu. Cet utilisateur a besoin des autorisations pour déployer du contenu à distance sur votre site Web.

**Pour configurer les autorisations de site Web IIS pour un utilisateur de domaine non-administrateur**

1. Dans le gestionnaire des services Internet, dans le volet **connexions** , cliquez avec le bouton droit sur le nœud de votre site Web (par exemple, **DemoSite**), pointez sur **déployer**, puis cliquez sur **configurer la publication Web Deploy**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. Dans la boîte de dialogue **configurer la publication de Web Deploy** , à droite de la liste **Sélectionner un utilisateur pour fournir des autorisations de publication** , cliquez sur le bouton de sélection.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. Dans la boîte de dialogue **autoriser l’utilisateur** , tapez le domaine et le nom d’utilisateur du compte que vous souhaitez utiliser pour déployer le contenu, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. Dans la boîte de dialogue **configurer la publication de Web Deploy** , cliquez sur **configuration**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Cette opération exécute deux fonctions clés en une seule étape. Tout d’abord, il accorde à l’utilisateur l’autorisation de modifier le site Web à distance par le biais du service de gestion Web, conformément aux règles de délégation que vous avez examinées dans la section précédente. Deuxièmement, il accorde à l’utilisateur le contrôle total du dossier source pour le site Web, ce qui permet à l’utilisateur d’ajouter, de modifier et de définir des autorisations sur le contenu du site Web.
5. Dans la boîte de dialogue **configurer la publication de Web Deploy** , cliquez sur **Fermer**.

## <a name="configure-firewall-exceptions"></a>Configurer des exceptions de pare-feu

Par défaut, le service de gestion Web IIS écoute le port TCP 8172. Si le pare-feu Windows est activé sur votre serveur Web, vous devez créer une nouvelle règle de trafic entrant pour autoriser le trafic TCP sur le port 8172 (tout le trafic sortant est autorisé par défaut dans le pare-feu Windows). Si vous utilisez un pare-feu tiers, vous devez créer des règles pour autoriser le trafic.

| Sens | À partir du port | Vers le port | Type de port |
| --- | --- | --- | --- |
| Trafic entrant | Any | 8172 | TCP |
| Règle de trafic sortant | 8172 | Any | TCP |

Pour plus d’informations sur la configuration des règles dans le pare-feu Windows, consultez [configuration de règles de pare-feu](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Pour les pare-feu tiers, consultez la documentation de votre produit.

## <a name="conclusion"></a>Conclusion

Votre serveur Web doit maintenant être prêt à accepter les déploiements à distance vers le gestionnaire de Web Deploy via le service de gestion Web. Avant de tenter de déployer une application Web sur le serveur, vous souhaiterez peut-être vérifier les points clés suivants :

- Avez-vous activé l’authentification de base au niveau du serveur dans IIS ?
- Avez-vous activé les connexions à distance vers le service de gestion Web ?
- Avez-vous démarré le service de gestion Web ?
- Existe-t-il des règles de délégation de service de gestion ?
- L’identité du pool d’applications dispose-t-elle d’un accès en lecture au dossier source pour votre site Web ?
- Le compte d’utilisateur non-administrateur dispose-t-il d’autorisations au niveau du site dans IIS ?
- Votre pare-feu autorise-t-il les connexions entrantes vers le serveur sur le port TCP 8172 ?

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la façon de configurer des fichiers projet de Microsoft Build Engine personnalisé (MSBuild) pour déployer des packages Web sur le gestionnaire de Web Deploy, consultez [Configuration des propriétés de déploiement pour un environnement cible](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Précédent](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [Suivant](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
