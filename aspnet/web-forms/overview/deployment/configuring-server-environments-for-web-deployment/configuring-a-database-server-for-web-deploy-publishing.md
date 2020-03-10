---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Configuration d’un serveur de base de données pour la publication Web Deploy | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment configurer un serveur de base de données SQL Server 2008 R2 pour prendre en charge le déploiement et la publication Web. Les tâches décrites dans cette rubrique sont co...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: ade3c1ba1c470092f512436f39b8831458408c2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634616"
---
# <a name="configuring-a-database-server-for-web-deploy-publishing"></a>Configuration d’un serveur de base de données pour la publication Web Deploy

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment configurer un serveur de base de données SQL Server 2008 R2 pour prendre en charge le déploiement et la publication Web.
> 
> Les tâches décrites dans cette rubrique sont communes à chaque scénario&#x2014;de déploiement, peu importe si vos serveurs Web sont configurés pour utiliser le service de l’agent distant (Web Deploy) IIS, le gestionnaire de Web Deploy ou le déploiement hors connexion, ou si votre application s’exécute sur un serveur Web ou une batterie de serveurs unique. La façon dont vous déployez la base de données peut changer en fonction des exigences de sécurité et d’autres considérations. Par exemple, vous pouvez déployer la base de données avec ou sans exemples de données, et vous pouvez déployer des mappages de rôles d’utilisateur ou les configurer manuellement après le déploiement. Toutefois, la façon dont vous configurez le serveur de base de données reste la même.

Vous n’êtes pas obligé d’installer des produits ou des outils supplémentaires pour configurer un serveur de base de données afin de prendre en charge le déploiement Web. En supposant que votre serveur de base de données et votre serveur Web s’exécutent sur des machines différentes, vous devez simplement :

- Autoriser SQL Server à communiquer à l’aide de TCP/IP.
- Autorisez le trafic SQL Server à travers les pare-feu.
- Accordez au compte d’ordinateur du serveur Web une connexion SQL Server.
- Mappez la connexion du compte d’ordinateur aux rôles de base de données requis.
- Donnez au compte qui exécutera le déploiement un SQL Server autorisations de connexion et de créateur de base de données.
- Pour prendre en charge les déploiements répétés, mappez la connexion du compte de déploiement au rôle de base de données propriétaire de la base de données **\_** .

Cette rubrique vous indique comment effectuer chacune de ces procédures. Les tâches et procédures pas à pas de cette rubrique partent du principe que vous commencez avec une instance par défaut de SQL Server 2008 R2 s’exécutant sur Windows Server 2008 R2. Avant de continuer, assurez-vous que :

- Windows Server 2008 R2 Service Pack 1 et toutes les mises à jour disponibles sont installées.
- Le serveur est joint à un domaine.
- Le serveur possède une adresse IP statique.
- SQL Server 2008 R2 Service Pack 1 et toutes les mises à jour disponibles sont installées.

L’instance de SQL Server doit inclure uniquement le rôle **Services moteur de base de données** , qui est inclus automatiquement dans toute installation SQL Server. Toutefois, pour faciliter la configuration et la maintenance, nous vous recommandons d’inclure les **outils de gestion – de base** et les outils de **gestion –** rôles serveur complets.

> [!NOTE]
> Pour plus d’informations sur la jonction d’ordinateurs à un domaine, consultez [jonction d’ordinateurs au domaine et ouverture de session](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Pour plus d’informations sur la configuration d’adresses IP statiques, consultez [configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Pour plus d’informations sur l’installation de SQL Server, consultez [installation de SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).

## <a name="enable-remote-access-to-sql-server"></a>Activer l’accès à distance à SQL Server

SQL Server utilise le protocole TCP/IP pour communiquer avec les ordinateurs distants. Si votre serveur de base de données et votre serveur Web se trouvent sur des ordinateurs différents, vous devez :

- Configurez SQL Server paramètres réseau pour permettre la communication via TCP/IP.
- Configurez tous les pare-feu matériels ou logiciels pour autoriser le trafic TCP (et, dans certains cas, le trafic UDP (User Datagram Protocol)) sur les ports utilisés par l’instance SQL Server.

Pour permettre à SQL Server de communiquer via TCP/IP, utilisez Gestionnaire de configuration SQL Server pour modifier la configuration réseau de votre instance SQL Server.

**Pour permettre à SQL Server de communiquer à l’aide de TCP/IP**

1. Dans le menu **Démarrer** , pointez **sur tous les programmes**, cliquez sur **Microsoft SQL Server 2008 R2**, sur **outils de configuration**, puis sur **Gestionnaire de configuration SQL Server**.
2. Dans le volet de l’arborescence, développez **SQL Server Configuration réseau**, puis cliquez sur **protocoles pour MSSQLSERVER**.

   > [!NOTE]
   > Si vous avez installé plusieurs instances de SQL Server, vous verrez un élément <strong>protocoles pour</strong>l’élément<em>[nom de l’instance]</em> pour chaque instance. Vous devez configurer les paramètres réseau en fonction de l’instance.
3. Dans le volet d’informations, cliquez avec le bouton droit sur la ligne **TCP/IP** , puis cliquez sur **activer**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. Dans la boîte de dialogue d' **Avertissement** , cliquez sur **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Vous devez redémarrer le service MSSQLSERVER pour que la nouvelle configuration du réseau prenne effet. Vous pouvez le faire à partir d’une invite de commandes, de la console Services ou de SQL Server Management Studio. Dans cette procédure, vous allez utiliser SQL Server Management Studio.
6. Fermez le Gestionnaire de configuration de SQL Server.
7. Dans le menu **Démarrer** , pointez sur **tous les programmes**, cliquez sur **Microsoft SQL Server 2008 R2**, puis sur **SQL Server Management Studio**.
8. Dans la boîte de dialogue **se connecter au serveur** , dans la zone **nom du serveur** , tapez le nom du serveur de base de données, puis cliquez sur **se connecter**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. Dans le volet **Explorateur d’objets** , cliquez avec le bouton droit sur le nœud du serveur parent (par exemple, **TESTDB1**), puis cliquez sur **redémarrer**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. Dans la boîte de dialogue **Microsoft SQL Server Management Studio** , cliquez sur **Oui**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Une fois le service redémarré, fermez SQL Server Management Studio.

Pour autoriser le trafic SQL Server à travers un pare-feu, vous devez d’abord connaître les ports utilisés par votre instance de SQL Server. Cela dépend de la façon dont l’instance de SQL Server a été créée et configurée :

- Une *instance par défaut* de SQL Server écoute les demandes (et répond à) sur le port TCP 1433.
- Une *instance nommée* de SQL Server écoute les demandes (et répond à) sur un port TCP attribué dynamiquement.
- Si le service SQL Server Browser est activé, les clients peuvent interroger le service sur le port UDP 1434 pour identifier le port TCP à utiliser pour une instance de SQL Server particulière. Toutefois, ce service est souvent désactivé pour des raisons de sécurité.

En supposant que vous utilisez une instance par défaut de SQL Server, vous devez configurer votre pare-feu pour autoriser le trafic.

| Sens | À partir du port | Vers le port | Type de port |
| --- | --- | --- | --- |
| Trafic entrant | Any | 1433 | TCP |
| Règle de trafic sortant | 1433 | Any | TCP |

> [!NOTE]
> Techniquement, un ordinateur client utilise un port TCP attribué de façon aléatoire entre 1024 et 5000 pour communiquer avec SQL Server, et vous pouvez limiter vos règles de pare-feu en conséquence. Pour plus d’informations sur les ports de SQL Server et les pare-feu, consultez [numéros de port TCP/IP requis pour communiquer avec SQL via un pare-feu](https://go.microsoft.com/?linkid=9805125) et [procédure : configurer un serveur pour écouter un port TCP spécifique (gestionnaire de configuration SQL Server)](https://msdn.microsoft.com/library/ms177440.aspx).

Dans la plupart des environnements Windows Server, vous devrez probablement configurer le pare-feu Windows sur le serveur de base de données. Par défaut, le pare-feu Windows autorise tout le trafic sortant, sauf si une règle l’interdit spécifiquement. Pour permettre à votre serveur Web d’atteindre votre base de données, vous devez configurer une règle de trafic entrant qui autorise le trafic TCP sur le numéro de port utilisé par l’instance SQL Server. Si vous utilisez une instance par défaut de SQL Server, vous pouvez utiliser la procédure suivante pour configurer cette règle.

**Pour configurer le pare-feu Windows afin d’autoriser la communication avec une instance de SQL Server par défaut**

1. Sur le serveur de base de données, dans le menu **Démarrer** , pointez sur **Outils d’administration**, puis cliquez sur **pare-feu Windows avec fonctions avancées de sécurité**.
2. Dans le volet de l’arborescence, cliquez sur **règles de trafic entrant**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. Dans le volet **actions** , sous **règles de trafic entrant**, cliquez sur **nouvelle règle**.
4. Dans l’Assistant Nouvelle règle de trafic entrant, dans la page **type de règle** , sélectionnez **port**, puis cliquez sur **suivant**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Dans la page **protocole et ports** , vérifiez que **TCP** est sélectionné et, dans la zone **Ports locaux spécifiques** , tapez **1433**, puis cliquez sur **suivant**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Dans la page **action** , laissez **l’option autoriser la connexion** sélectionnée, puis cliquez sur **suivant**.
7. Dans la page **Profil** , laissez l’option **domaine** sélectionné, désactivez les cases à cocher **privé** et **public** , puis cliquez sur **suivant**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Dans la page **nom** , attribuez un nom descriptif approprié à la règle (par exemple, **SQL Server instance par défaut – accès réseau**), puis cliquez sur **Terminer**.

Pour plus d’informations sur la configuration du pare-feu Windows pour SQL Server, en particulier si vous devez communiquer avec SQL Server sur des ports non standard ou dynamiques, consultez [procédure : configurer un pare-feu Windows pour l’accès à la moteur de base de données](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Configurer les connexions et les autorisations de base de données

Lorsque vous déployez une application Web sur Internet Information Services (IIS), l’application s’exécute à l’aide de l’identité du pool d’applications. Dans un environnement de domaine, les identités du pool d’applications utilisent le compte d’ordinateur du serveur sur lequel elles s’exécutent pour accéder aux ressources réseau. Les comptes d’ordinateur prennent la forme <em>[nom de domaine]</em><strong>\</strong ><em>[nom de l’ordinateur]</em> <strong>$</strong> &#x2014;par exemple, <strong>FABRIKAM\TESTWEB1 $</strong>. Pour permettre à votre application Web d’accéder à une base de données sur le réseau, vous devez :

- Ajoutez une connexion pour le compte d’ordinateur du serveur Web à l’instance SQL Server.
- Mappez la connexion du compte d’ordinateur aux rôles de base de données requis (en général, **db\_DataReader** et **DB\_DataWriter**).

Si votre application Web s’exécute sur une batterie de serveurs, plutôt que sur un serveur unique, vous devez répéter ces procédures pour chaque serveur Web de la batterie de serveurs.

> [!NOTE]
> Pour plus d’informations sur les identités du pool d’applications et l’accès aux ressources réseau, consultez [identités du pool d’applications](https://go.microsoft.com/?linkid=9805123).

Vous pouvez aborder ces tâches de différentes façons. Pour créer la connexion, vous pouvez :

- Créez la connexion manuellement sur le serveur de base de données, à l’aide de Transact-SQL ou SQL Server Management Studio.
- Utilisez un projet de serveur SQL Server 2008 dans Visual Studio pour créer et déployer la connexion.

Une connexion SQL Server est un objet au niveau du serveur, plutôt qu’un objet au niveau de la base de données, elle n’est donc pas dépendante de la base de données que vous souhaitez déployer. Par conséquent, vous pouvez créer la connexion à tout moment, et l’approche la plus simple consiste à créer manuellement la connexion sur le serveur de base de données avant de commencer le déploiement des bases de données. Vous pouvez utiliser la procédure suivante pour créer une connexion dans SQL Server Management Studio.

**Pour créer une connexion SQL Server pour le compte d’ordinateur du serveur Web**

1. Sur le serveur de base de données, dans le menu **Démarrer** , pointez sur **tous les programmes**, cliquez sur **Microsoft SQL Server 2008 R2**, puis sur **SQL Server Management Studio**.
2. Dans la boîte de dialogue **se connecter au serveur** , dans la zone **nom du serveur** , tapez le nom du serveur de base de données, puis cliquez sur **se connecter**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. Dans le volet **Explorateur d’objets** , cliquez avec le bouton droit sur **sécurité**, pointez sur **nouveau**, puis cliquez sur **connexion**.
4. Dans la boîte de dialogue **nouvelle connexion** , dans la zone **nom de connexion** , tapez le nom de votre compte d’ordinateur de serveur Web (par exemple, **FABRIKAM\TESTWEB1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Cliquez sur **OK**.

À ce stade, votre serveur de base de données est prêt pour la publication Web Deploy. Toutefois, toutes les solutions que vous déployez ne fonctionneront pas tant que vous n’aurez pas mappé la connexion du compte d’ordinateur aux rôles de base de données requis. Le mappage de la connexion aux rôles de base de données nécessite beaucoup plus de réflexion, car vous ne pouvez pas mapper les rôles tant que vous n’avez pas déployé la base de données. Pour mapper la connexion du compte d’ordinateur aux rôles de base de données requis, vous pouvez :

- Affectez manuellement les rôles de base de données à la connexion, une fois que vous avez déployé la base de données pour la première fois.
- Utilisez un script de prédéploiement pour affecter les rôles de base de données à la connexion.

Pour plus d’informations sur l’automatisation de la création de connexions et de mappages de rôles de base de données, consultez [déploiement d’appartenances à des rôles de base de données dans des environnements de test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Vous pouvez également utiliser la procédure suivante pour mapper manuellement la connexion du compte d’ordinateur aux rôles de base de données requis. N’oubliez pas que vous ne pouvez pas effectuer cette *procédure tant que vous n’avez pas* déployé la base de données.

**Pour mapper des rôles de base de données à la connexion du compte d’ordinateur du serveur Web**

1. Ouvrez SQL Server Management Studio comme avant.
2. Dans le volet **Explorateur d’objets** , développez le nœud **sécurité** , développez le nœud **connexions** , puis double-cliquez sur la connexion du compte d’ordinateur (par exemple, **FABRIKAM\TESTWEB1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. Dans la boîte de dialogue Propriétés de la **connexion** , cliquez sur mappage de l' **utilisateur**.
4. Dans la table **utilisateurs mappés à cette connexion** , sélectionnez le nom de votre base de données (par exemple, **ContactManager**).
5. Dans la liste **appartenance au rôle de base de données pour :** *[nom de la base de données]* , sélectionnez les autorisations requises. Dans le cas de l’exemple de solution du gestionnaire de contacts, vous devez sélectionner les rôles **db\_DataReader** et **DB\_DataWriter** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Cliquez sur **OK**.

Bien que le mappage manuel des rôles de base de données soit souvent plus adapté pour les environnements de test, il est moins souhaitable pour les déploiements automatisés ou en un seul clic dans des environnements intermédiaires ou de production. Vous trouverez plus d’informations sur l’automatisation de ce type de tâche à l’aide de scripts de prédéploiement dans [déploiement des appartenances de rôle de base de données](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)dans des environnements de test.

> [!NOTE]
> Pour plus d’informations sur les projets de serveur et les projets de base de données, consultez [Visual Studio 2010 SQL Server des projets de base de données](https://msdn.microsoft.com/library/ff678491.aspx).

## <a name="configure-permissions-for-the-deployment-account"></a>Configurer les autorisations pour le compte de déploiement

Si le compte que vous allez utiliser pour exécuter le déploiement n’est pas un administrateur SQL Server, vous devez également créer une connexion pour ce compte. Pour créer la base de données, le compte doit être membre du rôle de serveur **dbcreator** ou posséder des autorisations équivalentes.

> [!NOTE]
> Quand vous utilisez Web Deploy ou VSDBCMD pour déployer une base de données, vous pouvez utiliser les informations d’identification Windows ou les informations d’identification de SQL Server (si votre instance SQL Server est configurée pour prendre en charge l’authentification en mode mixte). La procédure suivante suppose que vous souhaitez utiliser les informations d’identification Windows, mais rien ne vous empêche de spécifier un nom d’utilisateur et un mot de passe SQL Server dans votre chaîne de connexion lorsque vous configurez le déploiement.

**Pour configurer des autorisations pour le compte de déploiement**

1. Ouvrez SQL Server Management Studio comme avant.
2. Dans le volet **Explorateur d’objets** , cliquez avec le bouton droit sur **sécurité**, pointez sur **nouveau**, puis cliquez sur **connexion**.
3. Dans la boîte de dialogue **nouvelle connexion** , dans la zone **nom de connexion** , tapez le nom de votre compte de déploiement (par exemple, **FABRIKAM\matt**).
4. Dans le volet **Sélectionner une page** , cliquez sur **rôles serveur**.
5. Sélectionnez **dbcreator**, puis cliquez sur **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Pour prendre en charge les déploiements suivants, vous devez également ajouter le compte de déploiement au rôle de **propriétaire de\_DB** sur la base de données après le premier déploiement. Cela est dû au fait que lors des déploiements suivants, vous modifiez le schéma d’une base de données existante, plutôt que de créer une nouvelle base de données. Comme décrit dans la section précédente, vous ne pouvez pas ajouter un utilisateur à un rôle de base de données tant que vous n’avez pas créé la base de données, pour des raisons évidentes.

**Pour mapper la connexion du compte de déploiement au rôle de base de données du propriétaire de la base de données\_**

1. Ouvrez SQL Server Management Studio comme avant.
2. Dans la fenêtre **Explorateur d’objets** , développez le nœud **sécurité** , développez le nœud **connexions** , puis double-cliquez sur la connexion du compte d’ordinateur (par exemple, **FABRIKAM\matt**).
3. Dans la boîte de dialogue Propriétés de la **connexion** , cliquez sur mappage de l' **utilisateur**.
4. Dans la table **utilisateurs mappés à cette connexion** , sélectionnez le nom de votre base de données (par exemple, **ContactManager**).
5. Dans la liste **appartenance au rôle de base de données pour :** *[nom de la base de données]* , sélectionnez le rôle propriétaire de la **\_DB** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Cliquez sur **OK**.

## <a name="conclusion"></a>Conclusion

Votre serveur de base de données doit maintenant être prêt à accepter les déploiements de bases de données distantes et à autoriser les serveurs Web IIS distants à accéder à vos bases de données. Avant de tenter de déployer et d’utiliser des bases de données, vous souhaiterez peut-être vérifier les points clés suivants :

- Avez-vous configuré SQL Server pour accepter les connexions TCP/IP distantes ?
- Avez-vous configuré des pare-feu pour autoriser le trafic SQL Server ?
- Avez-vous créé une connexion de compte d’ordinateur pour chaque serveur Web qui accédera à SQL Server ?
- Votre déploiement de base de données inclut-il un script pour créer des mappages de rôle d’utilisateur, ou devez-vous les créer manuellement après avoir déployé la base de données pour la première fois ?
- Avez-vous créé une connexion pour le compte de déploiement et vous l’avez ajoutée au rôle de serveur **dbcreator** ?

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur le déploiement de projets de base de données, consultez [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pour obtenir des conseils sur la création d’appartenances à un rôle de base de données en exécutant un script de publication, consultez [déploiement d’appartenances à des rôles de base de données dans des environnements de test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Pour obtenir des conseils sur la façon de répondre aux défis de déploiement uniques que posent les bases de données d’appartenance, consultez [déploiement de bases de données d’appartenance dans des environnements d’entreprise](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Précédent](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Suivant](creating-a-server-farm-with-the-web-farm-framework.md)
