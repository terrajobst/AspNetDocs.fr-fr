---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.NET Identity ASP.NET 4. x
author: Rick-Anderson
description: ASP.NET Identity est un système extensible qui vous permet de créer votre propre fournisseur de stockage et de l’intégrer à votre application sans avoir à réutiliser l’applicateur...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 21baedf6285b411f89627df9ca25d47a2a42e387
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584412"
---
# <a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.NET Identity

par [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity est un système extensible qui vous permet de créer votre propre fournisseur de stockage et de l’intégrer à votre application sans avoir à retravailler l’application. Cette rubrique explique comment créer un fournisseur de stockage personnalisé pour ASP.NET Identity. Il couvre les concepts importants pour la création de votre propre fournisseur de stockage, mais il ne s’agit pas d’une procédure pas à pas d’implémentation d’un fournisseur de stockage personnalisé.
> 
> Pour obtenir un exemple d’implémentation d’un fournisseur de stockage personnalisé, consultez [implémentation d’un fournisseur de stockage ASP.net Identity MySQL personnalisé](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Cette rubrique a été mise à jour pour ASP.NET Identity 2,0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Visual Studio 2013 avec Update 2
> - ASP.NET Identity 2

## <a name="introduction"></a>Introduction

Par défaut, le système de ASP.NET Identity stocke les informations utilisateur dans une base de données SQL Server et utilise Entity Framework Code First pour créer la base de données. Pour de nombreuses applications, cette approche fonctionne bien. Toutefois, vous préférerez peut-être utiliser un autre type de mécanisme de persistance, tel que le stockage table Azure, ou vous avez peut-être déjà des tables de base de données avec une structure très différente de celle de l’implémentation par défaut. Dans les deux cas, vous pouvez écrire un fournisseur personnalisé pour votre mécanisme de stockage et connecter ce fournisseur à votre application.

ASP.NET Identity est inclus par défaut dans la plupart des modèles de Visual Studio 2013. Vous pouvez récupérer des mises à jour de ASP.NET Identity par le biais du [package NuGet d’identité Microsoft ASPNET Identity](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Cette rubrique comporte les sections suivantes :

- [Comprendre l’architecture](#architecture)
- [Comprendre les données stockées](#data)
- [Créer la couche d’accès aux données](#dal)
- [Personnaliser la classe utilisateur](#user)
- [Personnaliser le magasin de l’utilisateur](#userstore)
- [Personnaliser la classe de rôle](#role)
- [Personnaliser le magasin de rôles](#rolestore)
- [Reconfigurer l’application pour utiliser le nouveau fournisseur de stockage](#reconfigure)
- [Autres implémentations de fournisseurs de stockage personnalisés](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Comprendre l’architecture

ASP.NET Identity se compose de classes appelées gestionnaires et magasins. Les gestionnaires sont des classes de haut niveau qu’un développeur d’applications utilise pour effectuer des opérations, telles que la création d’un utilisateur, dans le système de ASP.NET Identity. Les magasins sont des classes de niveau inférieur qui spécifient la façon dont les entités, telles que les utilisateurs et les rôles, sont conservées. Les magasins sont étroitement couplés avec le mécanisme de persistance, mais les gestionnaires sont dissociés des magasins, ce qui signifie que vous pouvez remplacer le mécanisme de persistance sans interrompre la totalité de l’application.

Le diagramme suivant montre comment votre application Web interagit avec les gestionnaires, et les magasins interagissent avec la couche d’accès aux données.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Pour créer un fournisseur de stockage personnalisé pour ASP.NET Identity, vous devez créer la source de données, la couche d’accès aux données et les classes de magasin qui interagissent avec cette couche d’accès aux données. Vous pouvez continuer à utiliser les mêmes API de gestionnaire pour effectuer des opérations de données sur l’utilisateur, mais maintenant que les données sont enregistrées dans un autre système de stockage.

Vous n’avez pas besoin de personnaliser les classes Manager, car lors de la création d’une nouvelle instance de UserManager ou de RoleManager, vous fournissez le type de la classe User et vous transmettez une instance de la classe Store comme argument. Cette approche vous permet de connecter vos classes personnalisées à la structure existante. Vous verrez comment instancier UserManager et RoleManager avec vos classes de magasin personnalisées dans la section [reconfigure application to use New Storage Provider](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Comprendre les données stockées

Pour implémenter un fournisseur de stockage personnalisé, vous devez comprendre les types de données utilisés avec ASP.NET Identity et déterminer les fonctionnalités pertinentes pour votre application.

| Données | Description |
| --- | --- |
| Utilisateurs | Utilisateurs inscrits de votre site Web. Comprend l’ID d’utilisateur et le nom d’utilisateur. Peut inclure un mot de passe haché si les utilisateurs se connectent avec des informations d’identification spécifiques à votre site (plutôt que d’utiliser les informations d’identification d’un site externe comme Facebook) et un tampon de sécurité pour indiquer si des modifications ont été apportées aux informations d’identification de l’utilisateur. Peut également inclure l’adresse de messagerie, le numéro de téléphone, si l’authentification à deux facteurs est activée, le nombre actuel de connexions ayant échoué et si un compte a été verrouillé. |
| Revendications utilisateur | Ensemble d’instructions (ou de revendications) sur l’utilisateur qui représente l’identité de l’utilisateur. Peut permettre une plus grande expression de l’identité de l’utilisateur que le permet d’obtenir des rôles. |
| Connexions utilisateur | Informations sur le fournisseur d’authentification externe (par exemple, Facebook) à utiliser lors de la connexion à un utilisateur. |
| Rôles | Groupes d’autorisations pour votre site. Comprend l’ID de rôle et le nom de rôle (par exemple, « admin » ou « Employee »). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Créer la couche d’accès aux données

Cette rubrique suppose que vous êtes familiarisé avec le mécanisme de persistance que vous allez utiliser et comment créer des entités pour ce mécanisme. Cette rubrique ne fournit pas de détails sur la création des référentiels ou des classes d’accès aux données. au lieu de cela, il fournit des suggestions sur les décisions de conception que vous devez prendre lors de l’utilisation de ASP.NET Identity.

Vous avez beaucoup de liberté lors de la conception des référentiels pour un fournisseur de magasin personnalisé. Vous devez uniquement créer des référentiels pour les fonctionnalités que vous envisagez d’utiliser dans votre application. Par exemple, si vous n’utilisez pas de rôles dans votre application, vous n’avez pas besoin de créer de stockage pour les rôles ou les rôles d’utilisateur. Votre technologie et votre infrastructure existante peuvent nécessiter une structure très différente de l’implémentation par défaut de ASP.NET Identity. Dans votre couche d’accès aux données, vous fournissez la logique permettant de travailler avec la structure de vos référentiels.

Pour une implémentation MySQL des référentiels de données pour ASP.NET Identity 2,0, consultez [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

Dans la couche d’accès aux données, vous fournissez la logique pour enregistrer les données de ASP.NET Identity dans votre source de données. La couche d’accès aux données de votre fournisseur de stockage personnalisé peut inclure les classes suivantes pour stocker les informations d’utilisateur et de rôle.

| Class | Description | Exemple |
| --- | --- | --- |
| Contexte | Encapsule les informations pour se connecter à votre mécanisme de persistance et exécuter des requêtes. Cette classe est centrale à votre couche d’accès aux données. Les autres classes de données requièrent une instance de cette classe pour effectuer leurs opérations. Vous allez également initialiser vos classes Store avec une instance de cette classe. | [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Stockage utilisateur | Stocke et récupère les informations utilisateur (telles que le nom d’utilisateur et le hachage de mot de passe). | [UserTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Stockage de rôle | Stocke et récupère les informations de rôle (par exemple, le nom du rôle). | [RoleTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Stockage UserClaims | Stocke et récupère les informations de revendication de l’utilisateur (par exemple, le type et la valeur de la revendication). | [UserClaimsTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Stockage UserLogins | Stocke et récupère les informations de connexion de l’utilisateur (par exemple, un fournisseur d’authentification externe). | [UserLoginsTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Stockage UserRole | Stocke et récupère les rôles auxquels un utilisateur est affecté. | [UserRoleTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Là encore, il vous suffit d’implémenter les classes que vous envisagez d’utiliser dans votre application.

Dans les classes d’accès aux données, vous fournissez du code pour effectuer des opérations de données pour votre mécanisme de persistance particulier. Par exemple, dans l’implémentation MySQL, la classe UserTable contient une méthode pour insérer un nouvel enregistrement dans la table de base de données Users. La variable nommée `_database` est une instance de la classe MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Après avoir créé vos classes d’accès aux données, vous devez créer des classes de magasin qui appellent les méthodes spécifiques dans la couche d’accès aux données.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Personnaliser la classe utilisateur

Lors de l’implémentation de votre propre fournisseur de stockage, vous devez créer une classe d’utilisateur qui est équivalente à la classe [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) dans l’espace de noms [Microsoft. asp. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

Le diagramme suivant montre la classe IdentityUser que vous devez créer et l’interface à implémenter dans cette classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

L’interface [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) définit les propriétés que le usermanager tente d’appeler lors de l’exécution des opérations demandées. L’interface contient deux propriétés : ID et nom d’utilisateur. L’interface [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) vous permet de spécifier le type de la clé pour l’utilisateur par le biais du paramètre generic **TKey** . Le type de la propriété ID correspond à la valeur du paramètre TKey.

L’infrastructure d’identité fournit également l’interface [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) (sans le paramètre générique) lorsque vous souhaitez utiliser une valeur de chaîne pour la clé.

La classe IdentityUser implémente IUser et contient des propriétés ou constructeurs supplémentaires pour les utilisateurs sur votre site Web. L’exemple suivant montre une classe IdentityUser qui utilise un entier pour la clé. Le champ ID est défini sur **int** pour correspondre à la valeur du paramètre générique. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Pour une implémentation complète, consultez [IdentityUser (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Personnaliser le magasin de l’utilisateur

Vous créez également une classe UserStore qui fournit les méthodes pour toutes les opérations de données sur l’utilisateur. Cette classe est équivalente à la classe [UserStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) de l’espace de noms [Microsoft. asp. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) . Dans votre classe UserStore, vous implémentez le [IUserStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) et toutes les interfaces facultatives. Vous sélectionnez les interfaces facultatives à implémenter en fonction des fonctionnalités que vous souhaitez fournir dans votre application.

L’illustration suivante montre la classe UserStore que vous devez créer et les interfaces appropriées.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Le modèle de projet par défaut dans Visual Studio contient du code qui suppose que la plupart des interfaces facultatives ont été implémentées dans le magasin de l’utilisateur. Si vous utilisez le modèle par défaut avec un magasin d’utilisateurs personnalisé, vous devez soit implémenter des interfaces facultatives dans votre magasin d’utilisateurs, soit modifier le code du modèle pour ne plus appeler les méthodes des interfaces que vous n’avez pas implémentées.

 L’exemple suivant illustre une classe de magasin utilisateur simple. Le paramètre générique **TUser** prend le type de votre classe d’utilisateur qui est généralement la classe IdentityUser que vous avez définie. Le paramètre générique **TKey** prend le type de votre clé utilisateur. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 Dans cet exemple, le constructeur qui accepte un paramètre nommé *base de données* de type ExampleDatabase n’est qu’une illustration de la manière de passer votre classe d’accès aux données. Par exemple, dans l’implémentation MySQL, ce constructeur accepte un paramètre de type MySQLDatabase. 

Au sein de votre classe UserStore, vous utilisez les classes d’accès aux données que vous avez créées pour effectuer des opérations. Par exemple, dans l’implémentation MySQL, la classe UserStore a la méthode CreateAsync qui utilise une instance de UserTable pour insérer un nouvel enregistrement. La méthode **Insert** sur l’objet **userTable** est la même que celle présentée dans la section précédente. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfaces à implémenter lors de la personnalisation du magasin d’utilisateurs

L’image suivante montre plus de détails sur les fonctionnalités définies dans chaque interface. Toutes les interfaces facultatives héritent de IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  L’interface [IUserStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) est la seule interface que vous devez implémenter dans votre magasin d’utilisateurs. Il définit des méthodes pour la création, la mise à jour, la suppression et la récupération des utilisateurs.
- **IUserClaimStore**  
  L’interface [IUserClaimStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) définit les méthodes que vous devez implémenter dans votre magasin d’utilisateurs pour activer les revendications d’utilisateur. Elle contient des méthodes ou l’ajout, la suppression et la récupération de revendications d’utilisateur.
- **IUserLoginStore**  
  [IUserLoginStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) définit les méthodes que vous devez implémenter dans votre magasin d’utilisateurs pour activer les fournisseurs d’authentification externes. Il contient des méthodes permettant d’ajouter, de supprimer et de récupérer des connexions utilisateur, ainsi qu’une méthode pour récupérer un utilisateur en fonction des informations de connexion.
- **IUserRoleStore**  
  L’interface [IUserRoleStore&lt;TKey, TUser&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) définit les méthodes que vous devez implémenter dans votre magasin d’utilisateurs pour mapper un utilisateur à un rôle. Elle contient des méthodes permettant d’ajouter, de supprimer et de récupérer les rôles d’un utilisateur, ainsi qu’une méthode permettant de vérifier si un utilisateur est affecté à un rôle.
- **IUserPasswordStore**  
  L’interface [IUserPasswordStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) définit les méthodes que vous devez implémenter dans votre magasin d’utilisateurs pour rendre les mots de passe hachés persistants. Elle contient des méthodes permettant d’obtenir et de définir le mot de passe haché, ainsi qu’une méthode qui indique si l’utilisateur a défini un mot de passe.
- **IUserSecurityStampStore**  
  L’interface [IUserSecurityStampStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) définit les méthodes que vous devez implémenter dans votre magasin d’utilisateurs afin d’utiliser un tampon de sécurité pour indiquer si les informations de compte de l’utilisateur ont été modifiées. Ce marquage est mis à jour lorsqu’un utilisateur modifie le mot de passe, ou ajoute ou supprime des connexions. Elle contient des méthodes permettant d’obtenir et de définir le tampon de sécurité.
- **IUserTwoFactorStore**  
  L’interface [IUserTwoFactorStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) définit les méthodes que vous devez implémenter pour implémenter l’authentification à deux facteurs. Elle contient des méthodes permettant d’obtenir et de définir si l’authentification à deux facteurs est activée pour un utilisateur.
- **IUserPhoneNumberStore**  
  L’interface [IUserPhoneNumberStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) définit les méthodes que vous devez implémenter pour stocker les numéros de téléphone des utilisateurs. Il contient des méthodes permettant d’obtenir et de définir le numéro de téléphone et si le numéro de téléphone est confirmé.
- **IUserEmailStore**  
  L’interface [IUserEmailStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) définit les méthodes que vous devez implémenter pour stocker les adresses de messagerie des utilisateurs. Il contient des méthodes permettant d’obtenir et de définir l’adresse de messagerie et si l’e-mail est confirmé.
- **IUserLockoutStore**  
  L’interface [IUserLockoutStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) définit les méthodes que vous devez implémenter pour stocker des informations sur le verrouillage d’un compte. Il contient des méthodes permettant d’obtenir le nombre actuel de tentatives d’accès ayant échoué, d’obtenir et de définir si le compte peut être verrouillé, d’obtenir et de définir la date de fin du verrouillage, d’incrémenter le nombre de tentatives ayant échoué et de réinitialiser le nombre de tentatives ayant échoué.
- **IQueryableUserStore**  
  L’interface [IQueryableUserStore&lt;tUser, TKey&gt;](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) définit les membres que vous devez implémenter pour fournir un magasin d’utilisateurs interrogeable. Il contient une propriété qui contient les utilisateurs interrogeables.

  Implémentez les interfaces nécessaires dans votre application ; par exemple, les interfaces IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore et IUserSecurityStampStore comme indiqué ci-dessous. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Pour une implémentation complète (y compris toutes les interfaces), consultez [UserStore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin et IdentityUserRole

L’espace de noms Microsoft. AspNet. Identity. EntityFramework contient des implémentations des classes [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)et [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) . Si vous utilisez ces fonctionnalités, vous souhaiterez peut-être créer vos propres versions de ces classes et définir les propriétés de votre application. Toutefois, il est parfois plus efficace de ne pas charger ces entités dans la mémoire lors de l’exécution d’opérations de base (telles que l’ajout ou la suppression d’une revendication d’utilisateur). Au lieu de cela, les classes du magasin principal peuvent exécuter ces opérations directement sur la source de données. Par exemple, la méthode UserStore. GetClaimsAsync () peut appeler userClaimTable. FindByUserId (User. ID) pour exécuter directement une requête sur cette table et retourner une liste de revendications.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Personnaliser la classe de rôle

Lors de l’implémentation de votre propre fournisseur de stockage, vous devez créer une classe de rôle qui est équivalente à la classe [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) dans l’espace de noms [Microsoft. asp. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

Le diagramme suivant montre la classe IdentityRole que vous devez créer et l’interface à implémenter dans cette classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

L’interface [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) définit les propriétés que le roleManager tente d’appeler lors de l’exécution des opérations demandées. L’interface contient deux propriétés : ID et Name. L’interface [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) vous permet de spécifier le type de la clé pour le rôle par le biais du paramètre generic **TKey** . Le type de la propriété ID correspond à la valeur du paramètre TKey.

L’infrastructure d’identité fournit également l’interface [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) (sans le paramètre générique) lorsque vous souhaitez utiliser une valeur de chaîne pour la clé.

L’exemple suivant montre une classe IdentityRole qui utilise un entier pour la clé. Le champ ID est défini sur int pour correspondre à la valeur du paramètre générique. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Pour une implémentation complète, consultez [IdentityRole (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Personnaliser le magasin de rôles

Vous créez également une classe au rolestore qui fournit les méthodes pour toutes les opérations de données sur les rôles. Cette classe est équivalente à la classe [au rolestore&lt;TRole&gt;](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) de l’espace de noms Microsoft. asp. net. Identity. EntityFramework. Dans votre classe au rolestore, vous implémentez [IRoleStore&lt;TRole, TKey&gt;](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) et éventuellement l’interface [IQueryableRoleStore&lt;TRole, TKey&gt;](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) .

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

L’exemple suivant illustre une classe de magasin de rôles. Le paramètre générique TRole prend le type de votre classe de rôle qui est généralement la classe IdentityRole que vous avez définie. Le paramètre générique TKey prend le type de votre clé de rôle. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  L’interface [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) définit les méthodes à implémenter dans votre classe de magasin de rôles. Elle contient des méthodes pour la création, la mise à jour, la suppression et la récupération de rôles.
- **Au rolestore&lt;TRole&gt;**  
  Pour personnaliser au rolestore, créez une classe qui implémente l’interface IRoleStore. Il vous suffit d’implémenter cette classe si vous souhaitez utiliser des rôles sur votre système. Le constructeur qui accepte un paramètre nommé *base de données* de type ExampleDatabase n’est qu’une illustration de la manière de passer votre classe d’accès aux données. Par exemple, dans l’implémentation MySQL, ce constructeur accepte un paramètre de type MySQLDatabase.  
  
  Pour une implémentation complète, consultez [au rolestore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Reconfigurer l’application pour utiliser le nouveau fournisseur de stockage

Vous avez implémenté votre nouveau fournisseur de stockage. Maintenant, vous devez configurer votre application pour utiliser ce fournisseur de stockage. Si le fournisseur de stockage par défaut a été inclus dans votre projet, vous devez supprimer le fournisseur par défaut et le remplacer par votre fournisseur.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Remplacer le fournisseur de stockage par défaut dans le projet MVC

1. Dans la fenêtre **gérer les packages NuGet** , désinstallez le Microsoft ASP.net package d' **identité EntityFramework** . Vous pouvez trouver ce package en effectuant une recherche dans les packages installés pour Identity. EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) vous serez invité à désinstaller également Entity Framework. Si vous n’en avez pas besoin dans d’autres parties de votre application, vous pouvez la désinstaller.
2. Dans le fichier IdentityModels.cs du dossier Models, supprimez ou commentez les classes **ApplicationUser** et **ApplicationDbContext** . Dans une application MVC, vous pouvez supprimer l’intégralité du fichier IdentityModels.cs. Dans une application Web Forms, supprimez les deux classes, mais assurez-vous que vous conservez la classe d’assistance qui se trouve également dans le fichier IdentityModels.cs.
3. Si votre fournisseur de stockage se trouve dans un projet distinct, ajoutez une référence à ce dernier dans votre application Web.
4. Remplacez toutes les références à `using Microsoft.AspNet.Identity.EntityFramework;` par une instruction using pour l’espace de noms de votre fournisseur de stockage.
5. Dans la classe **Startup.auth.cs** , modifiez la méthode **ConfigureAuth** pour qu’elle utilise une seule instance du contexte approprié. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. Dans le dossier de démarrage de l’application\_, ouvrez **IdentityConfig.cs**. Dans la classe ApplicationUserManager, modifiez la méthode **Create** pour retourner un gestionnaire utilisateur qui utilise votre magasin d’utilisateurs personnalisé. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Remplacez toutes les références à **ApplicationUser** par **IdentityUser**.
8. Le projet par défaut comprend certains membres de la classe User qui ne sont pas définis dans l’interface IUser ; par exemple, E-mail, PasswordHash et GenerateUserIdentityAsync. Si votre classe utilisateur ne possède pas ces membres, vous devez les implémenter ou modifier le code qui les utilise.
9. Si vous avez créé des instances de RoleManager, modifiez ce code pour utiliser votre nouvelle classe au rolestore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Le projet par défaut est conçu pour une classe d’utilisateur qui a une valeur de chaîne pour la clé. Si votre classe utilisateur a un type différent pour la clé (par exemple, un entier), vous devez modifier le projet pour qu’il fonctionne avec votre type. Consultez [modifier la clé primaire pour les utilisateurs dans ASP.net Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Si nécessaire, ajoutez la chaîne de connexion au fichier Web. config.

<a id="other"></a>
## <a name="other-resources"></a>Autres ressources

- Blog : [implémentation d’ASP.net Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Didacticiel et code GIT : [simple. Data ASP.net Identity Provider](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Didacticiel :[Configuration des comptes d’identité de base et pointer vers une base de connaissances externe](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Par [@xivSolutions](https://twitter.com/xivSolutions).
- Didacticiel[: implémentation d’un fournisseur de stockage ASP.net Identity MySQL personnalisé](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entités codefluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) par [SoftFluent](http://www.softfluent.com/)
- [Stockage table Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) par James Randall.
- Stockage table Azure : [Aspnet. Identity. TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) par [@stuartleeks](https://twitter.com/stuartleeks).
- [CouchDB/Cloud de Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Élastique Rec[h : identité élastique](https://github.com/bmbsqd/elastic-identity) par Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) de jonathan Sheely Jonathan Sheely.
- [NHibernate. Aspnet. Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) par Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) par [@tourismgeek](https://twitter.com/tourismgeek).
- [RavenDB. Aspnet. Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) par [ILMServices](http://www.ilmservice.com/).
- ReDim : [redims. Aspnet. Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Modèles T4 pour générer du code EF pour un magasin d’utilisateurs « base de données First » : [Aspnet. Identity. EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
