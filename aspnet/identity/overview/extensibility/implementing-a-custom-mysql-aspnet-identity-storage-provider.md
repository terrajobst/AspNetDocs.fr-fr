---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implémentation d’un fournisseur de stockage ASP.NET Identity MySQL personnalisé-ASP.NET 4. x
author: raquelsa
description: ASP.NET Identity est un système extensible qui vous permet de créer votre propre fournisseur de stockage et de l’intégrer à votre application sans avoir à réutiliser l’applicateur...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 2f0b47d45bce82c71d1864536309f9e2ffed2d63
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519126"
---
# <a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implémentation d’un fournisseur de stockage ASP.NET Identity MySQL personnalisé

par [Raquel Soares de Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity est un système extensible qui vous permet de créer votre propre fournisseur de stockage et de l’intégrer à votre application sans avoir à retravailler l’application. Cette rubrique explique comment créer un fournisseur de stockage MySQL pour ASP.NET Identity. Pour obtenir une vue d’ensemble de la création de fournisseurs de stockage personnalisés, consultez [vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.net Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Pour suivre ce didacticiel, vous devez disposer d’Visual Studio 2013 avec Update 2.
> 
> Ce didacticiel vous permet d’effectuer les opérations suivantes :
> 
> - Montre comment créer une instance de base de données MySQL sur Azure.
> - Montrez comment utiliser un outil client MySQL (MySQL Workbench) pour créer des tables et gérer votre base de données distante sur Azure.
> - Montre comment remplacer la valeur par défaut de l’implémentation de stockage ASP.NET Identity par notre implémentation personnalisée sur un projet d’application MVC.
> 
> Ce didacticiel a été écrit à l’origine par Raquel Soares de Almeida et Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ). L’exemple de projet a été mis à jour pour l’identité 2,0 par Suhas Joshi. La rubrique a été mise à jour pour l’identité 2,0 de Tom FitzMacken.

## <a name="download-completed-project"></a>Télécharger le projet terminé

À la fin de ce didacticiel, vous disposerez d’un projet d’application MVC avec ASP.NET Identity utilisant une base de données MySQL hébergée sur Azure.

Vous pouvez télécharger le fournisseur de stockage MySQL complet sur [Aspnet. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL).

## <a name="the-steps-you-will-perform"></a>Les étapes à effectuer

Dans ce tutoriel vous allez :

1. Créer une base de données MySQL sur Azure
2. Créer les tables ASP.NET Identity dans MySQL
3. Créer une application MVC et la configurer pour utiliser le fournisseur MySQL
4. Exécuter l'application

Cette rubrique ne couvre pas l’architecture de ASP.NET Identity et les décisions que vous devez prendre lors de l’implémentation d’un fournisseur de stockage client. Pour plus d’informations, consultez [vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.net Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Examiner les classes du fournisseur de stockage MySQL

Avant de passer aux étapes de création du fournisseur de stockage MySQL, observons les classes qui composent le fournisseur de stockage. Vous aurez besoin de classes qui gèrent les opérations et les classes de base de données qui sont appelées à partir de l’application pour gérer les utilisateurs et les rôles.

### <a name="storage-classes"></a>Classes de stockage

- [IdentityUser](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs) : contient les propriétés de l’utilisateur.
- [UserStore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs) : contient les opérations d’ajout, de mise à jour ou de récupération des utilisateurs.
- [IdentityRole](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs) : contient les propriétés des rôles.
- [Au rolestore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) : contient les opérations d’ajout, de suppression, de mise à jour et de récupération des rôles.

### <a name="data-access-layer-classes"></a>Classes de couche d’accès aux données

Pour cet exemple, les classes de la couche d’accès aux données contiennent des instructions SQL pour l’utilisation des tables. Toutefois, dans votre code, vous souhaiterez peut-être utiliser le mappage objet/relationnel (ORM), tel que Entity Framework ou NHibernate. En particulier, votre application peut présenter des performances médiocres sans un ORM qui comprend le chargement différé et la mise en cache d’objets. Pour plus d’informations, consultez [ASP.NET Identity 2,0 sans Entity Framework ?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) : contient la connexion de base de données MySQL et les méthodes permettant d’effectuer des opérations de base de données. UserStore et au rolestore sont instanciés avec une instance de cette classe.
- [RoleTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) : contient des opérations de base de données pour la table qui stocke des rôles.
- [UserClaimsTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) : contient les opérations de base de données pour la table qui stocke les revendications de l’utilisateur.
- [UserLoginsTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) : contient les opérations de base de données pour la table qui stocke les informations de connexion de l’utilisateur.
- [UserRoleTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) : contient les opérations de base de données pour la table qui stocke les utilisateurs qui sont affectés à quels rôles.
- [UserTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) : contient les opérations de base de données pour la table qui stocke les utilisateurs.

## <a name="create-a-mysql-database-instance-on-azure"></a>Créer une instance de base de données MySQL sur Azure

1. Connectez-vous au [portail Azure](https://manage.windowsazure.com/).
2. Cliquez sur **+ nouveau** en bas de la page, puis sélectionnez **Store**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. Dans l’Assistant **Choose and Add-on** , sélectionnez **ClearDB MySQL Database** , puis cliquez sur la flèche suivant en bas à droite de la boîte de dialogue.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Conservez le plan **gratuit** par défaut et remplacez le **nom** par **IdentityMySQLDatabase**. Sélectionnez la région la plus proche de vous, puis cliquez sur la flèche suivant.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Cliquez sur la coche pour terminer la création de la base de données.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Une fois la base de données créée, vous pouvez la gérer à partir de l’onglet **ADD-ONS** du portail de gestion.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Vous pouvez accéder aux informations de connexion à la base de données en cliquant sur **informations de connexion** en bas de la page.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Copiez la chaîne de connexion en cliquant sur le bouton Copier, puis enregistrez-la afin de pouvoir l’utiliser ultérieurement dans votre application MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Créer les tables ASP.NET Identity dans une base de données MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Installer l’outil MySQL Workbench pour la connexion et la gestion de la base de données MySQL

1. Installer l’outil **MySQL Workbench** à partir de la [page téléchargements MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Lancez l’application et ajoutez cliquez sur le bouton **MySQLConnections +** pour ajouter une nouvelle connexion. Utilisez les données de chaîne de connexion que vous avez copiées à partir de la base de données Azure MySQL que vous avez créée précédemment dans ce didacticiel.
3. Après avoir établi la connexion, ouvrez un nouvel onglet de **requête** . collez les commandes de [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) dans la requête et exécutez-les pour créer les tables de base de données.
4. Vous disposez maintenant de toutes les tables ASP.NET Identity nécessaires créées sur une base de données MySQL hébergée sur Azure, comme indiqué ci-dessous.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Créer un projet d’application MVC à partir du modèle et le configurer pour utiliser le fournisseur MySQL

Si nécessaire, installez [Visual Studio Express 2013 pour Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) avec Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-github"></a>Téléchargez le projet ASP. NET. Identity. MySQL à partir de GitHub

1. Accédez à l’URL du référentiel sur [Aspnet. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL/).
2. Téléchargez le code source.
3. Extrayez le fichier. zip dans un dossier local.
4. Ouvrez la solution AspNet. Identity. MySQL et générez-la.

### <a name="create-a-new-mvc-application-project-from-template"></a>Créer un projet d’application MVC à partir d’un modèle

1. Cliquez avec le bouton droit sur la solution **Aspnet. Identity. MySQL** et **Ajoutez**, **nouveau projet**
2. Dans la boîte de dialogue **Ajouter un nouveau projet** , sélectionnez **visuel C#**  à gauche, puis **Web** , puis sélectionnez **application Web ASP.net**. Nommez votre projet **IdentityMySQLDemo**; puis cliquez sur OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle MVC avec les options par défaut (qui incluent les **comptes d’utilisateur individuels** comme méthode d’authentification), puis cliquez sur **OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. Dans Explorateur de solutions, cliquez avec le bouton droit sur votre projet IdentityMySQLDemo, puis sélectionnez **gérer les packages NuGet**. Dans la boîte de dialogue zone de texte de recherche, tapez **Identity. EntityFramework**. Sélectionnez ce package dans la liste des résultats, puis cliquez sur **désinstaller**. Vous serez invité à désinstaller le package de dépendances EntityFramework. Cliquez sur Oui, car ce package ne sera plus présent sur cette application.
5. Cliquez avec le bouton droit sur le projet IdentityMySQLDemo, sélectionnez **Ajouter**, **référence, solution, projets,** sélectionnez le projet Aspnet. Identity. MySQL, puis cliquez sur **OK**.
6. Dans le projet IdentityMySQLDemo, remplacez toutes les références à  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   avec  
     `using AspNet.Identity.MySQL;`
7. Dans IdentityModels.cs, définissez **ApplicationDbContext** pour qu’elle dérive de **MySqlDatabase** et incluez un constructeur qui prend un seul paramètre avec le nom de la connexion.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Ouvrez le fichier IdentityConfig.cs. Dans la méthode **ApplicationUserManager. Create** , remplacez instanciing usermanager par le code suivant :  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Ouvrez le fichier Web. config et remplacez la chaîne DefaultConnection par cette entrée, en remplaçant les valeurs en surbrillance par la chaîne de connexion de la base de données MySQL que vous avez créée lors des étapes précédentes :  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Exécuter l’application et se connecter à la base de BDD MySQL

1. Cliquez avec le bouton droit sur le projet **IdentityMySQLDemo** et sélectionnez **définir comme projet de démarrage** .
2. Appuyez sur **CTRL + F5** pour générer et exécuter l’application.
3. Cliquez sur l’onglet **Register** en haut de la page.
4. Entrez un nouveau nom d’utilisateur et un nouveau mot de passe, puis cliquez sur **inscrire**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Le nouvel utilisateur est maintenant inscrit et connecté.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Revenez à l’outil MySQL Workbench et examinez le contenu de la table **IdentityMySQLDatabase** . Examinez la table users pour les entrées lorsque vous inscrivez de nouveaux utilisateurs.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la façon d’activer d’autres méthodes d’authentification sur cette application, consultez [créer une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et authentification OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Pour savoir comment intégrer votre base de connaissances avec OAuth et configurer des rôles pour limiter l’accès des utilisateurs à votre application, consultez [déployer une application secure ASP.NET MVC 5 avec appartenance, OAuth et SQL Database sur Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
