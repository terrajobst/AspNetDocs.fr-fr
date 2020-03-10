---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement de SQL Server Compact bases de données-2 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 56ceabc79947967846d342354fd033510be5f05a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568116"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement de bases de données SQL Server Compact-2 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Présentation

Ce didacticiel montre comment configurer deux bases de données SQL Server Compact et le moteur de base de données pour le déploiement.

Pour l’accès à la base de données, l’application Contoso University requiert les logiciels suivants qui doivent être déployés avec l’application, car elle n’est pas incluse dans le .NET Framework :

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (moteur de base de données).
- [Fournisseurs universels ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (qui permettent au système d’appartenance ASP.net d’utiliser SQL Server Compact)
- [Entity Framework 5,0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(code First avec des migrations).

La structure de la base de données et certaines (pas toutes) des données dans les deux bases de données de l’application doivent également être déployées. En règle générale, lorsque vous développez une application, vous entrez des données de test dans une base de données que vous ne souhaitez pas déployer sur un site actif. Toutefois, vous pouvez également entrer des données de production que vous souhaitez déployer. Dans ce didacticiel, vous allez configurer le projet Contoso University afin que les logiciels requis et les données correctes soient inclus au moment du déploiement.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact contre SQL Server Express

L’exemple d’application utilise SQL Server Compact 4,0. Ce moteur de base de données est une option relativement nouvelle pour les sites Web. les versions antérieures de SQL Server Compact ne fonctionnent pas dans un environnement d’hébergement Web. SQL Server Compact offre un certain nombre d’avantages par rapport au scénario plus courant de développement avec SQL Server Express et de déploiement sur un SQL Server complet. En fonction du fournisseur d’hébergement que vous choisissez, SQL Server Compact peut être moins coûteuse à déployer, car certains fournisseurs chargent des compléments pour prendre en charge une base de données SQL Server complète. Aucun frais supplémentaire n’est facturé pour SQL Server Compact, car vous pouvez déployer le moteur de base de données lui-même dans le cadre de votre application Web.

Toutefois, vous devez également être conscient de ses limitations. SQL Server Compact ne prend pas en charge les procédures stockées, les déclencheurs, les vues ou la réplication. (Pour obtenir la liste complète des fonctionnalités de SQL Server qui ne sont pas prises en charge par SQL Server Compact, consultez [différences entre SQL Server Compact et SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) En outre, certains des outils que vous pouvez utiliser pour manipuler des schémas et des données dans SQL Server Express et SQL Server bases de données ne fonctionnent pas avec SQL Server Compact. Par exemple, vous ne pouvez pas utiliser SQL Server Management Studio ou SQL Server Data Tools dans Visual Studio avec SQL Server Compact bases de données. Vous disposez d’autres options pour travailler avec des bases de données SQL Server Compact :

- Vous pouvez utiliser Explorateur de serveurs dans Visual Studio, qui offre des fonctionnalités de manipulation de base de données limitées pour les SQL Server Compact.
- Vous pouvez utiliser la fonctionnalité de manipulation de base de données de [WebMatrix](https://www.microsoft.com/web/webmatrix/), qui offre plus de fonctionnalités que Explorateur de serveurs.
- Vous pouvez utiliser des outils tiers ou open source relativement complets, tels que la [boîte à outils SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) et l' [utilitaire de script de schéma et de données SQL Compact](https://github.com/ErikEJ/SqlCeToolbox).
- Vous pouvez écrire et exécuter vos propres scripts DDL (Data Definition Language) pour manipuler le schéma de la base de données.

Vous pouvez commencer par SQL Server Compact, puis effectuer une mise à niveau ultérieurement à mesure que vos besoins évoluent. Les didacticiels ultérieurs de cette série vous montrent comment migrer de SQL Server Compact vers SQL Server Express et vers SQL Server. Toutefois, si vous créez une nouvelle application et que vous vous attendez à ce que SQL Server dans un avenir proche, il est probablement préférable de commencer par SQL Server ou SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Configuration de la Moteur de base de données SQL Server Compact pour le déploiement

Les logiciels requis pour l’accès aux données dans l’application Contoso University ont été ajoutés en installant les packages NuGet suivants :

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System. Web. Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.net Universal Providers)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework. SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Les liens pointent vers les versions actuelles de ces packages, qui peuvent être plus récents que ce qui est installé dans le projet de démarrage que vous avez téléchargé pour ce didacticiel. Pour le déploiement sur un fournisseur d’hébergement, assurez-vous d’utiliser Entity Framework 5,0 ou une version ultérieure. Les versions antérieures de Migrations Code First requièrent une confiance totale et, dans de nombreux fournisseurs d’hébergement, votre application s’exécutera en mode de confiance moyenne. Pour plus d’informations sur la confiance moyenne, consultez le didacticiel sur le [déploiement d’IIS en tant qu’environnement de test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

L’installation du package NuGet se charge généralement de tout ce dont vous avez besoin pour déployer ce logiciel avec l’application. Dans certains cas, cela implique des tâches telles que la modification du fichier Web. config et l’ajout de scripts PowerShell qui s’exécutent chaque fois que vous générez la solution. **Si vous souhaitez ajouter la prise en charge de l’une de ces fonctionnalités (telles que SQL Server Compact et Entity Framework) sans utiliser NuGet, assurez-vous que vous savez à quoi sert l’installation du package NuGet pour pouvoir effectuer le même travail manuellement.**

Il existe une exception dans laquelle NuGet ne tient pas compte de tout ce que vous devez faire pour garantir la réussite du déploiement. Le package NuGet SqlServerCompact ajoute un script de publication à votre projet qui copie les assemblys natifs dans des sous-dossiers *x86* et *amd64* sous le dossier *bin* du projet, mais le script n’inclut pas ces dossiers dans le projet. Par conséquent, Web Deploy ne les copie pas sur le site Web de destination, sauf si vous les incluez manuellement dans le projet. (Ce comportement résulte de la configuration de déploiement par défaut ; une autre option, que vous n’utiliserez pas dans ces didacticiels, consiste à modifier le paramètre qui contrôle ce comportement. Le paramètre que vous pouvez modifier n’est que les **fichiers nécessaires à l’exécution de l’application** sous **éléments à déployer** sous l’onglet **Package/Publication Web** de la fenêtre **Propriétés du projet** . La modification de ce paramètre n’est généralement pas recommandée, car cela peut entraîner le déploiement de nombreux autres fichiers dans l’environnement de production. Pour plus d’informations sur les alternatives, consultez le didacticiel sur la [Configuration des propriétés de projet](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) .)

Générez le projet, puis, dans **Explorateur de solutions** cliquez sur **Afficher tous les fichiers** si vous ne l’avez pas déjà fait. Vous devrez peut-être également cliquer sur **Actualiser**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Développez le dossier **bin** pour voir les dossiers **amd64** et **x86** , puis sélectionnez ces dossiers, cliquez avec le bouton droit et sélectionnez **inclure dans le projet**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Les icônes de dossier changent pour indiquer que le dossier a été inclus dans le projet.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Configuration de Migrations Code First pour le déploiement d’une base de données d’application

Lorsque vous déployez une base de données d’application, vous ne déployez généralement pas simplement votre base de données de développement avec toutes les données qu’elle contient en production, car la plupart des données qu’elle contient sont probablement à des fins de test. Par exemple, les noms des étudiants dans une base de données de test sont fictifs. En revanche, il est souvent impossible de déployer uniquement la structure de la base de données, sans aucune donnée. Certaines des données de votre base de données de test peuvent être des données réelles et doivent être utilisées lorsque les utilisateurs commencent à utiliser l’application. Par exemple, votre base de données peut avoir une table qui contient des valeurs de classe valides ou des noms de service réels.

Pour simuler ce scénario courant, vous configurerez une méthode Migrations Code First Seed qui insère dans la base de données uniquement les données que vous souhaitez utiliser en production. Cette méthode Seed n’insère pas de données de test, car elle s’exécute en production après Code First crée la base de données en production.

Dans les versions antérieures de Code First avant la publication des migrations, il était courant que les méthodes de départ insèrent également des données de test, car avec chaque modification de modèle au cours du développement, la base de données devait être complètement supprimée et recréée à partir de zéro. Avec Migrations Code First, les données de test sont conservées après la modification de la base de données, de sorte que les données de test dans la méthode Seed ne sont pas nécessaires. Le projet que vous avez téléchargé utilise la méthode de pré-migrations pour inclure toutes les données dans la méthode Seed d’une classe d’initialiseur. Dans ce didacticiel, vous allez désactiver la classe de l’initialiseur et activer les migrations. Ensuite, vous allez mettre à jour la méthode Seed dans la classe de configuration migrations afin qu’elle insère uniquement les données que vous souhaitez insérer en production.

Le diagramme suivant illustre le schéma de la base de données d’application :

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Pour ces didacticiels, vous supposez que les tables `Student` et `Enrollment` doivent être vides lorsque le site est déployé pour la première fois. Les autres tables contiennent des données qui doivent être préchargées lorsque l’application est en ligne.

Dans la mesure où vous utiliserez Migrations Code First, vous n’avez plus besoin d’utiliser l’initialiseur de Code First **DropCreateDatabaseIfModelChanges** . Le code de cet initialiseur se trouve dans le fichier SchoolInitializer.cs du projet ContosoUniversity. DAL. Un paramètre dans l’élément **appSettings** du fichier Web. config provoque l’exécution de cet initialiseur chaque fois que l’application tente d’accéder à la base de données pour la première fois :

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Ouvrez le fichier Web. config de l’application et supprimez l’élément qui spécifie la classe d’initialiseur Code First de l’élément appSettings. L’élément appSettings ressemble maintenant à ceci :

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Pour spécifier une classe d’initialiseur, vous pouvez également appeler `Database.SetInitializer` dans la méthode `Application_Start` dans le fichier *global. asax* . Si vous activez des migrations dans un projet qui utilise cette méthode pour spécifier l’initialiseur, supprimez cette ligne de code.

Ensuite, activez Migrations Code First.

La première étape consiste à s’assurer que le projet ContosoUniversity est défini comme projet de démarrage. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet ContosoUniversity et sélectionnez **définir comme projet de démarrage**. Migrations Code First Rechercher la chaîne de connexion de base de données dans le projet de démarrage.

Dans le menu **Outils**, cliquez sur **Gestionnaire de package NuGet**, puis sur **Console du Gestionnaire de package**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

En haut de la fenêtre de la **console du gestionnaire de package** , sélectionnez CONTOSOUNIVERSITY. dal comme projet par défaut puis, à l’invite de `PM>`, entrez « activer-migrations ».

![activer-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Cette commande crée un fichier *Configuration.cs* dans un nouveau dossier *migrations* dans le projet ContosoUniversity. dal.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Vous avez sélectionné le projet DAL, car la commande « Activer-migrations » doit être exécutée dans le projet qui contient la classe de contexte Code First. Lorsque cette classe se trouve dans un projet de bibliothèque de classes, Migrations Code First recherche la chaîne de connexion de la base de données dans le projet de démarrage pour la solution. Dans la solution ContosoUniversity, le projet Web a été défini comme projet de démarrage. (Si vous ne souhaitez pas désigner le projet qui a la chaîne de connexion comme projet de démarrage dans Visual Studio, vous pouvez spécifier le projet de démarrage dans la commande PowerShell. Pour afficher la syntaxe de commande pour la commande Enable-migrations, vous pouvez entrer la commande « obtenir-Help Enable-migrations ».)

Ouvrez le fichier Configuration.cs et remplacez les commentaires de la méthode `Seed` par le code suivant :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Les références à `List` comportent des lignes ondulées rouges, car vous n’avez pas encore d’instruction `using` pour son espace de noms. Cliquez avec le bouton droit sur l’une des instances de `List` puis cliquez sur **résoudre**, puis sur **utilisation de System. Collections. Generic**.

![Résoudre avec l’instruction using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Cette sélection de menu ajoute le code suivant aux instructions `using` en haut du fichier.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> L’ajout de code à la méthode `Seed` est l’une des nombreuses façons dont vous pouvez insérer des données fixes dans la base de données. Une alternative consiste à ajouter du code aux méthodes `Up` et `Down` de chaque classe de migration. Les méthodes `Up` et `Down` contiennent du code qui implémente des modifications de base de données. Vous y verrez des exemples dans le didacticiel sur le [déploiement d’une mise à jour de base de données](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .
> 
> Vous pouvez également écrire du code qui exécute des instructions SQL à l’aide de la méthode `Sql`. Par exemple, si vous ajoutez une colonne budget à la table Department et souhaitez initialiser tous les budgets de service à $1 000,00 dans le cadre d’une migration, vous pouvez ajouter la ligne de code suivante à la méthode `Up` pour cette migration :
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Cet exemple présenté pour ce didacticiel utilise la méthode `AddOrUpdate` dans la méthode `Seed` de la classe `Configuration` Migrations Code First. Migrations Code First appelle la méthode `Seed` après chaque migration, et cette méthode met à jour les lignes qui ont déjà été insérées, ou les insère si elles n’existent pas encore. La méthode `AddOrUpdate` peut ne pas être le meilleur choix pour votre scénario. Pour plus d’informations, consultez la rubrique se [préoccuper de la méthode EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sur le blog de Julie Lerman.

Appuyez sur CTRL-SHIFT-B pour générer le projet.

L’étape suivante consiste à créer une classe de `DbMigration` pour la migration initiale. Vous souhaitez que cette migration crée une nouvelle base de données. vous devez donc supprimer la base de données qui existe déjà. SQL Server Compact bases de données sont contenues dans des fichiers *. sdf* dans le dossier de données de l' *application\_* . Dans **Explorateur de solutions**, développez *application\_données* dans le projet ContosoUniversity pour voir les deux bases de données SQL Server Compact, qui sont représentées par des fichiers *. sdf* .

Cliquez avec le bouton droit sur le fichier *School. sdf* , puis cliquez sur **supprimer**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

Dans la fenêtre **console du gestionnaire de package** , entrez la commande « Add-migration initial » pour créer la migration initiale et la nommer « initial ».

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrations Code First crée un autre fichier de classe dans le dossier *migrations* , et cette classe contient le code qui crée le schéma de la base de données.

Dans la **console du gestionnaire de package**, entrez la commande Update-Database pour créer la base de données et exécuter la méthode **Seed** .

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Si vous recevez une erreur indiquant qu’une table existe déjà et ne peut pas être créée, cela est probablement dû au fait que vous avez exécuté l’application après avoir supprimé la base de données et avant d’avoir exécuté `update-database`. Dans ce cas, supprimez de nouveau le fichier *School. sdf* et relancez la commande `update-database`.)

Exécutez l'application. La page Students est désormais vide, mais la page des formateurs contient des instructeurs. C’est ce que vous obtiendrez en production après avoir déployé l’application.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Le projet est maintenant prêt à déployer la base de données *School* .

## <a name="creating-a-membership-database-for-deployment"></a>Création d’une base de données d’appartenance pour le déploiement

L’application Contoso University utilise le système d’appartenance ASP.NET et l’authentification par formulaire pour authentifier et autoriser les utilisateurs. L’une de ses pages est accessible uniquement aux administrateurs. Pour afficher cette page, exécutez l’application et sélectionnez **mettre à jour les crédits** dans le menu contextuel sous **cours**. L’application affiche la page **de connexion** , car seuls les administrateurs sont autorisés à utiliser la page **mettre à jour les crédits** .

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Connectez-vous en tant que « admin » à l’aide du mot de passe « pas $ w0rd » (Notez le nombre zéro à la place de la lettre « o » dans « w0rd »). Une fois que vous êtes connecté, la page **mettre à jour les crédits** s’affiche.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Lorsque vous déployez un site pour la première fois, il est courant d’exclure la plupart ou la totalité des comptes d’utilisateur que vous créez pour le test. Dans ce cas, vous allez déployer un compte d’administrateur et aucun compte d’utilisateur. Au lieu de supprimer manuellement les comptes de test, vous allez créer une nouvelle base de données d’appartenance qui n’a que le seul compte d’utilisateur administrateur dont vous avez besoin en production.

> [!NOTE]
> La base de données d’appartenance stocke un hachage de mots de passe de compte. Pour déployer des comptes d’un ordinateur à un autre, vous devez vous assurer que les routines de hachage ne génèrent pas d’autres hachages sur le serveur de destination que sur l’ordinateur source. Ils génèrent les mêmes hachages lorsque vous utilisez la Fournisseurs universels ASP.NET, à condition de ne pas modifier l’algorithme par défaut. L’algorithme par défaut est HMACSHA256 et est spécifié dans l’attribut de **validation** de l’élément **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** dans le fichier Web. config.

La base de données d’appartenance n’est pas gérée par Migrations Code First, et il n’existe aucun initialiseur automatique qui amorce la base de données avec des comptes de test (comme c’est le cas pour la base de données School). Par conséquent, pour conserver les données de test disponibles, vous allez effectuer une copie de la base de données de test avant d’en créer une nouvelle.

Dans **Explorateur de solutions**, renommez le fichier *Aspnet. sdf* dans le dossier *application\_Data* en *ASPNET-dev. sdf*. (Ne faites pas de copie, renommez-la, vous allez créer une nouvelle base de données dans un moment.)

Dans **Explorateur de solutions**, assurez-vous que le projet Web (ContosoUniversity, et non CONTOSOUNIVERSITY. dal) est sélectionné. Ensuite, dans le menu **projet** , sélectionnez **configuration de ASP.net** pour exécuter l' **outil Administration de site Web**(WAT).

Sélectionnez l’onglet **Sécurité**.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Cliquez sur **créer ou gérer des rôles** et ajouter un rôle d' **administrateur** .

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Revenez à l’onglet **sécurité** , cliquez sur **créer un utilisateur**, puis ajoutez l’utilisateur « administrateur » en tant qu’administrateur. Avant de cliquer sur le bouton **créer un utilisateur** dans la page **créer un utilisateur** , assurez-vous que vous activez la case à cocher **administrateur** . Le mot de passe utilisé dans ce didacticiel est « pas $ w0rd » et vous pouvez entrer une adresse de messagerie.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Fermez le navigateur. Dans **Explorateur de solutions**, cliquez sur le bouton Actualiser pour afficher le nouveau fichier *Aspnet. sdf* .

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Cliquez avec le bouton droit sur **Aspnet. sdf** et sélectionnez **inclure dans le projet**.

## <a name="distinguishing-development-from-production-databases"></a>Distinction entre le développement et les bases de données de production

Dans cette section, vous allez renommer les bases de données afin que les versions de développement soient School-Dev. sdf et aspnet-Dev. sdf et que les versions de production soient School-Prod. sdf et aspnet-Prod. sdf. Cela n’est pas nécessaire, mais cela vous empêchera d’obtenir des versions de test et de production des bases de données confuses.

Dans **Explorateur de solutions**, cliquez sur **Actualiser** , puis développez le dossier application\_Data pour afficher la base de données School que vous avez créée précédemment. cliquez dessus avec le bouton droit et sélectionnez **inclure dans le projet**.

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Renommez *Aspnet. sdf* en *ASPNET-prod. sdf*.

Renommez *School. sdf* en *School-dev. sdf*.

Quand vous exécutez l’application dans Visual Studio, vous ne souhaitez pas utiliser les versions *-prod* des fichiers de base de données, mais vous souhaitez utiliser les versions *-dev* . Par conséquent, vous devez modifier les chaînes de connexion dans le fichier Web. config afin qu’elles pointent vers les versions *-dev* des bases de données. (Vous n’avez pas créé de fichier School-Prod. sdf, mais cela est OK, car Code First créera cette base de données en production la première fois que vous exécuterez votre application ici.)

Ouvrez le fichier Web. config de l’application, puis localisez les chaînes de connexion :

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Remplacez « Aspnet. sdf » par « aspnet-Dev. sdf » et remplacez « School. sdf » par « School-Dev. sdf » :

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Le moteur de base de données SQL Server Compact et les deux bases de données sont maintenant prêts à être déployés. Dans le didacticiel suivant, vous configurez des transformations de fichier *Web. config* automatiques pour les paramètres qui doivent être différents dans les environnements de développement, de test et de production. (Parmi les paramètres qui doivent être modifiés sont les chaînes de connexion, mais vous allez les configurer ultérieurement lorsque vous créez un profil de publication.)

## <a name="more-information"></a>Plus d'informations

Pour plus d’informations sur NuGet, consultez [gérer les bibliothèques de projets avec NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) et [la documentation NuGet](http://docs.nuget.org/docs/start-here/overview). Si vous ne souhaitez pas utiliser NuGet, vous devez apprendre à analyser un package NuGet pour déterminer ce qu’il fait lorsqu’il est installé. (Par exemple, il peut configurer des transformations *Web. config* , configurer des scripts PowerShell pour qu’ils s’exécutent au moment de la génération, etc.) Pour en savoir plus sur le fonctionnement de NuGet, consultez la page [création et publication d’un package et d’un](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) [fichier de configuration et des transformations de code source](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
