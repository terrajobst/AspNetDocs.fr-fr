---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : migration vers SQL Server-10 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: c5281a42596d95e725b32e652c75785abe0fd64e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640573"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : migration vers SQL Server-10 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Vue d'ensemble de

Ce didacticiel vous montre comment effectuer une migration de SQL Server Compact vers SQL Server. Cela peut être utile pour tirer parti des fonctionnalités de SQL Server que SQL Server Compact ne prend pas en charge, telles que les procédures stockées, les déclencheurs, les vues ou la réplication. Pour plus d’informations sur les différences entre les SQL Server Compact et les SQL Server, consultez le didacticiel sur le [déploiement de SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express et SQL Server complet pour le développement

Une fois que vous avez décidé de mettre à niveau vers SQL Server, vous souhaiterez peut-être utiliser SQL Server ou SQL Server Express dans vos environnements de développement et de test. Outre les différences de prise en charge des outils et dans les fonctionnalités du moteur de base de données, il existe des différences entre les implémentations de fournisseur entre SQL Server Compact et d’autres versions de SQL Server. Ces différences peuvent entraîner un même code pour générer des résultats différents. Par conséquent, si vous décidez de conserver SQL Server Compact comme base de données de développement, vous devez tester minutieusement votre site dans SQL Server ou SQL Server Express dans un environnement de test avant chaque déploiement en production.

Contrairement à SQL Server Compact, SQL Server Express est essentiellement le même moteur de base de données et utilise le même fournisseur .NET que le SQL Server complet. Lorsque vous testez avec SQL Server Express, vous pouvez être certain d’obtenir les mêmes résultats que dans le cadre d’SQL Server. Vous pouvez utiliser la plupart des mêmes outils de base de données avec des SQL Server Express que vous pouvez utiliser avec SQL Server (une exception notable [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)) et il prend en charge d’autres fonctionnalités de SQL Server comme les procédures stockées, les vues, les déclencheurs et la réplication. (En général, vous devez utiliser la SQL Server complète sur un site Web de production. SQL Server Express peut s’exécuter dans un environnement d’hébergement partagé, mais il n’a pas été conçu pour cela, et de nombreux fournisseurs d’hébergement ne le prennent pas en charge.)

Si vous utilisez Visual Studio 2012, vous choisissez généralement SQL Server Express base de données locale pour votre environnement de développement, car c’est ce qui est installé par défaut dans Visual Studio. Toutefois, la base de données locale ne fonctionne pas dans IIS. par conséquent, pour votre environnement de test, vous devez utiliser SQL Server ou SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Combiner les bases de données et les conserver séparées

L’application Contoso University dispose de deux bases de données SQL Server Compact : la base de données d’appartenance (*Aspnet. sdf*) et la base de données d’application (*School. sdf*). Lorsque vous migrez, vous pouvez migrer ces bases de données vers deux bases de données distinctes ou vers une base de données unique. Vous pouvez les combiner pour faciliter les jointures de base de données entre votre base de données d’application et votre base de données d’appartenance. Votre plan d’hébergement peut également fournir une raison pour les combiner. Par exemple, le fournisseur d’hébergement peut être facturé pour plusieurs bases de données ou même autoriser plusieurs bases de données. C’est le cas avec le compte d’hébergement Cytanium Lite utilisé pour ce didacticiel, qui n’autorise qu’une seule base de données SQL Server.

Dans ce didacticiel, vous allez migrer vos deux bases de données de cette façon :

- Migrez vers deux bases de données de base de données locale dans l’environnement de développement.
- Migrer vers deux bases de données de SQL Server Express dans l’environnement de test.
- Migrer vers une base de données SQL Server complète combinée dans l’environnement de production.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Installation de SQL Server Express

SQL Server Express est automatiquement installé par défaut avec Visual Studio 2010, mais par défaut, il n’est pas installé avec Visual Studio 2012. Pour installer SQL Server 2012 Express, cliquez sur le lien suivant.

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Choisissez *fra/x64/SQLEXPR\_x64\_fra. exe* ou *fra/x86/SQLEXPR\_x86\_fra. exe*, et dans l’Assistant Installation, acceptez les paramètres par défaut. Pour plus d’informations sur les options d’installation, consultez [installer SQL Server 2012 à partir de l’Assistant Installation (programme d’installation)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Création de bases de données SQL Server Express pour l’environnement de test

L’étape suivante consiste à créer les bases de données d’appartenance ASP.NET et School.

Dans le **menu Affichage** , sélectionnez **Explorateur de serveurs** (**Explorateur de base de données** dans Visual Web Developer), puis cliquez avec le bouton droit sur **connexions de données** et sélectionnez créer une **nouvelle base de données SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

Dans la boîte **de dialogue créer une nouvelle base de données SQL Server** , entrez « .\SQLEXPRESS » dans la zone **nom du serveur** et « ASPNET-test » dans la zone **nom de la nouvelle base de données** , puis cliquez sur **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Suivez la même procédure pour créer une nouvelle base de données SQL Server Express School nommée « School-test ».

(Vous ajoutez « test » à ces noms de base de données, car ultérieurement, vous créerez une instance supplémentaire de chaque base de données pour l’environnement de développement, et vous devez être en mesure de différencier les deux ensembles de bases de données.)

**Explorateur de serveurs** affiche à présent les deux nouvelles bases de données.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Création d’un script Grant pour les nouvelles bases de données

Lorsque l’application s’exécute dans IIS sur votre ordinateur de développement, l’application accède à la base de données à l’aide des informations d’identification du pool d’applications par défaut. Toutefois, par défaut, l’identité du pool d’applications n’a pas l’autorisation d’ouvrir les bases de données. Vous devez donc exécuter un script pour accorder cette autorisation. Dans cette section, vous allez créer le script que vous exécuterez ultérieurement pour vous assurer que l’application peut ouvrir les bases de données lorsqu’elle s’exécute dans IIS.

Dans le dossier *SolutionFiles* de la solution que vous avez créé dans le didacticiel [déploiement dans l’environnement de production](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) , créez un nouveau fichier SQL nommé *Grant. SQL*. Copiez les commandes SQL suivantes dans le fichier, puis enregistrez et fermez le fichier :

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Ce script est conçu pour fonctionner avec SQL Server 2008 et avec les paramètres IIS dans Windows 7 tels qu’ils sont spécifiés dans ce didacticiel. Si vous utilisez une autre version de SQL Server ou de Windows, ou si vous configurez les services IIS sur votre ordinateur différemment, les modifications apportées à ce script peuvent être nécessaires. Pour plus d’informations sur les scripts SQL Server, consultez [documentation en ligne de SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> 
> **Note de sécurité** Ce script donne aux utilisateurs de base de données\_autorisations de propriétaire l’accès à la base de données au moment de l’exécution, ce que vous aurez dans l’environnement de production. Dans certains scénarios, vous souhaiterez peut-être spécifier un utilisateur qui dispose d’autorisations de mise à jour complète du schéma de base de données uniquement pour le déploiement, et spécifier pour l’exécution un autre utilisateur disposant d’autorisations uniquement pour lire et écrire des données. Pour plus d’informations, consultez **examen des modifications automatiques de Web. config pour migrations code First** dans [déploiement sur IIS en tant qu’environnement de test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).

## <a name="configuring-database-deployment-for-the-test-environment"></a>Configuration du déploiement de base de données pour l’environnement de test

Ensuite, vous allez configurer Visual Studio pour qu’il effectue les tâches suivantes pour chaque base de données :

- Générez un script SQL qui crée la structure de la base de données source (tables, colonnes, contraintes, etc.) dans la base de données de destination.
- Générez un script SQL qui insère les données de la base de données source dans les tables de la base de données de destination.
- Exécutez les scripts générés et le script Grant que vous avez créé dans la base de données de destination.

Ouvrez la fenêtre **Propriétés du projet** et sélectionnez l’onglet **Package/Publication SQL** .

Assurez-vous que l’option **active (Release)** ou **Release** est sélectionnée dans la liste déroulante **configuration** .

Cliquez sur **activer cette page**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

L’onglet **Package/Publication SQL** est normalement désactivé, car il spécifie une méthode de déploiement héritée. Pour la plupart des scénarios, vous devez configurer le déploiement de la base de données dans l’Assistant **publier le site Web** . La migration à partir de SQL Server Compact vers SQL Server ou SQL Server Express est un cas particulier pour lequel cette méthode est un bon choix.

Cliquez sur **Importer à partir de Web. config**.

![Selecting_Import_from_Web. config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio recherche des chaînes de connexion dans le fichier *Web. config* , en trouve un pour la base de données d’appartenance et une pour la base de données School, et ajoute une ligne correspondant à chaque chaîne de connexion dans le tableau **entrées de la base de données** . Les chaînes de connexion qu’il trouve correspondent aux bases de données SQL Server Compact existantes, et l’étape suivante consiste à configurer le mode et l’emplacement de déploiement de ces bases de données.

Vous entrez les paramètres de déploiement de base de données dans la section **Détails de l’entrée de base** de données sous le tableau **entrées de base de** données. Les paramètres affichés dans la section **Détails de l’entrée de base de données** se rapportent à la ligne de la table **entrées de la base de** données qui est sélectionnée, comme indiqué dans l’illustration suivante.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configuration des paramètres de déploiement pour la base de données d’appartenance

Sélectionnez la ligne **DefaultConnection-Deployment** dans le tableau **entrées de la base de données** afin de configurer les paramètres qui s’appliquent à la base de données d’appartenance.

Dans **chaîne de connexion de la base de données de destination**, entrez une chaîne de connexion qui pointe vers la nouvelle base de données d’appartenance SQL Server Express. Vous pouvez récupérer la chaîne de connexion dont vous avez besoin à partir de **Explorateur de serveurs**. Dans **Explorateur de serveurs**, développez **connexions de données** , sélectionnez la base de données **aspnetTest** , puis dans la fenêtre **Propriétés** , copiez la valeur de la **chaîne de connexion** .

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

La même chaîne de connexion est reproduite ici :

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Copiez et collez cette chaîne de connexion dans la chaîne de connexion de la **base de données de destination** dans l’onglet **Package/Publication SQL** .

Assurez-vous que **les données d’extraction et/ou le schéma d’une base de données existante** sont sélectionnés. C’est ainsi que les scripts SQL sont automatiquement générés et exécutés dans la base de données de destination.

La **chaîne de connexion pour la valeur de la base de données source** est extraite du fichier *Web. config* et pointe vers la base de données de développement SQL Server Compact. Il s’agit de la base de données source qui sera utilisée pour générer les scripts qui s’exécuteront ultérieurement dans la base de données de destination. Étant donné que vous souhaitez déployer la version de production de la base de données, remplacez « aspnet-Dev. sdf » par « aspnet-Prod. sdf ».

Modifiez les **options de script de base de données** du **schéma uniquement** en **schéma et données**, car vous souhaitez copier vos données (comptes d’utilisateur et rôles), ainsi que la structure de la base de données.

Pour configurer le déploiement afin d’exécuter les scripts d’octroi que vous avez créés précédemment, vous devez les ajouter à la section **scripts de base de données** . Cliquez sur **Ajouter un script**, puis dans la boîte de dialogue Ajouter des **scripts SQL** , accédez au dossier dans lequel vous avez stocké le script Grant (il s’agit du dossier qui contient votre fichier solution). Sélectionnez le fichier nommé *Grant. SQL*, puis cliquez sur **ouvrir**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Les paramètres de la ligne **DefaultConnection-Deployment** des **entrées de base de données** ressemblent maintenant à l’illustration suivante :

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configuration des paramètres de déploiement pour la base de données School

Ensuite, sélectionnez la ligne **SchoolContext-Deployment** dans le tableau **entrées de la base de données** afin de configurer les paramètres de déploiement de la base de données School.

Vous pouvez utiliser la même méthode que celle utilisée précédemment pour obtenir la chaîne de connexion pour la nouvelle base de données de SQL Server Express. Copiez cette chaîne de connexion dans la chaîne de connexion de la **base de données de destination** sous l’onglet **Package/Publication SQL** .

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Assurez-vous que **les données d’extraction et/ou le schéma d’une base de données existante** sont sélectionnés.

La **chaîne de connexion pour la valeur de la base de données source** est extraite du fichier *Web. config* et pointe vers la base de données de développement SQL Server Compact. Remplacez « School-Dev. sdf » par « School-Prod. sdf » pour déployer la version de production de la base de données. (Vous n’avez jamais créé de fichier School-Prod. sdf dans le dossier de données de l’application\_. vous allez donc copier ce fichier de l’environnement de test vers le dossier de données de l’application\_dans le dossier de projet ContosoUniversity, plus tard.)

Modifiez les **options de script de base de données** en **schéma et données**.

Vous souhaitez également exécuter le script pour accorder l’autorisation de lecture et d’écriture pour cette base de données à l’identité du pool d’applications. par conséquent, ajoutez le fichier de script *Grant. SQL* comme vous l’avez fait pour la base de données d’appartenance.

Lorsque vous avez terminé, les paramètres de la ligne **SchoolContext-Deployment** des **entrées de base de données** ressemblent à l’illustration suivante :

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Enregistrez les modifications apportées à l’onglet **Package/Publication SQL** .

Copiez le fichier *School-prod. sdf* à partir du dossier *c:\inetpub\wwwroot\ContosoUniversity\App\_Data* dans le dossier *application\_Data* du projet ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Spécification du mode traité pour le script Grant

Le processus de déploiement génère des scripts qui déploient le schéma et les données de la base de données. Par défaut, ces scripts s’exécutent dans une transaction. Toutefois, les scripts personnalisés (comme les scripts d’octroi) par défaut ne s’exécutent pas dans une transaction. Si le processus de déploiement combine les modes de transaction, vous pouvez obtenir une erreur de délai d’attente lorsque les scripts s’exécutent pendant le déploiement. Dans cette section, vous allez modifier le fichier projet afin de configurer les scripts personnalisés à exécuter dans une transaction.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **ContosoUniversity** et sélectionnez **décharger le projet**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Cliquez ensuite avec le bouton droit sur le projet et sélectionnez **modifier ContosoUniversity. csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

L’éditeur Visual Studio affiche le contenu XML du fichier projet. Notez qu’il existe plusieurs éléments `PropertyGroup`. (Dans l’image, le contenu des éléments de `PropertyGroup` a été omis.)

![Fenêtre de l’éditeur du fichier projet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Le premier, qui n’a pas d’attribut `Condition`, est destiné aux paramètres qui s’appliquent indépendamment de la configuration de Build. Un élément `PropertyGroup` s’applique uniquement à la configuration de build debug (Notez l’attribut `Condition`), l’un s’applique uniquement à la configuration de build Release, et l’autre à la configuration de build de test. Dans l’élément `PropertyGroup` de la configuration de build Release, vous verrez un élément `PublishDatabaseSettings` qui contient les paramètres que vous avez entrés dans l’onglet **Package/Publication SQL** . Il existe un élément `Object` qui correspond à chacun des scripts Grant que vous avez spécifiés (Notez les deux instances de « Grant. Sql »). Par défaut, l’attribut `Transacted` de l’élément `Source` de chaque script Grant est `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Remplacez la valeur de l’attribut `Transacted` de l’élément `Source` par `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Enregistrez et fermez le fichier projet, puis cliquez avec le bouton droit sur le projet dans **Explorateur de solutions** et sélectionnez **recharger le projet**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Configuration des transformations Web. config pour les chaînes de connexion

Les chaînes de connexion pour les nouvelles bases de données SQL Express que vous avez entrées dans l’onglet **Package/Publication SQL** sont utilisées par les Web Deploy uniquement pour la mise à jour de la base de données de destination pendant le déploiement. Vous devez toujours configurer des transformations *Web. config* afin que les chaînes de connexion dans le fichier *Web. config* déployé pointent vers les nouvelles bases de données de SQL Server Express. (Lorsque vous utilisez l’onglet **Package/Publication SQL** , vous ne pouvez pas configurer les chaînes de connexion dans le profil de publication.)

Ouvrez le *fichier Web. test. config* et remplacez l’élément `connectionStrings` par l’élément `connectionStrings` dans l’exemple suivant. (Assurez-vous de copier uniquement l’élément connectionStrings, et non le code environnant qui est indiqué ici pour fournir le contexte.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Ce code entraîne le remplacement des attributs `connectionString` et `providerName` de chaque élément `add` dans le fichier *Web. config* déployé. Ces chaînes de connexion ne sont pas identiques à celles que vous avez entrées dans l’onglet **Package/Publication SQL** . Le paramètre « MultipleActiveResultSets = true » a été ajouté, car il est requis pour le Entity Framework et le Fournisseurs universels.

## <a name="installing-sql-server-compact"></a>Installation de SQL Server Compact

Le package NuGet SqlServerCompact fournit les assemblys du moteur de base de données SQL Server Compact pour l’application Contoso University. Mais maintenant, il ne s’agit pas de l’application, mais Web Deploy qui doit être en mesure de lire les bases de données SQL Server Compact, afin de créer des scripts à exécuter dans les bases de données SQL Server. Pour permettre à Web Deploy de lire SQL Server Compact bases de données, installez SQL Server Compact sur l’ordinateur de développement à l’aide du lien suivant : [Microsoft SQL Server Compact 4,0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Déploiement dans l’environnement de test

Pour publier dans l’environnement de test, vous devez créer un profil de publication configuré pour utiliser l’onglet **Package/Publication SQL** pour la publication de base de données à la place des paramètres de base de données de profil de publication.

Tout d’abord, supprimez le profil de test existant.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet ContosoUniversity, puis cliquez sur **publier**.

Sélectionnez l’onglet **Profil** .

Cliquez sur **gérer les profils**.

Sélectionnez **test**, cliquez sur **supprimer**, puis sur **Fermer**.

Fermez l’Assistant **publier le site Web** pour enregistrer cette modification.

Ensuite, créez un nouveau profil de test et utilisez-le pour publier le projet.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet ContosoUniversity, puis cliquez sur **publier**.

Sélectionnez l’onglet **Profil** .

Sélectionnez **&lt;nouveau...&gt;** dans la liste déroulante, puis entrez « test » comme nom de profil.

Dans la zone **URL du service** , entrez *localhost*.

Dans la zone **site/application** , entrez *Default Web site/ContosoUniversity*.

Dans la zone **URL de destination** , entrez `http://localhost/ContosoUniversity/`.

Cliquez sur **Next**.

L’onglet **paramètres** vous avertit que l’onglet **Package/Publication SQL** a été configuré et vous donne la possibilité de les remplacer en cliquant sur Activer les nouvelles améliorations de la publication de base de données. Pour ce déploiement, vous ne souhaitez pas remplacer les paramètres de l’onglet **Package/Publication SQL** , il vous suffit de cliquer sur **suivant**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Un message dans l’onglet **Aperçu** indique qu' **aucune base de données n’est sélectionnée pour la publication**, mais cela signifie uniquement que la publication de base de données n’est pas configurée dans le profil de publication.

Cliquez sur **Publier**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio déploie l’application et ouvre le navigateur sur la page d’hébergement du site dans l’environnement de test. Exécutez la page des enseignants pour voir qu’elle affiche les mêmes données que celles que vous avez vues précédemment. Exécutez la page **Ajouter des élèves** , ajoutez un nouvel étudiant, puis affichez le nouvel étudiant dans la page des **étudiants** . Cela permet de vérifier que vous pouvez mettre à jour la base de données. Sélectionnez la page **mettre à jour les crédits** (vous devez vous connecter) pour vérifier que la base de données d’appartenance a été déployée et que vous y avez accès.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Création d’une base de données SQL Server pour l’environnement de production

Maintenant que vous avez déployé dans l’environnement de test, vous êtes prêt à configurer le déploiement en production. Vous commencez comme vous l’avez fait pour l’environnement de test en créant une base de données sur laquelle effectuer le déploiement. Comme vous l’avez vu dans la vue d’ensemble, le plan d’hébergement Cytanium Lite n’autorise qu’une seule base de données SQL Server, de sorte que vous ne configurez qu’une seule base de données, et non pas deux. Toutes les tables et données des bases de données d’appartenance et scolaires SQL Server Compact sont déployées dans une base de données SQL Server en production.

Accédez au panneau de configuration Cytanium à [http://panel.cytanium.com](http://panel.cytanium.com). Maintenez la souris sur **bases de données** , puis cliquez sur **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

Dans la page **SQL Server 2008** , cliquez sur **créer une base de données**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nommez la base de données « School », puis cliquez sur **Enregistrer**. (La page ajoute automatiquement le préfixe « contoso », donc le nom effectif sera « contosouSchool ».)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Sur la même page, cliquez sur **créer un utilisateur**. Sur les serveurs de Cytanium, au lieu d’utiliser la sécurité intégrée de Windows et de permettre à l’identité du pool d’applications d’ouvrir votre base de données, vous allez créer un utilisateur habilité à ouvrir votre base de données. Vous allez ajouter les informations d’identification de l’utilisateur aux chaînes de connexion qui se trouvent dans le fichier *Web. config* de production. Au cours de cette étape, vous allez créer ces informations d’identification.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Renseignez les champs requis dans la page Propriétés de l' **utilisateur SQL** :

- Entrez « ContosoUniversityUser » comme nom.
- Entrez un mot de passe
- Sélectionnez **contosouSchool** comme base de données par défaut.
- Activez la case à cocher **contosouSchool** .

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Configuration du déploiement de base de données pour l’environnement de production

Vous êtes maintenant prêt à configurer les paramètres de déploiement de base de données dans l’onglet **Package/Publication SQL** , comme vous l’avez fait précédemment pour l’environnement de test.

Ouvrez la fenêtre **Propriétés du projet** , sélectionnez l’onglet **Package/Publication SQL** , et assurez-vous que l’option **active (version finale)** ou **mise en sortie** est sélectionnée dans la liste déroulante **configuration** .

Quand vous configurez les paramètres de déploiement pour chaque base de données, la principale différence entre ce que vous faites pour les environnements de production et de test réside dans la façon dont vous configurez les chaînes de connexion. Pour l’environnement de test, vous avez entré des chaînes de connexion de base de données de destination différentes, mais pour l’environnement de production, la chaîne de connexion de destination sera la même pour les deux bases de données. Cela est dû au fait que vous déployez les deux bases de données dans une base de données en production.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configuration des paramètres de déploiement pour la base de données d’appartenance

Pour configurer les paramètres qui s’appliquent à la base de données d’appartenance, sélectionnez la ligne **DefaultConnection-Deployment** dans le tableau **entrées de la base de données** .

Dans **chaîne de connexion de la base de données de destination**, entrez une chaîne de connexion qui pointe vers la nouvelle base de données de production SQL Server que vous venez de créer. Vous pouvez récupérer la chaîne de connexion à partir de votre e-mail de bienvenue. La partie correspondante de l’e-mail contient l’exemple de chaîne de connexion suivant :

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Une fois que vous avez remplacé les trois variables, la chaîne de connexion dont vous avez besoin ressemble à l’exemple suivant :

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Copiez et collez cette chaîne de connexion dans la chaîne de connexion de la **base de données de destination** dans l’onglet **Package/Publication SQL** .

Assurez-vous que les **données extraites et/ou le schéma d’une base de données existante** sont toujours sélectionnés, et que les **options de script de base de données** sont toujours **schéma et données**.

Dans la zone **scripts de base de données** , désactivez la case à cocher en regard du script Grant. Sql.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configuration des paramètres de déploiement pour la base de données School

Ensuite, sélectionnez la ligne **SchoolContext-Deployment** dans le tableau **entrées de la base de données** afin de configurer les paramètres de la base de données School.

Copiez la même chaîne de connexion dans la **chaîne de connexion de la base de données de destination** que vous avez copiée dans ce champ pour la base de données d’appartenance.

Assurez-vous que les **données extraites et/ou le schéma d’une base de données existante** sont toujours sélectionnés, et que les **options de script de base de données** sont toujours **schéma et données**.

Dans la zone **scripts de base de données** , désactivez la case à cocher en regard du script Grant. Sql.

Enregistrez les modifications apportées à l’onglet **Package/Publication SQL** .

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Configuration des transformations Web. config pour les chaînes de connexion aux bases de données de production

Ensuite, vous allez configurer les transformations *Web. config* afin que les chaînes de connexion dans le fichier *Web. config* déployés pointent vers la nouvelle base de données de production. La chaîne de connexion que vous avez entrée sous l’onglet **Package/Publication SQL** pour Web Deploy à utiliser est identique à celle que l’application doit utiliser, à l’exception de l’ajout de l’option `MultipleResultSets`.

Ouvrez *Web. production. config* et remplacez l’élément `connectionStrings` par un élément `connectionStrings` ressemblant à l’exemple suivant. (Copiez uniquement l’élément `connectionStrings`, pas les balises environnantes fournies pour afficher le contexte.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Vous pouvez parfois voir des conseils qui vous indiquent de toujours chiffrer les chaînes de connexion dans le fichier *Web. config* . Cela peut être approprié si vous déployez sur des serveurs sur le réseau de votre propre entreprise. Toutefois, lorsque vous déployez dans un environnement d’hébergement partagé, vous faites confiance aux pratiques de sécurité du fournisseur d’hébergement et il n’est pas nécessaire ou pratique de chiffrer les chaînes de connexion.

## <a name="deploying-to-the-production-environment"></a>Déploiement dans l’environnement de production

Vous êtes maintenant prêt à effectuer le déploiement en production. Web Deploy lit les bases de données SQL Server Compact dans le dossier des *données de l’application\_* de votre projet et recrée toutes leurs tables et données dans la base de données de SQL Server de production. Pour publier à l’aide des paramètres de l’onglet **Package/Publication Web** , vous devez créer un nouveau profil de publication pour la production.

Tout d’abord, supprimez le profil de production existant comme vous l’avez fait précédemment.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet ContosoUniversity, puis cliquez sur **publier**.

Sélectionnez l’onglet **Profil** .

Cliquez sur **gérer les profils**.

Sélectionnez **production**, cliquez sur **supprimer**, puis sur **Fermer**.

Fermez l’Assistant **publier le site Web** pour enregistrer cette modification.

Ensuite, créez un nouveau profil de production et utilisez-le pour publier le projet.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet ContosoUniversity, puis cliquez sur **publier**.

Sélectionnez l’onglet **Profil** .

Cliquez sur **Importer**, puis sélectionnez le fichier. publishsettings que vous avez téléchargé précédemment.

Sous l’onglet **connexion** , remplacez l' **URL de destination** par l’URL temporaire correcte, qui, dans cet exemple, est http://contosouniversity.com.vserver01.cytanium.com.

Renommez le profil en production. (Sélectionnez l’onglet **Profil** , puis cliquez sur **gérer les profils** pour effectuer cette opération).

Fermez l’Assistant **publier le site Web** pour enregistrer vos modifications.

Dans une application réelle dans laquelle la base de données a été mise à jour en production, vous devez effectuer deux étapes supplémentaires maintenant avant de publier :

1. Téléchargez l' *application\_offline. htm*, comme indiqué dans le didacticiel [sur le déploiement dans l’environnement de production](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) .
2. Utilisez la **fonctionnalité gestionnaire de fichiers** du panneau de configuration Cytanium pour copier les fichiers *ASPNET-prod. sdf* et *School-prod. sdf* à partir du site de production vers le dossier de données de l' *application\_* du projet ContosoUniversity. Cela permet de s’assurer que les données que vous déployez dans la nouvelle base de données SQL Server incluent les dernières mises à jour apportées par votre site Web de production.

Dans la barre d’outils **publication Web en un clic** , assurez-vous que le profil de **production** est sélectionné, puis cliquez sur **publier**.

Si vous avez téléchargé l' <em>application\_offline. htm</em> avant la publication, vous devez utiliser l’utilitaire <strong>Gestionnaire de fichiers</strong> dans le panneau de configuration Cytanium pour supprimer l' <em>application\_hors connexion.</em> htm avant de tester. Vous pouvez également supprimer les fichiers <em>. sdf</em> du dossier de données de l' <em>application\_</em> .

Vous pouvez maintenant ouvrir un navigateur et accéder à l’URL de votre site public pour tester l’application de la même façon que vous l’avez fait après le déploiement dans l’environnement de test.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Passage à SQL Server Express base de données locale en développement

Comme expliqué dans la vue d’ensemble, il est généralement préférable d’utiliser le moteur de base de données en développement que vous utilisez dans les tests et la production. (N’oubliez pas que l’avantage d’utiliser SQL Server Express dans le développement est que la base de données fonctionnera de la même façon dans vos environnements de développement, de test et de production.) Dans cette section, vous allez configurer le projet ContosoUniversity pour utiliser SQL Server Express base de données locale lorsque vous exécutez l’application à partir de Visual Studio.

La façon la plus simple d’effectuer cette migration consiste à laisser Code First et le système d’appartenance créera les deux bases de données de développement pour vous. L’utilisation de cette méthode pour migrer requiert trois étapes :

1. Modifiez les chaînes de connexion pour spécifier de nouvelles bases de données SQL Express de base de données SQL.
2. Exécutez l’outil Administration de site Web pour créer un utilisateur administrateur. La base de données d’appartenance est alors créée.
3. Utilisez la commande Migrations Code First Update-Database pour créer et amorcer la base de données d’application.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Mise à jour des chaînes de connexion dans le fichier Web. config

Ouvrez le fichier *Web. config* et remplacez l’élément `connectionStrings` par le code suivant :

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Création de la base de données d’appartenance

Dans **Explorateur de solutions**, sélectionnez le projet ContosoUniversity, puis cliquez sur **configuration de ASP.net** dans le menu **projet** .

Cliquez sur l'onglet Sécurité.

Cliquez sur **créer ou gérer des rôles**, puis créez un rôle d' **administrateur** .

Revenez à l’onglet sécurité.

Cliquez sur **créer un utilisateur**, puis activez la case à cocher **administrateur** et créez un utilisateur nommé Admin.

Fermez l' **outil Administration de site Web**.

### <a name="creating-the-school-database"></a>Création de la base de données School

Ouvrez la fenêtre de console du gestionnaire de package.

Dans la liste déroulante **projet par défaut** , sélectionnez le projet CONTOSOUNIVERSITY. dal.

Entrez la commande suivante :

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migrations Code First applique la migration initiale qui crée la base de données, puis applique la migration AddBirthDate, puis il exécute la méthode Seed.

Exécutez le site en appuyant sur CTRL + F5. Comme vous l’avez fait pour les environnements de test et de production, exécutez la page **Ajouter des élèves** , ajoutez un nouvel étudiant, puis affichez le nouvel étudiant dans la page des **étudiants** . Cela permet de vérifier que la base de données School a été créée et initialisée et que vous disposez d’un accès en lecture et en écriture à celle-ci.

Sélectionnez la page **mettre à jour les crédits** et connectez-vous pour vérifier que la base de données d’appartenance a été déployée et que vous y avez accès. Si vous n’avez pas migré vos comptes d’utilisateur, créez un compte d’administrateur, puis sélectionnez la page **mettre à jour les crédits** pour vérifier qu’elle fonctionne.

## <a name="cleaning-up-sql-server-compact-files"></a>Nettoyage des fichiers SQL Server Compact

Vous n’avez plus besoin de fichiers et de packages NuGet inclus pour prendre en charge SQL Server Compact. Si vous le souhaitez (cette étape n’est pas requise), vous pouvez nettoyer les fichiers et références inutiles.

Dans **Explorateur de solutions**, supprimez les fichiers *. sdf* du dossier *app\_Data* et des dossiers *amd64* et *x86* du dossier *bin* .

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution (et non sur l’un des projets), puis cliquez sur **gérer les packages NuGet pour la solution**.

Dans le volet gauche de la boîte de dialogue **gérer les packages NuGet** , sélectionnez **packages installés**.

Sélectionnez le package **EntityFramework. SqlServerCompact** , puis cliquez sur **gérer**.

Dans la boîte de dialogue **Sélectionner les projets** , les deux projets sont sélectionnés. Pour désinstaller le package dans les deux projets, désactivez les deux cases à cocher, puis cliquez sur **OK**.

Dans la boîte de dialogue qui vous demande si vous souhaitez désinstaller également les packages dépendants, cliquez sur non. L’une d’entre elles est le package de Entity Framework que vous devez conserver.

Suivez la même procédure pour désinstaller le package **SqlServerCompact** . (Les packages doivent être désinstallés dans cet ordre, car le package **EntityFramework. SqlServerCompact** dépend du package **SqlServerCompact** .)

Vous avez maintenant effectué une migration vers SQL Server Express et une SQL Server complète. Dans le didacticiel suivant, vous allez modifier une autre base de données, et vous verrez comment déployer les modifications de la base de données lorsque vos bases de données de test et de production utilisent la SQL Server Express et la SQL Server complète.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
