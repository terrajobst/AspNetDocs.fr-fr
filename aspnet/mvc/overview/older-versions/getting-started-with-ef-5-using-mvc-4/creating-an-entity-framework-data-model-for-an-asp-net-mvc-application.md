---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Création d’un modèle de données Entity Framework pour une application ASP.NET MVC (1 sur 10) | Microsoft Docs
author: tdykstra
description: Une version plus récente de cette série de didacticiels est disponible pour Visual Studio 2013, Entity Framework 6 et MVC 5. Exemple d’application Web Contoso University de...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8ee5aa22b6b2329b01d41437f30508e28a2288b2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595972"
---
# <a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Création d’un modèle de données Entity Framework pour une application ASP.NET MVC (1 sur 10)

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Une [version plus récente de cette série de didacticiels](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) est disponible pour Visual Studio 2013, Entity Framework 6 et MVC 5.
> 
> 
> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET MVC 4 à l’aide de la Entity Framework 5 et de Visual Studio 2012. L’exemple d’application est un site web pour une université Contoso fictive. Il comprend des fonctionnalités telles que l’admission des étudiants, la création des cours et les affectations des formateurs. Cette série de didacticiels explique comment créer l’exemple d’application Contoso University. Vous pouvez [Télécharger l’application terminée](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Il existe trois façons de travailler avec des données dans le Entity Framework : *Database First*, *Model First*et *Code First*. Ce didacticiel est destiné à Code First. Pour plus d’informations sur les différences entre ces flux de travail et des conseils sur la façon de choisir le meilleur pour votre scénario, consultez [Entity Framework des flux de travail de développement](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> L’exemple d’application repose sur [ASP.NET MVC](../../../index.md). Si vous préférez utiliser le modèle ASP.NET Web Forms, consultez la série de didacticiels [et de Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) et le [plan de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Versions de logiciels
> 
> | **Indiqué dans le didacticiel** | **Fonctionne également avec** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express pour le Web. Il est installé automatiquement par le kit de développement logiciel (SDK) Windows Azure si vous n’avez pas encore VS 2012 ou VS 2012 Express pour le Web. Visual Studio 2013 doit fonctionner, mais le didacticiel n’a pas été testé et certaines sélections de menu et boîtes de dialogue sont différentes. La [version Visual studio 2013 du kit de développement logiciel (SDK) Windows Azure](https://go.microsoft.com/fwlink/p/?linkid=323510) est requise pour le déploiement de Windows Azure. |
> | .NET 4.5 | La plupart des fonctionnalités présentées fonctionnent dans .NET 4, mais d’autres ne le feront pas. Par exemple, la prise en charge d’enum dans EF requiert .NET 4,5. |
> | Entity Framework 5 |  |
> | [Kit de développement logiciel (SDK) Windows Azure 2,1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Si vous ignorez les étapes de déploiement de Windows Azure, vous n’avez pas besoin du kit de développement logiciel (SDK). Quand une nouvelle version du kit de développement logiciel (SDK) est publiée, le lien installe la version la plus récente. Dans ce cas, vous devrez peut-être adapter certaines instructions aux nouvelles fonctionnalités et à l’interface utilisateur. |
> 
> ## <a name="questions"></a>Questions
> 
> Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), sur le [forum Entity Framework et LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), ou sur [StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Remerciements
> 
> Consultez le dernier didacticiel de la série pour les [accusés de réception et une remarque sur VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Version d’origine du didacticiel
> 
> La version d’origine du didacticiel est disponible dans le [livre électronique EF 4,1/MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).

## <a name="the-contoso-university-web-application"></a>L’application Web Contoso University

L’application que vous allez générer dans ces didacticiels est un site web simple d’université.

Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs. Voici quelques écrans que vous allez créer.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Le style d’interface utilisateur de ce site a été maintenu proche de ce qui est généré par les modèles intégrés, afin que le didacticiel puisse principalement se concentrer sur la façon d’utiliser Entity Framework.

## <a name="prerequisites"></a>Configuration requise

Les directions et captures d’écran de ce didacticiel partent du principe que vous utilisez [Visual studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) ou [Visual Studio 2012 Express pour le Web](https://go.microsoft.com/fwlink/?LinkID=275131), avec la dernière mise à jour et le kit de développement logiciel (SDK) Azure pour .net installés depuis juillet, 2013. Pour ce faire, vous pouvez utiliser le lien suivant :

[Kit de développement logiciel (SDK) Azure pour .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Si vous avez installé Visual Studio, le lien ci-dessus installe tous les composants manquants. Si vous n’avez pas Visual Studio, le lien installe Visual Studio 2012 Express pour le Web. Vous pouvez utiliser Visual Studio 2013, mais certaines des procédures et écrans requis diffèrent.

## <a name="create-an-mvc-web-application"></a>Créer une application Web MVC

Ouvrez Visual Studio et créez un nouveau C# projet nommé « ContosoUniversity » à l’aide du modèle d' **application Web ASP.NET MVC 4** . Assurez-vous que vous ciblez **.NET Framework 4,5** (vous utiliserez les [Propriétés de`enum`](https://msdn.microsoft.com/data/hh859576.aspx), et cela nécessite .net 4,5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle **application Internet** .

Laissez le moteur d’affichage **Razor** sélectionné et laissez la case à cocher **créer un projet de test unitaire** désactivée.

Cliquez sur **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Configurer le style de site

Quelques changements simples configureront le menu, la disposition et la page d’accueil du site.

Ouvrez *Views\Shared\\_Layout. cshtml*et remplacez le contenu du fichier par le code suivant. Les modifications sont mises en surbrillance.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Ce code apporte les modifications suivantes :

- Remplace les instances de modèle de « My ASP.NET MVC application » et « Your logo here » par « Contoso University ».
- Ajoute plusieurs liens d’action qui seront utilisés ultérieurement dans le didacticiel.

Dans *Views\Home\Index.cshtml*, remplacez le contenu du fichier par le code suivant pour éliminer les paragraphes de modèle concernant ASP.net et MVC :

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Dans *Controllers\HomeController.cs*, remplacez la valeur de `ViewBag.Message` dans la méthode d’action `Index` par « Welcome to Contoso University ! », comme illustré dans l’exemple suivant :

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Appuyez sur CTRL + F5 pour exécuter le site. La page d’accueil s’affiche avec le menu principal.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Créer le modèle de données

Ensuite, vous allez créer des classes d’entités pour l’application Contoso University. Vous allez commencer par les trois entités suivantes :

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Il existe une relation un-à-plusieurs entre les entités `Student` et `Enrollment`, et une relation un-à-plusieurs entre les entités `Course` et `Enrollment`. En d’autres termes, un étudiant peut être inscrit dans un nombre quelconque de cours et un cours peut avoir un nombre quelconque d’élèves inscrits.

Dans les sections suivantes, vous allez créer une classe pour chacune de ces entités.

> [!NOTE]
> Si vous essayez de compiler le projet avant de terminer la création de toutes ces classes d’entité, vous obtiendrez des erreurs de compilation.

### <a name="the-student-entity"></a>Entité Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Dans le dossier *Models* , créez *Student.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

La propriété `StudentID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe. Par défaut, le Entity Framework interprète une propriété nommée `ID` ou *classname* `ID` comme clé primaire.

La propriété `Enrollments` est une *propriété de navigation*. Les propriétés de navigation contiennent d’autres entités qui sont associées à cette entité. Dans ce cas, la propriété `Enrollments` d’une entité `Student` contiendra toutes les entités `Enrollment` associées à cette `Student` entité. En d’autres termes, si une ligne de `Student` donnée dans la base de données a deux lignes `Enrollment` associées (lignes qui contiennent la valeur de clé primaire de cet étudiant dans leur colonne de clé étrangère `StudentID`), cette propriété de navigation `Enrollments` de l’entité `Student` contient ces deux entités `Enrollment`.

Les propriétés de navigation sont généralement définies comme `virtual` afin qu’elles puissent tirer parti de certaines fonctionnalités Entity Framework telles que le *chargement différé*. (Le chargement différé sera expliqué ultérieurement, dans le didacticiel sur la [lecture des données associées](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) plus loin dans cette série.

Si une propriété de navigation peut contenir plusieurs entités (comme dans des relations plusieurs à plusieurs ou un -à-plusieurs), son type doit être une liste dans laquelle les entrées peuvent être ajoutées, supprimées et mises à jour, telle que `ICollection`.

### <a name="the-enrollment-entity"></a>L’entité d’inscription

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Dans le dossier *Models*, créez *Enrollment.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La propriété de la classe est une [énumération](https://msdn.microsoft.com/data/hh859576.aspx). La présence du point d’interrogation après la déclaration de type `Grade` indique que la propriété `Grade` [accepte les valeurs Null](https://msdn.microsoft.com/library/2cf62fcy.aspx). Un niveau null est différent d’un niveau zéro (null signifie qu’un niveau n’est pas connu ou n’a pas encore été affecté).

La propriété `StudentID` est une clé étrangère, et la propriété de navigation correspondante est `Student`. Une entité `Enrollment` est associée à une entité `Student`, donc la propriété peut contenir uniquement une entité `Student` unique (contrairement à la propriété de navigation `Student.Enrollments` que vous avez vue précédemment, qui peut contenir plusieurs entités `Enrollment`).

La propriété `CourseID` est une clé étrangère, et la propriété de navigation correspondante est `Course`. Une entité `Enrollment` est associée à une entité `Course`.

### <a name="the-course-entity"></a>Entité Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Dans le dossier *Models* , créez *course.cs*, en remplaçant le code existant par le code suivant :

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

La propriété `Enrollments` est une propriété de navigation. Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.

Nous en indiquons davantage sur le [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Aucun)]» dans le didacticiel suivant. En fait, cet attribut vous permet d’entrer la clé primaire pour le cours plutôt que de laisser la base de données la générer.

## <a name="create-the-database-context"></a>Créer le contexte de base de données

La classe principale qui coordonne Entity Framework fonctionnalité pour un modèle de données donné est la classe *de contexte de base de données* . Vous créez cette classe en dérivant de la classe [System. Data. Entity. DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) . Dans votre code, vous spécifiez les entités qui sont incluses dans le modèle de données. Vous pouvez également personnaliser un certain comportement d’Entity Framework. Dans ce projet, la classe se nomme `SchoolContext`.

Créez un dossier nommé *dal* (pour la couche d’accès aux données). Dans ce dossier, créez un nouveau fichier de classe nommé *SchoolContext.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Ce code crée une propriété [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) pour chaque jeu d’entités. Dans Entity Framework terminologie, un *jeu d’entités* correspond généralement à une table de base de données, et une *entité* correspond à une ligne de la table.

L’instruction `modelBuilder.Conventions.Remove` dans la méthode [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) empêche la pluralisation des noms de table. Si vous ne l’avez pas fait, les tables générées sont nommées `Students`, `Courses`et `Enrollments`. Au lieu de cela, les noms de table sont `Student`, `Course`et `Enrollment`. Les développeurs ne sont pas tous d’accord sur la nécessité d’utiliser des noms de table au pluriel. Ce didacticiel utilise la forme singulière, mais le point important est que vous pouvez sélectionner le formulaire que vous préférez en incluant ou en omettant cette ligne de code.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La base de données [locale est une](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) version allégée de l’SQL Server Express moteur de base de données qui démarre à la demande et s’exécute en mode utilisateur. La base de données locale s’exécute dans un mode d’exécution spécial de SQL Server Express qui vous permet d’utiliser des bases de données en tant que fichiers *. mdf* . En règle générale, les fichiers de base de données de base de données locale sont conservés dans le dossier *application\_Data* d’un projet Web. La fonctionnalité d’instance utilisateur de SQL Server Express vous permet également d’utiliser des fichiers *. mdf* , mais la fonctionnalité d’instance utilisateur est déconseillée. par conséquent, la base de données locale est recommandée pour travailler avec des fichiers *. mdf* .

En général SQL Server Express n’est pas utilisé pour les applications Web de production. En particulier, la base de données locale n’est pas recommandée pour une utilisation en production avec une application Web, car elle n’est pas conçue pour fonctionner avec IIS.

Dans Visual Studio 2012 et versions ultérieures, la base de données locale est installée par défaut dans Visual Studio. Dans Visual Studio 2010 et les versions antérieures, SQL Server Express (sans base de données locale) est installé par défaut dans Visual Studio. vous devez l’installer manuellement si vous utilisez Visual Studio 2010.

Dans ce didacticiel, vous allez utiliser la base de données locale afin que la base de données puisse être stockée dans le dossier de données de l' *application\_* en tant que fichier *. mdf* . Ouvrez le fichier *Web. config* racine et ajoutez une nouvelle chaîne de connexion à la collection `connectionStrings`, comme indiqué dans l’exemple suivant. (Veillez à mettre à jour le fichier *Web. config* dans le dossier racine du projet. Il y a également un fichier *Web. config* qui se trouve dans le sous-dossier *views* que vous n’avez pas besoin de mettre à jour.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Par défaut, le Entity Framework recherche une chaîne de connexion nommée de la même façon que la classe `DbContext` (`SchoolContext` pour ce projet). La chaîne de connexion que vous avez ajoutée spécifie une base de données de base de données locale nommée *ContosoUniversity. mdf* située dans le dossier de données de l' *application\_* . Pour plus d’informations, consultez [SQL Server des chaînes de connexion pour les applications Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx).

Vous n’avez pas besoin de spécifier la chaîne de connexion. Si vous ne fournissez pas de chaîne de connexion, Entity Framework en créera une pour vous ; Toutefois, il se peut que la base de données ne se trouve pas dans le dossier de données de l' *application\_* de votre application. Pour plus d’informations sur l’emplacement où la base de données sera créée, consultez [Code First à une nouvelle base de données](https://msdn.microsoft.com/data/jj193542).

La collection `connectionStrings` possède également une chaîne de connexion nommée `DefaultConnection` qui est utilisée pour la base de données d’appartenance. Vous n’utiliserez pas la base de données d’appartenance dans ce didacticiel. La seule différence entre les deux chaînes de connexion est le nom de la base de données et la valeur de l’attribut Name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Configurer et exécuter une migration Code First

Lorsque vous commencez à développer une application, votre modèle de données change fréquemment, et chaque fois que le modèle change, il n’est plus synchronisé avec la base de données. Vous pouvez configurer la Entity Framework pour supprimer et recréer automatiquement la base de données chaque fois que vous modifiez le modèle de données. Il ne s’agit pas d’un problème au début du développement, car les données de test sont facilement recréées, mais une fois que vous avez déployé en production, vous souhaitez généralement mettre à jour le schéma de base de données sans supprimer la base de données. La fonctionnalité migrations permet à Code First de mettre à jour la base de données sans la supprimer ni la recréer. Au début du cycle de développement d’un nouveau projet, vous souhaiterez peut-être utiliser [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) pour supprimer, recréer et réamorcer la base de données chaque fois que le modèle change. Une fois que vous êtes prêt à déployer votre application, vous pouvez passer à l’approche des migrations. Pour ce didacticiel, vous allez utiliser uniquement des migrations. Pour plus d’informations, consultez [migrations code First](https://msdn.microsoft.com/data/jj591621) et [migrations série de captures vidéo](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Activer Migrations Code First

1. Dans le menu **Outils** , cliquez sur **Gestionnaire de package NuGet** , puis sur **console du gestionnaire de package**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. À l’invite `PM>`, entrez la commande suivante :

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![commande Enable-migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Cette commande crée un dossier *migrations* dans le projet ContosoUniversity et place dans ce dossier un fichier *Configuration.cs* que vous pouvez modifier pour configurer des migrations.

    ![Dossier migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    La classe `Configuration` comprend une méthode `Seed` qui est appelée lorsque la base de données est créée et chaque fois qu’elle est mise à jour après la modification d’un modèle de données.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    L’objectif de cette `Seed` méthode est de vous permettre d’insérer des données de test dans la base de données après que Code First les a créées ou mises à jour.

### <a name="set-up-the-seed-method"></a>Configurer la méthode Seed

La méthode [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) s’exécute lorsque migrations code First crée la base de données et chaque fois qu’elle met à jour la base de données vers la dernière migration. L’objectif de la méthode Seed est de vous permettre d’insérer des données dans vos tables avant que l’application accède pour la première fois à la base de données.

Dans les versions antérieures de Code First, avant la sortie des migrations, il était courant pour `Seed` méthodes d’insérer des données de test, car avec chaque modification de modèle lors du développement, la base de données devait être complètement supprimée et recréée à partir de zéro. Avec Migrations Code First, les données de test sont conservées après la modification de la base de données, de sorte que l’inclusion de données de test dans la méthode [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) n’est généralement pas nécessaire. En fait, vous ne souhaitez pas que la méthode `Seed` insère des données de test si vous utilisez des migrations pour déployer la base de données en production, car la méthode `Seed` s’exécute en production. Dans ce cas, vous souhaitez que la méthode `Seed` insère dans la base de données uniquement les données que vous souhaitez insérer en production. Par exemple, vous souhaiterez peut-être que la base de données inclue les noms de service réels dans la table `Department` lorsque l’application devient disponible en production.

Pour ce didacticiel, vous allez utiliser des migrations pour le déploiement, mais votre méthode de `Seed` insère les données de test quand même afin de faciliter la visibilité du fonctionnement des fonctionnalités de l’application sans avoir à insérer manuellement de nombreuses données.

1. Remplacez le contenu du fichier *Configuration.cs* par le code suivant, qui chargera les données de test dans la nouvelle base de données. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    La méthode [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) prend l’objet de contexte de base de données en tant que paramètre d’entrée, et le code de la méthode utilise cet objet pour ajouter de nouvelles entités à la base de données. Pour chaque type d’entité, le code crée une collection de nouvelles entités, les ajoute à la propriété [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) appropriée, puis enregistre les modifications dans la base de données. Il n’est pas nécessaire d’appeler la méthode [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) après chaque groupe d’entités, comme cela est fait ici, mais cela vous permet de localiser la source d’un problème si une exception se produit pendant que le code écrit dans la base de données.

    Certaines des instructions qui insèrent des données utilisent la méthode [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) pour effectuer une opération « upsert ». Étant donné que la méthode `Seed` s’exécute avec chaque migration, vous ne pouvez pas simplement insérer des données, car les lignes que vous essayez d’ajouter sont déjà présentes après la première migration qui crée la base de données. L’opération « upsert » empêche les erreurs qui se produisent si vous essayez d’insérer une ligne qui existe déjà, mais elle ***remplace*** toutes les modifications apportées aux données que vous avez pu effectuer lors du test de l’application. Avec les données de test dans certaines tables, vous pouvez ne pas souhaiter que cela se produise : dans certains cas, lorsque vous modifiez des données pendant un test, vous souhaitez conserver les modifications après les mises à jour de la base de données. Dans ce cas, vous devez effectuer une opération d’insertion conditionnelle : Insérez une ligne uniquement si elle n’existe pas déjà. La méthode Seed utilise les deux approches.

    Le premier paramètre passé à la méthode [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) spécifie la propriété à utiliser pour vérifier si une ligne existe déjà. Pour les données d’étudiant de test que vous fournissez, la propriété `LastName` peut être utilisée à cet effet puisque chaque nom de famille de la liste est unique :

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Ce code suppose que les noms sont uniques. Si vous ajoutez manuellement un étudiant avec un nom en double, vous obtenez l’exception suivante la prochaine fois que vous effectuez une migration.

    La séquence contient plusieurs éléments

    Pour plus d’informations sur la méthode `AddOrUpdate`, consultez se [préoccuper de la méthode EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) sur le blog de Julie Lerman.

    Le code qui ajoute `Enrollment` entités n’utilise pas la méthode `AddOrUpdate`. Il vérifie si une entité existe déjà et insère l’entité si elle n’existe pas. Cette approche permet de conserver les modifications apportées à un niveau d’inscription lors de l’exécution des migrations. Le code parcourt chaque membre de la [liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) `Enrollment`et si l’inscription est introuvable dans la base de données, il ajoute l’inscription à la base de données. La première fois que vous mettez à jour la base de données, la base de données est vide. par conséquent, elle ajoute chaque inscription.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Pour plus d’informations sur le débogage de la méthode `Seed` et la gestion des données redondantes, telles que les deux étudiants nommés « Alexander Carson », consultez la page relative aux bases de données d' [amorçage et de débogage Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) sur le blog de Rick Anderson.
2. créer le projet ;

### <a name="create-and-execute-the-first-migration"></a>Créer et exécuter la première migration

1. Dans la fenêtre console du gestionnaire de package, entrez les commandes suivantes : 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    La commande `add-migration` ajoute au dossier migrations un fichier *[date]\_InitialCreate.cs* qui contient le code qui crée la base de données. Le premier paramètre (`InitialCreate)` est utilisé pour le nom de fichier et peut être tout ce que vous voulez ; vous choisissez généralement un mot ou une expression qui résume ce qui est effectué dans la migration. Par exemple, vous pouvez nommer une migration ultérieure &quot;Ajoutertabledépartement&quot;.

    ![Dossier migrations avec migration initiale](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités du modèle de données, et la méthode `Down` les supprime. Les migrations appellent la méthode `Up` pour implémenter les modifications du modèle de données pour une migration. Quand vous entrez une commande pour annuler la mise à jour, Migrations appelle la méthode `Down`. Le code suivant affiche le contenu du fichier `InitialCreate` :

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    La commande `update-database` exécute la méthode `Up` pour créer la base de données, puis exécute la méthode `Seed` pour remplir la base de données.

Une base de données SQL Server a maintenant été créée pour votre modèle de données. Le nom de la base de données est *ContosoUniversity*, et le fichier *. mdf* se trouve dans le dossier de données de l' *application\_* de votre projet, car c’est ce que vous avez spécifié dans votre chaîne de connexion.

Vous pouvez utiliser **Explorateur de serveurs** ou **Explorateur d’objets SQL Server** (SSOX) pour afficher la base de données dans Visual Studio. Pour ce didacticiel, vous allez utiliser **Explorateur de serveurs**. Dans Visual Studio Express 2012 pour le Web, **Explorateur de serveurs** est appelé **Explorateur de base de données**.

1. Dans le menu **affichage** , cliquez sur **Explorateur de serveurs**.
2. Cliquez sur l’icône **Ajouter une connexion** .

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Si la boîte de dialogue Choisir la **source de données** s’affiche, cliquez sur **Microsoft SQL Server**, puis sur **Continuer**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. Dans la boîte de dialogue **Ajouter une connexion** , entrez **(\v11.0)** pour le **nom du serveur**. Sous **Sélectionner ou entrer un nom de base de données**, sélectionnez **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Cliquez sur **OK**.
6. Développez **SchoolContext** , puis **tables**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Cliquez avec le bouton droit sur la table **Student** et cliquez sur **afficher les données** de la table pour afficher les colonnes qui ont été créées et les lignes qui ont été insérées dans la table.

    ![Table Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Création d’un contrôleur et de vues d’étudiant

L’étape suivante consiste à créer un contrôleur MVC ASP.NET et des vues dans votre application qui peuvent fonctionner avec l’une de ces tables.

1. Pour créer un contrôleur de `Student`, cliquez avec le bouton droit sur le dossier **Controllers** dans **Explorateur de solutions**, sélectionnez **Ajouter**, puis cliquez sur **contrôleur**. Dans la boîte de dialogue **Ajouter un contrôleur** , effectuez les sélections suivantes, puis cliquez sur **Ajouter**: 

   - Nom du contrôleur : **StudentController**.
   - Modèle : **contrôleur MVC avec des actions et des vues en lecture/écriture, à l’aide de Entity Framework**.
   - Classe de modèle : **Student (ContosoUniversity. Models)** . (Si vous ne voyez pas cette option dans la liste déroulante, générez le projet et réessayez.)
   - Classe de contexte de données : **SchoolContext (ContosoUniversity. Models)** .
   - Vues : **Razor (cshtml)** . (Valeur par défaut).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio ouvre le fichier *Controllers\StudentController.cs* . Une variable de classe créée instanciant un objet de contexte de base de données s’affiche :

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     La méthode d’action `Index` obtient une liste des étudiants du jeu d’entités *students* en lisant la propriété `Students` de l’instance de contexte de base de données :

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     La vue *Student\Index.cshtml* affiche cette liste dans une table :

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Appuyez sur CTRL+F5 pour exécuter le projet.

     Cliquez sur l’onglet **students** pour afficher les données de test insérées par la méthode `Seed`.

     ![Page d’index des étudiants](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Conventions

La quantité de code que vous devez écrire pour que le Entity Framework être en mesure de créer une base de données complète pour vous est minime en raison de l’utilisation de *conventions*ou d’hypothèses que la Entity Framework effectue. Certains d’entre eux ont déjà été notés :

- Les formes plusieurs des noms de classes d’entité sont utilisées comme noms de tables.
- Les noms des propriétés d’entités sont utilisés comme noms de colonnes.
- Les propriétés d’entité nommées `ID` ou *classname* `ID` sont reconnues en tant que propriétés de clé primaire.

Vous avez vu que les conventions peuvent être remplacées (par exemple, vous avez spécifié que les noms de table ne doivent pas être mis en valeur) et vous en apprendrez davantage sur les conventions et sur la façon de les remplacer dans le didacticiel [création d’un modèle de données plus complexe](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) plus loin dans cette série. Pour plus d’informations, consultez [conventions de code First](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Récapitulatif

Vous avez maintenant créé une application simple qui utilise les Entity Framework et SQL Server Express pour stocker et afficher des données. Dans le didacticiel suivant, vous allez apprendre à effectuer des opérations CRUD (création, lecture, mise à jour, suppression) de base. Vous pouvez conserver vos commentaires en bas de cette page. Faites-nous savoir comment vous avez aimé cette partie du didacticiel et comment nous pourrions l’améliorer.

Vous trouverez des liens vers d’autres ressources de Entity Framework dans le [plan de contenu d’accès aux données ASP.net](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Suivant](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
