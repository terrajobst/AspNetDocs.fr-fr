---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Didacticiel : utiliser des migrations EF dans une application ASP.NET MVC et la déployer sur Azure'
author: tdykstra
description: Dans ce didacticiel, vous allez activer Code First migrations et déployer l’application dans le Cloud dans Azure.
ms.author: riande
ms.date: 01/16/2019
ms.topic: tutorial
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 989dd0f0e18b338be057b9c5657586eff996d8ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616080"
---
# <a name="tutorial-use-ef-migrations-in-an-aspnet-mvc-app-and-deploy-to-azure"></a>Didacticiel : utiliser des migrations EF dans une application ASP.NET MVC et la déployer sur Azure

Jusqu’à présent, l’exemple d’application Web Contoso University s’exécutait localement dans IIS Express sur votre ordinateur de développement. Pour mettre une application réelle à la disposition d’autres personnes à utiliser sur Internet, vous devez la déployer sur un fournisseur d’hébergement Web. Dans ce didacticiel, vous allez activer Code First migrations et déployer l’application dans le Cloud dans Azure :

- Activez Migrations Code First. La fonctionnalité migrations vous permet de modifier le modèle de données et de déployer vos modifications en production en mettant à jour le schéma de base de données sans avoir à supprimer et recréer la base de données.
- Déployez sur Azure. Cette étape est facultative. vous pouvez continuer avec les autres didacticiels sans avoir déployé le projet.

Nous vous recommandons d’utiliser un processus d’intégration continue avec le contrôle de code source pour le déploiement, mais ce didacticiel ne couvre pas ces sujets. Pour plus d’informations, consultez les chapitres [contrôle de code source](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) et [intégration continue](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) de la [création d’applications Cloud réalistes avec Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Activer les migrations de Code First
> * Déployer l’application dans Azure (facultatif)

## <a name="prerequisites"></a>Conditions préalables requises

- [Résilience des connexions et interception des commandes](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-code-first-migrations"></a>Activer les migrations de Code First

Quand vous développez une nouvelle application, votre modèle de données change fréquemment et, chaque fois que le modèle change, il n’est plus en synchronisation avec la base de données. Vous avez configuré la Entity Framework pour supprimer et recréer automatiquement la base de données chaque fois que vous modifiez le modèle de données. Lorsque vous ajoutez, supprimez ou modifiez des classes d’entité ou modifiez votre classe de `DbContext`, la prochaine fois que vous exécutez l’application, elle supprime automatiquement votre base de données existante, en crée une nouvelle qui correspond au modèle et l’amorce avec des données de test.

Cette méthode pour conserver la base de données en synchronisation avec le modèle de données fonctionne bien jusqu’au déploiement de l’application en production. Lorsque l’application s’exécute en production, elle stocke généralement les données que vous souhaitez conserver et vous ne souhaitez pas perdre tout chaque fois que vous apportez une modification, telle que l’ajout d’une nouvelle colonne. La fonctionnalité [migrations code First](https://msdn.microsoft.com/data/jj591621) résout ce problème en permettant à code First de mettre à jour le schéma de base de données au lieu de supprimer et de recréer la base de données. Dans ce didacticiel, vous allez déployer l’application et vous préparer à l’activation des migrations.

1. Désactivez l’initialiseur que vous avez configuré précédemment en commentant ou en supprimant l’élément `contexts` que vous avez ajouté au fichier Web. config de l’application.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Également dans le fichier *Web. config* de l’application, remplacez le nom de la base de données dans la chaîne de connexion par par contosouniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Cette modification configure le projet de sorte que la première migration crée une nouvelle base de données. Cela n’est pas obligatoire, mais vous verrez plus tard pourquoi c’est une bonne idée.
3. Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du gestionnaire de package**.

1. À l’invite `PM>`, entrez les commandes suivantes :

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    La commande `enable-migrations` crée un dossier *migrations* dans le projet ContosoUniversity et place dans ce dossier un fichier *Configuration.cs* que vous pouvez modifier pour configurer des migrations.

    (Si vous avez manqué l’étape ci-dessus qui vous indique de modifier le nom de la base de données, les migrations trouveront la base de données existante et exécuteront automatiquement la commande `add-migration`. C’est parfait, mais cela signifie simplement que vous n’allez pas exécuter un test du code des migrations avant de déployer la base de données. Plus tard, lorsque vous exécuterez la commande `update-database` rien ne se produira, car la base de données existe déjà.)

    Ouvrez le fichier *ContosoUniversity\Migrations\Configuration.cs* . À l’instar de la classe d’initialiseur que vous avez vu précédemment, la classe `Configuration` comprend une méthode `Seed`.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    L’objectif de la méthode [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) est de vous permettre d’insérer ou de mettre à jour les données de test après code First crée ou met à jour la base de données. La méthode est appelée lorsque la base de données est créée et chaque fois que le schéma de base de données est mis à jour après la modification d’un modèle de données.

### <a name="set-up-the-seed-method"></a>Configurer la méthode Seed

Lorsque vous supprimez et recréez la base de données pour chaque modification du modèle de données, vous utilisez la méthode `Seed` de la classe initializer pour insérer les données de test, car après chaque modification de modèle, la base de données est supprimée et toutes les données de test sont perdues. Avec Migrations Code First, les données de test sont conservées après la modification de la base de données, de sorte que l’inclusion de données de test dans la méthode [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) n’est généralement pas nécessaire. En fait, vous ne souhaitez pas que la méthode `Seed` insère des données de test si vous utilisez des migrations pour déployer la base de données en production, car la méthode `Seed` s’exécute en production. Dans ce cas, vous souhaitez que la méthode `Seed` insère dans la base de données uniquement les données dont vous avez besoin en production. Par exemple, vous souhaiterez peut-être que la base de données inclue les noms de service réels dans la table `Department` lorsque l’application devient disponible en production.

Pour ce didacticiel, vous allez utiliser des migrations pour le déploiement, mais votre méthode de `Seed` insère les données de test quand même afin de faciliter la visibilité du fonctionnement des fonctionnalités de l’application sans avoir à insérer manuellement de nombreuses données.

1. Remplacez le contenu du fichier *Configuration.cs* par le code suivant, qui charge les données de test dans la nouvelle base de données.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    La méthode [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) prend l’objet de contexte de base de données en tant que paramètre d’entrée, et le code de la méthode utilise cet objet pour ajouter de nouvelles entités à la base de données. Pour chaque type d’entité, le code crée une collection de nouvelles entités, les ajoute à la propriété [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) appropriée, puis enregistre les modifications dans la base de données. Il n’est pas nécessaire d’appeler la méthode [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) après chaque groupe d’entités, comme cela est fait ici, mais cela vous permet de localiser la source d’un problème si une exception se produit pendant que le code écrit dans la base de données.

    Certaines des instructions qui insèrent des données utilisent la méthode [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) pour effectuer une opération « upsert ». Étant donné que la méthode `Seed` s’exécute chaque fois que vous exécutez la commande `update-database`, généralement après chaque migration, vous ne pouvez pas simplement insérer des données, car les lignes que vous essayez d’ajouter sont déjà présentes après la première migration qui a créé la base de données. L’opération « upsert » empêche les erreurs qui se produisent si vous essayez d’insérer une ligne qui existe déjà, mais elle ***remplace*** toutes les modifications apportées aux données que vous avez pu effectuer lors du test de l’application. Avec les données de test dans certaines tables, vous pouvez ne pas souhaiter que cela se produise : dans certains cas, lorsque vous modifiez des données pendant un test, vous souhaitez conserver les modifications après les mises à jour de la base de données. Dans ce cas, vous devez effectuer une opération d’insertion conditionnelle : Insérez une ligne uniquement si elle n’existe pas déjà. La méthode Seed utilise les deux approches.

    Le premier paramètre passé à la méthode [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) spécifie la propriété à utiliser pour vérifier si une ligne existe déjà. Pour les données d’étudiant de test que vous fournissez, la propriété `LastName` peut être utilisée à cet effet puisque chaque nom de famille de la liste est unique :

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Ce code suppose que les noms sont uniques. Si vous ajoutez manuellement un étudiant avec un nom en double, vous obtenez l’exception suivante la prochaine fois que vous effectuez une migration :

    **La séquence contient plusieurs éléments**

    Pour plus d’informations sur la gestion des données redondantes, telles que les deux étudiants nommés « Alexander Carson », consultez la page relative aux bases de données d' [amorçage et de débogage Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) sur le blog de Rick Anderson. Pour plus d’informations sur la méthode `AddOrUpdate`, consultez se [préoccuper de la méthode EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sur le blog de Julie Lerman.

    Le code qui crée `Enrollment` entités suppose que vous disposez de la valeur `ID` dans les entités de la collection `students`, même si vous n’avez pas défini cette propriété dans le code qui crée la collection.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Vous pouvez utiliser la propriété `ID` ici, car la valeur `ID` est définie lorsque vous appelez `SaveChanges` pour la collection `students`. EF obtient automatiquement la valeur de clé primaire lors de l’insertion d’une entité dans la base de données, et met à jour la propriété `ID` de l’entité en mémoire.

    Le code qui ajoute chaque `Enrollment` entité au jeu d’entités `Enrollments` n’utilise pas la méthode `AddOrUpdate`. Il vérifie si une entité existe déjà et insère l’entité si elle n’existe pas. Cette approche permet de conserver les modifications apportées à un niveau d’inscription à l’aide de l’interface utilisateur de l’application. Le code parcourt chaque membre de la [liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) `Enrollment`et si l’inscription est introuvable dans la base de données, il ajoute l’inscription à la base de données. La première fois que vous mettez à jour la base de données, la base de données est vide. par conséquent, elle ajoute chaque inscription.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. Générez le projet.

### <a name="execute-the-first-migration"></a>Exécuter la première migration

Lorsque vous avez exécuté la commande `add-migration`, les migrations ont généré le code qui créerait la base de données à partir de zéro. Ce code se trouve également dans le dossier *migrations* , dans le fichier nommé *&lt;timestamp&gt;\_InitialCreate.cs*. La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités du modèle de données, et la méthode `Down` les supprime.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Les migrations appellent la méthode `Up` pour implémenter les modifications du modèle de données pour une migration. Quand vous entrez une commande pour annuler la mise à jour, Migrations appelle la méthode `Down`.

Il s’agit de la migration initiale qui a été créée lorsque vous avez entré la commande `add-migration InitialCreate`. Le paramètre (`InitialCreate` dans l’exemple) est utilisé pour le nom de fichier et peut être ce que vous souhaitez. en général, vous choisissez un mot ou une expression qui résume ce qui est effectué dans la migration. Par exemple, vous pouvez nommer une migration ultérieure &quot;Ajoutertabledépartement&quot;.

Si vous avez créé la migration initiale alors que la base de données existait déjà, le code de création de la base de données est généré, mais il n’est pas nécessaire de l’exécuter, car la base de données correspond déjà au modèle de données. Quand vous déployez l’application sur un autre environnement où la base de données n’existe pas encore, ce code est exécuté pour créer votre base de données : il est donc judicieux de le tester au préalable. C’est la raison pour laquelle vous avez modifié le nom de la base de données dans la chaîne de connexion précédemment&mdash;pour que les migrations puissent en créer une nouvelle à partir de zéro.

1. Dans la fenêtre **Console du Gestionnaire de package** , entrez la commande suivante :

    `update-database`

    La commande `update-database` exécute la méthode `Up` pour créer la base de données, puis exécute la méthode `Seed` pour remplir la base de données. Le même processus s’exécutera automatiquement en production après le déploiement de l’application, comme vous le verrez dans la section suivante.
2. Utilisez **Explorateur de serveurs** pour inspecter la base de données comme vous l’avez fait dans le premier didacticiel et exécutez l’application pour vérifier que tout fonctionne toujours de la même façon qu’auparavant.

## <a name="deploy-to-azure"></a>Déployer sur Azure

Jusqu’à présent, l’application s’exécutait localement dans IIS Express sur votre ordinateur de développement. Pour qu’il soit disponible pour d’autres personnes à utiliser sur Internet, vous devez le déployer sur un fournisseur d’hébergement Web. Dans cette section du didacticiel, vous allez le déployer dans Azure. Cette section est facultative. vous pouvez ignorer ce didacticiel et poursuivre le didacticiel suivant, ou vous pouvez adapter les instructions de cette section pour un autre fournisseur d’hébergement de votre choix.

### <a name="use-code-first-migrations-to-deploy-the-database"></a>Utiliser Code First migrations pour déployer la base de données

Pour déployer la base de données, vous utiliserez Migrations Code First. Lorsque vous créez le profil de publication que vous utilisez pour configurer les paramètres de déploiement à partir de Visual Studio, vous activez une case à cocher intitulée **mettre à jour la base de données**. Ce paramètre fait que le processus de déploiement configure automatiquement le fichier *Web. config* de l’application sur le serveur de destination afin que code First utilise la classe d’initialiseur `MigrateDatabaseToLatestVersion`.

Visual Studio ne fait rien avec la base de données pendant le processus de déploiement pendant qu’il copie votre projet sur le serveur de destination. Lorsque vous exécutez l’application déployée et qu’elle accède à la base de données pour la première fois après le déploiement, Code First vérifie si la base de données correspond au modèle de données. En cas d’incompatibilité, Code First crée automatiquement la base de données (si elle n’existe pas déjà) ou met à jour le schéma de base de données vers la dernière version (si une base de données existe mais ne correspond pas au modèle). Si l’application implémente une méthode de migration `Seed`, la méthode s’exécute après la création de la base de données ou lors de la mise à jour du schéma.

Votre méthode de migration `Seed` insère les données de test. Si vous déployez dans un environnement de production, vous devez modifier la méthode `Seed` afin qu’elle insère uniquement les données que vous souhaitez insérer dans votre base de données de production. Par exemple, dans votre modèle de données actuel, vous souhaiterez peut-être avoir des cours réels, mais des étudiants fictifs dans la base de données de développement. Vous pouvez écrire une méthode de `Seed` pour charger les deux en développement, puis commenter les étudiants fictifs avant de procéder au déploiement en production. Ou vous pouvez écrire une méthode de `Seed` pour charger uniquement les cours, puis entrer manuellement les étudiants fictifs dans la base de données de test à l’aide de l’interface utilisateur de l’application.

### <a name="get-an-azure-account"></a>Procurez-vous un compte Azure

Vous aurez besoin d’un compte Azure. Si vous n’en avez pas déjà un, mais que vous disposez d’un abonnement Visual Studio, vous pouvez [activer les avantages de votre abonnement](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
). Sinon, vous pouvez créer un compte d’évaluation gratuit en quelques minutes. Pour plus d’informations, consultez [Essai gratuit Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Créer un site Web et une base de données SQL dans Azure

Votre application Web dans Azure s’exécute dans un environnement d’hébergement partagé, ce qui signifie qu’elle s’exécute sur des machines virtuelles partagées avec d’autres clients Azure. Un environnement d'hébergement partagé permet de commencer à utiliser le cloud à moindre frais. Plus tard, si votre trafic Web augmente, vous pourrez mettre votre application à l'échelle pour répondre à vos besoins, en exécutant des machines virtuelles dédiées. Pour en savoir plus sur les options de tarification pour Azure App Service, consultez la [tarification App service](https://azure.microsoft.com/pricing/details/app-service/).

Vous allez déployer la base de données sur la base de données SQL Azure. SQL Database est un service de base de données relationnelle basé sur le Cloud qui repose sur les technologies SQL Server. Les outils et les applications qui fonctionnent avec SQL Server fonctionnent également avec la base de données SQL.

1. Dans le [portail de gestion Azure](https://portal.azure.com), choisissez **créer une ressource** dans l’onglet de gauche, puis choisissez **Afficher tout** dans le **nouveau** volet ( *ou panneau*) pour afficher toutes les ressources disponibles. Choisissez **application Web + SQL** dans la section **Web** du panneau **tout** . Enfin, choisissez **créer**.

    ![Créer une ressource dans le portail Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   Le formulaire de création d’une nouvelle ressource **application Web + SQL** s’ouvre.

2. Entrez une chaîne dans la zone nom de l' **application** à utiliser comme URL unique pour votre application. L’URL complète sera composée de ce que vous entrez ici et du domaine par défaut des services de Azure App (. azurewebsites.net). Si le **nom** de l’application est déjà utilisé, l’Assistant vous avertit par un message indiquant que *le nom de l’application n’est pas disponible* . Si le **nom** de l’application est disponible, une coche verte s’affiche.

3. Dans la zone **abonnement** , choisissez l’abonnement Azure dans lequel vous souhaitez que les **app service** résident.

4. Dans la zone de texte **groupe de ressources** , choisissez un groupe de ressources ou créez-en un. Ce paramètre spécifie le centre de données dans lequel votre site Web s’exécutera. Pour plus d’informations sur les groupes de ressources, consultez [groupes de ressources](/azure/azure-resource-manager/resource-group-overview#resource-groups).

5. Créez un **plan de App service** en cliquant sur la *section App service*, sur **créer**, puis renseignez **app service plan** (il peut s’agir du même nom que app service), de l' **emplacement**et du **niveau tarifaire** (il existe une option gratuite).

6. Cliquez sur **SQL Database**, puis choisissez **créer une nouvelle base de données** ou sélectionnez une base de données existante.

7. Dans la zone **nom** , entrez un nom pour votre base de données.
8. Cliquez sur la zone **serveur cible** , puis sélectionnez **créer un nouveau serveur**. Si vous avez déjà créé un serveur, vous pouvez également sélectionner ce serveur dans la liste des serveurs disponibles.
9. Sélectionnez la section **niveau tarifaire** , puis cliquez sur *gratuit*. Si des ressources supplémentaires sont nécessaires, la base de données peut être mise à l’échelle à tout moment. Pour en savoir plus sur la tarification Azure SQL, consultez la page [tarification de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).
10. Modifiez le [classement](/sql/relational-databases/collations/collation-and-unicode-support) selon vos besoins.
11. Entrez un **nom d’utilisateur admin SQL** administrateur et un **mot de passe**d’administrateur SQL.

    - Si vous avez sélectionné **nouveau SQL Database serveur**, définissez un nouveau nom et mot de passe que vous utiliserez ultérieurement lorsque vous accéderez à la base de données.
    - Si vous avez sélectionné un serveur que vous avez créé précédemment, entrez les informations d’identification de ce serveur.

12. La collecte de données de télémétrie peut être activée pour App Service à l’aide de Application Insights. Avec peu de configuration, Application Insights collecte des informations précieuses sur les événements, les exceptions, les dépendances, les demandes et les traces. Pour en savoir plus sur les Application Insights, consultez [Azure Monitor](https://azure.microsoft.com/services/monitor/).
13. Cliquez sur **créer** en bas pour indiquer que vous avez terminé.

    Le Portail de gestion retourne à la page Tableau de bord, et la zone **notifications** en haut de la page indique que le site est en cours de création. Après un certain temps (généralement inférieur à une minute), une notification indique que le déploiement a réussi. Dans la barre de navigation à gauche, le nouveau App Service s’affiche dans la section **app services** et la nouvelle base de données SQL s’affiche dans la section **bases de données SQL** .

### <a name="deploy-the-app-to-azure"></a>Déployer l’application sur Azure

1. Dans **l’Explorateur de solutions** de Visual Studio, cliquez avec le bouton droit sur le projet, puis dans le menu contextuel, sélectionnez **Publier**.

2. Sur la page **choisir une cible de publication** , choisissez **app service** puis **Sélectionnez existant**, puis **publier**.

    ![Page choisir une cible de publication](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. Si vous n’avez pas encore ajouté votre abonnement Azure dans Visual Studio, effectuez les étapes à l’écran. Ces étapes permettent à Visual Studio de se connecter à votre abonnement Azure pour que la liste des **app services** inclue votre site Web.

4. Sur la page **app service** , sélectionnez l' **abonnement** auquel vous avez ajouté le App service. Sous **affichage**, sélectionnez **groupe de ressources**. Développez le groupe de ressources auquel vous avez ajouté le App Service, puis sélectionnez le App Service. Choisissez **OK** pour publier l’application.

5. La fenêtre **Output** indique les actions de déploiement entreprises et signale la réussite du déploiement.

6. Une fois le déploiement réussi, le navigateur par défaut s’ouvre automatiquement à l’URL du site Web déployé.

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    Votre application s’exécute maintenant dans le Cloud.

À ce stade, la base de données *SchoolContext* a été créée dans la base de données SQL Azure, car vous avez sélectionné **exécuter migrations code First (s’exécute au démarrage de l’application)** . Le fichier *Web. config* du site Web déployé a été modifié afin que l’initialiseur [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) s’exécute la première fois que votre code lit ou écrit des données dans la base de données (qui s’est produite lorsque vous avez sélectionné l’onglet **students** ) :

![Extrait de fichier Web. config](https://asp.net/media/4367421/mig.png)

Le processus de déploiement a également créé une nouvelle chaîne de connexion *(SchoolContext\_DatabasePublish*) pour migrations code First à utiliser pour la mise à jour du schéma de base de données et l’amorçage de la base de données.

![Chaîne de connexion dans le fichier Web. config](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Vous pouvez trouver la version déployée du fichier Web. config sur votre propre ordinateur dans *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Vous pouvez accéder au fichier *Web. config* déployé lui-même à l’aide de FTP. Pour obtenir des instructions, consultez [déploiement Web ASP.net à l’aide de Visual Studio : déploiement d’une mise à jour de code](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Suivez les instructions qui commencent par « pour utiliser un outil FTP, vous avez besoin de trois choses : l’URL FTP, le nom d’utilisateur et le mot de passe ».

> [!NOTE]
> L’application Web n’implémente pas la sécurité, de sorte que toute personne qui trouve l’URL peut modifier les données. Pour obtenir des instructions sur la façon de sécuriser le site Web, consultez [déploiement d’une application ASP.NET MVC sécurisée avec appartenance, OAuth et SQL Database sur Azure](/aspnet/core/security/authorization/secure-data). Vous pouvez empêcher d’autres personnes d’utiliser le site en arrêtant le service à l’aide d’Azure Portail de gestion ou **Explorateur de serveurs** dans Visual Studio.

![Arrêter l’élément de menu App service](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>Scénarios de migration avancée

Si vous déployez une base de données en exécutant automatiquement des migrations, comme indiqué dans ce didacticiel, et que vous déployez sur un site Web qui s’exécute sur plusieurs serveurs, vous pouvez obtenir plusieurs serveurs tentant d’exécuter des migrations en même temps. Les migrations sont atomiques. par conséquent, si deux serveurs essaient d’exécuter la même migration, l’un d’eux réussira et l’autre échouera (en supposant que les opérations ne peuvent pas être effectuées deux fois). Dans ce scénario, si vous souhaitez éviter ces problèmes, vous pouvez appeler des migrations manuellement et configurer votre propre code afin qu’il ne se produise qu’une seule fois. Pour plus d’informations, consultez [exécution et scripts de migration à partir du code](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) sur le blog de Rowan Miller et [Migrate. exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) (pour exécuter des migrations à partir de la ligne de commande).

Pour plus d’informations sur les autres scénarios de migration, consultez [série de captures vidéo sur les migrations](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="update-specific-migration"></a>Mettre à jour une migration spécifique

`update-database -target MigrationName`

La commande `update-database -target MigrationName` exécute la migration ciblée.

## <a name="ignore-migration-changes-to-database"></a>Ignorer les modifications de migration dans la base de données

`Add-migration MigrationName -ignoreChanges`

`ignoreChanges` crée une migration vide avec le modèle actuel sous la forme d’un instantané.

## <a name="code-first-initializers"></a>Initialiseurs Code First

Dans la section déploiement, vous avez vu l’initialiseur [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) utilisé. Code First fournit également d’autres initialiseurs, y compris [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (valeur par défaut), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (que vous avez utilisé précédemment) et [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). L’initialiseur de `DropCreateAlways` peut être utile pour configurer des conditions pour les tests unitaires. Vous pouvez également écrire vos propres initialiseurs, et vous pouvez appeler un initialiseur de manière explicite si vous ne souhaitez pas attendre la lecture ou l’écriture de l’application dans la base de données.

Pour plus d’informations sur les initialiseurs, consultez [Présentation des initialiseurs de base de données dans Entity Framework code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) et le chapitre 6 du livre de [programmation Entity Framework : code First](http://shop.oreilly.com/product/0636920022220.do) de Julie Lerman et Rowan Miller.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Des liens vers d’autres ressources de Entity Framework sont disponibles dans [accès aux données ASP.net-ressources recommandées](xref:whitepapers/aspnet-data-access-content-map).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Migrations de Code First activées
> * Déploiement de l’application dans Azure (facultatif)

Passez à l’article suivant pour apprendre à créer un modèle de données plus complexe pour une application ASP.NET MVC.
> [!div class="nextstepaction"]
> [Créer un modèle de données plus complexe](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
