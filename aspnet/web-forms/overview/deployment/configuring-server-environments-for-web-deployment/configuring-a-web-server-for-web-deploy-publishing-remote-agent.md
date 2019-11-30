---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Configuration d’un serveur Web pour la publication de Web Deploy (agent distant) | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment configurer un serveur Web Internet Information Services (IIS) pour prendre en charge la publication et le déploiement Web à l’aide du déploiement Web IIS...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: ce0d246afdfb65c2ea15a287064511e7d1d58622
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74589051"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Configuration d’un serveur web pour la publication Web Deploy (Agent distant)

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment configurer un serveur Web Internet Information Services (IIS) pour prendre en charge la publication et le déploiement Web à l’aide du service d’agent à distance de l’outil de déploiement Web (Web Deploy) IIS.
> 
> Lorsque vous travaillez avec Web Deploy 2,0 ou une version ultérieure, vous pouvez utiliser trois approches principales pour obtenir vos applications ou sites sur un serveur Web. Vous pouvez effectuer les tâches suivantes :
> 
> - Utilisez le *service de l’agent distant Web Deploy*. Cette approche requiert moins de configuration du serveur Web, mais vous devez fournir les informations d’identification d’un administrateur de serveur local pour pouvoir déployer n’importe quoi sur le serveur.
> - Utilisez le *Gestionnaire de Web Deploy*. Cette approche est beaucoup plus complexe et nécessite plus de travail initial pour configurer le serveur Web. Toutefois, lorsque vous utilisez cette approche, vous pouvez configurer IIS pour permettre aux utilisateurs non-administrateurs d’effectuer le déploiement. Le gestionnaire de Web Deploy est disponible uniquement dans IIS version 7 ou ultérieure.
> - Utilisez le *déploiement en mode hors connexion*. Cette approche nécessite la configuration minimale du serveur Web, mais un administrateur de serveur doit copier manuellement le package Web sur le serveur et l’importer par le biais du gestionnaire des services Internet.
> 
> Pour plus d’informations sur les fonctionnalités principales, les avantages et les inconvénients de ces approches, consultez [choix de l’approche appropriée pour le déploiement Web](choosing-the-right-approach-to-web-deployment.md).

## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>L’agent distant Web Deploy est-il la bonne approche pour vous ?

Oui, si l’utilisateur qui déploie le contenu peut fournir les informations d’identification d’un administrateur sur le serveur de destination. Cette approche est souvent souhaitable dans ces types de scénarios :

- Environnements de développement ou de test, où le développeur a un contrôle total sur le serveur Web de destination et le serveur de base de données.
- Organisations plus petites dans lesquelles un seul utilisateur ou un petit groupe d’utilisateurs contrôle le cycle de vie de l’application tout entière.

Dans de nombreuses organisations plus grandes, et en particulier pour les environnements intermédiaires ou de production, il n’est souvent pas réaliste de fournir aux utilisateurs des droits d’administrateur sur les serveurs Web. Dans le cas des serveurs Web hébergés, cela est très improbable. En outre, si vous envisagez d’automatiser le déploiement à partir d’un serveur de builds, vous ne souhaiterez peut-être pas utiliser les informations d’identification d’administrateur pour le processus de déploiement. Dans ces scénarios, la configuration de vos serveurs Web pour prendre en charge le déploiement à l’aide du [Gestionnaire de Web Deploy](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) peut fournir un choix plus satisfaisant.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Cette rubrique explique comment configurer un serveur Web Internet Information Services (IIS) 7,5 pour accepter et déployer des packages Web à partir d’un ordinateur distant à l’aide de l’approche Web Deploy de l’agent distant. Vous devez effectuer les opérations suivantes :

- Installez IIS 7,5 et la configuration IIS 7 recommandée.
- Installez Web Deploy 2,1 ou une version ultérieure.
- Créez un site Web IIS pour héberger le contenu déployé.
- Assurez-vous que le service Web Deployment Agent est en cours d’exécution.

Pour héberger l’exemple de solution, vous devez également effectuer les opérations suivantes :

- Installez le .NET Framework 4,0.
- Installez ASP.NET MVC 3.

Cette rubrique vous indique comment effectuer chacune de ces procédures. Les tâches et procédures pas à pas décrites dans cette rubrique partent du principe que vous commencez avec une nouvelle génération de serveur exécutant Windows Server 2008 R2. Avant de continuer, assurez-vous que :

- Windows Server 2008 R2 Service Pack 1 et toutes les mises à jour disponibles sont installées.
- Le serveur est joint à un domaine.
- Le serveur possède une adresse IP statique.

> [!NOTE]
> Pour plus d’informations sur la jonction d’ordinateurs à un domaine, consultez [jonction d’ordinateurs au domaine et ouverture de session](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Pour plus d’informations sur la configuration d’adresses IP statiques, consultez [configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Le service de l’agent distant est pris en charge par IIS 6 et ne nécessite pas que vous soyez joint à un domaine. Toutefois, les étapes de ce didacticiel ont été développées et testées sur IIS 7,5 et les procédures pour d’autres versions peuvent varier.

## <a name="install-products-and-components"></a>Installer les produits et les composants

Cette section vous guide tout au long de l’installation des produits et composants requis sur le serveur Web. Avant de commencer, il est recommandé d’exécuter Windows Update pour vous assurer que votre serveur est entièrement à jour.

Dans ce cas, vous devez installer les éléments suivants :

- **Configuration recommandée pour IIS 7**. Cela active le rôle de **serveur Web (IIS)** sur votre serveur Web et installe l’ensemble des modules et composants IIS dont vous avez besoin pour héberger une application ASP.net.
- **.NET Framework 4,0**. Cela est nécessaire pour exécuter des applications qui ont été générées sur cette version du .NET Framework.
- **Outil de déploiement Web 2,1 ou version ultérieure**. Cela installe Web Deploy (et son exécutable sous-jacent, MSDeploy. exe) sur votre serveur. Dans le cadre de ce processus, il installe et démarre le service Web Deployment Agent. Ce service vous permet de déployer des packages Web à partir d’un ordinateur distant.
- **ASP.NET MVC 3**. Cela installe les assemblys dont vous avez besoin pour exécuter les applications MVC 3.

> [!NOTE]
> Cette procédure pas à pas décrit l’utilisation de la Web Platform Installer pour installer et configurer les composants requis. Bien qu’il ne soit pas nécessaire d’utiliser la Web Platform Installer, elle simplifie le processus d’installation en détectant automatiquement les dépendances et en garantissant que vous recevez toujours les dernières versions du produit. Pour plus d’informations, consultez [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/?linkid=9805118).

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

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. Dans la ligne **ASP.NET MVC 3 (Visual Studio 2010)** , cliquez sur **Ajouter**.
7. Dans le volet de navigation, cliquez sur **serveur**.
8. Dans la ligne **Configuration recommandée pour IIS 7** , cliquez sur **Ajouter**.
9. Dans la ligne de l' **outil de déploiement Web 2,1** , cliquez sur **Ajouter**.
10. Cliquez sur **Suivant**. Le Web Platform Installer affiche une liste des produits&#x2014;avec les dépendances&#x2014;associées à installer et vous invite à accepter les termes du contrat de licence.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. Passez en revue les termes du contrat de licence et, si vous en acceptez les termes, cliquez sur **J’accepte**.
12. Une fois l’installation terminée, cliquez sur **Terminer**, puis fermez la fenêtre **Web Platform Installer 3,0** .

Si vous avez installé le .NET Framework 4,0 avant d’installer IIS, vous devez exécuter l' [outil d’inscription ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) pour inscrire la dernière version de ASP.net auprès d’IIS. Si vous ne le faites pas, vous constaterez que le service IIS traite le contenu statique (comme les fichiers HTML) sans aucun problème, mais renvoie l' **erreur HTTP 404,0 – introuvable** lorsque vous tentez d’accéder au contenu ASP.net. Vous pouvez utiliser cette procédure pour vous assurer que ASP.NET 4,0 est inscrit.

**Pour inscrire ASP.NET 4,0 avec IIS**

1. Cliquez sur **Démarrer**, puis tapez **invite de commandes**.
2. Dans les résultats de la recherche, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.
3. Dans la fenêtre d’invite de commandes, accédez au répertoire **%windir%\Microsoft.NET\Framework\v4.0.30319**
4. Tapez cette commande et appuyez sur entrée :

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. Si vous envisagez d’héberger des applications Web 64 bits à tout moment, vous devez également enregistrer la version 64 bits de ASP.NET avec IIS. Pour ce faire, dans la fenêtre d’invite de commandes, accédez au répertoire **%windir%\Microsoft.NET\Framework64\v4.0.30319**
6. Tapez cette commande et appuyez sur entrée :

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

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
3. Dans le gestionnaire des services Internet, dans le volet **connexions** , développez le nœud du serveur (par exemple, **TESTWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. Cliquez avec le bouton droit sur le nœud **sites** , puis cliquez sur **Ajouter un site Web**.
5. Dans la zone **nom du site** , tapez un nom pour le site Web IIS (par exemple, **DemoSite**).
6. Dans la zone **chemin d’accès physique** , tapez (ou accédez à) le chemin d’accès à votre dossier local (par exemple, **C:\DemoSite**).
7. Dans la zone **port** , tapez le numéro de port sur lequel vous souhaitez héberger le site Web (par exemple, **85**).

    > [!NOTE]
    > Les numéros de port standard sont 80 pour HTTP et 443 pour HTTPs. Toutefois, si vous hébergez ce site Web sur le port 80, vous devez arrêter le site Web par défaut pour pouvoir accéder à votre site.
8. Laissez la zone **nom d’hôte** vide, à moins que vous ne souhaitiez configurer un enregistrement DNS (Domain Name System) pour le site Web, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > Dans un environnement de production, vous souhaiterez probablement héberger votre site Web sur le port 80 et configurer un en-tête d’hôte, ainsi que les enregistrements DNS correspondants. Pour plus d’informations sur la configuration des en-têtes d’hôte dans IIS 7, consultez [configurer un en-tête d’hôte pour un site Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Pour plus d’informations sur le rôle de serveur DNS dans Windows Server 2008 R2, consultez [vue d’ensemble du serveur DNS](https://technet.microsoft.com/library/cc770392.aspx) et [serveur DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. Dans le volet **Actions** , sous **Modifier le Site**, cliquez sur **Liaisons**.
10. Dans la boîte de dialogue **liaisons de site** , cliquez sur **Ajouter**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. Dans la boîte de dialogue **Ajouter la liaison de site** , définissez l' **adresse IP** et le **port** correspondant à la configuration de votre site existant.
12. Dans la zone **nom d’hôte** , tapez le nom de votre serveur Web (par exemple, **TESTWEB1**), puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > La première liaison de site vous permet d’accéder au site localement à l’aide de l’adresse IP et du port ou `http://localhost:85`. La deuxième liaison de site vous permet d’accéder au site à partir d’autres ordinateurs sur le domaine à l’aide du nom de l’ordinateur (par exemple, http://testweb1:85).
13. Dans la boîte de dialogue **liaisons de sites** , cliquez sur **Fermer**.
14. Dans le volet **connexions** , cliquez sur **pools d’applications**.
15. Dans le volet **pools d’applications** , cliquez avec le bouton droit sur le nom de votre pool d’applications, puis cliquez sur **paramètres de base**. Par défaut, le nom de votre pool d’applications correspond au nom de votre site Web (par exemple, **DemoSite**).
16. Dans la liste **.NET Framework version** , sélectionnez **.NET Framework v 4.0.30319**, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > L’exemple de solution requiert .NET Framework 4,0. Cela n’est pas obligatoire pour les Web Deploy en général.

Pour que votre site Web puisse traiter du contenu, l’identité du pool d’applications doit avoir des autorisations de lecture sur le dossier local qui stocke le contenu. Dans IIS 7,5, les pools d’applications s’exécutent avec une identité de pool d’applications unique par défaut (contrairement aux versions antérieures d’IIS, où les pools d’applications s’exécutent généralement en utilisant le compte de service réseau). L’identité du pool d’applications n’est pas un compte d’utilisateur réel et n’apparaît pas sur les listes d'&#x2014;utilisateurs ou de groupes à la place, elle est créée dynamiquement au démarrage du pool d’applications. Chaque identité du pool d’applications est ajoutée au groupe de sécurité **IIS\_IUSRS** local en tant qu’élément masqué.

Pour accorder des autorisations à l’identité d’un pool d’applications sur un fichier ou un dossier, deux options s’offrent à vous :

- Affectez directement des autorisations à l’identité du pool d’applications, à l’aide du format <strong>\<d’IIS AppPool/strong ><em>[nom du pool d’applications]</em>(par exemple, <strong>IIS AppPool\DemoSite</strong>).
- Attribuez des autorisations au groupe **IUSRS IIS\_** .

L’approche la plus courante consiste à assigner des autorisations au groupe local **IIS\_IUSRS** , car cette approche vous permet de modifier des pools d’applications sans reconfigurer les autorisations du système de fichiers. La procédure suivante utilise cette approche basée sur les groupes.

> [!NOTE]
> Pour plus d’informations sur les identités du pool d’applications dans IIS 7,5, consultez [identités du pool d’applications](https://go.microsoft.com/?linkid=9805123).

**Pour configurer les autorisations de dossier pour un site Web IIS**

1. Dans l’Explorateur Windows, accédez à l’emplacement de votre dossier local.
2. Cliquez avec le bouton droit sur le dossier, puis cliquez sur **Propriétés**.
3. Sous l’onglet **sécurité** , cliquez sur **modifier**, puis sur **Ajouter**.
4. Cliquez sur **emplacements**. Dans la boîte de dialogue **emplacements** , sélectionnez le serveur local, puis cliquez sur **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , tapez **IIS\_IUSRS**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.
6. Dans la boîte de dialogue <strong>autorisations pour</strong><em>[nom du dossier]</em>, Notez que les autorisations <strong>lecture &amp; exécuter</strong>, <strong>afficher le contenu du dossier</strong>et <strong>lecture</strong> sont affectées par défaut au nouveau groupe. Laissez ce n’est pas modifié, puis cliquez sur <strong>OK</strong>.
7. Cliquez sur <strong>OK</strong> pour fermer la boîte de dialogue<strong>Propriétés</strong> de <em>[nom du dossier]</em>.

Avant de tenter de déployer des packages Web sur votre serveur, vous devez vous assurer que le service Web Deployment Agent est en cours d’exécution. Lorsque vous déployez un package à partir d’un ordinateur distant, le service de Deployment Agent Web est responsable de l’extraction et de l’installation du contenu du package. Le service est démarré par défaut lorsque vous installez l’outil de déploiement Web et s’exécute sous l’identité du service réseau.

Vous pouvez vérifier si un service s’exécute de plusieurs façons différentes, à l’aide de différents utilitaires de ligne de commande ou cmdlets Windows PowerShell. Cette procédure décrit une approche simple basée sur l’interface utilisateur.

**Pour vérifier que le service Web Deployment Agent est en cours d’exécution**

1. Sur le menu **Démarrer** , pointez sur **Outils d'administration**, puis cliquez sur **Services**.
2. Recherchez la ligne **Service Deployment agent Web** et vérifiez que l' **État** est défini sur **démarré**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. Si le service n’est pas déjà démarré, cliquez sur **Démarrer**.

## <a name="configure-firewall-exceptions"></a>Configurer des exceptions de pare-feu

Par défaut, le service de l’agent distant écoute le port TCP 80, à l’adresse suivante :

<http://servername.com/MSDEPLOYAGENTSERVICE>

Dans la plupart des cas, vous n’avez pas besoin de configurer des règles de pare-feu supplémentaires pour le service de l’agent distant, car les serveurs Web écoutent généralement les requêtes HTTP sur le port 80. Si vous avez personnalisé votre installation pour écouter sur un port non standard, vous devez configurer les exceptions de pare-feu en fonction des besoins.

## <a name="conclusion"></a>Conclusion

À ce stade, votre serveur Web est prêt à accepter et à installer des packages Web à partir d’un ordinateur distant. Avant de tenter de déployer une application Web sur le serveur, vous souhaiterez peut-être vérifier les points clés suivants :

- Avez-vous enregistré ASP.NET 4,0 avec IIS ?
- L’identité du pool d’applications dispose-t-elle d’un accès en lecture au dossier source pour votre site Web ?
- Le service Web Deployment Agent est-il en cours d’exécution ?

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la configuration des fichiers de projet de Microsoft Build Engine personnalisé (MSBuild) pour déployer des packages Web sur le service de l’agent distant, consultez [Configuration des propriétés de déploiement pour un environnement cible](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Précédent](scenario-configuring-a-production-environment-for-web-deployment.md)
> [Suivant](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
