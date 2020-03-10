---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Présentation de ASP.NET Identity-ASP.NET 4. x
author: jongalloway
description: Le système d’appartenance ASP.NET a été introduit avec ASP.NET 2,0 dans 2005, et depuis, de nombreuses modifications ont été apportées aux applications Web...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0268dfc16cd2cfb1e79ee14997a4c5eb247af950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583845"
---
# <a name="introduction-to-aspnet-identity"></a>Introduction à ASP.NET Identity

> Le système d’appartenance ASP.NET a été introduit avec ASP.NET 2,0 dans 2005, et depuis, de nombreuses modifications ont été apportées à la façon dont les applications Web gèrent généralement l’authentification et l’autorisation. ASP.NET Identity est un nouvel aperçu de ce que le système d’appartenance doit être lorsque vous créez des applications modernes pour le Web, le téléphone ou la tablette.

## <a name="background-membership-in-aspnet"></a>Arrière-plan : appartenance à ASP.NET

### <a name="aspnet-membership"></a>Appartenance ASP.NET

[L’appartenance à ASP.net](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) a été conçue pour répondre aux exigences d’appartenance de site qui étaient courantes dans 2005, qui impliquaient l’authentification par formulaire, et une base de données SQL Server pour les noms d’utilisateur, les mots de passe et les données de profil. Aujourd’hui, il existe un vaste éventail d’options de stockage des données pour les applications Web, et la plupart des développeurs souhaitent permettre à leurs sites d’utiliser des fournisseurs d’identité sociale pour la fonctionnalité d’authentification et d’autorisation. Les limitations de la conception de l’appartenance à ASP.NET rendent cette transition difficile :

- Le schéma de base de données a été conçu pour SQL Server et vous ne pouvez pas le modifier. Vous pouvez ajouter des informations de profil, mais les données supplémentaires sont empaquetées dans une table différente, ce qui rend difficile l’accès à tous les moyens, sauf via l’API du fournisseur de profils.
- Le système fournisseur vous permet de modifier le magasin de données de stockage, mais le système est conçu autour des hypothèses appropriées pour une base de données relationnelle. Vous pouvez écrire un fournisseur pour stocker les informations d’appartenance dans un mécanisme de stockage non relationnel, tel que les tables de stockage Azure, mais vous devez ensuite contourner la conception relationnelle en écrivant beaucoup de code et de nombreuses exceptions de `System.NotImplementedException` pour les méthodes qui ne s’appliquent pas aux bases de données NoSQL.
- Étant donné que la fonctionnalité de connexion/déconnexion est basée sur l’authentification par formulaire, le système d’appartenance ne peut pas utiliser [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN inclut des composants d’intergiciel (middleware) pour l’authentification, notamment la prise en charge des connexions utilisant des fournisseurs d’identité externes (tels que les comptes Microsoft, Facebook, Google, Twitter) et les connexions à l’aide de comptes professionnels à partir de Active Directory locaux ou Azure Active Directory. OWIN prend également en charge OAuth 2,0, JWT et CORS.

### <a name="aspnet-simple-membership"></a>ASP.NET simple

[ASP.NET simple Membership](../../../web-pages/overview/security/16-adding-security-and-membership.md) a été développé en tant que système d’appartenance pour pages Web ASP.net. Elle a été publiée avec WebMatrix et Visual Studio 2010 SP1. L’objectif de l’appartenance simple était de faciliter l’ajout de la fonctionnalité d’appartenance à une application Web pages.

L’appartenance simple a facilité la personnalisation des informations de profil utilisateur, mais elle partage toujours les autres problèmes avec l’appartenance ASP.NET et présente certaines limitations :

- Il était difficile de conserver les données du système d’appartenance dans un magasin non relationnel.
- Vous ne pouvez pas l’utiliser avec OWIN.
- Il ne fonctionne pas correctement avec les fournisseurs d’appartenances ASP.NET existants et n’est pas extensible.

### <a name="aspnet-universal-providers"></a>Fournisseurs universels ASP.NET

[Fournisseurs universels ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) ont été développées pour permettre la conservation des informations d’appartenance dans Microsoft Azure SQL Database, et elles fonctionnent également avec SQL Server Compact. Les Fournisseurs universels ont été créés sur Entity Framework Code First, ce qui signifie que le Fournisseurs universels peut être utilisé pour conserver les données dans n’importe quel magasin pris en charge par EF. Avec la Fournisseurs universels, le schéma de la base de données a également été nettoyé beaucoup.

Les Fournisseurs universels reposent sur l’infrastructure d’appartenance ASP.NET. elles présentent donc les mêmes limitations que le fournisseur SqlMembership. Autrement dit, elles ont été conçues pour les bases de données relationnelles et il est difficile de personnaliser les informations sur les profils et les utilisateurs. Ces fournisseurs continuent également à utiliser l’authentification par formulaire pour les fonctionnalités de connexion et de déconnexion.

## <a name="aspnet-identity"></a>ASP.NET Identity

À mesure que le témoignage d’appartenance dans ASP.NET a évolué au fil des années, l’équipe ASP.NET a appris beaucoup de commentaires des clients.

L’hypothèse selon laquelle les utilisateurs se connecteront en entrant un nom d’utilisateur et un mot de passe qu’ils ont inscrits dans votre propre application n’est plus valide. Le Web est devenu plus social. Les utilisateurs interagissent les uns avec les autres en temps réel via des réseaux sociaux tels que Facebook, Twitter et d’autres sites Web sociaux. Les développeurs souhaitent que les utilisateurs puissent se connecter à l’aide de leurs identités sociales afin qu’ils puissent bénéficier d’une expérience riche sur leurs sites Web. Un système d’appartenance moderne doit activer les journaux de redirection pour les fournisseurs d’authentification tels que Facebook, Twitter et d’autres.

Au fur et à mesure de l’évolution du développement Web, les modèles de développement Web étaient les mêmes. Le test unitaire du code d’application est devenu un problème fondamental pour les développeurs d’applications. Dans 2008, ASP.NET a ajouté une nouvelle infrastructure basée sur le modèle MVC (Model-View-Controller), en partie pour aider les développeurs à créer des applications ASP.NET de test unitaire. Les développeurs souhaitant effectuer des tests unitaires sur leur logique d’application souhaitaient également pouvoir le faire avec le système d’appartenance.

En tenant compte de ces modifications dans le développement d’applications Web, ASP.NET Identity a été développé avec les objectifs suivants :

- **Un système ASP.NET Identity**

    - ASP.NET Identity peut être utilisé avec toutes les infrastructures ASP.NET, telles que ASP.NET MVC, Web Forms, les pages Web, l’API Web et Signalr.
    - ASP.NET Identity peut être utilisé lorsque vous créez des applications Web, de téléphone, de stockage ou hybrides.
- **Facilité de connexion des données de profil relatives à l’utilisateur**

    - Vous contrôlez le schéma des informations d’utilisateur et de profil. Par exemple, vous pouvez facilement permettre au système de stocker les dates de naissance entrées par les utilisateurs lorsqu’ils inscrivent un compte dans votre application.

- **Contrôle de persistance**

    - Par défaut, le système de ASP.NET Identity stocke toutes les informations utilisateur dans une base de données. ASP.NET Identity utilise Entity Framework Code First pour implémenter l’ensemble de son mécanisme de persistance.
    - Étant donné que vous contrôlez le schéma de base de données, les tâches courantes telles que la modification des noms de tables ou la modification du type de données des clés primaires sont simples à effectuer.
    - Il est facile de brancher différents mécanismes de stockage tels que SharePoint, le service de table de stockage Azure, les bases de données NoSQL, etc., sans avoir à lever `System.NotImplementedExceptions` exceptions.
- **Testabilité unitaire**

    - ASP.NET Identity rend l’application Web plus testable en unités. Vous pouvez écrire des tests unitaires pour les parties de votre application qui utilisent ASP.NET Identity.
- **Fournisseur de rôles**

    - Il existe un fournisseur de rôle qui vous permet de restreindre l’accès aux parties de votre application par rôles. Vous pouvez facilement créer des rôles tels que « admin » et ajouter des utilisateurs aux rôles.
- **Basée sur les revendications**

    - ASP.NET Identity prend en charge l’authentification basée sur les revendications, où l’identité de l’utilisateur est représentée sous la forme d’un ensemble de revendications. Les revendications permettent aux développeurs d’être beaucoup plus expressifs pour décrire l’identité d’un utilisateur que les rôles le permettent. Tandis que l’appartenance à un rôle n’est qu’une valeur booléenne (membre ou non membre), une revendication peut inclure des informations détaillées sur l’identité et l’appartenance de l’utilisateur.
- **Fournisseurs de connexion sociale**

    - Vous pouvez facilement ajouter des journaux sociaux tels que Microsoft Account, Facebook, Twitter, Google et d’autres à votre application, et stocker les données spécifiques à l’utilisateur dans votre application.

- **Intégration de OWIN**

    - L’authentification ASP.NET est désormais basée sur un intergiciel (middleware) OWIN qui peut être utilisé sur n’importe quel hôte OWIN. ASP.NET Identity n’a aucune dépendance vis-à-vis de System. Web. Il s’agit d’une infrastructure OWIN entièrement compatible qui peut être utilisée dans n’importe quelle application hébergée par OWIN.
    - ASP.NET Identity utilise l’authentification OWIN pour la connexion et la déconnexion des utilisateurs du site Web. Cela signifie qu’au lieu d’utiliser FormsAuthentication pour générer le cookie, l’application utilise OWIN CookieAuthentication pour ce faire.
- **Package NuGet**

    - ASP.NET Identity est redistribuée sous la forme d’un package NuGet qui est installé dans le ASP.NET MVC, Web Forms et les modèles d’API Web fournis avec Visual Studio 2017. Vous pouvez télécharger ce package NuGet à partir de la galerie NuGet.
    - La libération d’ASP.NET Identity en tant que package NuGet permet à l’équipe ASP.NET d’itérer plus facilement sur les nouvelles fonctionnalités et les correctifs de bogues, et de les fournir aux développeurs de manière agile.

## <a name="get-started-with-aspnet-identity"></a>Prise en main de ASP.NET Identity

ASP.NET Identity est utilisé dans les modèles de projet Visual Studio 2017 pour ASP.NET MVC, Web Forms, l’API Web et SPA. Dans cette procédure pas à pas, nous allons illustrer comment les modèles de projet utilisent ASP.NET Identity pour ajouter des fonctionnalités permettant de s’inscrire, de se connecter et de déconnecter un utilisateur.

ASP.NET Identity est implémentée à l’aide de la procédure suivante. L’objectif de cet article est de vous offrir une vue d’ensemble de haut niveau de ASP.NET Identity ; vous pouvez le suivre pas à pas ou simplement lire les détails. Pour obtenir des instructions plus détaillées sur la création d’applications à l’aide de ASP.NET Identity, notamment l’utilisation de la nouvelle API pour ajouter des utilisateurs, des rôles et des informations de profil, consultez la section étapes suivantes à la fin de cet article.

1. Créer une application ASP.NET MVC avec des comptes individuels. Vous pouvez utiliser ASP.NET Identity dans ASP.NET MVC, Web Forms, API Web, Signalr, etc. Dans cet article, nous allons commencer par une application ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Le projet créé contient les trois packages suivants pour ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Ce package a l’implémentation Entity Framework de ASP.NET Identity qui rend persistantes les données et le schéma ASP.NET Identity pour SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Ce package possède les interfaces principales pour ASP.NET Identity. Ce package peut être utilisé pour écrire une implémentation de ASP.NET Identity ciblant différents magasins de persistance, tels que le stockage table Azure, les bases de données NoSQL, etc.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Ce package contient les fonctionnalités utilisées pour la connexion à l’authentification OWIN avec ASP.NET Identity dans les applications ASP.NET. Cela est utilisé lorsque vous ajoutez une fonctionnalité de connexion à votre application et appelez dans OWIN cookie Authentication middleware pour générer un cookie.
3. Création d’un utilisateur.  
   Lancez l’application, puis cliquez sur le lien **Register** pour créer un utilisateur. L’illustration suivante montre la page de Registre qui collecte le nom d’utilisateur et le mot de passe.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Lorsque l’utilisateur sélectionne le bouton **inscrire** , le `Register` action du contrôleur de compte crée l’utilisateur en appelant l’API ASP.net Identity, comme indiqué ci-dessous :

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Connectez-vous.  
   Si l’utilisateur a été créé avec succès, elle est connectée par la méthode `SignInAsync`.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample6.cs?highlight=12)]

   La méthode `SignInManager.SignInAsync` génère un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Étant donné que ASP.NET Identity et l’authentification des cookies OWIN sont des systèmes basés sur les revendications, l’infrastructure requiert que l’application génère un ClaimsIdentity pour l’utilisateur. ClaimsIdentity contient des informations sur toutes les revendications pour l’utilisateur, telles que les rôles auxquels l’utilisateur appartient.   
 
5. Fermez la session.  
   Sélectionnez le lien **Fermer la session** pour appeler l’action de déconnexion dans le contrôleur de compte. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Le code en surbrillance ci-dessus montre la méthode OWIN `AuthenticationManager.SignOut`. Cela est analogue à la méthode [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) utilisée par le module [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) dans Web Forms.

## <a name="components-of-aspnet-identity"></a>Composants de ASP.NET Identity

Le diagramme ci-dessous montre les composants du système de ASP.NET Identity (sélectionnez dans [ce](introduction-to-aspnet-identity/_static/image3.png) diagramme ou dans le diagramme pour l’agrandir). Les packages en vert constituent le système ASP.NET Identity. Tous les autres packages sont des dépendances qui sont nécessaires pour utiliser le système de ASP.NET Identity dans les applications ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Vous trouverez ci-dessous une brève description des packages NuGet non mentionnés précédemment :

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Intergiciel qui permet à une application d’utiliser l’authentification basée sur les cookies, similaire à ASP. Authentification par formulaire du réseau.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework est la technologie d’accès aux données recommandée de Microsoft pour les bases de données relationnelles.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migration de l’appartenance à ASP.NET Identity

Nous espérons bientôt fournir des conseils sur la migration de vos applications existantes qui utilisent l’appartenance ASP.NET ou l’appartenance simple au nouveau système de ASP.NET Identity.

## <a name="next-steps"></a>Étapes suivantes

- [Créer une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et l’authentification OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Ce didacticiel utilise l’API ASP.NET Identity pour ajouter des informations de profil à la base de données utilisateur, et comment s’authentifier avec Google et Facebook.
- [Créer une application ASP.NET MVC avec authentification et base de données SQL et la déployer dans Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Ce didacticiel montre comment utiliser l’API d’identité pour ajouter des utilisateurs et des rôles.
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Exemple d’application qui montre comment ajouter des rôles de base et un support utilisateur et comment effectuer des rôles et la gestion des utilisateurs.
