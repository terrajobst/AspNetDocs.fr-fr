---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: Création du schéma d’appartenance dans SQL Server (VB) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel commence par examiner les techniques permettant d’ajouter le schéma nécessaire à la base de données afin d’utiliser le SqlMembershipProvider. Après cela, nous travaillons...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 96fd72d1f368b1f7947ef0a2293161d97aaf7065
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632054"
---
# <a name="creating-the-membership-schema-in-sql-server-vb"></a>Création du schéma d’appartenance dans SQL Server (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> Ce didacticiel commence par examiner les techniques permettant d’ajouter le schéma nécessaire à la base de données afin d’utiliser le SqlMembershipProvider. À la suite de cela, nous examinerons les tables clés dans le schéma et nous étudierons leur objectif et leur importance. Ce didacticiel se termine par une présentation de la façon d’indiquer à une application ASP.NET le fournisseur que l’infrastructure d’appartenance doit utiliser.

## <a name="introduction"></a>Introduction

Les deux didacticiels précédents ont examiné l’utilisation de l’authentification par formulaire pour identifier les visiteurs du site Web. L’infrastructure d’authentification par formulaire permet aux développeurs de connecter facilement un utilisateur à un site Web et de le mémoriser sur les visites de page via l’utilisation de tickets d’authentification. La classe `FormsAuthentication` comprend des méthodes permettant de générer le ticket et de l’ajouter aux cookies du visiteur. Le `FormsAuthenticationModule` examine toutes les demandes entrantes et, pour celles avec un ticket d’authentification valide, crée et associe un `GenericPrincipal` et un objet `FormsIdentity` à la requête actuelle. L’authentification par formulaire est simplement un mécanisme d’octroi d’un ticket d’authentification à un visiteur lors de la connexion et, lors des demandes suivantes, de l’analyse de ce ticket pour déterminer l’identité de l’utilisateur. Pour qu’une application Web prenne en charge les comptes d’utilisateur, nous devons toujours implémenter un magasin d’utilisateurs et ajouter des fonctionnalités pour valider les informations d’identification, inscrire de nouveaux utilisateurs et la myriade d’autres tâches liées au compte d’utilisateur.

Avant ASP.NET 2,0, les développeurs étaient sur le crochet pour implémenter toutes ces tâches liées aux comptes d’utilisateur. Heureusement, l’équipe ASP.NET a reconnu cette lacune et a introduit l’infrastructure d’appartenance avec ASP.NET 2,0. L’infrastructure d’appartenance est un ensemble de classes dans le .NET Framework qui fournissent une interface de programmation pour accomplir les tâches principales relatives au compte d’utilisateur. Cette infrastructure est générée au-dessus du [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), qui permet aux développeurs de brancher une implémentation personnalisée dans une API standardisée.

Comme indiqué dans le <a id="Tutorial1"> </a>didacticiel sur la [*sécurité et la prise en charge ASP.net*](../introduction/security-basics-and-asp-net-support-vb.md) , le .NET Framework est fourni avec deux fournisseurs d’appartenances intégrés : [`ActiveDirectoryMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) et [`SqlMembershipProvider`](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Comme son nom l’indique, le `SqlMembershipProvider` utilise une base de données Microsoft SQL Server comme magasin de l’utilisateur. Pour pouvoir utiliser ce fournisseur dans une application, nous devons indiquer au fournisseur la base de données à utiliser comme magasin. Comme vous pouvez l’imaginer, le `SqlMembershipProvider` s’attend à ce que la base de données du magasin de l’utilisateur dispose de certaines tables de base de données, vues et procédures stockées. Nous devons ajouter ce schéma attendu à la base de données sélectionnée.

Ce didacticiel commence par examiner les techniques permettant d’ajouter le schéma nécessaire à la base de données afin d’utiliser le `SqlMembershipProvider`. À la suite de cela, nous examinerons les tables clés dans le schéma et nous étudierons leur objectif et leur importance. Ce didacticiel se termine par une présentation de la façon d’indiquer à une application ASP.NET le fournisseur que l’infrastructure d’appartenance doit utiliser.

C’est parti !

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Étape 1 : choix de l’emplacement du magasin d’utilisateurs

Les données d’une application ASP.NET sont généralement stockées dans plusieurs tables d’une base de données. Lors de l’implémentation du schéma de base de données `SqlMembershipProvider`, nous devons décider si le schéma d’appartenance doit être placé dans la même base de données que les données d’application ou dans une autre base de données.

Je vous recommande de localiser le schéma d’appartenance dans la même base de données que les données d’application pour les raisons suivantes :

- **Maintenabilité** une application dont les données sont encapsulées dans une base de données est plus facile à comprendre, à gérer et à déployer qu’une application qui a deux bases de données distinctes.
- **Intégrité relationnelle** en localisant les tables associées à l’appartenance dans la même base de données que les tables d’application, il est possible d’établir des [contraintes de clé étrangère](http://en.wikipedia.org/wiki/Foreign_key) entre les clés primaires dans les tables liées aux appartenances et les tables d’application associées.

Le découplage du magasin d’utilisateurs et des données d’application dans des bases de données distinctes n’a de sens que si vous avez plusieurs applications qui utilisent chacune des bases de données distinctes, mais qui doivent partager un magasin d’utilisateurs commun.

### <a name="creating-a-database"></a>Création d’une base de données

L’application que nous avons créé depuis le deuxième didacticiel n’a pas encore besoin d’une base de données. Toutefois, nous avons besoin d’une fois pour le magasin de l’utilisateur. Nous allons en créer un, puis y ajouter le schéma requis par le fournisseur `SqlMembershipProvider` (Voir l’étape 2).

> [!NOTE]
> Tout au long de cette série de didacticiels, nous allons utiliser une base de données [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) pour stocker nos tables d’application et le schéma `SqlMembershipProvider`. Cette décision a été prise pour deux raisons : tout d’abord, en raison de son coût-gratuit, l’édition Express est la version la plus accessible en lecture de SQL Server 2005 ; Deuxièmement, SQL Server 2005 Express Edition bases de données peuvent être placées directement dans le dossier de `App_Data` de l’application Web, ce qui permet de créer un package de la base de données et de l’application Web dans un fichier ZIP et de la redéployer sans aucune instruction de configuration ou d’option de configuration spéciale. Si vous préférez suivre l’utilisation d’une version de non-Express Edition de SQL Server, n’hésitez pas. Les étapes sont virtuellement identiques. Le schéma `SqlMembershipProvider` fonctionne avec n’importe quelle version de Microsoft SQL Server 2000 et versions.

Dans le Explorateur de solutions, cliquez avec le bouton droit sur le dossier `App_Data` et choisissez Ajouter un nouvel élément. (Si vous ne voyez pas de dossier `App_Data` dans votre projet, cliquez avec le bouton droit sur le projet dans Explorateur de solutions, sélectionnez Ajouter un dossier ASP.NET, puis choisissez `App_Data`.) Dans la boîte de dialogue Ajouter un nouvel élément, choisissez d’ajouter un nouveau SQL Database nommé `SecurityTutorials.mdf`. Dans ce didacticiel, nous allons ajouter le schéma `SqlMembershipProvider` à cette base de données. dans les didacticiels suivants, nous allons créer des tables supplémentaires pour capturer les données de l’application.

[![ajouter une nouvelle SQL Database à la base de données SecurityTutorials. mdf dans le dossier App_Data](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Figure 1**: ajouter une nouvelle SQL Database nommée `SecurityTutorials.mdf` base de données au dossier `App_Data` ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))

L’ajout d’une base de données au dossier `App_Data` l’intègre automatiquement dans la vue explorateur de base de données. (Dans la version non Express Edition de Visual Studio, le explorateur de base de données est appelé le Explorateur de serveurs.) Accédez à la explorateur de base de données et développez la base de données `SecurityTutorials` ajoutée. Si vous ne voyez pas l’explorateur de base de données à l’écran, accédez au menu Affichage et choisissez explorateur de base de données ou appuyez sur Ctrl + Alt + S. Comme le montre la figure 2, la base de données `SecurityTutorials` est vide, elle ne contient aucune table, aucune vue et aucune procédure stockée.

[![la base de données SecurityTutorials est actuellement vide](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Figure 2**: la base de données `SecurityTutorials` est actuellement vide ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))

## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Étape 2 : ajout du schéma`SqlMembershipProvider`à la base de données

L' `SqlMembershipProvider` nécessite un ensemble particulier de tables, de vues et de procédures stockées à installer dans la base de données du magasin de l’utilisateur. Ces objets de base de données nécessaires peuvent être ajoutés à l’aide de l' [outil`aspnet_regsql.exe`](https://msdn.microsoft.com/library/ms229862.aspx). Ce fichier se trouve dans le dossier `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\`.

> [!NOTE]
> L’outil `aspnet_regsql.exe` offre des fonctionnalités de ligne de commande et une interface utilisateur graphique. L’interface graphique est plus conviviale et c’est ce que nous allons examiner dans ce didacticiel. L’interface de ligne de commande est utile lorsque l’ajout du schéma de `SqlMembershipProvider` doit être automatisé, par exemple dans les scripts de génération ou les scénarios de test automatisé.

L’outil `aspnet_regsql.exe` est utilisé pour ajouter ou supprimer des *services d’application ASP.net* dans une base de données SQL Server spécifiée. Les services d’application ASP.NET englobent les schémas pour les `SqlMembershipProvider` et les `SqlRoleProvider`, ainsi que les schémas pour les fournisseurs SQL pour d’autres frameworks ASP.NET 2,0. Nous devons fournir deux bits d’informations à l’outil `aspnet_regsql.exe` :

- Si vous souhaitez ajouter ou supprimer des services d’application, et
- Base de données à partir de laquelle ajouter ou supprimer le schéma des services d’application

Dans l’invite de la base de données à utiliser, l’outil `aspnet_regsql.exe` nous invite à fournir le nom du serveur sur lequel réside la base de données, les informations d’identification de sécurité pour la connexion à la base de données et le nom de la base de données. Si vous utilisez l’édition non Express de SQL Server, vous devez déjà connaître ces informations, car il s’agit des mêmes informations que vous devez fournir via une chaîne de connexion lors de l’utilisation de la base de données via une page Web ASP.NET. Toutefois, le fait de déterminer le nom du serveur et de la base de données lors de l’utilisation d’une base de données SQL Server 2005 Express Edition dans le dossier `App_Data` est un peu plus complexe.

La section suivante examine un moyen simple de spécifier le nom du serveur et de la base de données pour une base de données SQL Server 2005 Express Edition dans le dossier `App_Data`. Si vous n’utilisez pas SQL Server 2005 Express Edition n’hésitez pas à passer directement à la section installation de la Services d’application.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theapp_datafolder"></a>Détermination du nom du serveur et de la base de données d’une base de données SQL Server 2005 Express Edition dans le dossier`App_Data`

Pour pouvoir utiliser l’outil `aspnet_regsql.exe`, nous devons connaître les noms de serveur et de base de données. Le nom du serveur est `localhost\InstanceName`. La plupart du possible, *InstanceName* est `SQLExpress`. Toutefois, si vous avez installé SQL Server 2005 Express Edition manuellement (c’est-à-dire que vous ne l’avez pas installé automatiquement lors de l’installation de Visual Studio), il est possible que vous ayez sélectionné un autre nom d’instance.

Le nom de la base de données est un peu plus difficile à déterminer. Dans le dossier `App_Data`, les bases de données ont généralement un nom de base de données qui comprend un [identificateur global unique](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) , ainsi que le chemin d’accès au fichier de base de données. Nous devons déterminer ce nom de base de données afin d’ajouter le schéma des services d’application à `aspnet_regsql.exe`.

Le moyen le plus simple de déterminer le nom de la base de données consiste à l’examiner dans SQL Server Management Studio. SQL Server Management Studio fournit une interface graphique pour la gestion des bases de données SQL Server 2005, mais n’est pas fournie avec l’édition Express de SQL Server 2005. La bonne nouvelle, c’est que [vous pouvez télécharger](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) l’édition Express gratuite de SQL Server Management Studio.

> [!NOTE]
> Si vous disposez également d’une version de non-Express Edition de SQL Server 2005 installée sur votre bureau, la version complète de Management Studio est probablement installée. Vous pouvez utiliser la version complète pour déterminer le nom de la base de données, en suivant les mêmes étapes que celles décrites ci-dessous pour l’édition Express.

Commencez par fermer Visual Studio pour vous assurer que tous les verrous imposés par Visual Studio sur le fichier de base de données sont fermés. Ensuite, lancez SQL Server Management Studio et connectez-vous à la base de données `localhost\InstanceName` pour SQL Server 2005 Express Edition. Comme indiqué précédemment, il est probable que le nom de l’instance soit `SQLExpress`. Pour l’option d’authentification, sélectionnez authentification Windows.

[![se connecter à l’instance SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Figure 3**: connexion à l’instance SQL Server 2005 Express Edition ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))

Après vous être connecté à l’instance SQL Server 2005 Express Edition, Management Studio affiche les dossiers des bases de données, les paramètres de sécurité, les objets serveur, etc. Si vous développez l’onglet bases de données, vous verrez que la base de données `SecurityTutorials.mdf` n’est *pas* inscrite dans l’instance de base de données, nous devons d’abord attacher la base de données.

Cliquez avec le bouton droit sur le dossier bases de données, puis choisissez attacher dans le menu contextuel. La boîte de dialogue attacher des bases de données s’affiche. À partir de là, cliquez sur le bouton Ajouter, accédez à la base de données `SecurityTutorials.mdf`, puis cliquez sur OK. La figure 4 illustre la boîte de dialogue attacher des bases de données une fois que la base de données `SecurityTutorials.mdf` a été sélectionnée. La figure 5 montre l’Explorateur d’objets de Management Studio une fois que la base de données a été attachée avec succès.

[![attacher la base de données SecurityTutorials. mdf](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Figure 4**: attacher la base de données `SecurityTutorials.mdf` ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))

[![la base de données SecurityTutorials. mdf s’affiche dans le dossier bases de données](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Figure 5**: la base de données `SecurityTutorials.mdf` s’affiche dans le dossier bases de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))

Comme le montre la figure 5, le nom de la base de données `SecurityTutorials.mdf` est plutôt abstruse. Nous allons le remplacer par un nom plus mémorable (et plus facile à taper). Cliquez avec le bouton droit sur la base de données, choisissez Renommer dans le menu contextuel, puis renommez-la `SecurityTutorialsDatabase`. Cela ne modifie pas le nom de fichier, mais uniquement le nom que la base de données utilise pour s’identifier auprès de SQL Server.

[![renommez la base de données en SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Figure 6**: renommer la base de données en `SecurityTutorialsDatabase`([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))

À ce stade, nous savons les noms de serveur et de base de données pour le fichier de base de données `SecurityTutorials.mdf` : `localhost\InstanceName` et `SecurityTutorialsDatabase`, respectivement. Nous sommes maintenant prêts à installer les services d’application via l’outil `aspnet_regsql.exe`.

### <a name="installing-the-application-services"></a>Installation du Services d’application

Pour lancer l’outil `aspnet_regsql.exe`, accédez au menu Démarrer et choisissez Exécuter. Entrez `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` dans la zone de texte, puis cliquez sur OK. Vous pouvez également utiliser l’Explorateur Windows pour explorer le dossier approprié et double-cliquer sur le fichier `aspnet_regsql.exe`. Les deux approches ont les mêmes résultats.

L’exécution de l’outil `aspnet_regsql.exe` sans argument de ligne de commande lance l’interface utilisateur graphique de l’Assistant Installation de ASP.NET SQL Server. L’Assistant facilite l’ajout ou la suppression des services d’application ASP.NET sur une base de données spécifiée. Le premier écran de l’Assistant, illustré à la figure 7, décrit l’objectif de l’outil.

[![utiliser l’Assistant Installation de ASP.NET SQL Server permet d’ajouter le schéma d’appartenance](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Figure 7**: utilisation de l’Assistant installation de ASP.net SQL Server permet d’ajouter le schéma d’appartenance ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))

La deuxième étape de l’Assistant nous demande si vous souhaitez ajouter les services d’application ou les supprimer. Étant donné que nous souhaitons ajouter les tables, les vues et les procédures stockées nécessaires à l' `SqlMembershipProvider`, choisissez l’option configurer les SQL Server pour les services d’application. Par la suite, si vous souhaitez supprimer ce schéma de votre base de données, réexécutez cet Assistant, mais choisissez plutôt l’option Supprimer les informations des services d’application d’une base de données existante.

[![choisissez l’option configurer SQL Server pour Services d’application](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Figure 8**: choisissez l’option configurer le SQL Server pour services d’application ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))

La troisième étape vous invite à entrer les informations de la base de données : le nom du serveur, les informations d’authentification et le nom de la base de données. Si vous avez déjà utilisé ce didacticiel et que vous avez ajouté la base de données `SecurityTutorials.mdf` à `App_Data`, attachez-la à `localhost\InstanceName`, puis renommez-la `SecurityTutorialsDatabase`, puis utilisez les valeurs suivantes :

- Serveur : `localhost\InstanceName`
- Authentification Windows
- Base de données : `SecurityTutorialsDatabase`

[![entrer les informations de la base de données](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Figure 9**: entrer les informations de la base de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))

Après avoir entré les informations de la base de données, cliquez sur suivant. La dernière étape résume les étapes à effectuer. Cliquez sur suivant pour installer les services d’application, puis sur Terminer pour terminer l’Assistant.

> [!NOTE]
> Si vous avez utilisé Management Studio pour attacher la base de données et renommer le fichier de base de données, veillez à détacher la base de données et à fermer Management Studio avant de rouvrir Visual Studio. Pour détacher la base de données `SecurityTutorialsDatabase`, cliquez avec le bouton droit sur le nom de la base de données et, dans le menu tâches, choisissez détacher.

Une fois l’Assistant terminé, revenez à Visual Studio et accédez au explorateur de base de données. Développez le dossier Tables. Vous devez voir une série de tables dont les noms commencent par le préfixe `aspnet_`. De même, un grand nombre de vues et de procédures stockées se trouvent sous les dossiers vues et procédures stockées. Ces objets de base de données composent le schéma des services d’application. Nous allons examiner les objets de base de données spécifiques aux appartenances et aux rôles à l’étape 3.

[![plusieurs tables, vues et procédures stockées ont été ajoutées à la base de données.](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Figure 10**: plusieurs tables, vues et procédures stockées ont été ajoutées à la base de données ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))

> [!NOTE]
> L’interface utilisateur graphique de `aspnet_regsql.exe` Tool installe l’ensemble du schéma des services d’application. Toutefois, lors de l’exécution de `aspnet_regsql.exe` à partir de la ligne de commande, vous pouvez spécifier les composants de services d’application à installer (ou supprimer). Par conséquent, si vous souhaitez ajouter uniquement les tables, les vues et les procédures stockées nécessaires à l' `SqlMembershipProvider` et aux fournisseurs de `SqlRoleProvider`, exécutez `aspnet_regsql.exe` à partir de la ligne de commande. Vous pouvez également exécuter manuellement le sous-ensemble approprié de scripts T-SQL CREATE utilisés par `aspnet_regsql.exe`. Ces scripts se trouvent dans le dossier `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` avec des noms comme `InstallCommon.sql`, `InstallMembership.sql`, `InstallRoles.sql`, `InstallProfile.sql`, `InstallSqlState.sql`, et ainsi de suite.

À ce stade, nous avons créé les objets de base de données requis par le `SqlMembershipProvider`. Toutefois, nous devons toujours indiquer à l’infrastructure d’appartenance qu’il doit utiliser le `SqlMembershipProvider` (par opposition à l' `ActiveDirectoryMembershipProvider`) et que le `SqlMembershipProvider` doit utiliser la base de données `SecurityTutorials`. Nous allons voir comment spécifier le fournisseur à utiliser et comment personnaliser les paramètres du fournisseur sélectionné à l’étape 4. Mais tout d’abord, examinons plus en détail les objets de base de données qui viennent d’être créés.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Étape 3 : examiner les tables principales du schéma

Lorsque vous travaillez avec les infrastructures d’appartenance et de rôles dans une application ASP.NET, les détails de l’implémentation sont encapsulés par le fournisseur. Dans les prochains didacticiels, nous allons interagir avec ces frameworks via les classes `Membership` et `Roles` du .NET Framework. Lorsque vous utilisez ces API de haut niveau, nous n’avons pas besoin de nous tenir au fait des détails de bas niveau, tels que les requêtes exécutées ou les tables modifiées par le `SqlMembershipProvider` et `SqlRoleProvider`.

À partir de cela, nous pourrions utiliser en toute confiance les infrastructures d’appartenance et de rôles sans avoir exploré le schéma de base de données créé à l’étape 2. Toutefois, lors de la création des tables pour stocker des données d’application, nous pouvons être amenés à créer des entités liées aux utilisateurs ou aux rôles. Elle vous permet de vous familiariser avec les schémas `SqlMembershipProvider` et `SqlRoleProvider` lors de l’établissement de contraintes de clé étrangère entre les tables de données d’application et les tables créées à l’étape 2. En outre, dans certains cas rares, nous pouvons être amenés à interagir avec l’utilisateur et les magasins de rôles directement au niveau de la base de données (plutôt que par le biais des classes `Membership` ou `Roles`).

### <a name="partitioning-the-user-store-into-applications"></a>Partitionnement du magasin de l’utilisateur en applications

Les infrastructures d’appartenance et de rôles sont conçues de façon à ce qu’un seul utilisateur et magasin de rôles puissent être partagés entre plusieurs applications différentes. Une application ASP.NET qui utilise les infrastructures d’appartenance ou de rôles doit spécifier la partition d’application à utiliser. En bref, plusieurs applications Web peuvent utiliser le même utilisateur et les mêmes magasins de rôles. La figure 11 décrit les magasins d’utilisateurs et de rôles qui sont partitionnés en trois applications : HRSite, CustomerSite et SalesSite. Ces trois applications Web ont chacune leurs propres rôles et utilisateurs uniques, mais elles stockent toutes physiquement leurs informations de compte d’utilisateur et de rôle dans les mêmes tables de base de données.

[![comptes d’utilisateur peuvent être partitionnés entre plusieurs applications](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Figure 11**: les comptes d’utilisateurs peuvent être partitionnés entre plusieurs applications ([cliquez pour afficher l’image en taille réelle](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))

La table `aspnet_Applications` est ce qui définit ces partitions. Chaque application qui utilise la base de données pour stocker les informations de compte d’utilisateur est représentée par une ligne dans cette table. La table `aspnet_Applications` contient quatre colonnes : `ApplicationId`, `ApplicationName`, `LoweredApplicationName`et `Description`.`ApplicationId` est de type [`uniqueidentifier`](https://msdn.microsoft.com/library/ms187942.aspx) et est la clé primaire de la table ; `ApplicationName` fournit un nom convivial unique pour chaque application.

Les autres tables liées à l’appartenance et au rôle renvoient au champ `ApplicationId` dans `aspnet_Applications`. Par exemple, la table `aspnet_Users`, qui contient un enregistrement pour chaque compte d’utilisateur, possède un champ de clé étrangère `ApplicationId`. Idem pour la table `aspnet_Roles`. Le champ `ApplicationId` dans ces tables spécifie la partition d’application à laquelle le compte d’utilisateur ou le rôle appartient.

### <a name="storing-user-account-information"></a>Stockage des informations de compte d’utilisateur

Les informations de compte d’utilisateur sont hébergées dans deux tables : `aspnet_Users` et `aspnet_Membership`. La table `aspnet_Users` contient des champs qui contiennent les informations de compte d’utilisateur essentielles. Les trois colonnes les plus pertinentes sont les suivantes :

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` est la clé primaire (et de type `uniqueidentifier`). `UserName` est de type `nvarchar(256)` et, avec le mot de passe, constitue les informations d’identification de l’utilisateur. (Le mot de passe d’un utilisateur est stocké dans la table `aspnet_Membership`.) `ApplicationId` lie le compte d’utilisateur à une application particulière dans `aspnet_Applications`. Il existe une [contrainte de`UNIQUE`](https://msdn.microsoft.com/library/ms191166.aspx) composite sur les colonnes `UserName` et `ApplicationId`. Cela permet de s’assurer que dans une application donnée chaque nom d’utilisateur est unique, mais qu’il permet d’utiliser le même `UserName` dans différentes applications.

La table `aspnet_Membership` contient des informations supplémentaires sur le compte d’utilisateur, telles que le mot de passe de l’utilisateur, l’adresse de messagerie, la date et l’heure de la dernière connexion, etc. Il existe une correspondance un-à-un entre les enregistrements dans les tables `aspnet_Users` et `aspnet_Membership`. Cette relation est assurée par le champ `UserId` dans `aspnet_Membership`, qui sert de clé primaire de la table. À l’instar de la table `aspnet_Users`, `aspnet_Membership` comprend un champ `ApplicationId` qui associe ces informations à une partition d’application particulière.

### <a name="securing-passwords"></a>Sécurisation des mots de passe

Les informations de mot de passe sont stockées dans la table `aspnet_Membership`. Le `SqlMembershipProvider` permet de stocker les mots de passe dans la base de données à l’aide de l’une des trois techniques suivantes :

- **Clear** : le mot de passe est stocké dans la base de données en tant que texte brut. Je déconseille fortement d’utiliser cette option. Si la base de données est compromise, qu’il s’agisse d’un pirate qui trouve une porte dérobée ou un employé mécontent disposant d’un accès à la base de données, les informations d’identification de chaque utilisateur sont là pour le prélèvement.
- Les mots de passe **hachés** sont hachés à l’aide d’un algorithme de hachage unidirectionnel et d’une valeur Salt générée de façon aléatoire. Cette valeur hachée (avec le Salt) est stockée dans la base de données.
- **Chiffré** : une version chiffrée du mot de passe est stockée dans la base de données.

La technique de stockage du mot de passe utilisée dépend des paramètres de `SqlMembershipProvider` spécifiés dans `Web.config`. Nous allons examiner la personnalisation des paramètres de `SqlMembershipProvider` à l’étape 4. Le comportement par défaut consiste à stocker le hachage du mot de passe.

Les colonnes responsables du stockage du mot de passe sont `Password`, `PasswordFormat`et `PasswordSalt`. `PasswordFormat` est un champ de type `int` dont la valeur indique la technique utilisée pour stocker le mot de passe : 0 pour Clear ; 1 pour le hachage ; 2 pour le chiffrement. `PasswordSalt` est assignée à une chaîne générée de manière aléatoire, quelle que soit la technique utilisée pour le stockage du mot de passe ; la valeur de `PasswordSalt` est utilisée uniquement lors du calcul du hachage du mot de passe. Enfin, la colonne `Password` contient les données de mot de passe réelles, qu’il s’agit du mot de passe en texte brut, du hachage du mot de passe ou du mot de passe chiffré.

Le tableau 1 illustre ce à quoi peuvent ressembler ces trois colonnes pour les différentes techniques de stockage lors du stockage du mot de passe MySecret ! .

| **Technique de stockage&lt;\_O3A\_p/&gt;** | **Mot de passe&lt;\_O3A\_p/&gt;** | **PasswordFormat&lt;\_O3A\_p/&gt;** | **PasswordSalt&lt;\_O3A\_p/&gt;** |
| --- | --- | --- | --- |
| Effacer | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| Haché | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Chiffré | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tableau 1**: exemples de valeurs pour les champs liés au mot de passe lors du stockage du mot de passe mysecret !

> [!NOTE]
> Le chiffrement ou l’algorithme de hachage particulier utilisé par le `SqlMembershipProvider` est déterminé par les paramètres de l’élément `<machineKey>`. Nous avons abordé cet élément de configuration à l' <a id="Tutorial3"> </a>étape 3 du didacticiel configuration de l' [*authentification par formulaire et Rubriques avancées*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) .

### <a name="storing-roles-and-role-associations"></a>Stockage des rôles et des associations de rôles

L’infrastructure de rôles permet aux développeurs de définir un ensemble de rôles et de spécifier les utilisateurs qui appartiennent aux rôles. Ces informations sont capturées dans la base de données par le biais de deux tables : `aspnet_Roles` et `aspnet_UsersInRoles`. Chaque enregistrement de la table `aspnet_Roles` représente un rôle pour une application particulière. À l’instar du tableau `aspnet_Users`, le tableau `aspnet_Roles` comporte trois colonnes pertinentes pour notre discussion :

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` est la clé primaire (et de type `uniqueidentifier`). `RoleName` est de type `nvarchar(256)`. Et `ApplicationId` lie le compte d’utilisateur à une application particulière dans `aspnet_Applications`. Il existe une contrainte de `UNIQUE` composite sur les colonnes `RoleName` et `ApplicationId`, garantissant que chaque nom de rôle est unique dans une application donnée.

La table `aspnet_UsersInRoles` sert de mappage entre les utilisateurs et les rôles. Il n’y a que deux colonnes : `UserId` et `RoleId`-et ensemble, elles constituent une clé primaire composite.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Étape 4 : spécification du fournisseur et personnalisation de ses paramètres

Tous les frameworks qui prennent en charge le modèle de fournisseur (tels que les infrastructures d’appartenance et de rôles) n’ont pas de détails d’implémentation eux-mêmes et délèguent plutôt cette responsabilité à une classe de fournisseur. Dans le cadre de l’infrastructure d’appartenance, la classe `Membership` définit l’API pour la gestion des comptes d’utilisateur, mais elle n’interagit pas directement avec les magasins de l’utilisateur. Au lieu de cela, les méthodes de la classe `Membership` déplacent la demande au fournisseur configuré. nous allons utiliser le `SqlMembershipProvider`. Quand vous appelez l’une des méthodes de la classe `Membership`, comment l’infrastructure d’appartenance sait-elle déléguer l’appel à la `SqlMembershipProvider`?

La classe `Membership` a une [propriété`Providers`](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) qui contient une référence à toutes les classes de fournisseur inscrites pouvant être utilisées par l’infrastructure d’appartenance. Chaque fournisseur inscrit a un nom et un type associés. Le nom offre une méthode conviviale pour référencer un fournisseur particulier dans la collection `Providers`, tandis que le type identifie la classe de fournisseur. En outre, chaque fournisseur inscrit peut inclure des paramètres de configuration. Les paramètres de configuration de l’infrastructure d’appartenance incluent `PasswordFormat` et `requiresUniqueEmail`, entre autres. Pour obtenir la liste complète des paramètres de configuration utilisés par le `SqlMembershipProvider`, consultez le tableau 2.

Le contenu de la propriété `Providers` est spécifié via les paramètres de configuration de l’application Web. Par défaut, toutes les applications Web ont un fournisseur nommé `AspNetSqlMembershipProvider` de type `SqlMembershipProvider`. Ce fournisseur d’appartenances par défaut est inscrit dans `machine.config` (situé dans `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`) :

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Comme le montre le balisage ci-dessus, l' [élément`<membership>`](https://msdn.microsoft.com/library/1b9hw62f.aspx) définit les paramètres de configuration de l’infrastructure d’appartenance, tandis que l' [élément enfant`<providers>`](https://msdn.microsoft.com/library/6d4936ht.aspx) spécifie les fournisseurs inscrits. Les fournisseurs peuvent être ajoutés ou supprimés à l’aide des éléments [`<add>`](https://msdn.microsoft.com/library/whae3t94.aspx) ou [`<remove>`](https://msdn.microsoft.com/library/aykw9a6d.aspx) ; Utilisez l’élément [`<clear>`](https://msdn.microsoft.com/library/t062y6yc.aspx) pour supprimer tous les fournisseurs actuellement inscrits. Comme le montre le balisage ci-dessus, `machine.config` ajoute un fournisseur nommé `AspNetSqlMembershipProvider` de type `SqlMembershipProvider`.

Outre les attributs `name` et `type`, l’élément `<add>` contient des attributs qui définissent les valeurs de divers paramètres de configuration. Le tableau 2 répertorie les paramètres de configuration disponibles pour `SqlMembershipProvider`, ainsi qu’une description de chacun d’eux.

> [!NOTE]
> Toutes les valeurs par défaut indiquées dans le tableau 2 font référence aux valeurs par défaut définies dans la classe `SqlMembershipProvider`. Notez que tous les paramètres de configuration de `AspNetSqlMembershipProvider` correspondent pas aux valeurs par défaut de la classe `SqlMembershipProvider`. Par exemple, s’il n’est pas spécifié dans un fournisseur d’appartenances, le paramètre `requiresUniqueEmail` a la valeur true par défaut. Toutefois, l' `AspNetSqlMembershipProvider` remplace cette valeur par défaut en spécifiant explicitement une valeur de `false`.

| **Définition de&lt;\_O3A\_p/&gt;** | **Description&lt;\_O3A\_p/&gt;** |
| --- | --- |
| `ApplicationName` | Souvenez-vous que l’infrastructure d’appartenance permet de partitionner un seul magasin d’utilisateur entre plusieurs applications. Ce paramètre indique le nom de la partition d’application utilisée par le fournisseur d’appartenances. Si cette valeur n’est pas explicitement spécifiée, elle est définie, au moment de l’exécution, à la valeur du chemin d’accès de la racine virtuelle de l’application. |
| `commandTimeout` | Spécifie la valeur du délai d’expiration de la commande SQL (en secondes). La valeur par défaut est 30. |
| `connectionStringName` | Nom de la chaîne de connexion dans l’élément `<connectionStrings>` à utiliser pour se connecter à la base de données du magasin de l’utilisateur. Cette valeur est requise. |
| `description` | Fournit une description conviviale du fournisseur inscrit. |
| `enablePasswordRetrieval` | Spécifie si les utilisateurs peuvent récupérer leur mot de passe oublié. La valeur par défaut est `false`. |
| `enablePasswordReset` | Indique si les utilisateurs sont autorisés à réinitialiser leur mot de passe. La valeur par défaut est `true`. |
| `maxInvalidPasswordAttempts` | Nombre maximal de tentatives de connexion qui peuvent se produire pour un utilisateur donné pendant la `passwordAttemptWindow` spécifiée avant que l’utilisateur ne soit verrouillé. La valeur par défaut est 5. |
| `minRequiredNonalphanumericCharacters` | Nombre minimal de caractères non alphanumériques qui doivent apparaître dans le mot de passe d’un utilisateur. Cette valeur doit être comprise entre 0 et 128 ; la valeur par défaut est 1. |
| `minRequiredPasswordLength` | Nombre minimal de caractères requis dans un mot de passe. Cette valeur doit être comprise entre 0 et 128 ; la valeur par défaut est 7. |
| `name` | Nom du fournisseur inscrit. Cette valeur est requise. |
| `passwordAttemptWindow` | Nombre de minutes pendant lesquelles les échecs de tentatives de connexion sont suivis. Si un utilisateur fournit des informations d’identification de connexion non valides `maxInvalidPasswordAttempts` fois dans cette fenêtre spécifiée, il est verrouillé. La valeur par défaut est 10. |
| `PasswordFormat` | Format de stockage du mot de passe : `Clear`, `Hashed`ou `Encrypted`. La valeur par défaut est `Hashed`. |
| `passwordStrengthRegularExpression` | Si elle est fournie, cette expression régulière est utilisée pour évaluer la force du mot de passe sélectionné par l’utilisateur lors de la création d’un nouveau compte ou lors de la modification de son mot de passe. La valeur par défaut est une chaîne vide. |
| `requiresQuestionAndAnswer` | Spécifie si un utilisateur doit répondre à sa question de sécurité lors de la récupération ou de la réinitialisation de son mot de passe. La valeur par défaut est `true`. |
| `requiresUniqueEmail` | Indique si tous les comptes d’utilisateur d’une partition d’application donnée doivent avoir une adresse de messagerie unique. La valeur par défaut est `true`. |
| `type` | Spécifie le type du fournisseur. Cette valeur est requise. |

**Tableau 2**: paramètres de configuration d’appartenance et de `SqlMembershipProvider`

En plus de `AspNetSqlMembershipProvider`, d’autres fournisseurs d’appartenances peuvent être inscrits au niveau application par application en ajoutant un balisage similaire au fichier `Web.config`.

> [!NOTE]
> L’infrastructure Roles fonctionne à peu près de la même façon : il existe un fournisseur de rôle inscrit par défaut dans `machine.config` et les fournisseurs inscrits peuvent être personnalisés pour chaque application dans `Web.config`. Nous examinerons en détail l’infrastructure des rôles et son balisage de configuration dans un prochain didacticiel.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Personnalisation des paramètres de l'`SqlMembershipProvider`

L’attribut `connectionStringName` de l' `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) par défaut est défini sur `LocalSqlServer`. À l’instar du fournisseur `AspNetSqlMembershipProvider`, le nom de la chaîne de connexion `LocalSqlServer` est défini dans `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Comme vous pouvez le voir, cette chaîne de connexion définit une base de données SQL 2005 Express Edition située dans | DataDirectory | aspnetdb. mdf. La chaîne | DataDirectory | est traduite au moment de l’exécution pour pointer vers le répertoire `~/App_Data/`, donc le chemin d’accès de la base de données | DataDirectory | aspnetdb. mdf traduit en `~/App_Data`/`aspnet.mdf`.

Si nous n’avons pas spécifié d’informations sur le fournisseur d’appartenances dans le fichier `Web.config` de l’application, l’application utilise le fournisseur d’appartenances inscrit par défaut, `AspNetSqlMembershipProvider`. Si la base de données `~/App_Data/aspnet.mdf` n’existe pas, le runtime ASP.NET le crée automatiquement et ajoute le schéma des services d’application. Toutefois, nous ne souhaitons pas utiliser la base de données `aspnet.mdf` ; au lieu de cela, nous souhaitons utiliser la base de données `SecurityTutorials.mdf` que nous avons créée à l’étape 2. Cette modification peut être effectuée de deux manières :

- <strong>Spécifiez une valeur pour le</strong> nom de la chaîne de connexion<strong>`LocalSqlServer`</strong> <strong>dans</strong> <strong>`Web.config`</strong> <strong>.</strong> En remplaçant la valeur de nom de la chaîne de connexion `LocalSqlServer` dans `Web.config`, nous pouvons utiliser le fournisseur d’appartenances inscrit par défaut (`AspNetSqlMembershipProvider`) et le faire fonctionner correctement avec la base de données `SecurityTutorials.mdf`. Cette approche est correcte si vous avez du contenu avec les paramètres de configuration spécifiés par `AspNetSqlMembershipProvider`. Pour plus d’informations sur cette technique, consultez le billet de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [configuring ASP.NET 2,0 Services d’application to use SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Ajoutez un nouveau fournisseur inscrit de type</strong> <strong>`SqlMembershipProvider`</strong> <strong>et configurez son</strong> paramètre<strong>`connectionStringName`</strong> <strong>pour qu’il pointe vers la</strong> <strong>base de données</strong> <strong>`SecurityTutorials.mdf`</strong>. Cette approche est utile dans les scénarios où vous souhaitez personnaliser d’autres propriétés de configuration en plus de la chaîne de connexion à la base de données. Dans mes propres projets, j’utilise toujours cette approche en raison de sa flexibilité et de sa lisibilité.

Avant de pouvoir ajouter un nouveau fournisseur inscrit qui référence la base de données `SecurityTutorials.mdf`, nous devons d’abord ajouter une valeur de chaîne de connexion appropriée dans la section `<connectionStrings>` de `Web.config`. Le balisage suivant ajoute une nouvelle chaîne de connexion nommée `SecurityTutorialsConnectionString` qui fait référence à la base de données SQL Server 2005 Express Edition `SecurityTutorials.mdf` dans le dossier `App_Data`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Si vous utilisez un autre fichier de base de données, mettez à jour la chaîne de connexion en fonction des besoins. Pour plus d’informations sur la façon de former la chaîne de connexion correcte, consultez [connectionStrings.com](http://www.connectionstrings.com/).

Ensuite, ajoutez la balise de configuration d’appartenance suivante au fichier `Web.config`. Ce balisage inscrit un nouveau fournisseur nommé `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Outre l’inscription du fournisseur `SecurityTutorialsSqlMembershipProvider`, le balisage ci-dessus définit le `SecurityTutorialsSqlMembershipProvider` comme fournisseur par défaut (via l’attribut `defaultProvider` de l’élément `<membership>`). Rappelez-vous que l’infrastructure d’appartenance peut avoir plusieurs fournisseurs inscrits. Étant donné que `AspNetSqlMembershipProvider` est inscrit en tant que premier fournisseur dans `machine.config`, il sert de fournisseur par défaut, sauf indication contraire.

Actuellement, notre application a deux fournisseurs inscrits : `AspNetSqlMembershipProvider` et `SecurityTutorialsSqlMembershipProvider`. Toutefois, avant d’inscrire le fournisseur `SecurityTutorialsSqlMembershipProvider`, nous aurions pu éliminer tous les fournisseurs précédemment inscrits en ajoutant un [élément`<clear />`](https://msdn.microsoft.com/library/t062y6yc.aspx) juste avant notre élément `<add>`. Cela supprime le `AspNetSqlMembershipProvider` de la liste des fournisseurs inscrits, ce qui signifie que le `SecurityTutorialsSqlMembershipProvider` est le seul fournisseur d’appartenances inscrit. Si nous avons utilisé cette approche, il n’est pas nécessaire de marquer le `SecurityTutorialsSqlMembershipProvider` comme fournisseur par défaut, car il s’agit du seul fournisseur d’appartenances inscrit. Pour plus d’informations sur l’utilisation de `<clear />`, consultez [utilisation de `<clear />` lors](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)de l’ajout de fournisseurs.

Notez que le paramètre de `connectionStringName` de l' `SecurityTutorialsSqlMembershipProvider`fait référence au nom de la chaîne de connexion `SecurityTutorialsConnectionString` qui vient d’être ajouté, et que son paramètre de `applicationName` a été défini sur la valeur SecurityTutorials. En outre, le paramètre `requiresUniqueEmail` a été défini sur `true`. Toutes les autres options de configuration sont identiques aux valeurs de `AspNetSqlMembershipProvider`. N’hésitez pas à apporter des modifications de configuration ici, si vous le souhaitez. Par exemple, vous pouvez renforcer la force du mot de passe en exigeant deux caractères non alphanumériques au lieu d’un, ou en accroissant la longueur du mot de passe à huit caractères au lieu de sept.

> [!NOTE]
> Souvenez-vous que l’infrastructure d’appartenance permet de partitionner un seul magasin d’utilisateur entre plusieurs applications. Le paramètre `applicationName` du fournisseur d’appartenances indique l’application utilisée par le fournisseur lors de l’utilisation du magasin de l’utilisateur. Il est important de définir explicitement une valeur pour le paramètre de configuration `applicationName`, car si la `applicationName` n’est pas définie explicitement, elle est assignée au chemin d’accès racine virtuel de l’application Web au moment de l’exécution. Cela fonctionne correctement tant que le chemin d’accès de la racine virtuelle de l’application ne change pas, mais si vous déplacez l’application vers un chemin d’accès différent, le paramètre `applicationName` change également. Dans ce cas, le fournisseur d’appartenances commencera à utiliser une partition d’application différente de celle précédemment utilisée. Les comptes d’utilisateur créés avant le déplacement se trouvent dans une partition d’application différente et ces utilisateurs ne pourront plus se connecter au site. Pour une discussion plus approfondie sur ce sujet, consultez [toujours définir la propriété `applicationName` lors de la configuration de l’appartenance à ASP.NET 2,0 et d’autres fournisseurs](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Récapitulatif

À ce stade, nous disposons d’une base de données avec les services d’application configurés (`SecurityTutorials.mdf`) et nous avons configuré notre application Web afin que l’infrastructure d’appartenance utilise le fournisseur `SecurityTutorialsSqlMembershipProvider` que nous venons d’inscrire. Ce fournisseur inscrit est de type `SqlMembershipProvider` et a son `connectionStringName` défini sur la chaîne de connexion appropriée (`SecurityTutorialsConnectionString`) et sa valeur de `applicationName` définie explicitement.

Nous sommes maintenant prêts à utiliser l’infrastructure d’appartenance de notre application. Dans le didacticiel suivant, nous allons examiner comment créer des comptes d’utilisateur. Après cela, nous explorerons l’authentification des utilisateurs, l’exécution de l’autorisation basée sur l’utilisateur et le stockage d’informations supplémentaires relatives à l’utilisateur dans la base de données.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Définir toujours la propriété `applicationName` lors de la configuration de l’appartenance à ASP.NET 2,0 et d’autres fournisseurs](https://weblogs.asp.net/scottgu/443634)
- [Configuration de ASP.NET 2,0 Services d’application pour utiliser SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Télécharger SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Examen de l’appartenance, des rôles et du profil de ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Élément `<add>` pour les fournisseurs pour l’appartenance](https://msdn.microsoft.com/library/whae3t94.aspx)
- [Élément `<membership>`](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [Élément `<providers>` pour l’appartenance](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Utilisation de `<clear />` lors de l’ajout de fournisseurs](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Utilisation directe de l' `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formation vidéo sur les rubriques contenues dans ce didacticiel

- [Présentation des appartenances d’ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configuration de SQL pour l’utilisation de schémas d’appartenance](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Modification des paramètres d’appartenance dans le schéma d’appartenance par défaut](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Alicja maziarz. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](storing-additional-user-information-cs.md)
> [Suivant](creating-user-accounts-vb.md)
