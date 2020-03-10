---
uid: whitepapers/aspnet-data-access-content-map
title: Accès aux données ASP.NET-ressources recommandées | Microsoft Docs
author: rick-anderson
description: Cette rubrique fournit des liens vers des ressources de documentation sur l’accès aux données dans les applications Web ASP.NET, principalement à l’aide de la Entity Framework et de SQL se...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 357851f195bf233c7c34a32bd156e4408d3e1b24
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633132"
---
# <a name="aspnet-data-access---recommended-resources"></a>Accès aux données ASP.NET - Ressources recommandées

> Cette rubrique fournit des liens vers des ressources de documentation sur l’accès aux données dans les applications Web ASP.NET, principalement à l’aide des Entity Framework et SQL Server.
> 
> Si vous connaissez un billet de blog, un [StackOverflow](http://stackoverflow.com) thread ou tout autre lien utile, [envoyez-nous un e-mail](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) avec le lien.
> 
> Dernière mise à jour 4/3/2014

La rubrique contient les sections suivantes :

- [Prise en main avec accès aux données dans ASP.NET](#gettingstarted)
- [Utilisation de l’Entity Framework](#ef)

    - [Utilisation de Entity Framework Code First](#cf)
    - [Utilisation de Migrations Entity Framework Code First](#efcfmigrations)
    - [Utilisation de Entity Framework Database First ou Model First (le concepteur EF)](#efdbf)
    - [Chargement de données associées dans Entity Framework (chargement différé, chargement hâtif et chargement explicite)](#efrelateddata)
    - [Optimisation des performances de Entity Framework](#optimizingef)
    - [Gestion de l’accès concurrentiel dans une application Entity Framework](#efconcurrency)
    - [Ouvrages sur le Entity Framework](#efbooks)
    - [Ressources de Entity Framework supplémentaires](#otherefresources)
- [Liaison de données dans les applications de Web Forms ASP.NET](#wfdatabinding)

    - [Utilisation de la liaison de modèle Web Forms](#wfmodelbinding)
    - [Utilisation de Web Forms contrôles de source de données](#wfdsc)
    - [Utilisation d’Web Forms des contrôles liés aux données et des expressions de liaison de données](#wfdbc)
- [Utilisation des bases de données SQL Server](#sqlserver)

    - [Utilisation des bases de données SQL Server Express de base de données locale](#sslocaldb)
    - [Utilisation des bases de données SQL Server Express](#sse)
    - [Utilisation de Windows Azure SQL Database](#ssdb)
    - [Choix entre SQL Server et Windows Azure SQL Database](#ssdbchoosing)
- [Utilisation des systèmes de gestion de base de données NoSQL](#nosql)
- [Utilisation de requêtes LINQ dans des applications ASP.NET](#linq)
- [Utilisation de la génération de modèles automatique Dynamic Data](#dd)
- [Sécurisation de l’accès aux données](#securing)
- [Optimisation des performances d’accès aux données](#optimizingdataaccess)
- [Déploiement d’une base de données](#deploying)
- [Accès aux données via un service Web](#webservice)
- [Ressources supplémentaires pour MSBuild](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Prise en main avec accès aux données dans ASP.NET

- [Options de stockage des données (création d’applications Cloud réalistes avec Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Chapitre d’un livre électronique sur le développement pour le Cloud. Introduit les bases de données NoSQL comme alternative que de nombreux développeurs habitués aux bases de données relationnelles ont tendance à négliger. Présente des instructions sur les éléments à prendre en compte lors du choix de la relation ou de la NoSQL, ou le choix d’une plateforme particulière.
- [Options d’accès aux données ASP.net](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Présentation des options d’accès aux données pour les bases de données relationnelles pour ASP.NET et conseils sur la façon de choisir des plateformes et des méthodes d’accès adaptées à votre scénario.
- [Base de données relationnelle](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Si vous n’avez pas travaillé avec les bases de données relationnelles, consultez cette page pour obtenir une présentation de la terminologie et des concepts des bases de données relationnelles. Pour obtenir une présentation des SQL Server en particulier, consultez [utilisation des bases de données SQL Server](#sqlserver) plus loin dans cette rubrique.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Utilisation de l’Entity Framework

- [Approches de développement Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Aide sur le choix d’une approche de développement Entity Framework Database First, Model First ou Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Utilisation de Entity Framework Code First

Les didacticiels suivants proposent des exemples d’applications téléchargeables :

- [Prise en main avec EF 6 à l’aide de MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Couvre un large éventail de scénarios de Entity Framework Code First, y compris les migrations et les fonctionnalités EF 6, telles que la résilience des connexions, l’interception des commandes et Async. Il s’agit d’une version mise à jour de la [série EF 5/MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La série précédente inclut un didacticiel sur le référentiel et les modèles d’unité de travail qui ne sont pas inclus dans la nouvelle série.
- [Présentation de ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Couvre un éventail plus étroit de Entity Framework Code First scénarios, mais effectue un travail plus complet d’introduction des fonctionnalités MVC.
- [Liaison de modèle et Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Utilise Code First dans une application Web Forms.
- [Prise en main avec ASP.NET 4,5 Web Forms](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Présentation de la Web Forms avec une couverture de Code First. Utilise la liaison de modèle.
- [Magasin de musique MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Utilise Code First dans une application e-commerce MVC 3 qui implémente également l’appartenance et l’autorisation. La version MVC et le système d’appartenance ASP.NET (authentification et autorisation) utilisés ici sont obsolètes. Pour plus d’informations à jour sur l’appartenance à ASP.NET, consultez [https://asp.net/identity](https://asp.net/identity).

Autres ressources :

- [Entity Framework-code First à une base de données existante](https://msdn.microsoft.com/data/jj200620). HTTP://msdn.Microsoft.com/library/default.asp. Vidéo et procédure pas à pas qui montrent comment utiliser Code First avec une base de données existante.
- [Centre de développement de données-Entity Framework](https://msdn.microsoft.com/data/ef). HTTP://msdn.Microsoft.com/library/default.asp. Pour obtenir un guide sur Entity Framework documentation qui a été créée et gérée par l’équipe de Entity Framework, consultez le lien [prise en main](https://msdn.microsoft.com/data/ee712907) .

Consultez également [la documentation sur les Entity Framework et les](#efbooks) [ressources supplémentaires Entity Framework](#otherefresources) plus loin dans cette rubrique.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Utilisation de Migrations Entity Framework Code First

La plupart des didacticiels de Code First listés ci-dessus couvrent les migrations. Consultez également les ressources suivantes.

- [Déploiement web ASP.NET en utilisant Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). série de didacticiels en deux parties qui montre comment utiliser Migrations Code First pour déployer une base de données.
- [Déployez une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et SQL Database à un site Web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Comment utiliser les migrations pour déployer des données d’appartenance et d’application sur Azure.
- [Présentation du déploiement Web pour Visual Studio et ASP.net](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Consultez la section **configuration du déploiement de base de données dans Visual Studio** pour obtenir une explication sur la façon dont migrations code First est intégré aux fonctionnalités de déploiement Web de Visual Studio.
- [Centre de développement de données-migrations code First](https://msdn.microsoft.com/data/jj591621) (MSDN). La documentation sur les migrations de l’équipe Entity Framework.
- [Série de captures vidéo sur les migrations](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog EF). Trois vidéos sur les sujets avancés dans Migrations Code First.
- [Migrations code First avec les Sites pages Web ASP.net](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog Mikesdotnetting). Montre comment utiliser Code First migrations avec un site pages Web ASP.NET en plaçant le contexte de données dans un projet de bibliothèque de classes Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Utilisation de Entity Framework Database First ou Model First (le concepteur EF)

- [Prise en main avec Entity Framework 6 Database First à l’aide de MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Exécutez un script dans Explorateur de serveurs pour créer une base de données, puis utilisez le concepteur de Entity Framework pour créer le modèle de données. Montre comment créer des pages Web CRUD simples, et pour d’autres fonctions de gestion des données, vous pouvez suivre l’un des didacticiels de Code First dans la mesure où tous les flux de travail EF utilisent la même API DbContext.

Les ressources suivantes sont plus anciennes. Elles sont utiles si vous souhaitez utiliser la version 4,0 du Entity Framework et que vous souhaitez utiliser un contrôle de source de données pour la liaison de données dans une application Web Forms.

- [Prise en main avec le Entity Framework 4,0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Montre comment utiliser le contrôle **EntityDataSource** .
- [Poursuivre avec le Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(montre comment utiliser le contrôle **ObjectDataSource** . Contient un didacticiel sur la gestion de l’accès concurrentiel, un didacticiel sur les performances d’EF et un didacticiel sur les nouveautés d’EF 4,0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Gestion des données associées dans Entity Framework (chargement différé, chargement hâtif et chargement explicite)

- [Lecture des données associées avec l’Entity Framework dans une application MVC ASP.net](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, exemple d’application MVC. Les méthodes indiquées s’appliquent également à Web Forms la liaison de modèle et le flux de travail Database First.
- [Centre de développement de données-chargement des entités associées](https://msdn.microsoft.com/data/jj574232) (MSDN). La documentation de l’équipe de Entity Framework sur le chargement des données associées.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimisation des performances de Entity Framework

- [Scénarios de Entity Framework avancés pour une Application ASP.net](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Montre comment exécuter vos propres instructions SQL ou appeler vos propres procédures stockées, comment désactiver la détection des modifications et comment désactiver la validation lors de l’enregistrement des modifications.
- [Considérations relatives aux performances de Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Considérations sur les performances (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Optimisation des performances avec la Entity Framework dans une application Web ASP.net](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). S’applique à Entity Framework 4,0.
- Consultez aussi [optimisation de l’accès aux données ASP.net](#optimizingdataaccess) plus loin dans cette rubrique.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Gestion de l’accès concurrentiel dans une application Entity Framework

- [Gestion de l’accès concurrentiel avec l’Entity Framework dans une application MVC ASP.net](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, l’API DbContext, à l’aide d’un exemple d’application MVC.
- [Centre de développement de données – modèles d’accès concurrentiel optimiste](https://msdn.microsoft.com/data/jj592904) (MSDN). La documentation sur l’accès concurrentiel de l’équipe Entity Framework.
- [Gestion de l’accès concurrentiel avec l’Entity Framework dans une application Web ASP.net](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). S’applique à Entity Framework 4,0. Database First, API ObjectContext, à l’aide d’un exemple d’application Web Forms.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Ouvrages sur le Entity Framework

- [Programmation Entity Framework : DbContext](http://shop.oreilly.com/product/0636920022237.do) par Julie Lerman et Rowan Miller.
- [Entity Framework de programmation : code First](http://shop.oreilly.com/product/0636920022220.do) par Julie Lerman et Rowan Miller.

Ces deux livres sont à jour avec les techniques recommandées actuelles. Ils fournissent une présentation plus complète mais facile à suivre de la Entity Framework que tout ce qui est disponible sur Internet. Un autre livre, la [programmation Entity Framework](http://shop.oreilly.com/product/9780596807252.do) par Julie Lerman, est plus grand et plus complet, mais il est plus ancien et un grand nombre des techniques qu’il couvre ne sont plus la méthode recommandée pour utiliser le Entity Framework. Consultez également la liste des manuels recommandés par l’équipe Entity Framework dans le [Centre de développement de données-livres](https://msdn.microsoft.com/data/aa937716) sur le site MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Autres ressources Entity Framework

- [Blog de l’équipe Entity Framework (ADO.net)](https://blogs.msdn.com/b/adonet/). L’une des meilleures ressources pour les informations les plus récentes et les annonces de nouvelles améliorations. Pour d’autres blogs relatifs à EF, consultez le blogroll sur la page [prise en main de Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [Magazine MSDN](https://msdn.microsoft.com/magazine/default.aspx). Consultez la colonne des **points de données** , qui est fréquemment à propos des rubriques relatives à l’Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Liaison de données dans les applications de Web Forms ASP.NET

- [ASP.NET Web Forms les options d’accès aux données](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Utilisation de la liaison de modèle Web Forms

- [Liaison de modèle et Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Série de didacticiels utilisant EF Code First.
- [Web Forms liaison de modèle, partie 1 : sélection des données](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie). Dans ces billets de blog plus anciens, la propriété qui est actuellement nommée ItemType était nommée ModelType, mais les informations qu’elle contient sont valides.
- [Web Forms liaison de modèle, partie 2 : filtrage des données](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Web Forms liaison de modèle, partie 3 : mise à jour et validation](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog de Scott Guthrie).
- [ASP.NET 4,5 Web Forms liaison de modèle](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (vidéo).
- [Liaison de modèle, partie 1 : sélection des données](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (vidéo).
- [Liaison de modèle, partie 2 : filtrage](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vidéo).
- [Prise en main avec ASP.NET 4,5 Web Forms-Affichez les éléments de données et les détails](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Utilisation de Web Forms contrôles de source de données

- [Contrôles serveur Web de source de données](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Annonce de la publication de Dynamic Data fournisseur et du contrôle EntityDataSource pour Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog de développement Web Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Utilisation d’Web Forms des contrôles liés aux données et des expressions de liaison de données

- [Liaison de modèle et Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Série de didacticiels utilisant EF Code First.
- [Prise en main avec ASP.NET 4,5 Web Forms-Affichez les éléments de données et les détails](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Contrôles de données fortement typés](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Contrôles de données fortement typés](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vidéo).
- [ASP.NET 4,5 Web Forms des contrôles de données fortement typés](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vidéo).
- [Contrôles de serveur Web liés aux données](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Vue d’ensemble des expressions de liaison de données](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Cette page traite uniquement des **évaluations** et des **liaisons**. Il n’a pas été mis à jour pour inclure **Item** et **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Utilisation des bases de données SQL Server

- [Fonctionnalités de base de données SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Pour obtenir une présentation générale d’une large gamme de rubriques de SQL Server, consultez les entrées sous celle-ci dans la table des matières.
- [Éditions SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Résumé des éditions SQL Server disponibles, avec des liens vers des informations supplémentaires sur chacun d’eux.)
- [SQL Server les chaînes de connexion pour les applications Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Utilisation de SQL Server Compact pour les applications Web ASP.net](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server : exemples de produits de base de données](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Exemples de bases de données AdventureWorks.
- [Installation des exemples de bases de données](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). En plus des méthodes indiquées ici, vous pouvez également télécharger l’un des exemples de fichiers. mdf dans le dossier application\_Data d’un projet Web, convertir la base de données en base de données locale et créer une chaîne de connexion de base de données locale. Pour plus d’informations sur la procédure à suivre, consultez [procédure : mise à niveau vers la base de](https://msdn.microsoft.com/library/hh873188.aspx)données locale.

Consultez également les sections suivantes sur l’utilisation de SQL Server Express et de la base de données locale, et le choix entre SQL Server et SQL Database.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Utilisation des bases de données SQL Server Express de base de données locale

- [SQL Server Express 2012](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) de la base de données locale (MSDN). Présentation MSDN officielle de la base de données locale.
- [SQL Server les chaînes de connexion pour les applications Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Comment : effectuer une mise à niveau vers la base de](https://msdn.microsoft.com/library/hh873188.aspx) données locale (MSDN). Migration d’un fichier. mdf à partir d’une version antérieure de SQL Server Express vers la base de données locale. Vous devez également suivre ce processus si vous téléchargez l’un des [exemples de bases de données SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Présentation de la base de données locale, une amélioration de SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (blog SQL Server Express). A plus d’informations sur la raison pour laquelle la base de données locale a été créée que celle incluse dans MSDN.
- [Base de données locale : où se trouve ma base de données ?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog SQL Server Express). Informations sur la création des fichiers de base de données de base de données locale.
- [Utilisation de la base de données locale avec IIS complet, partie 1 : profil utilisateur](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog SQL Server Express). La base de données locale n’est pas conçue pour fonctionner avec IIS. Cette série de billets de blog explique les problèmes et des solutions de contournement.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Utilisation des bases de données SQL Server Express

- [SQL Server les chaînes de connexion pour les applications Web ASP.net](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Si vous utilisez le paramètre de chaîne de connexion AttachDBFileName avec SQL Server Express, consultez en particulier la section instance utilisateur de cette page.
- [Comment prendre possession de votre SQL Server Express local 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog SQL Server Express). Un problème courant n’est pas de pouvoir fonctionner avec les bases de données SQL Server Express, car vous n’êtes pas administrateur sur l’instance de SQL Server Express. Par défaut, seule la personne qui a installé SQL Server Express est un administrateur. Ce blog explique comment vous faire un administrateur SQL Server Express si vous êtes administrateur de l’ordinateur.
- [Mon application Web ASP.NET peut-elle utiliser une base de données SQL Server Express en production ?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Utilisation de Windows Azure SQL Database

- [Déployez une application ASP.NET MVC sécurisée avec appartenance, OAuth et SQL Database à un site Web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (site Microsoft Azure).
- [Bases de données SQL](https://docs.microsoft.com/azure/sql-database/) (site Microsoft Azure). Didacticiels de prise en main et guides de procédures.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Nœud de niveau supérieur de la table des matières pour SQL Database dans MSDN.
- [Index des articles wiki TechNet Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (site Microsoft TechNet).
- [Bloc applicatif de gestion des erreurs temporaires](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Une infrastructure qui vous permet de gérer les erreurs réseau temporaires et les erreurs de connexion qui résultent de la limitation. Disponible dans un package NuGet : [Enterprise Library 5,0-bloc applicatif de gestion des erreurs temporaires](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Prise en main avec SQL Database et Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Kit de formation Windows Azure](https://www.microsoft.com/download/details.aspx?id=8396) (Centre de téléchargement Microsoft). Comprend des ateliers pratiques pour SQL Database.
- [Forum de la communauté Windows Azure SQL Database](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Passage à Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Un chapitre d’un scénario complet de bout en bout de l’équipe Microsoft Patterns and Practices. Explique pourquoi vous pouvez migrer et comment migrer de SQL Server vers SQL Database.
- [Migration de bases de données SQL Server vers Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [SQL Database l’Assistant Migration](http://sqlazuremw.codeplex.com/). Outil open source pour la migration de bases de données vers et à partir de SQL Database.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Choix entre SQL Server et Windows Azure SQL Database

- [Comparez SQL Server avec Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (site Microsoft TechNet).
- [Migration des données vers Windows Azure SQL Database : outils et techniques](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Contient des sections qui comparent SQL Server à SQL Database et fournissent des conseils sur la migration de SQL Server vers SQL Database.
- [Guide de remise Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (site Microsoft TechNet).
- [SQL Server limitations relatives aux fonctionnalités (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Stockage table Windows Azure et windows Azure SQL Database-comparés et différences](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Pour une application que vous déployez sur Windows Azure, le stockage de tables Windows Azure peut être une alternative à Windows Azure SQL Database. Cette rubrique vous aide à choisir entre ces alternatives.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Instructions et limitations (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Utilisation des systèmes de gestion de base de données NoSQL

- [Windows Azure Data Services](https://www.windowsazure.com/develop/net/data/) (site Microsoft Azure). Consultez le [Guide des fonctionnalités du service de table](https://docs.microsoft.com/azure/) et la section **Big Data** de la page.
- [ASP.net application multiniveau à l’aide de tables de stockage, de files d’attente et d’objets BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (site Microsoft Azure). Didacticiel de bout en bout avec l’exemple d’application téléchargeable qui utilise les tables NoSQL du stockage Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Utilisation de requêtes LINQ dans des applications ASP.NET

- [Options d’accès aux données ASP.net](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Contient une introduction à LINQ.
- [Vidéos de formation LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe stagnation).
- [ASP.net forum de discussion avec des liens vers des ressources LINQ dynamiques](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Utilisation de la génération de modèles automatique Dynamic Data

- [Dynamic Data les modèles de projet](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Aide sur le moment où utiliser des projets Dynamic Data.
- [ASP.NET Dynamic Data](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Sécurisation de l’accès aux données

- [Sécurisation de l’accès aux données dans ASP.net](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Considérations sur la sécurité (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Comment : sécuriser des chaînes de connexion lors de l’utilisation de contrôles de source de données](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimisation des performances d’accès aux données

- [Vue d’ensemble des performances ASP.net](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Mise en cache ASP.net](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Amélioration des performances de ASP.net](https://msdn.microsoft.com/library/ff647787) (MSDN). Il y a un avertissement « contenu retiré » en haut de cette page, mais la plupart des informations sont toujours pertinentes et il n’existe aucune ressource mise à jour comparable.
- [Amélioration des performances des SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Même commentaire que le lien précédent.

Consultez aussi [optimisation des performances de Entity Framework](#optimizingef) plus haut dans cette rubrique.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Déploiement d’une base de données

- [Déploiement Web ASP.net-ressources recommandées](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Accès aux données via un service Web

- [Accès aux données via un service Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Conseils sur l’utilisation de l’API Web par rapport à WCF.
- [Prise en main avec API Web ASP.net](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Ressources supplémentaires

- [FAQ sur l’accès aux données ASP.net](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET Web Forms les didacticiels-données](../web-forms/overview/data-access/index.md). La plupart de ces didacticiels sont relativement anciens. Veillez à lire les options d' [accès aux données ASP.net](https://msdn.microsoft.com/library/ms178359.aspx) et les [options de stockage de données (création d’applications Cloud réelles avec Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) pour ne pas être trop loin dans une méthode d’accès aux données qui n’est pas adaptée à votre scénario.
- [Mappage de contenu ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Didacticiels de pages Web ASP.net-données](../web-pages/overview/data/index.md).
- [Accès aux données dans Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Fournit une liste de liens similaires à ce mappage de contenu, mais avec un focus sur Visual Studio plutôt que sur ASP.NET.
