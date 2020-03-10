---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : préparation pour le déploiement de base de données | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: cdcb3578725c41e3c801afd54e6d34455bc4b281
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636996"
---
# <a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : préparation pour le déploiement de base de données

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

## <a name="overview"></a>Présentation

Ce didacticiel montre comment préparer le projet pour le déploiement de base de données. La structure de la base de données et certaines (pas toutes) des données dans les deux bases de données de l’application doivent être déployées dans des environnements de test, intermédiaire et de production.

En règle générale, lorsque vous développez une application, vous entrez des données de test dans une base de données que vous ne souhaitez pas déployer sur un site actif. Toutefois, vous pouvez également disposer de données de production que vous souhaitez déployer. Dans ce didacticiel, vous allez configurer le projet Contoso University et préparer les scripts SQL afin que les données correctes soient incluses lors du déploiement.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

L’exemple d’application utilise SQL Server Express base de données locale. SQL Server Express est l’édition gratuite de SQL Server. Elle est couramment utilisée lors du développement, car elle est basée sur le même moteur de base de données que les versions complètes de SQL Server. Vous pouvez tester avec SQL Server Express et être sûr que l’application se comporte de la même façon en production, à quelques exceptions près pour les fonctionnalités qui varient d’un SQL Server à l’autre.

La base de données locale est un mode d’exécution spécial de SQL Server Express qui vous permet d’utiliser des bases de données en tant que fichiers *. mdf* . En règle générale, les fichiers de base de données de base de données locale sont conservés dans le dossier *application\_Data* d’un projet Web. La fonctionnalité d’instance utilisateur de SQL Server Express vous permet également d’utiliser des fichiers *. mdf* , mais la fonctionnalité d’instance utilisateur est déconseillée. par conséquent, la base de données locale est recommandée pour travailler avec des fichiers *. mdf* .

En général SQL Server Express n’est pas utilisé pour les applications Web de production. En particulier, la base de données locale n’est pas recommandée pour une utilisation en production avec une application Web, car elle n’est pas conçue pour fonctionner avec IIS.

Dans Visual Studio 2012, la base de données locale est installée par défaut dans Visual Studio. Dans Visual Studio 2010 et les versions antérieures, SQL Server Express (sans base de données locale) est installé par défaut dans Visual Studio. C’est pourquoi vous l’avez installé comme l’une des conditions préalables dans [le premier didacticiel de cette série](introduction.md).

Pour plus d’informations sur les éditions SQL Server, y compris la base de données locale, consultez les ressources suivantes, [utilisation des bases de données SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework et Fournisseurs universels

Pour l’accès à la base de données, l’application Contoso University requiert les logiciels suivants qui doivent être déployés avec l’application, car elle n’est pas incluse dans le .NET Framework :

- [Fournisseurs universels ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (permet au système d’appartenance ASP.net d’utiliser Azure SQL Database)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Étant donné que ce logiciel est inclus dans les packages NuGet, le projet est déjà configuré de sorte que les assemblys requis soient déployés avec le projet. (Les liens pointent vers les versions actuelles de ces packages, qui peuvent être plus récents que ce qui est installé dans le projet de démarrage que vous avez téléchargé pour ce didacticiel.)

Si vous déployez sur un fournisseur d’hébergement tiers au lieu d’Azure, assurez-vous d’utiliser Entity Framework 5,0 ou une version ultérieure. Les versions antérieures de Migrations Code First requièrent une confiance totale, et la plupart des fournisseurs d’hébergement exécuteront votre application avec une confiance moyenne. Pour plus d’informations sur la confiance moyenne, consultez le didacticiel [déployer dans IIS en tant qu’environnement de test](deploying-to-iis.md) .

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurer Migrations Code First pour le déploiement de la base de données d’application

La base de données d’application Contoso University est gérée par Code First, et vous la déployez à l’aide de Migrations Code First. Pour obtenir une vue d’ensemble du déploiement de base de données à l’aide de Migrations Code First, consultez [le premier didacticiel de cette série](introduction.md).

Lorsque vous déployez une base de données d’application, vous ne déployez généralement pas simplement votre base de données de développement avec toutes les données qu’elle contient en production, car la plupart des données qu’elle contient sont probablement à des fins de test. Par exemple, les noms des étudiants dans une base de données de test sont fictifs. En revanche, il est souvent impossible de déployer uniquement la structure de la base de données, sans aucune donnée. Certaines des données de votre base de données de test peuvent être des données réelles et doivent être utilisées lorsque les utilisateurs commencent à utiliser l’application. Par exemple, votre base de données peut avoir une table qui contient des valeurs de classe valides ou des noms de service réels.

Pour simuler ce scénario courant, vous configurez une méthode de `Seed` Migrations Code First qui insère dans la base de données uniquement les données que vous souhaitez mettre en production. Cette méthode de `Seed` ne doit pas insérer les données de test, car elles s’exécuteront en production après que Code First a créé la base de données en production.

Dans les versions antérieures de Code First avant la publication des migrations, il était courant que `Seed` méthodes insèrent également des données de test, car avec chaque modification de modèle au cours du développement, la base de données devait être complètement supprimée et recréée à partir de zéro. Avec Migrations Code First, les données de test sont conservées après la modification de la base de données, de sorte que les données de test dans la méthode `Seed` ne sont pas nécessaires. Le projet que vous avez téléchargé utilise la méthode d’inclusion de toutes les données dans la méthode `Seed` d’une classe d’initialiseur. Dans ce didacticiel, vous allez désactiver cette classe d’initialiseur et activer les migrations. Ensuite, vous allez mettre à jour la méthode `Seed` dans la classe de configuration migrations afin qu’elle insère uniquement les données que vous souhaitez insérer en production.

Le diagramme suivant illustre le schéma de la base de données d’application :

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Pour ces didacticiels, vous supposez que les tables `Student` et `Enrollment` doivent être vides lorsque le site est déployé pour la première fois. Les autres tables contiennent des données qui doivent être préchargées lorsque l’application est en ligne.

### <a name="disable-the-initializer"></a>Désactiver l’initialiseur

Dans la mesure où vous utiliserez Migrations Code First, vous n’avez pas besoin d’utiliser l’initialiseur de Code First `DropCreateDatabaseIfModelChanges`. Le code de cet initialiseur se trouve dans le fichier *SchoolInitializer.cs* du projet CONTOSOUNIVERSITY. dal. Un paramètre dans l’élément `appSettings` du fichier *Web. config* provoque l’exécution de cet initialiseur chaque fois que l’application tente d’accéder à la base de données pour la première fois :

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Ouvrez le fichier *Web. config* de l’application et supprimez ou commentez l’élément `add` qui spécifie la classe d’initialiseur code First. L’élément `appSettings` se présente désormais comme suit :

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Pour spécifier une classe d’initialiseur, vous pouvez également appeler `Database.SetInitializer` dans la méthode `Application_Start` dans le fichier *global. asax* . Si vous activez des migrations dans un projet qui utilise cette méthode pour spécifier l’initialiseur, supprimez cette ligne de code.

> [!NOTE]
> Si vous utilisez Visual Studio 2013, ajoutez les étapes suivantes entre les étapes 2 et 3 : (a) dans PMC, entrez « Update-Package EntityFramework-version 6.1.1 » pour obtenir la version actuelle d’EF. Puis (b) générez le projet pour obtenir une liste des erreurs de build et corrigez-les. Supprimez les instructions using pour les espaces de noms qui n’existent plus, cliquez avec le bouton droit et cliquez sur résoudre pour ajouter des instructions using quand elles sont nécessaires, puis modifiez les occurrences de System. Data. EntityState en System. Data. Entity. EntityState.

### <a name="enable-code-first-migrations"></a>Activer Migrations Code First

1. Assurez-vous que le projet ContosoUniversity (et non ContosoUniversity. DAL) est défini comme projet de démarrage. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet ContosoUniversity et sélectionnez **définir comme projet de démarrage**. Migrations Code First Rechercher la chaîne de connexion de base de données dans le projet de démarrage.
2. Dans le menu **Outils** , choisissez **Gestionnaire de package NuGet** > **console du gestionnaire de package**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. En haut de la fenêtre de la **console du gestionnaire de package** , sélectionnez CONTOSOUNIVERSITY. dal comme projet par défaut puis, à l’invite de `PM>`, entrez « activer-migrations ».

    ![commande Enable-migrations](preparing-databases/_static/image4.png)

    (Si vous recevez une erreur indiquant que la commande *Enable-migrations* n’est pas reconnue, entrez la commande *Update-Package EntityFramework-REINSTALL* , puis réessayez.)

    Cette commande crée un dossier *migrations* dans le projet CONTOSOUNIVERSITY. dal et place dans ce dossier deux fichiers : un fichier *Configuration.cs* que vous pouvez utiliser pour configurer des migrations et un fichier *InitialCreate.cs* pour la première migration qui crée la base de données.

    ![Dossier migrations](preparing-databases/_static/image5.png)

    Vous avez sélectionné le projet DAL dans la liste déroulante **projet par défaut** de la **console du gestionnaire de package** , car la commande `enable-migrations` doit être exécutée dans le projet qui contient la classe de contexte code First. Lorsque cette classe se trouve dans un projet de bibliothèque de classes, Migrations Code First recherche la chaîne de connexion de la base de données dans le projet de démarrage pour la solution. Dans la solution ContosoUniversity, le projet Web a été défini comme projet de démarrage. Si vous ne souhaitez pas désigner le projet qui a la chaîne de connexion comme projet de démarrage dans Visual Studio, vous pouvez spécifier le projet de démarrage dans la commande PowerShell. Pour afficher la syntaxe de la commande, entrez la commande `get-help enable-migrations`.

    La commande `enable-migrations` a créé automatiquement la première migration, car la base de données existe déjà. Une alternative consiste à faire en sorte que les migrations créent la base de données. Pour ce faire, utilisez **Explorateur de serveurs** ou **Explorateur d’objets SQL Server** pour supprimer la base de données ContosoUniversity avant d’activer les migrations. Après avoir activé les migrations, créez la première migration manuellement en entrant la commande « Add-migration InitialCreate ». Vous pouvez ensuite créer la base de données en entrant la commande « Update-Database ».

### <a name="set-up-the-seed-method"></a>Configurer la méthode Seed

Pour ce didacticiel, vous allez ajouter des données fixes en ajoutant du code à la méthode `Seed` de la classe `Configuration` Migrations Code First. Migrations Code First appelle la méthode `Seed` après chaque migration.

Étant donné que la méthode `Seed` s’exécute après chaque migration, il existe déjà des données dans les tables après la première migration. Pour gérer cette situation, vous allez utiliser la méthode `AddOrUpdate` pour mettre à jour des lignes qui ont déjà été insérées ou les insérer si elles n’existent pas encore. La méthode `AddOrUpdate` peut ne pas être le meilleur choix pour votre scénario. Pour plus d’informations, consultez la rubrique se [préoccuper de la méthode EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sur le blog de Julie Lerman.

1. Ouvrez le fichier *Configuration.cs* et remplacez les commentaires de la méthode `Seed` par le code suivant :

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Les références à `List` comportent des lignes ondulées rouges, car vous n’avez pas encore d’instruction `using` pour son espace de noms. Cliquez avec le bouton droit sur l’une des instances de `List` puis cliquez sur **résoudre**, puis sur **utilisation de System. Collections. Generic**.

    ![Résoudre avec l’instruction using](preparing-databases/_static/image6.png)

    Cette sélection de menu ajoute le code suivant aux instructions `using` en haut du fichier.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Appuyez sur CTRL-SHIFT-B pour générer le projet.

Le projet est maintenant prêt à déployer la base de données *ContosoUniversity* . Une fois l’application déployée, la première fois que vous l’exécutez et accédez à une page qui accède à la base de données, Code First crée la base de données et exécute cette `Seed` méthode.

> [!NOTE]
> L’ajout de code à la méthode `Seed` est l’une des nombreuses façons dont vous pouvez insérer des données fixes dans la base de données. Une alternative consiste à ajouter du code aux méthodes `Up` et `Down` de chaque classe de migration. Les méthodes `Up` et `Down` contiennent du code qui implémente des modifications de base de données. Vous y verrez des exemples dans le didacticiel sur le [déploiement d’une mise à jour de base de données](deploying-a-database-update.md) .
> 
> Vous pouvez également écrire du code qui exécute des instructions SQL à l’aide de la méthode `Sql`. Par exemple, si vous ajoutez une colonne budget à la table Department et souhaitez initialiser tous les budgets de service à $1 000,00 dans le cadre d’une migration, vous pouvez ajouter la ligne de code suivante à la méthode `Up` pour cette migration :
> 
> `Sql("UPDATE Department SET Budget = 1000");`

## <a name="create-scripts-for-membership-database-deployment"></a>Créer des scripts pour le déploiement de base de données d’appartenance

L’application Contoso University utilise le système d’appartenance ASP.NET et l’authentification par formulaire pour authentifier et autoriser les utilisateurs. La page **mettre à jour les crédits** est accessible uniquement aux utilisateurs qui appartiennent au rôle administrateur.

Exécutez l’application et cliquez sur **courses**, puis sur **mettre à jour les crédits**.

![Cliquez sur mettre à jour les crédits](preparing-databases/_static/image7.png)

La page **de connexion** s’affiche car la page **mettre à jour les crédits** requiert des privilèges d’administrateur.

Entrez *admin* comme nom d’utilisateur et *devpwd* comme mot de passe, puis cliquez sur **se connecter**.

![Page de connexion](preparing-databases/_static/image8.png)

La page **mettre à jour les crédits** s’affiche.

![Page mettre à jour les crédits](preparing-databases/_static/image9.png)

Les informations d’utilisateur et de rôle se trouvent dans la base de données *ASPNET-ContosoUniversity* spécifiée par la chaîne de connexion **DefaultConnection** dans le fichier *Web. config* .

Cette base de données n’est pas gérée par Entity Framework Code First. vous ne pouvez donc pas utiliser les migrations pour la déployer. Vous allez utiliser le fournisseur dbDacFx pour déployer le schéma de base de données, et vous configurerez le profil de publication pour exécuter un script qui insère les données initiales dans les tables de base de données.

> [!NOTE]
> Un nouveau système d’appartenance ASP.NET (maintenant nommé ASP.NET Identity) a été introduit avec Visual Studio 2013. Le nouveau système vous permet de conserver les tables d’application et d’appartenance dans la même base de données, et vous pouvez utiliser Migrations Code First pour déployer les deux. L’exemple d’application utilise le système d’appartenance ASP.NET antérieur, qui ne peut pas être déployé à l’aide de Migrations Code First. Les procédures de déploiement de cette base de données d’appartenance s’appliquent également à tout autre scénario dans lequel votre application doit déployer une SQL Server base de données qui n’est pas créée par Entity Framework Code First.

En règle générale, vous ne souhaitez généralement pas les mêmes données en production que vous avez en cours de développement. Lorsque vous déployez un site pour la première fois, il est courant d’exclure la plupart ou la totalité des comptes d’utilisateur que vous créez pour le test. Par conséquent, le projet téléchargé comprend deux bases de données d’appartenance : *ASPNET-ContosoUniversity. mdf* avec des utilisateurs de développement et *ASPNET-ContosoUniversity-prod. mdf* avec des utilisateurs de production. Pour ce didacticiel, les noms d’utilisateur sont les mêmes dans les deux bases de données : *admin* *et autre*. Les deux utilisateurs ont le mot de passe *devpwd* dans la base de données de développement et *prodpwd* dans la base de données de production.

Vous allez déployer les utilisateurs de développement vers l’environnement de test et les utilisateurs de production vers l’environnement intermédiaire et de production. Pour ce faire, vous allez créer deux scripts SQL dans ce didacticiel, l’un pour le développement et l’autre pour la production, et dans les didacticiels ultérieurs, vous configurerez le processus de publication pour les exécuter.

> [!NOTE]
> La base de données d’appartenance stocke un hachage de mots de passe de compte. Pour déployer des comptes d’un ordinateur à un autre, vous devez vous assurer que les routines de hachage ne génèrent pas d’autres hachages sur le serveur de destination que sur l’ordinateur source. Ils génèrent les mêmes hachages lorsque vous utilisez la Fournisseurs universels ASP.NET, à condition de ne pas modifier l’algorithme par défaut. L’algorithme par défaut est HMACSHA256 et est spécifié dans l’attribut de **validation** de l’élément **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** dans le fichier Web. config.

Vous pouvez créer des scripts de déploiement de données manuellement à l’aide de SQL Server Management Studio (SSMS) ou à l’aide d’un outil tiers. Ce didacticiel vous indiquera comment le faire dans SSMS, mais si vous ne souhaitez pas installer et utiliser SSMS, vous pouvez obtenir les scripts à partir de la version terminée du projet et passer à la section où vous les stockez dans le dossier de solution.

Pour installer SSMS, installez-le à partir du [Centre de téléchargement : Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) en cliquant sur [ENU\x64\SQLManagementStudio\_x64\_fra. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) ou [ENU\x86\SQLManagementStudio\_x86\_fra. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Si vous choisissez une valeur incorrecte pour votre système, l’installation échoue et vous pouvez essayer l’autre.

(Notez qu’il s’agit d’un téléchargement de 600 mégaoctets. L’installation peut prendre beaucoup de temps et nécessiter un redémarrage de votre ordinateur.)

Sur la première page du centre d’installation SQL Server, cliquez sur **nouvelle SQL Server installation autonome ou ajout de fonctionnalités à une installation existante**, puis suivez les instructions, en acceptant les options par défaut.

### <a name="create-the-development-database-script"></a>Créer le script de base de données de développement

1. Exécutez SSMS.
2. Dans la boîte de dialogue **se connecter au serveur** , entrez (base de données locale *) \v11.0* comme **nom de serveur**, laissez **authentification** définie sur **authentification Windows**, puis cliquez sur **se connecter**.

    ![SSMS se connecter au serveur](preparing-databases/_static/image10.png)
3. Dans la fenêtre de l' **Explorateur d’objets** , développez **bases de données**, cliquez avec le bouton droit sur **ASPNET-ContosoUniversity**, cliquez sur **tâches**, puis sur **générer des scripts**.

    ![Scripts de génération SSMS](preparing-databases/_static/image11.png)
4. Dans la boîte de dialogue **générer et publier des scripts** , cliquez sur **définir les options de script**.

    Vous pouvez ignorer l’étape **choisir des objets** , car la valeur par défaut est **base de données entière et tous les objets de base de données** , et c’est ce que vous souhaitez.
5. Cliquez sur **Avancé**.

    ![Options de script SSMS](preparing-databases/_static/image12.png)
6. Dans la boîte de dialogue **options de script avancées** , faites défiler jusqu’à **types de données à script**, puis cliquez sur l’option **données uniquement** dans la liste déroulante.
7. Modifier le **script utiliser la base de données** avec la **valeur false**. Les instructions USE ne sont pas valides pour Azure SQL Database et ne sont pas nécessaires pour le déploiement sur SQL Server Express dans l’environnement de test.

    ![Données de script SSMS uniquement, aucune instruction USE](preparing-databases/_static/image13.png)
8. Cliquez sur **OK**.
9. Dans la boîte de dialogue **générer et publier des scripts** , la zone **nom de fichier** spécifie l’emplacement où le script sera créé. Modifiez le chemin d’accès à votre dossier de solution (le dossier qui contient votre fichier ContosoUniversity. sln) et le nom de fichier *ASPNET-Data-dev. SQL*.
10. Cliquez sur **suivant** pour accéder à l’onglet **Résumé** , puis cliquez à nouveau sur **suivant** pour créer le script.

    ![Script SSMS créé](preparing-databases/_static/image14.png)
11. Cliquez sur **Finish**.

### <a name="create-the-production-database-script"></a>Créer le script de base de données de production

Étant donné que vous n’avez pas exécuté le projet avec la base de données de production, il n’est pas encore attaché à l’instance de base de données locale. Par conséquent, vous devez d’abord attacher la base de données.

1. Dans l' **Explorateur d’objets**de SSMS, cliquez avec le bouton droit sur **bases de données** , puis cliquez sur **attacher**.

    ![Attachement de SSMS](preparing-databases/_static/image15.png)
2. Dans la boîte **de dialogue attacher des bases de données** , cliquez sur **Ajouter** , puis accédez au fichier *ASPNET-ContosoUniversity-prod. mdf* dans le dossier de données de l' *application\_* .

     ![SSMS ajouter un fichier. mdf à attacher](preparing-databases/_static/image16.png)
3. Cliquez sur **OK**.
4. Suivez la même procédure que celle utilisée précédemment pour créer un script pour le fichier de production. Nommez le fichier de script *ASPNET-Data-prod. SQL*.

## <a name="summary"></a>Récapitulatif

Les deux bases de données sont maintenant prêtes à être déployées et vous avez deux scripts de déploiement de données dans votre dossier de solution.

![Scripts de déploiement de données](preparing-databases/_static/image17.png)

Dans le didacticiel suivant, vous configurez les paramètres de projet qui affectent le déploiement et vous configurez des transformations de fichier *Web. config* automatiques pour les paramètres qui doivent être différents dans l’application déployée.

## <a name="more-information"></a>Plus d'informations

Pour plus d’informations sur NuGet, consultez [gérer les bibliothèques de projets avec NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) et [la documentation NuGet](http://docs.nuget.org/docs/start-here/overview). Si vous ne souhaitez pas utiliser NuGet, vous devez apprendre à analyser un package NuGet pour déterminer ce qu’il fait lorsqu’il est installé. (Par exemple, il peut configurer des transformations *Web. config* , configurer des scripts PowerShell pour qu’ils s’exécutent au moment de la génération, etc.) Pour en savoir plus sur le fonctionnement de NuGet, consultez [création et publication d’un package et d’un](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) [fichier de configuration et de transformations de code source](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Précédent](introduction.md)
> [Suivant](web-config-transformations.md)
