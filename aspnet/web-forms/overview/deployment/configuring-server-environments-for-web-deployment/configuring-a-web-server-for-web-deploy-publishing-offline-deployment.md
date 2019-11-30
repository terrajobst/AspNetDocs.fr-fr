---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Configuration d’un serveur Web pour la publication de Web Deploy (déploiement hors connexion) | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment configurer un serveur Web IIS pour prendre en charge la publication et le déploiement Web hors connexion. Quand vous travaillez avec des Internet Information Services (I...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: f93cf11085fb19afb97b71aca8f638bd88fe658b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621098"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Configuration d’un serveur web pour la publication Web Deploy (Déploiement hors connexion)

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment configurer un serveur Web IIS pour prendre en charge la publication et le déploiement Web hors connexion.
> 
> Lorsque vous utilisez l’outil de déploiement Web de Internet Information Services (IIS) (Web Deploy) 2,0 ou version ultérieure, vous disposez de trois approches principales pour obtenir vos applications ou sites sur un serveur Web. Vous pouvez effectuer les tâches suivantes :
> 
> - Utilisez le *service de l’agent distant Web Deploy*. Cette approche requiert moins de configuration du serveur Web, mais vous devez fournir les informations d’identification d’un administrateur de serveur local pour pouvoir déployer n’importe quoi sur le serveur.
> - Utilisez le *Gestionnaire de Web Deploy*. Cette approche est beaucoup plus complexe et nécessite plus de travail initial pour configurer le serveur Web. Toutefois, lorsque vous utilisez cette approche, vous pouvez configurer IIS pour permettre aux utilisateurs non-administrateurs d’effectuer le déploiement. Le gestionnaire de Web Deploy est disponible uniquement dans IIS version 7 ou ultérieure.
> - Utilisez le *déploiement en mode hors connexion*. Cette approche nécessite la configuration minimale du serveur Web, mais un administrateur de serveur doit copier manuellement le package Web sur le serveur et l’importer par le biais du gestionnaire des services Internet.
> 
> Pour plus d’informations sur les fonctionnalités principales, les avantages et les inconvénients de ces approches, consultez [choix de l’approche appropriée pour le déploiement Web](choosing-the-right-approach-to-web-deployment.md).

Oui, si votre infrastructure réseau ou des restrictions de sécurité empêchent le déploiement à distance. Cela est probablement le cas dans les environnements de production accessibles sur Internet, où les serveurs Web sont isolés&#x2014;soit physiquement, soit par des pare-feu et&#x2014;des sous-réseaux du reste de votre infrastructure de serveur.

Évidemment, cette approche devient moins souhaitable si vos applications Web sont mises à jour régulièrement. Si votre infrastructure l’autorise, vous souhaiterez peut-être activer le déploiement à distance à l’aide du gestionnaire de Web Deploy ou du service d’agent distant Web Deploy.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Pour configurer le serveur Web de sorte qu’il prenne en charge l’importation et le déploiement hors connexion des packages Web, vous devez :

- Installez IIS 7,5 et la configuration IIS 7 recommandée.
- Installez Web Deploy 2,1 ou une version ultérieure.
- Créez un site Web IIS pour héberger le contenu déployé.
- Désactivez le service de Deployment Agent Web.

Pour héberger l’exemple de solution, vous devez également effectuer les opérations suivantes :

- Installez le .NET Framework 4,0.
- Installez ASP.NET MVC 3.

Cette rubrique vous indique comment effectuer chacune de ces procédures. Les tâches et procédures pas à pas décrites dans cette rubrique partent du principe que vous commencez avec une nouvelle génération de serveur exécutant Windows Server 2008 R2. Avant de continuer, assurez-vous que :

- Windows Server 2008 R2 Service Pack 1 et toutes les mises à jour disponibles sont installées.
- Le serveur est joint à un domaine.
- Le serveur possède une adresse IP statique.

> [!NOTE]
> Pour plus d’informations sur la jonction d’ordinateurs à un domaine, consultez [jonction d’ordinateurs au domaine et ouverture de session](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Pour plus d’informations sur la configuration d’adresses IP statiques, consultez [configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Installer les produits et les composants

Cette section vous guide tout au long de l’installation des produits et composants requis sur le serveur Web. Avant de commencer, il est recommandé d’exécuter Windows Update pour vous assurer que votre serveur est entièrement à jour.

Dans ce cas, vous devez installer les éléments suivants :

- **Configuration recommandée pour IIS 7**. Cela active le rôle de **serveur Web (IIS)** sur votre serveur Web et installe l’ensemble des modules et composants IIS dont vous avez besoin pour héberger une application ASP.net.
- **.NET Framework 4,0**. Cela est nécessaire pour exécuter des applications qui ont été générées sur cette version du .NET Framework.
- **Outil de déploiement Web 2,1 ou version ultérieure**. Cela installe Web Deploy (et son exécutable sous-jacent, MSDeploy. exe) sur votre serveur. Web Deploy s’intègre à IIS et vous permet d’importer et d’exporter des packages Web.
- **ASP.NET MVC 3**. Cela installe les assemblys dont vous avez besoin pour exécuter les applications MVC 3.

> [!NOTE]
> Cette procédure pas à pas décrit l’utilisation de la Web Platform Installer pour installer et configurer différents composants. Bien qu’il ne soit pas nécessaire d’utiliser la Web Platform Installer, elle simplifie le processus d’installation en détectant automatiquement les dépendances et en garantissant que vous recevez toujours les dernières versions du produit. Pour plus d’informations, consultez [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/?linkid=9805118).

**Pour installer les produits et composants requis**

1. Téléchargez et installez le [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
2. Une fois l’installation terminée, l’Web Platform Installer est lancée automatiquement.

    > [!NOTE]
    > Vous pouvez à présent lancer le Web Platform Installer à tout moment à partir du menu **Démarrer** . Pour ce faire, dans le menu **Démarrer** , cliquez sur **tous les programmes**, puis sur **Microsoft Web Platform Installer**.
3. En haut de la fenêtre **Web Platform Installer 3,0** , cliquez sur **produits**.
4. Sur le côté gauche de la fenêtre, dans le volet de navigation, cliquez sur **frameworks**.
5. Dans la ligne **Microsoft .NET Framework 4** , si le .NET Framework n’est pas déjà installé, cliquez sur **Ajouter**.

    > [!NOTE]
    > Vous avez peut-être déjà installé le .NET Framework 4,0 à Windows Update. Si un produit ou un composant est déjà installé, le Web Platform Installer le signale en remplaçant le bouton **Ajouter** par le texte **installé**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. Dans la ligne **ASP.NET MVC 3 (Visual Studio 2010)** , cliquez sur **Ajouter**.
7. Dans le volet de navigation, cliquez sur **serveur**.
8. Dans la ligne **Configuration recommandée pour IIS 7** , cliquez sur **Ajouter**.
9. Dans la ligne de l' **outil de déploiement Web 2,1** , cliquez sur **Ajouter**.
10. Cliquez sur **Suivant**. Le Web Platform Installer affiche une liste des produits&#x2014;avec les dépendances&#x2014;associées à installer et vous invite à accepter les termes du contrat de licence.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Passez en revue les termes du contrat de licence et, si vous en acceptez les termes, cliquez sur **J’accepte**.
12. Une fois l’installation terminée, cliquez sur **Terminer**, puis fermez la fenêtre **Web Platform Installer 3,0** .

Si vous avez installé le .NET Framework 4,0 avant d’installer IIS, vous devez exécuter l' [outil d’inscription ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) pour inscrire la dernière version de ASP.net auprès d’IIS. Si vous ne le faites pas, vous constaterez que le service IIS traite le contenu statique (comme les fichiers HTML) sans aucun problème, mais renvoie l' **erreur HTTP 404,0 – introuvable** lorsque vous tentez d’accéder au contenu ASP.net. Vous pouvez utiliser la procédure suivante pour vous assurer que ASP.NET 4,0 est inscrit.

**Pour inscrire ASP.NET 4,0 avec IIS**

1. Cliquez sur **Démarrer**, puis tapez **invite de commandes**.
2. Dans les résultats de la recherche, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.
3. Dans la fenêtre d’invite de commandes, accédez au répertoire **%windir%\Microsoft.NET\Framework\v4.0.30319**
4. Tapez cette commande et appuyez sur entrée :

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Si vous envisagez d’héberger des applications Web 64 bits à tout moment, vous devez également enregistrer la version 64 bits de ASP.NET avec IIS. Pour ce faire, dans la fenêtre d’invite de commandes, accédez au répertoire **%windir%\Microsoft.NET\Framework64\v4.0.30319**
6. Tapez cette commande et appuyez sur entrée :

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Nous vous recommandons d’utiliser à nouveau Windows Update à ce stade pour télécharger et installer les mises à jour disponibles pour les nouveaux produits et composants que vous avez installés.

## <a name="configure-the-iis-website"></a>Configurer le site Web IIS

Avant de pouvoir déployer du contenu Web sur votre serveur, vous devez créer et configurer un site Web IIS pour héberger le contenu. Web Deploy pouvez déployer des packages Web uniquement sur un site Web IIS existant ; il ne peut pas créer le site Web pour vous. À un niveau élevé, vous devez effectuer les tâches suivantes :

- Créez un dossier sur le système de fichiers pour héberger votre contenu.
- Créez un site Web IIS pour servir le contenu, puis associez-le au dossier local.
- Accordez des autorisations de lecture à l’identité du pool d’applications sur le dossier local.

Bien que rien ne vous empêche de déployer du contenu vers le site Web par défaut dans IIS, cette approche n’est pas recommandée pour les scénarios de test ou de démonstration. Pour simuler un environnement de production, vous devez créer un nouveau site Web IIS avec des paramètres spécifiques à la configuration requise de votre application.

**Pour créer et configurer un site Web IIS**

1. Sur le système de fichiers local, créez un dossier pour stocker votre contenu (par exemple, **C:\DemoSite**).
2. Dans le menu **Démarrer** , pointez sur **Outils d’administration**, puis cliquez sur gestionnaire de **Internet Information Services (IIS)** .
3. Dans le gestionnaire des services Internet, dans le volet **connexions** , développez le nœud du serveur (par exemple, **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Cliquez avec le bouton droit sur le nœud **sites** , puis cliquez sur **Ajouter un site Web**.
5. Dans la zone **nom du site** , tapez un nom pour le site Web IIS (par exemple, **DemoSite**).
6. Dans la zone **chemin d’accès physique** , tapez (ou accédez à) le chemin d’accès à votre dossier local (par exemple, **C:\DemoSite**).
7. Dans la zone **port** , tapez le numéro de port sur lequel vous souhaitez héberger le site Web (par exemple, **85**).

    > [!NOTE]
    > Les numéros de port standard sont 80 pour HTTP et 443 pour HTTPs. Toutefois, si vous hébergez ce site Web sur le port 80, vous devez arrêter le site Web par défaut pour pouvoir accéder à votre site.
8. Laissez la zone **nom d’hôte** vide, à moins que vous ne souhaitiez configurer un enregistrement DNS (Domain Name System) pour le site Web, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > Dans un environnement de production, vous souhaiterez probablement héberger votre site Web sur le port 80 et configurer un en-tête d’hôte, ainsi que les enregistrements DNS correspondants. Pour plus d’informations sur la configuration des en-têtes d’hôte dans IIS 7, consultez [configurer un en-tête d’hôte pour un site Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Pour plus d’informations sur le rôle de serveur DNS dans Windows Server 2008 R2, consultez [vue d’ensemble du serveur DNS](https://technet.microsoft.com/library/cc770392.aspx) et [serveur DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Dans le volet **Actions** , sous **Modifier le Site**, cliquez sur **Liaisons**.
10. Dans la boîte de dialogue **liaisons de site** , cliquez sur **Ajouter**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. Dans la boîte de dialogue **Ajouter la liaison de site** , définissez l' **adresse IP** et le **port** correspondant à la configuration de votre site existant.
12. Dans la zone **nom d’hôte** , tapez le nom de votre serveur Web (par exemple, **PROWEB1**), puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > La première liaison de site vous permet d’accéder au site localement à l’aide de l’adresse IP et du port ou `http://localhost:85`. La deuxième liaison de site vous permet d’accéder au site à partir d’autres ordinateurs sur le domaine à l’aide du nom de l’ordinateur (par exemple, http://proweb1:85).
13. Dans la boîte de dialogue **liaisons de sites** , cliquez sur **Fermer**.
14. Dans le volet **connexions** , cliquez sur **pools d’applications**.
15. Dans le volet **pools d’applications** , cliquez avec le bouton droit sur le nom de votre pool d’applications, puis cliquez sur **paramètres de base**. Par défaut, le nom de votre pool d’applications correspond au nom de votre site Web (par exemple, **DemoSite**).
16. Dans la liste **.NET Framework version** , sélectionnez **.NET Framework v 4.0.30319**, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

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
3. Sous l’onglet **sécurité** , cliquez sur **modifier**, puis sur **Ajouter**.
4. Cliquez sur **emplacements**. Dans la boîte de dialogue **emplacements** , sélectionnez le serveur local, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , tapez **IIS\_IUSRS**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.
6. Dans la boîte de dialogue <strong>autorisations pour</strong><em>[nom du dossier]</em> , Notez que les autorisations <strong>lecture &amp; exécuter</strong>, <strong>afficher le contenu du dossier</strong>et <strong>lecture</strong> sont affectées par défaut au nouveau groupe. Laissez ce n’est pas modifié, puis cliquez sur <strong>OK</strong>.
7. Cliquez sur <strong>OK</strong> pour fermer la boîte de dialogue<strong>Propriétés</strong> de <em>[nom du dossier]</em>.

## <a name="disable-the-remote-agent-service"></a>Désactiver le service de l’agent distant

Lorsque vous installez Web Deploy, le service Web Deployment Agent est installé et démarré automatiquement. Ce service vous permet de déployer et de publier des packages Web à partir d’un emplacement distant. Vous n’utiliserez pas la fonctionnalité de déploiement à distance dans ce scénario. vous devez donc arrêter et désactiver le service.

> [!NOTE]
> Vous n’avez pas besoin d’arrêter le service de l’agent distant afin d’importer et de déployer un package Web manuellement. Toutefois, il est recommandé d’arrêter et de désactiver le service si vous n’envisagez pas de l’utiliser.

Vous pouvez arrêter et désactiver un service de plusieurs façons, à l’aide de différents utilitaires de ligne de commande ou cmdlets Windows PowerShell. Cette procédure décrit une approche simple basée sur l’interface utilisateur.

**Pour arrêter et désactiver le service de l’agent distant**

1. Sur le menu **Démarrer** , pointez sur **Outils d'administration**, puis cliquez sur **Services**.
2. Dans la console Services, localisez la ligne **Service Deployment agent Web** .

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Cliquez avec le bouton droit sur **service de Deployment agent Web**, puis cliquez sur **Propriétés**.
4. Dans la boîte de dialogue **Propriétés du service Web Deployment agent** , cliquez sur **arrêter**.
5. Dans la liste **type de démarrage** , sélectionnez **désactivé**, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Conclusion

À ce stade, votre serveur Web est prêt pour le déploiement de package Web hors connexion. Avant de tenter d’importer des packages Web sur un site Web IIS, vous souhaiterez peut-être vérifier les points clés suivants :

- Avez-vous enregistré ASP.NET 4,0 avec IIS ?
- L’identité du pool d’applications dispose-t-elle d’un accès en lecture au dossier source pour votre site Web ?
- Avez-vous arrêté le service Web Deployment Agent ?

> [!div class="step-by-step"]
> [Précédent](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [Suivant](configuring-a-database-server-for-web-deploy-publishing.md)
