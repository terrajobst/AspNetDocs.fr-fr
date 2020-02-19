---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migration d’un site Web existant de l’appartenance SQL à ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: Ce didacticiel décrit les étapes de migration d’une application Web existante avec les données d’utilisateur et de rôle créées à l’aide de l’appartenance SQL au nouvel ASP.NET Identity...
ms.author: riande
ms.date: 12/19/2014
ms.custom: seoapril2019
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 633229cc4311d151121bf6a91b9fa8aeecca1197
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456151"
---
# <a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migration d’un site web existant de l’appartenance SQL vers ASP.NET Identity

par [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> Ce didacticiel décrit les étapes de migration d’une application Web existante avec les données d’utilisateur et de rôle créées à l’aide de l’appartenance SQL au nouveau système de ASP.NET Identity. Cette approche implique de remplacer le schéma de base de données existant par celui qui est requis par le ASP.NET Identity et de se connecter aux anciennes/nouvelles classes. Après l’adoption de cette approche, une fois votre base de données migrée, les futures mises à jour de l’identité seront gérées sans effort.

Pour ce didacticiel, nous allons utiliser un modèle d’application Web (Web Forms) créé à l’aide de Visual Studio 2010 pour créer des données d’utilisateur et de rôle. Nous utiliserons ensuite des scripts SQL pour migrer la base de données existante vers les tables nécessaires au système d’identité. Ensuite, nous allons installer les packages NuGet nécessaires et ajouter de nouvelles pages de gestion des comptes qui utilisent le système d’identité pour la gestion des appartenances. En guise de test de migration, les utilisateurs créés à l’aide de l’appartenance SQL doivent pouvoir se connecter et de nouveaux utilisateurs doivent être en mesure de s’inscrire. Vous trouverez l’exemple complet [ici](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/). Voir aussi [migration de l’appartenance à ASP.net vers ASP.net Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Prise en main

### <a name="creating-an-application-with-sql-membership"></a>Création d’une application avec l’appartenance SQL

1. Nous devons commencer par une application existante qui utilise l’appartenance SQL et des données d’utilisateur et de rôle. Dans le cadre de cet article, nous allons créer une application Web dans Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. À l’aide de l’outil de configuration de ASP.NET, créez 2 utilisateurs : **oldAdminUser** et **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Créez un rôle nommé admin et ajoutez « oldAdminUser » en tant qu’utilisateur dans ce rôle.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Créez une section admin du site avec un default. aspx. Définissez la balise d’autorisation dans le fichier Web. config pour activer l’accès uniquement aux utilisateurs dans les rôles d’administrateur. Vous trouverez plus d’informations ici [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Affichez la base de données dans Explorateur de serveurs pour comprendre les tables créées par le système d’appartenance SQL. Les données de connexion de l’utilisateur sont stockées dans les tables ASPNET\_Users et ASPNET\_Membership, tandis que les données de rôle sont stockées dans la table de rôles\_Aspnet. Informations sur les utilisateurs dans lesquels les rôles sont stockés dans la table ASPNET\_UsersInRoles. Pour la gestion des appartenances de base, il suffit de porter les informations des tables ci-dessus dans le système de ASP.NET Identity.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migration vers Visual Studio 2013

1. Installez Visual Studio Express 2013 pour Web ou Visual Studio 2013 avec les [dernières mises à jour](https://www.microsoft.com/download/details.aspx?id=44921).
2. Ouvrez le projet ci-dessus dans votre version installée de Visual Studio. Si SQL Server Express n’est pas installé sur l’ordinateur, une invite s’affiche lorsque vous ouvrez le projet, car la chaîne de connexion utilise SQL Express. Vous pouvez choisir d’installer SQL Express ou de contourner la chaîne de connexion à la base de données locale. Pour cet article, nous allons le remplacer par la base de données locale.
3. Ouvrez le fichier Web. config et modifiez la chaîne de connexion à partir de. SQLExpress vers (base de données locale) v 11.0. Supprimez « user instance = true » de la chaîne de connexion.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Ouvrez Explorateur de serveurs et vérifiez que le schéma et les données de la table peuvent être observés.
5. Le système ASP.NET Identity fonctionne avec la version 4,5 ou une version ultérieure du Framework. Reciblez l’application sur 4,5 ou une version ultérieure.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Générez le projet pour vérifier qu’il n’y a pas d’erreurs.

### <a name="installing-the-nuget-packages"></a>Installation des packages NuGet

1. Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet &gt; **gérer les packages NuGet**. Dans la zone de recherche, entrez « Asp.net Identity ». Sélectionnez le package dans la liste des résultats, puis cliquez sur installer. Acceptez le contrat de licence en cliquant sur le bouton « J’accepte ». Notez que ce package installe les packages de dépendances : EntityFramework et Microsoft ASP.NET principal d’identité. Installez de la même façon les packages suivants (ignorez les 4 derniers packages OWIN si vous ne souhaitez pas activer la connexion OAuth) :

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrer la base de données vers le nouveau système d’identité

L’étape suivante consiste à migrer la base de données existante vers un schéma requis par le système ASP.NET Identity. Pour ce faire, nous exécutons un script SQL qui comporte un ensemble de commandes permettant de créer des tables et de migrer les informations utilisateur existantes vers les nouvelles tables. Le fichier de script se trouve [ici](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Ce fichier de script est spécifique à cet exemple. Si le schéma des tables créées à l’aide de l’appartenance SQL est personnalisé ou modifié, les scripts doivent être modifiés en conséquence.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Comment générer le script SQL pour la migration de schéma

Pour que les classes ASP.NET Identity soient prêtes à l’emploi avec les données des utilisateurs existants, nous devons migrer le schéma de base de données vers celui requis par ASP.NET Identity. Pour ce faire, vous pouvez ajouter de nouvelles tables et copier les informations existantes dans ces tables. Par défaut ASP.NET Identity utilise EntityFramework pour mapper les classes de modèle d’identité à la base de données pour stocker/récupérer des informations. Ces classes de modèle implémentent les interfaces d’identité principales qui définissent les objets utilisateur et rôle. Les tables et les colonnes de la base de données sont basées sur ces classes de modèle. Les classes de modèle EntityFramework dans Identity v 2.1.0 et leurs propriétés sont définies ci-dessous.

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| ID | chaîne | ID | RoleId | ProviderKey | ID |
| Nom d’utilisateur | chaîne | Nom | UserId | UserId | ClaimType |
| PasswordHash | chaîne |  |  | LoginProvider | ClaimValue |
| SecurityStamp | chaîne |  |  |  | ID de\_de l’utilisateur |
| Email | chaîne |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | chaîne |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Nous devons disposer de tables pour chacun de ces modèles avec des colonnes correspondant aux propriétés. Le mappage entre les classes et les tables est défini dans la méthode `OnModelCreating` de l' `IdentityDBContext`. C’est ce que l’on appelle la méthode de la configuration de l’API Fluent. vous trouverez [ici](https://msdn.microsoft.com/data/jj591617.aspx)des informations supplémentaires. La configuration des classes est indiquée ci-dessous

| **Classe** | **Table** | **Clé primaire** | **Clé étrangère** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | ID |  |
| IdentityRole | AspnetRoles | ID |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | ID d'\_utilisateur-&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey+UserId + LoginProvider | AspnetUsers UserId-&gt; |
| IdentityUserClaim | AspnetUserClaims | ID | ID d'\_utilisateur-&gt;AspnetUsers |

Ces informations nous permettent de créer des instructions SQL pour créer des tables. Nous pouvons soit écrire chaque instruction individuellement, soit générer le script entier à l’aide des commandes PowerShell EntityFramework que nous pouvons ensuite modifier en fonction des besoins. Pour ce faire, dans VS, ouvrez la **console du gestionnaire de package** à partir du menu **affichage** ou **Outils** .

- Exécutez la commande « Activer-migrations » pour activer les migrations EntityFramework.
- Exécutez la commande « Add-migration initial » qui crée le code d’installation initial pour créer la C#base de données dans/VB.
- La dernière étape consiste à exécuter la commande « Update-Database – script » qui génère le script SQL basé sur les classes de modèle.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Ce script de génération de base de données peut être utilisé comme un début où nous allons apporter des modifications supplémentaires pour ajouter de nouvelles colonnes et copier des données. L’avantage de cela est que nous générons la table `_MigrationHistory` qui est utilisée par EntityFramework pour modifier le schéma de base de données lorsque les classes de modèle changent pour les futures versions des mises en production d’identité.

Les informations de l’utilisateur d’appartenance SQL avaient d’autres propriétés en plus de celles de la classe de modèle d’utilisateur d’identité, à savoir la messagerie, les tentatives de mot de passe, la date de la dernière connexion, la date du dernier verrouillage, etc. Il s’agit d’informations utiles et nous aimerions qu’elles soient reportées sur le système d’identité. Pour ce faire, vous pouvez ajouter des propriétés supplémentaires au modèle utilisateur et les mapper aux colonnes de la table dans la base de données. Pour ce faire, nous pouvons ajouter une classe qui sous-classe le modèle de `IdentityUser`. Nous pouvons ajouter les propriétés à cette classe personnalisée et modifier le script SQL pour ajouter les colonnes correspondantes lors de la création de la table. Le code de cette classe est décrit plus en détail dans l’article. Le script SQL permettant de créer la table `AspnetUsers` après l’ajout des nouvelles propriétés est

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Ensuite, nous devons copier les informations existantes de la base de données d’appartenance SQL vers les tables récemment ajoutées pour l’identité. Pour ce faire, vous pouvez copier des données directement d’une table vers une autre à l’aide de SQL. Pour ajouter des données dans les lignes de la table, nous utilisons la construction `INSERT INTO [Table]`. Pour effectuer une copie à partir d’une autre table, nous pouvons utiliser l’instruction `INSERT INTO` avec l’instruction `SELECT`. Pour obtenir toutes les informations utilisateur nécessaires, interrogez les tables *aspnet\_Users* et *ASPNET\_Membership* , puis copiez les données dans la table *AspNetUsers* . Nous utilisons les `INSERT INTO` et `SELECT`, ainsi que les instructions `JOIN` et `LEFT OUTER JOIN`. Pour plus d’informations sur l’interrogation et la copie de données entre des tables, consultez [ce](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) lien. En outre, les tables AspnetUserLogins et AspnetUserClaims sont vides, car il n’y a pas d’informations dans l’appartenance SQL qui est mappée à ce paramètre par défaut. La seule information copiée concerne les utilisateurs et les rôles. Pour le projet créé au cours des étapes précédentes, la requête SQL permettant de copier des informations dans la table users est

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Dans l’instruction SQL ci-dessus, les informations relatives à chaque utilisateur des tables *aspnet\_Users* et *ASPNET\_Membership* sont copiées dans les colonnes de la table *AspnetUsers* . La seule modification effectuée ici est lorsque nous copions le mot de passe. Étant donné que l’algorithme de chiffrement des mots de passe dans l’appartenance SQL a utilisé « PasswordSalt » et « PasswordFormat », nous copions le mot de passe avec le mot de passe haché afin qu’il puisse être utilisé pour déchiffrer le mot de passe par identité. Cela est expliqué plus en détail dans l’article lors de la connexion d’un hachage de mot de passe personnalisé.

Ce fichier de script est spécifique à cet exemple. Pour les applications qui ont des tables supplémentaires, les développeurs peuvent suivre une approche similaire pour ajouter des propriétés supplémentaires sur la classe de modèle utilisateur et les mapper aux colonnes de la table AspnetUsers. Pour exécuter le script,

1. Ouvrez l'Explorateur de serveurs. Développez la connexion « ApplicationServices » pour afficher les tables. Cliquez avec le bouton droit sur le nœud tables et sélectionnez l’option « nouvelle requête ».

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Dans la fenêtre de requête, copiez et collez l’intégralité du script SQL à partir du fichier migrations. Sql. Exécutez le fichier de script en appuyant sur la flèche « exécuter ».

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Actualisez la fenêtre de Explorateur de serveurs. Cinq nouvelles tables sont créées dans la base de données.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Voici comment les informations contenues dans les tables d’appartenance SQL sont mappées sur le nouveau système d’identité.

    Rôles de\_ASPNET--&gt; AspNetRoles

    ASP\_netusers et ASP\_netmembre--&gt; AspNetUsers

    ASPNET\_UserInRoles--&gt; AspNetUserRoles

    Comme expliqué dans la section ci-dessus, les tables AspNetUserClaims et AspNetUserLogins sont vides. Le champ « discriminant » dans la table AspNetUser doit correspondre au nom de la classe de modèle qui est défini comme étape suivante. En outre, la colonne PasswordHash se présente sous la forme « mot de passe chiffré | Salt de mot de passe | format du mot de passe ». Cela vous permet d’utiliser une logique de chiffrement d’appartenance SQL spéciale afin que vous puissiez réutiliser les anciens mots de passe. Cela est expliqué dans plus loin dans cet article.

### <a name="creating-models-and-membership-pages"></a>Création de modèles et de pages d’appartenance

Comme indiqué précédemment, la fonctionnalité d’identité utilise Entity Framework pour communiquer avec la base de données afin de stocker les informations de compte par défaut. Pour travailler avec des données existantes dans la table, nous devons créer des classes de modèle qui mappent aux tables et les raccordent dans le système d’identité. Dans le cadre du contrat d’identité, les classes de modèle doivent implémenter les interfaces définies dans la dll Identity. Core ou étendre l’implémentation existante de ces interfaces disponibles dans Microsoft. AspNet. Identity. EntityFramework.

Dans notre exemple, les tables AspNetRoles, AspNetUserClaims, AspNetLogins et AspNetUserRole ont des colonnes similaires à l’implémentation existante du système d’identité. Par conséquent, nous pouvons réutiliser les classes existantes pour les mapper à ces tables. La table AspNetUser contient des colonnes supplémentaires qui sont utilisées pour stocker des informations supplémentaires à partir des tables d’appartenance SQL. Cela peut être mappé en créant une classe de modèle qui étend l’implémentation existante de’IdentityUser’et ajouter les propriétés supplémentaires.

1. Créez un dossier Models dans le projet et ajoutez un utilisateur de classe. Le nom de la classe doit correspondre aux données ajoutées dans la colonne « discriminant » de la table « AspnetUsers ».

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    La classe User doit étendre la classe IdentityUser trouvée dans la dll *Microsoft. Aspnet. Identity. EntityFramework* . Déclarez les propriétés de la classe qui mappent aux colonnes AspNetUser. Les propriétés ID, username, PasswordHash et SecurityStamp sont définies dans le IdentityUser et sont donc omises. Voici le code de la classe utilisateur qui possède toutes les propriétés

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Une classe DbContext Entity Framework est requise pour conserver les données des modèles dans les tables et récupérer les données des tables pour remplir les modèles. La dll *Microsoft. Aspnet. Identity. EntityFramework* définit la classe IdentityDbContext qui interagit avec les tables d’identité pour récupérer et stocker des informations. Le&gt; IdentityDbContext&lt;TUser prend une classe’TUser’qui peut être toute classe qui étend la classe IdentityUser.

    Créez une nouvelle classe ApplicationDBContext qui étend IdentityDbContext sous le dossier « Models », en transmettant la classe « User » créée à l’étape 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. La gestion des utilisateurs dans le nouveau système d’identité s’effectue à l’aide de la classe UserManager&lt;TUser&gt; définie dans la dll *Microsoft. Aspnet. Identity. EntityFramework* . Nous devons créer une classe personnalisée qui étend UserManager, en transmettant la classe « User » créée à l’étape 1.

    Dans le dossier Models, créez une nouvelle classe UserManager qui étend UserManager&lt;utilisateur&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Les mots de passe des utilisateurs de l’application sont chiffrés et stockés dans la base de données. L’algorithme de chiffrement utilisé dans l’appartenance SQL est différent de celui du nouveau système d’identité. Pour réutiliser les anciens mots de passe, nous devons déchiffrer les mots de passe de manière sélective lorsque les anciens utilisateurs se connectent à l’aide de l’algorithme d’appartenances SQL lors de l’utilisation de l’algorithme de chiffrement pour les nouveaux utilisateurs.

    La classe UserManager a une propriété « PasswordHasher » qui stocke une instance d’une classe qui implémente l’interface « IPasswordHasher ». Cela permet de chiffrer/déchiffrer les mots de passe lors des transactions d’authentification utilisateur. Dans la classe UserManager définie à l’étape 3, créez une nouvelle classe SQLPasswordHasher et copiez le code ci-dessous.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Résolvez les erreurs de compilation en important les espaces de noms System. Text et System. Security. Cryptography.

    La méthode EncodePassword chiffre le mot de passe en fonction de l’implémentation de chiffrement par défaut de l’appartenance SQL. Cette erreur provient de la DLL System. Web. Si l’ancienne application utilisait une implémentation personnalisée, elle devrait être reflétée ici. Nous devons définir deux autres méthodes *HashPassword* et *VerifyHashedPassword* qui utilisent la méthode *EncodePassword* pour hacher un mot de passe donné ou vérifier un mot de passe en texte brut avec celui existant dans la base de données.

    Le système d’appartenance SQL a utilisé PasswordHash, PasswordSalt et PasswordFormat pour hacher le mot de passe entré par les utilisateurs lorsqu’ils inscrivent ou modifient leur mot de passe. Pendant la migration, les trois champs sont stockés dans la colonne PasswordHash de la table AspNetUser, séparés par le caractère « | ». Lorsqu’un utilisateur se connecte et que le mot de passe comporte ces champs, nous utilisons le chiffrement d’appartenance SQL pour vérifier le mot de passe. dans le cas contraire, nous utilisons le chiffrement par défaut du système d’identité pour vérifier le mot de passe. De cette façon, les anciens utilisateurs n’ont pas besoin de modifier leurs mots de passe une fois l’application migrée.
5. Déclarez le constructeur pour la classe UserManager et transmettez-le en tant que SQLPasswordHasher à la propriété dans le constructeur.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Créer des pages de gestion de compte

L’étape suivante de la migration consiste à ajouter des pages de gestion des comptes permettant à un utilisateur de s’inscrire et de se connecter. Les anciennes pages de compte de l’appartenance SQL utilisent des contrôles qui ne fonctionnent pas avec le nouveau système d’identité. Pour ajouter les nouvelles pages de gestion des utilisateurs, suivez le didacticiel de ce lien [https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) en commençant par l’étape « ajout d’Web Forms pour l’inscription des utilisateurs à votre application », car nous avons déjà créé le projet et ajouté les packages NuGet.

Nous devons apporter des modifications à l’exemple pour qu’il fonctionne avec le projet que nous avons ici.

- Les classes Register.aspx.cs et Login.aspx.cs code-behind utilisent le `UserManager` des packages d’identité pour créer un utilisateur. Pour cet exemple, utilisez la UserManager ajoutée dans le dossier Models en suivant les étapes mentionnées précédemment.
- Utilisez la classe d’utilisateur créée à la place de IdentityUser dans les classes Register.aspx.cs et Login.aspx.cs code-behind. Ce hook dans notre classe d’utilisateur personnalisée dans le système d’identité.
- La partie pour créer la base de données peut être ignorée.
- Le développeur doit définir l’ID de l’application pour que le nouvel utilisateur corresponde à l’ID d’application actuel. Pour ce faire, vous pouvez interroger l’ApplicationId pour cette application avant qu’un objet utilisateur soit créé dans la classe Register.aspx.cs et le définir avant de créer l’utilisateur.

    Exemple :

    Définir une méthode dans la page Register.aspx.cs pour interroger la table ASPNET\_applications et obtenir l’ID d’application en fonction du nom de l’application

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Maintenant, définissez cette valeur sur l’objet utilisateur

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Utilisez l’ancien nom d’utilisateur et le mot de passe pour vous connecter à un utilisateur existant. Utilisez la page s’inscrire pour créer un nouvel utilisateur. Vérifiez également que les utilisateurs sont dans les rôles comme prévu.

Le portage vers le système d’identité permet à l’utilisateur d’ajouter l’authentification Open (OAuth) à l’application. Reportez-vous à l’exemple [ici](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/) , avec OAuth activé.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, nous avons montré comment porter les utilisateurs de l’appartenance SQL vers ASP.NET Identity, mais nous n’avions pas de données de profil de port. Dans le didacticiel suivant, nous allons examiner le portage des données de profil de l’appartenance SQL vers le nouveau système d’identité.

Vous pouvez conserver vos commentaires au bas de cet article.

*Merci à Tom Dykstra et Rick Anderson de passer en revue l’article.*
