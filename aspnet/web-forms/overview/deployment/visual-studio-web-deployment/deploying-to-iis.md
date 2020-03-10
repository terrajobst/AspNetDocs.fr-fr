---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement à tester | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 738318cce442fdc5d58dd1e4c992d4941be2487e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640601"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement à tester

par [Tom Dykstra](https://github.com/tdykstra)

Cette série de didacticiels montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers à l’aide de Visual Studio 2017. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

Pour obtenir une version actuelle du déploiement sur Azure, consultez [créer une application web ASP.net core dans Azure](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="overview"></a>Présentation

Dans ce didacticiel, vous allez déployer une application Web ASP.NET sur Internet Information Server (IIS) sur votre ordinateur local.

En général, lorsque vous développez une application, vous l’exécutez et la Testez dans Visual Studio. Par défaut, les projets d’application Web dans Visual Studio 2017 utilisent IIS Express comme serveur Web de développement. IIS Express se comporte davantage comme IIS complet que le Serveur Visual Studio Development (également appelé Cassini), que Visual Studio 2017 utilise par défaut. Mais aucun serveur Web de développement ne fonctionne exactement comme IIS. Par conséquent, une application peut s’exécuter et effectuer des tests correctement dans Visual Studio, mais échouer lorsqu’elle est déployée sur IIS.

Vous pouvez tester votre application de deux manières en toute fiabilité :

1. Déployez votre application sur IIS sur votre ordinateur de développement à l’aide du même processus que celui que vous utiliserez ultérieurement pour le déployer dans votre environnement de production.

   Vous pouvez configurer Visual Studio pour utiliser IIS quand vous exécutez un projet Web, mais cela ne testerait pas votre processus de déploiement. Cette méthode valide votre processus de déploiement et que votre application s’exécute correctement sous IIS.

2. Déployez votre application dans un environnement de test similaire à votre environnement de production. 
 
   L’environnement de production pour ces didacticiels est Web Apps dans Azure App Service. L’environnement de test idéal est une application Web supplémentaire créée dans le service Azure. Bien qu’elle soit configurée de la même façon qu’une application Web de production, vous ne pouvez l’utiliser que pour les tests.

L’option 2 est la méthode la plus fiable pour tester. Si vous utilisez l’option 2, vous n’avez pas nécessairement besoin d’utiliser l’option 1. Toutefois, si vous effectuez un déploiement sur un fournisseur d’hébergement tiers, l’option 2 n’est peut-être pas réalisable ou peut être coûteuse. cette série de didacticiels illustre donc les deux méthodes. Vous trouverez des conseils pour l’option 2 dans le didacticiel [sur le déploiement dans l’environnement de production](deploying-to-production.md) .

Pour plus d’informations sur l’utilisation de serveurs Web dans Visual Studio, consultez [serveurs Web dans Visual Studio pour les projets web ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Télécharger le projet de démarrage de Contoso University

Téléchargez et installez la solution et le projet de démarrage de Contoso University Visual Studio. Cette solution contient le didacticiel terminé. 

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>Installer IIS

Pour effectuer un déploiement sur IIS sur votre ordinateur de développement, vérifiez qu’IIS et Web Deploy sont installés. Par défaut, Visual Studio installe Web Deploy, mais IIS n’est pas inclus dans la configuration par défaut de Windows 10, Windows 8 ou Windows 7. Si vous avez déjà installé IIS et que le pool d’applications par défaut est déjà défini sur .NET 4, passez à [la section suivante](#sqlexpress).

1. Il est recommandé d’utiliser le [Web Platform Installer (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) pour installer IIS et Web Deploy. WPI installe une configuration IIS recommandée qui comprend IIS et Web Deploy conditions préalables requises.

   Si vous avez déjà installé IIS, Web Deploy ou l’un de ses composants requis, l’WPI installe uniquement ce qui est manquant.

   * Utilisez la Web Platform Installer pour installer IIS et Web Deploy :
    
     ![Installer IIS à l’aide d’WPI](deploying-to-iis/_static/image30.png)

     ![Installer Web Deploy à l’aide de WPI](deploying-to-iis/_static/image31.png)

     Des messages indiquant qu’IIS 7 sera installé s’affichent. Le lien fonctionne pour IIS 8 dans Windows 8 ; Toutefois, pour Windows 8 et versions ultérieures, suivez les étapes ci-dessous pour vous assurer que ASP.NET 4,7 est installé :

   * Ouvrez **le panneau de configuration** > **programmes** > **programmes et fonctionnalités** > **activer ou désactiver des fonctionnalités Windows**.

   * Développez **Internet Information Services**, **Services World Wide Web**et **fonctionnalités de développement d’applications**.
   
   * Vérifiez que **ASP.NET 4,7** est sélectionné.

     ![Sélectionner ASP.NET 4,7](deploying-to-iis/_static/image1a.png)

   * Vérifiez que **World Wide Web services** et la **console de gestion IIS** sont sélectionnés. Cela installe IIS et le gestionnaire des services Internet.
    
     ![Sélectionner les services World Wide Web](deploying-to-iis/_static/image24.png)    
  
   * Sélectionnez **OK**. Des messages de boîte de dialogue indiquant que l’installation est en cours d’exécution s’affichent.

Après l’installation d’IIS, exécutez le **Gestionnaire des services Internet** pour vous assurer que le .NET Framework version 4 est affecté au pool d’applications par défaut.

1. Appuyez sur WINDOWS + R pour ouvrir la boîte de dialogue **exécuter** .

   (Sous Windows 8 ou version ultérieure, entrez « exécuter » dans la page de **démarrage** . Dans Windows 7, sélectionnez **exécuter** dans le menu **Démarrer** . Si **exécuter** ne se trouve pas dans le menu **Démarrer** , cliquez avec le bouton droit sur la barre des tâches, sélectionnez **Propriétés**, sélectionnez l’onglet **menu Démarrer** , sélectionnez **personnaliser**, puis sélectionnez **exécuter la commande**.)

2. Entrez « inetmgr », puis sélectionnez **OK**.

3. Dans le volet **connexions** , développez le nœud du serveur, puis sélectionnez **pools d’applications**. Dans le volet **pools d’applications** , si **DefaultAppPool** est affecté à la version 4 du .NET Framework, comme dans l’illustration suivante, passez à la section suivante.

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. Si vous ne voyez que deux pools d’applications et que les deux sont définis sur .NET Framework 2,0, installez ASP.NET 4 dans IIS.

   Pour Windows 8 ou version ultérieure, consultez les instructions de la section précédente pour vous assurer que ASP.NET 4,7 est installé ou voir [Comment installer ASP.NET 4,5 sur Windows 8 et Windows Server 2012](https://support.microsoft.com/kb/2736284). Pour Windows 7, ouvrez une fenêtre d’invite de commandes en cliquant avec le bouton droit sur **invite de commandes** dans le menu **Démarrer** de Windows, puis en sélectionnant **exécuter en tant qu’administrateur**. Exécutez [aspnet\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) pour installer ASP.net 4 dans IIS à l’aide des commandes suivantes. (Dans les systèmes 32 bits, remplacez « Framework64 » par « Framework ».)

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   Cette commande permet de créer de nouveaux pools d’applications pour le .NET Framework 4, mais le pool d’applications par défaut reste défini sur 2,0. Si vous déployez une application qui cible .NET 4 vers ce pool d’applications, remplacez le pool d’applications par .NET 4.

5. Si vous avez fermé le **Gestionnaire des services Internet**, réexécutez-le, développez le nœud du serveur, puis sélectionnez **pools d’applications**.

6. Dans le volet **pools d’applications** , sélectionnez **DefaultAppPool**. Dans le volet **actions** , sélectionnez **paramètres de base**.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. Dans la boîte de dialogue **modifier le pool d’applications** , remplacez la **version CLR .net** par **.NET CLR v 4.0.30319**. Sélectionnez **OK**.

   ![Selecting_. NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

Vous êtes maintenant prêt à publier une application Web sur IIS. Toutefois, créez d’abord des bases de données à des fins de test.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Installation de SQL Server Express

La base de données locale n’étant pas conçue pour fonctionner dans IIS, votre environnement de test doit avoir SQL Server Express installé. Si vous utilisez Visual Studio 2010 SQL Server Express, il est déjà installé par défaut. Si vous utilisez Visual Studio 2012 ou une version ultérieure, installez SQL Server Express.

Pour installer SQL Server Express, téléchargez-le et installez-le à partir du [Centre de téléchargement : Microsoft SQL Server 2017 Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express). 

Sur la première page du centre d’installation SQL Server, sélectionnez **nouvelle SQL Server installation autonome ou ajout de fonctionnalités à une installation existante** et suivez les instructions qui acceptent les options par défaut. Dans l’Assistant Installation, acceptez les paramètres par défaut. Pour plus d’informations sur les options d’installation, consultez [installer SQL Server à partir de l’Assistant Installation (programme d’installation)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Créer des bases de données SQL Server Express pour l’environnement de test

L’application Contoso University a deux bases de données : 

1. Base de données d’appartenance 
2. Base de données d’application 

Vous pouvez déployer ces bases de données sur deux bases de données distinctes ou sur une base de données unique. Les combiner simplifient les jointures de base de données. 

Si vous effectuez un déploiement sur un fournisseur d’hébergement tiers, votre plan d’hébergement peut également vous fournir une raison de les combiner. Par exemple, le fournisseur peut payer davantage pour plusieurs bases de données ou même autoriser plusieurs bases de données.

Dans ce didacticiel, vous allez déployer sur deux bases de données dans l’environnement de test et sur une base de données dans les environnements intermédiaire et de production.

Dans le menu **affichage** de Visual Studio, sélectionnez **Explorateur de serveurs** (**Explorateur de base de données** dans Visual Web Developer). Cliquez avec le bouton droit sur **connexions de données** , puis sélectionnez **créer une nouvelle SQL Server base de données**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

Dans la boîte **de dialogue créer une nouvelle base de données SQL Server** , entrez « .\SQLEXPRESS » dans la zone **nom du serveur** et « ASPNET-ContosoUniversity » dans la zone **nom de la nouvelle base de données** . Sélectionnez **OK**.

![Créer ASPNET-ContosoUniversity](deploying-to-iis/_static/image9.png)

Suivez la même procédure pour créer une nouvelle base de données SQL Server Express School nommée `ContosoUniversity`.

**Explorateur de serveurs** affiche les deux nouvelles bases de données.

![Nouvelles bases de données dans Explorateur de serveurs](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Créer un script Grant pour les nouvelles bases de données

Lorsque l’application s’exécute dans IIS sur votre ordinateur de développement, l’application utilise les informations d’identification du pool d’applications par défaut pour accéder à la base de données. Toutefois, par défaut, le pool d’applications n’est pas autorisé à ouvrir les bases de données. Cela signifie que vous devez exécuter un script pour accorder cette autorisation. Dans cette section, vous allez créer ce script et l’exécuter ultérieurement pour vous assurer que l’application peut ouvrir les bases de données lorsqu’il s’exécute dans IIS.

Dans un éditeur de texte, copiez les commandes SQL suivantes dans un nouveau fichier et enregistrez-les sous *Grant. SQL*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

Dans Visual Studio, ouvrez la solution Contoso University. Cliquez avec le bouton droit sur la solution (et non sur l’un des projets), puis sélectionnez **Ajouter**. Sélectionnez **élément existant**, accédez à *Grant. SQL*, puis ouvrez-le.

> [!NOTE]
> Ce script est conçu pour fonctionner avec SQL Server Express 2012 ou version ultérieure et avec les paramètres IIS dans Windows 10, Windows 8 ou Windows 7 tels qu’ils sont spécifiés dans ce didacticiel. Si vous utilisez une autre version de SQL Server ou Windows, ou si vous configurez les services IIS sur votre ordinateur différemment, les modifications apportées à ce script peuvent être nécessaires. Pour plus d’informations sur les scripts SQL Server, consultez [documentation en ligne de SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Note de sécurité** Ce script donne `db_owner` autorisations à l’utilisateur qui accède à la base de données au moment de l’exécution, ce que vous aurez dans l’environnement de production. Dans certains scénarios, vous souhaiterez peut-être spécifier un utilisateur qui dispose d’autorisations de mise à jour complète du schéma de base de données uniquement pour le déploiement et spécifier pour l’exécution un autre utilisateur disposant d’autorisations uniquement pour lire et écrire des données. Pour plus d’informations, consultez [examen des modifications automatiques de Web. config pour migrations code First](#reviewingmigrations) plus loin dans ce didacticiel.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Exécuter le script Grant dans la base de données d’application

Vous pouvez configurer le profil de publication pour exécuter le script Grant dans la base de données d’appartenance pendant le déploiement, car ce déploiement de base de données utilise le fournisseur [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) . Vous ne pouvez pas exécuter de scripts pendant le déploiement de Migrations Code First, ce qui correspond à la façon dont vous déployez la base de données d’application. Cela signifie que vous devez exécuter manuellement le script avant le déploiement dans la base de données d’application.

1. Dans Visual Studio, ouvrez le fichier *Grant. SQL* que vous avez créé précédemment.

2. Sélectionnez **Connecter**. 

    ![Bouton Se connecter](deploying-to-iis/_static/image11.png)

3. Dans la boîte de dialogue **se connecter au serveur** , entrez *.\SQLEXPRESS* comme **nom de serveur**. Sélectionnez **Connecter**.

4. Dans la liste déroulante base de données, sélectionnez **ContosoUniversity**. Sélectionnez **Exécuter**. 

   ![](deploying-to-iis/_static/image12.png)

L’identité du pool d’applications par défaut dispose désormais des autorisations suffisantes dans la base de données d’application pour que Migrations Code First crée les tables de base de données lors de l’exécution de l’application.

## <a name="publish-to-iis"></a>Publier sur IIS

Il existe plusieurs façons de déployer sur IIS à l’aide de Visual Studio et Web Deploy :

* Utilisez la publication en un clic de Visual Studio.
* Publiez à partir de la ligne de commande.
* Créez un package de déploiement et installez-le à l’aide du gestionnaire des services Internet. Le package contient un fichier. zip contenant tous les fichiers et les métadonnées nécessaires à l’installation d’un site dans IIS.
* Créez un package de déploiement et installez-le à l’aide de la ligne de commande.

Le processus que vous avez parcouru dans les didacticiels précédents pour configurer Visual Studio afin d’automatiser les tâches de déploiement s’applique à toutes ces méthodes. Dans ces didacticiels, vous allez utiliser les deux premières méthodes. Pour plus d’informations sur l’utilisation des packages de déploiement, consultez [déploiement d’une application Web en créant et en installant un package de déploiement Web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) dans le plan de contenu de déploiement Web pour Visual Studio et ASP.net.

Avant la publication, assurez-vous que vous exécutez Visual Studio en mode administrateur. Si vous ne voyez pas **(administrateur)** dans la barre de titre, fermez Visual Studio. Dans la page de **démarrage** de Windows 8 (ou version ultérieure) ou dans le menu **Démarrer** de Windows 7, cliquez avec le bouton droit sur l’icône Visual Studio et sélectionnez **exécuter en tant qu’administrateur**. Le mode administrateur n’est nécessaire que pour la publication lors de la publication sur IIS sur l’ordinateur local.

### <a name="create-the-publish-profile"></a>Créer le profil de publication

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **ContosoUniversity** (pas sur le projet **ContosoUniversity. dal** ). Sélectionnez **Publier**. La page **publier** s’affiche.

2. Sélectionnez **nouveau profil**. La boîte **de dialogue choisir une cible de publication** s’affiche.

3. Sélectionnez **IIS, FTP, etc**. Sélectionnez **créer un profil**. L’Assistant **publication** s’affiche.

   ![Onglet de profil de l’Assistant publier le site Web](deploying-to-iis/_static/image26.png)

4. Dans le menu déroulant **méthode de publication** , sélectionnez **Web Deploy**.

5. Pour **serveur**, entrez *localhost*.

6. Pour **nom du site**, entrez *Default Web site/ContosoUniversity*.

7. Pour **URL de destination**, entrez *http://localhost/ContosoUniversity* .

   Le paramètre de l' **URL de destination** n’est pas obligatoire. Une fois le déploiement de l’application terminé, Visual Studio ouvre automatiquement votre navigateur par défaut à cette URL. Si vous ne souhaitez pas que le navigateur s’ouvre automatiquement après le déploiement, laissez cette zone vide.

8. Sélectionnez **valider la connexion** pour vérifier que les paramètres sont corrects et que vous pouvez vous connecter à IIS sur l’ordinateur local.

   Une coche verte vérifie que la connexion a réussi.

   ![Onglet de connexion de l’Assistant publier le site Web](deploying-to-iis/_static/image27.png)

9. Sélectionnez **suivant** pour passer à l’onglet **paramètres** .

10. La zone de liste déroulante **configuration** spécifie la configuration de build à déployer. Laissez la valeur par défaut de **Release**. Ce didacticiel ne vous permettra pas de déployer les versions Debug.

11. Développez **options de publication de fichiers**. Sélectionnez **exclure les fichiers du dossier de données de l’application\_** .

    Dans l’environnement de test, l’application accède aux bases de données que vous avez créées dans l’instance de SQL Server Express locale, et non aux fichiers. mdf du dossier de données de l' *application\_* .

12. Laissez les cases à cocher **précompiler pendant la publication** et **Supprimer les fichiers supplémentaires à la destination** désactivées.

    ![Options de publication de fichier dans l’onglet Paramètres](deploying-to-iis/_static/image15a.png)

    La précompilation est une option qui est utile principalement pour les sites de grande taille. Elle peut réduire le temps de démarrage lorsqu’une page est demandée pour la première fois après la publication du site.

    Vous n’avez pas besoin de supprimer des fichiers supplémentaires, car il s’agit de votre premier déploiement et il n’y aura pas encore de fichiers dans le dossier de destination.

    > [!NOTE] 
    > Si vous sélectionnez **Supprimer les fichiers supplémentaires à la destination** pour un déploiement ultérieur sur le même site, veillez à utiliser la fonctionnalité en version préliminaire afin de voir à l’avance quels fichiers seront supprimés avant le déploiement. Le comportement attendu est que Web Deploy supprime les fichiers sur le serveur de destination que vous avez supprimés dans votre projet. Toutefois, l’ensemble de la structure de dossiers sous les dossiers source et de destination est comparée ; et dans certains scénarios, Web Deploy pouvez supprimer les fichiers que vous ne souhaitez pas supprimer.
    > 
    > Par exemple, si vous avez une application Web dans un sous-dossier sur le serveur lorsque vous déployez un projet dans le dossier racine, le sous-dossier sera supprimé. Vous pouvez avoir un projet pour le site principal sur contoso.com et un autre projet pour un blog sur contoso.com/blog. L’application de blog se trouve dans un sous-dossier. Si vous sélectionnez **Supprimer les fichiers supplémentaires à la destination** lorsque vous déployez le site principal, l’application de blog sera supprimée.
    > 
    > Pour un autre exemple, votre application\_dossier de données peut être supprimé de manière inattendue. Certaines bases de données, telles que SQL Server Compact stockent les fichiers de base de données dans le dossier des données d’application\_. Après le déploiement initial, vous ne souhaitez pas continuer à copier les fichiers de base de données dans les déploiements suivants. vous devez donc sélectionner **exclure l’application\_données** dans l’onglet Package/Publication Web. Après cela, si vous avez sélectionné l’option **supprimer des fichiers supplémentaires à la destination** , vos fichiers de base de données et le dossier de données de l’application\_proprement dit seront supprimés lors de la prochaine publication.

### <a name="configure-deployment-for-the-membership-database"></a>Configurer le déploiement pour la base de données d’appartenance

Les étapes suivantes s’appliquent à la base de données **DefaultConnection** dans la section **bases de données** de la boîte de dialogue.

1. Dans la zone **chaîne de connexion à distance** , entrez la chaîne de connexion suivante qui pointe vers la nouvelle base de données d’appartenance SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   Le processus de déploiement place cette chaîne de connexion dans le fichier Web. config déployé, car l’option **utiliser cette chaîne de connexion au moment** de l’exécution est sélectionnée.

    Vous pouvez également récupérer la chaîne de connexion à partir de **Explorateur de serveurs**. Dans **Explorateur de serveurs**, développez **connexions de données** , sélectionnez la base de données **&lt;MachineName&gt;\SQLEXPRESS.AspNet-ContosoUniversity** , puis, dans la fenêtre **Propriétés** , copiez la valeur de la **chaîne de connexion** . Cette chaîne de connexion aura un paramètre supplémentaire que vous pouvez supprimer : `Pooling=False`.

2. Sélectionnez **mettre à jour la base de données**.

   Le schéma de base de données est alors créé dans la base de données de destination pendant le déploiement. Dans les étapes suivantes, vous spécifiez les scripts supplémentaires que vous devez exécuter : un pour accorder l’accès à la base de données au pool d’applications par défaut et l’autre pour déployer les données.

3. Sélectionnez **configurer les mises à jour de la base de données**.
 
4. Dans la boîte de dialogue **configurer les mises à jour de la base de données** , sélectionnez **Ajouter un script SQL**. Accédez au script *Grant. SQL* que vous avez enregistré précédemment dans le dossier de la solution.

5. Répétez le processus pour ajouter le script *ASPNET-Data-dev. SQL* .

   ![Configurer les mises à jour de la base de données d’appartenance](deploying-to-iis/_static/image16.png)

6. Sélectionnez **Fermer**.

### <a name="configure-deployment-for-the-application-database"></a>Configurer le déploiement pour la base de données d’application

Lorsque Visual Studio détecte une classe Entity Framework `DbContext`, il crée une entrée dans la section **bases de données** qui contient une case à cocher **exécuter migrations code First** au lieu d’une **mise à jour de la base de données** . Pour ce didacticiel, vous allez utiliser cette case à cocher pour spécifier Migrations Code First déploiement.

Dans certains scénarios, vous utilisez peut-être une base de données `DbContext`, mais vous souhaitez utiliser le fournisseur dbDacFx au lieu des migrations pour déployer la base de données. Dans ce cas, consultez [Comment faire déployer une base de données Code First sans migrations ?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) dans le Forum aux questions sur le déploiement Web ASP.net sur MSDN.

Les étapes suivantes s’appliquent à la base de données **SchoolContext** dans la section **bases de données** de la boîte de dialogue.

1. Dans la zone **chaîne de connexion à distance** , entrez la chaîne de connexion suivante qui pointe vers la nouvelle base de données d’application SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   Le processus de déploiement place cette chaîne de connexion dans le fichier Web. config déployé, car l’option **utiliser cette chaîne de connexion au moment** de l’exécution est sélectionnée.

   Vous pouvez également récupérer la chaîne de connexion à la base de données d’application à partir de **Explorateur de serveurs** de la même façon que la chaîne de connexion à la base de données d’appartenance.

2. Sélectionnez **exécuter migrations code First (s’exécute au démarrage de l’application)** .

   Avec cette option, le processus de déploiement configure le fichier Web. config déployé pour spécifier l’initialiseur de `MigrateDatabaseToLatestVersion`. Cet initialiseur met automatiquement à jour la base de données avec la version la plus récente lorsque l’application accède pour la première fois à la base de données après le déploiement.

### <a name="configure-publish-profile-transforms"></a>Configurer les transformations de profil de publication

1. Sélectionnez **Fermer**. Sélectionnez **Oui** lorsque vous êtes invité à enregistrer les modifications.

2. Dans **Explorateur de solutions**, développez **Propriétés**, puis **PublishProfiles**.

3. Cliquez avec le bouton droit sur *CustomProfile. pubxml* et renommez-le *test. pubxml*.

4. Cliquez avec le bouton droit sur *test. pubxml*. Sélectionnez **Ajouter une transformation de configuration**.

   ![Menu Ajouter une transformation de configuration](deploying-to-iis/_static/image17.png)

   Visual Studio crée le fichier de transformation *Web. test. config* et l’ouvre.

5. Dans le fichier de transformation *Web. test. config* , insérez le code suivant immédiatement après la balise de configuration d’ouverture.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Lorsque vous utilisez le profil de publication de test, cette transformation définit l’indicateur d’environnement sur « test ». Dans le site déployé, vous verrez « (test) » après l’en-tête « Contoso University » H1.

6. Enregistrez et fermez le fichier.

7. Cliquez avec le bouton droit sur le fichier *Web. test. config* et sélectionnez Aperçu de la **transformation** pour vous assurer que la transformation que vous avez codée produit les modifications attendues.

    La fenêtre d' **aperçu de Web. config** affiche le résultat de l’application des transformations *Web. Release. config* et des transformations *Web. test. config* .

### <a name="preview-the-deployment-updates"></a>Prévisualiser les mises à jour du déploiement

1. Rouvrez l’Assistant **publier le site Web** (cliquez avec le bouton droit sur le projet ContosoUniversity, sélectionnez **publier**, puis **Aperçu**).

2. Dans la boîte de dialogue **Aperçu** , sélectionnez **Démarrer l’aperçu** pour afficher la liste des fichiers qui seront copiés. 

   ![Publier l’aperçu](deploying-to-iis/_static/image29.png)

   Vous pouvez également sélectionner le lien **aperçu de la base de données** pour afficher les scripts qui s’exécuteront dans la base de données d’appartenance. (Aucun script n’est exécuté pour le déploiement Migrations Code First, donc il n’y a rien à prévisualiser pour la base de données d’application.)

3. Sélectionnez **Publier**.

   Si Visual Studio n’est pas en mode administrateur, vous risquez d’obtenir un message d’erreur d’autorisation. Dans ce cas, fermez Visual Studio, ouvrez-le en mode administrateur, puis réessayez de publier.

   Si Visual Studio est en mode administrateur, la fenêtre **sortie** signale la réussite de la génération et de la publication.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   Si vous avez entré l’URL dans la zone **URL de destination** sous l’onglet **connexion** au profil de publication, le navigateur s’ouvre automatiquement sur la page d’hébergement de Contoso University qui s’exécute dans IIS sur votre ordinateur.

## <a name="test-in-the-test-environment"></a>Test dans l’environnement de test

Notez que l’indicateur d’environnement affiche « (test) » au lieu de « (dev) », qui indique que la transformation *Web. config* de l’indicateur d’environnement a réussi.

Exécutez la page des **enseignants** pour vérifier que code First amorcé la base de données avec les données de l’instructeur. Lorsque vous sélectionnez cette page, le chargement peut prendre quelques minutes, car Code First crée la base de données, puis exécute la méthode `Seed`. (Ce n’est pas le cas lorsque vous étiez sur la page d’hébergement parce que l’application n’a pas encore essayé d’accéder à la base de données.)

Sélectionnez l’onglet **students** pour vérifier que la base de données déployée ne dispose d’aucun étudiant.

Sélectionnez **Ajouter des élèves** dans le menu **élèves** . Ajoutez un étudiant, puis affichez le nouvel étudiant dans la page des **étudiants** . Cela permet de vérifier que vous pouvez écrire correctement dans la base de données.

Dans le menu **cours** , sélectionnez **mettre à jour les crédits**. La page **mettre à jour les crédits** requiert des autorisations d’administrateur, donc la page **de connexion** s’affiche. Entrez les informations d’identification du compte d’administrateur que vous avez créées précédemment (« admin » et « devpwd »). La page **mettre à jour les crédits** s’affiche. Cela permet de vérifier que le compte d’administrateur que vous avez créé dans le didacticiel précédent a été correctement déployé dans l’environnement de test.

Vérifiez qu’un dossier *ELMAH* existe dans le dossier *c:\inetpub\wwwroot\ContosoUniversity* avec uniquement le fichier d’espace réservé.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Passez en revue les modifications automatiques de Web. config pour Migrations Code First

Ouvrez le fichier *Web. config* dans l’application déployée sur *C:\inetpub\wwwroot\ContosoUniversity* . vous pouvez voir où le processus de déploiement a configuré migrations code First pour mettre à jour automatiquement la base de données vers la dernière version.

![](deploying-to-iis/_static/image21.png)

Le processus de déploiement a également créé une nouvelle chaîne de connexion pour Migrations Code First à utiliser exclusivement pour la mise à jour du schéma de base de données :

![Chaîne de connexion Database_Publish](deploying-to-iis/_static/image22.png)

Cette chaîne de connexion supplémentaire vous permet de spécifier un compte d’utilisateur pour les mises à jour de schéma de base de données et un autre compte d’utilisateur pour l’accès aux données d’application. Par exemple, vous pouvez attribuer le rôle de propriétaire de la **base de\_DB** à migrations code First et **DB\_DataReader** avec des rôles de **\_base** de de base de à l’application. Il s’agit d’un modèle de défense en profondeur courant qui empêche le code potentiellement malveillant dans l’application de modifier le schéma de base de données. (Par exemple, cela peut se produire dans le cas d’une attaque par injection SQL réussie.) Ces didacticiels n’utilisent pas ce modèle. Pour implémenter ce modèle dans votre scénario, procédez comme suit :

1. Dans l’Assistant **publier le site Web** sous l’onglet **paramètres** , entrez la chaîne de connexion qui spécifie un utilisateur disposant d’autorisations de mise à jour complète du schéma de base de données. Désactivez la case à cocher **utiliser cette chaîne de connexion au moment de l’exécution** . Dans le fichier Web. config déployé, il s’agit de la chaîne de connexion `DatabasePublish`.

2. Créez une transformation de fichier Web. config pour la chaîne de connexion que vous souhaitez que l’application utilise au moment de l’exécution.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant déployé votre application sur IIS sur votre ordinateur de développement et vous l’avez testée ici.

![Page d’espace de test](deploying-to-iis/_static/image23.png)

Cela permet de vérifier que le processus de déploiement a copié le contenu de l’application à l’emplacement approprié (à l’exclusion des fichiers que vous ne souhaitez pas déployer) et que Web Deploy configuré IIS correctement au cours du déploiement. Dans le didacticiel suivant, vous allez exécuter un test supplémentaire qui recherche une tâche de déploiement qui n’a pas encore été effectuée : définition des autorisations de dossier sur le dossier de l' *Elm* .

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations sur l’exécution d’IIS ou de IIS Express dans Visual Studio, consultez les ressources suivantes :

- [IIS Express vue d’ensemble](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) du site IIS.net.
- [Présentation de IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) sur le blog de Scott Guthrie.
- [Serveurs Web dans Visual Studio pour les projets web ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Principales différences entre IIS et le serveur de développement ASP.net](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) sur le site ASP.net.

Pour plus d’informations sur les problèmes susceptibles de survenir lorsque votre application s’exécute en moyenne confiance, consultez [hébergement d’Applications ASP.net en confiance moyenne](http://www.4guysfromrolla.com/articles/100307-1.aspx) sur les quatre spécialistes à partir du site Rolla.

> [!div class="step-by-step"]
> [Précédent](project-properties.md)
> [Suivant](setting-folder-permissions.md)
