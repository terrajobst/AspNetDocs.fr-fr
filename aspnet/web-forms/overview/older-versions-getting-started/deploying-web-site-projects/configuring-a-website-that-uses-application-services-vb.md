---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Configuration d’un site Web qui utilise Services d’application (VB) | Microsoft Docs
author: rick-anderson
description: La version 2,0 de ASP.NET a introduit une série de services d’application, qui font partie de la .NET Framework et sert de suite de services de bloc de construction...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 19e7258b558372259c7554a36c6ad73ce572dfa8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642099"
---
# <a name="configuring-a-website-that-uses-application-services-vb"></a>Configuration d’un site web qui utilise les services d’application (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> ASP.NET version 2,0 a introduit une série de services d’application, qui font partie de la .NET Framework et sert de suite de services de bloc de construction que vous pouvez utiliser pour ajouter des fonctionnalités riches à votre application Web. Ce didacticiel explore la configuration d’un site Web dans l’environnement de production pour utiliser les services d’application et traite des problèmes courants liés à la gestion des comptes d’utilisateur et des rôles dans l’environnement de production.

## <a name="introduction"></a>Introduction

ASP.NET version 2,0 a introduit une série de *services d’application*, qui font partie de la .NET Framework et sert de suite de services de bloc de construction que vous pouvez utiliser pour ajouter des fonctionnalités riches à votre application Web. Les services d’application sont les suivants :

- **Appartenance** : API permettant de créer et de gérer des comptes d’utilisateur.
- **Rôles** : API permettant de classer les utilisateurs dans des groupes.
- **Profile** : API permettant de stocker du contenu personnalisé spécifique à l’utilisateur.
- **Plan de site** : API permettant de définir la structure logique d’un site sous la forme d’une hiérarchie, qui peut ensuite être affichée via des contrôles de navigation, tels que des menus et des barres de navigation.
- **Personnalisation** : API permettant de conserver les préférences de personnalisation, le plus souvent utilisé avec les [*composants WebPart*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Surveillance** de l’intégrité : API permettant de surveiller les performances, la sécurité, les erreurs et d’autres mesures d’intégrité du système pour une application Web en cours d’exécution.

Les API des services d’application ne sont pas liées à une implémentation spécifique. Au lieu de cela, vous indiquez aux services d’application d’utiliser un *fournisseur*particulier, et ce fournisseur implémente le service à l’aide d’une technologie particulière. Les fournisseurs les plus couramment utilisés pour les applications Web basées sur Internet hébergées sur une société d’hébergement Web sont ceux qui utilisent une implémentation de base de données SQL Server. Par exemple, le `SqlMembershipProvider` est un fournisseur de l’API d’appartenance qui stocke les informations de compte d’utilisateur dans une base de données Microsoft SQL Server.

L’utilisation des services d’application et des fournisseurs de SQL Server ajoute des défis lors du déploiement de l’application. Pour les initiateurs, les objets de base de données des services d’application doivent être correctement créés sur les bases de données de développement et de production et correctement initialisés. Des paramètres de configuration importants doivent également être définis.

> [!NOTE]
> Les API des services d’application ont été conçues à l’aide du [*modèle de fournisseur*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), un modèle de conception qui permet de fournir les détails de l’implémentation de l’API lors de l’exécution. Le .NET Framework est fourni avec un certain nombre de fournisseurs de services d’application qui peuvent être utilisés, tels que les `SqlMembershipProvider` et les `SqlRoleProvider`, qui sont des fournisseurs pour les API d’appartenance et de rôles qui utilisent une implémentation de base de données SQL Server. Vous pouvez également créer et connecter un fournisseur personnalisé. En fait, l’application Web de révisions de livres contient déjà un fournisseur personnalisé pour l’API de plan de site (`ReviewSiteMapProvider`), qui construit le plan de site à partir des données du `Genres` et `Books` tables de la base de données.

Ce didacticiel commence par un coup d’œil sur la façon dont j’ai étendu l’application Web de révisions de livres pour utiliser les API d’appartenance et de rôles. Il passe ensuite en revue le déploiement d’une application Web qui utilise les services d’application avec une implémentation de base de données SQL Server, et se termine en traitant les problèmes courants liés à la gestion des comptes d’utilisateur et des rôles dans l’environnement de production.

## <a name="updates-to-the-book-reviews-application"></a>Mises à jour de l’application de révisions de livres

Au cours des tutoriels, l’application Web de révisions de livres a été mise à jour à partir d’un site Web statique vers une application Web dynamique pilotée par les données complète avec un ensemble de pages d’administration pour la gestion des genres et des révisions. Toutefois, cette section d’administration n’est pas protégée pour l’instant : tout utilisateur qui connaît (ou devine) l’URL de la page d’administration peut Waltz dans et créer, modifier ou supprimer des avis sur notre site. Une méthode courante pour protéger certaines parties d’un site Web consiste à implémenter des comptes d’utilisateur, puis à utiliser des règles d’autorisation d’URL pour limiter l’accès à certains utilisateurs ou rôles. L’application Web de révisions de livres disponible en téléchargement dans ce didacticiel prend en charge les comptes d’utilisateurs et les rôles. Il a un rôle unique défini comme administrateur et seuls les utilisateurs de ce rôle peuvent accéder aux pages d’administration.

> [!NOTE]
> J’ai créé trois comptes d’utilisateur dans l’application Web de révisions de livres : Scott, Jisun et Alice. Les trois utilisateurs ont le même mot de passe : **mot de passe !** Scott et Jisun se trouvent dans le rôle d’administrateur, Alice ne l’est pas. Les pages non-administration du site sont toujours accessibles aux utilisateurs anonymes. Autrement dit, vous n’avez pas besoin de vous connecter pour visiter le site, sauf si vous souhaitez l’administrer. dans ce cas, vous devez vous connecter en tant qu’utilisateur du rôle d’administrateur.

La page maître des applications de révisions de livres a été mise à jour pour inclure une interface utilisateur différente pour les utilisateurs authentifiés et anonymes. Si un utilisateur anonyme visite le site, il voit un lien de connexion dans le coin supérieur droit. Un utilisateur authentifié voit le message « Welcome Welcome, *username*! » et un lien pour se déconnecter. Il y a également une page de connexion (`~/Login.aspx`), qui contient un contrôle Web de connexion qui fournit l’interface utilisateur et la logique d’authentification d’un visiteur. Seuls les administrateurs peuvent créer des comptes. (Il existe des pages pour la création et la gestion des comptes d’utilisateur dans le dossier `~/Admin`.)

### <a name="configuring-the-membership-and-roles-apis"></a>Configuration des API d’appartenance et de rôles

L’application Web de révisions de livres utilise les API d’appartenance et de rôles pour prendre en charge les comptes d’utilisateur et regrouper ces utilisateurs dans des rôles (à savoir, le rôle d’administrateur). Les classes de fournisseur `SqlMembershipProvider` et `SqlRoleProvider` sont utilisées, car nous voulons stocker les informations de compte et de rôle dans une base de données SQL Server.

> [!NOTE]
> Ce didacticiel n’est pas destiné à être un examen détaillé de la configuration d’une application Web pour prendre en charge les API d’appartenance et de rôles. Pour un examen approfondi de ces API et des étapes à suivre pour configurer un site Web afin de les utiliser, consultez les didacticiels sur la sécurité de votre [*site Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Pour utiliser les services d’application avec une base de données SQL Server vous devez d’abord ajouter les objets de base de données utilisés par ces fournisseurs à la base de données où vous souhaitez stocker les informations sur le compte et le rôle d’utilisateur. Ces objets de base de données requis incluent une variété de tables, de vues et de procédures stockées. Sauf indication contraire, les classes de fournisseur `SqlMembershipProvider` et `SqlRoleProvider` utilisent une base de données SQL Server Express Edition nommée `ASPNETDB` située dans le dossier application s `App_Data` ; Si une telle base de données n’existe pas, elle est automatiquement créée avec les objets de base de données nécessaires par ces fournisseurs lors de l’exécution.

Il est possible, et généralement idéal, de créer les objets de base de données des services d’application dans la même base de données que celle où sont stockées les données spécifiques à l’application du site Web. Le .NET Framework est fourni avec un outil nommé `aspnet_regsql.exe` qui installe les objets de base de données sur une base de données spécifiée. J’ai utilisé cet outil pour ajouter ces objets à la base de données `Reviews.mdf` dans le dossier `App_Data` (la base de données de développement). Nous verrons comment utiliser cet outil plus loin dans ce didacticiel lorsque nous ajouterons ces objets à la base de données de production.

Si vous ajoutez les objets de base de données des services d’application à une base de données autre que `ASPNETDB` vous devez personnaliser les configurations `SqlMembershipProvider` et `SqlRoleProvider` classes du fournisseur afin qu’elles utilisent la base de données appropriée. Pour personnaliser le fournisseur d’appartenances, ajoutez un [*élément&lt;appartenance&gt;* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) dans la section `<system.web>` de `Web.config`; Utilisez l' [*élément&lt;roleManager&gt;* ](https://msdn.microsoft.com/library/ms164660.aspx) pour configurer le fournisseur de rôles. L’extrait de code suivant provient de l’application de révisions de livres `Web.config` et indique les paramètres de configuration des API d’appartenance et de rôles. Notez que les deux éléments inscrivent un nouveau fournisseur-`ReviewMembership` et `ReviewRole` qui utilisent respectivement les fournisseurs `SqlMembershipProvider` et `SqlRoleProvider`.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

L’élément `Web.config` fichier s `<authentication>` a également été configuré pour prendre en charge l’authentification basée sur les formulaires.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Limitation de l’accès aux pages d’administration

ASP.NET permet d’accorder ou de refuser facilement l’accès à un fichier ou à un dossier particulier par l’utilisateur ou par rôle via sa fonctionnalité *d’autorisation d’URL* . (Nous avons brièvement abordé l’autorisation d’URL dans les *principales différences entre IIS et le didacticiel serveur de développement ASP.net* et montré comment IIS et le serveur de développement ASP.net appliquer différemment les règles d’autorisation d’URL pour le contenu statique et le contenu dynamique.) Étant donné que nous voulons interdire l’accès au dossier `~/Admin`, à l’exception des utilisateurs du rôle d’administrateur, nous devons ajouter des règles d’autorisation d’URL à ce dossier. Plus précisément, les règles d’autorisation d’URL doivent autoriser les utilisateurs du rôle d’administrateur et refuser tous les autres utilisateurs. Pour ce faire, ajoutez un fichier de `Web.config` au dossier `~/Admin` avec le contenu suivant :

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Pour plus d’informations sur la fonctionnalité d’autorisation d’URL ASP.NET s et sur son utilisation pour épeler des règles d’autorisation pour les utilisateurs et les rôles, consultez les didacticiels sur l' [*autorisation basée sur l’utilisateur*](../../older-versions-security/membership/user-based-authorization-vb.md) et les didacticiels d' [*autorisation basée*](../../older-versions-security/roles/role-based-authorization-vb.md) sur les rôles dans mes didacticiels sur la sécurité de mon [*site Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Déploiement d’une application Web qui utilise Services d’application

Lors du déploiement d’un site Web qui utilise les services d’application et d’un fournisseur qui stocke les informations des services d’application dans une base de données, il est impératif que les objets de base de données requis par les services d’application soient créés sur la base de données de production. Initialement, la base de données de production ne contient pas ces objets. ainsi, lorsque l’application est déployée pour la première fois (ou lorsqu’elle est déployée pour la première fois après l’ajout des services d’application), vous devez effectuer des étapes supplémentaires pour obtenir ces objets de base de données requis sur le base de données de production.

Un autre défi peut survenir lors du déploiement d’un site Web qui utilise les services d’application si vous envisagez de répliquer les comptes d’utilisateur créés dans l’environnement de développement vers l’environnement de production. Selon la configuration de l’appartenance et des rôles, il est possible que même si vous copiez correctement les comptes d’utilisateur qui ont été créés dans l’environnement de développement vers la base de données de production, ces utilisateurs ne peuvent pas se connecter à l’application Web en production. Nous allons examiner la cause de ce problème et expliquer comment l’empêcher de se produire.

ASP.NET est fourni avec un [*outil d’administration de site Web (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) , qui peut être lancé à partir de Visual Studio et permet de gérer le compte d’utilisateur, les rôles et les règles d’autorisation par le biais d’une interface Web. Malheureusement, le WSAT fonctionne uniquement pour les sites Web locaux, ce qui signifie qu’il ne peut pas être utilisé pour gérer à distance les comptes d’utilisateur, les rôles et les règles d’autorisation pour l’application Web dans l’environnement de production. Nous allons examiner différentes façons d’implémenter un comportement de type WSAT à partir de votre site Web de production.

### <a name="adding-the-database-objects-using-aspnet_regsqlexe"></a>Ajout des objets de base de données à l’aide de ASPNET\_RegSql. exe

Le didacticiel sur le *déploiement d’une base de données a* montré comment copier les tables et les données de la base de données de développement vers la base de données de production, et ces techniques peuvent certainement être utilisées pour copier les objets de base de données des services d’application dans la base de données de production. Une autre option est l’outil `aspnet_regsql.exe`, qui ajoute ou supprime les objets de base de données des services d’application d’une base de données.

> [!NOTE]
> L’outil `aspnet_regsql.exe` crée les objets de base de données sur une base de données spécifiée. Elle ne migre pas les données de ces objets de base de données de la base de données de développement vers la base de données de production. Si vous souhaitez copier le compte d’utilisateur et les informations de rôle dans la base de données de développement vers la base de données de production, utilisez les techniques décrites dans le didacticiel sur le *déploiement d’une base de données* .

Voyons comment ajouter les objets de base de données à la base de données de production à l’aide de l’outil `aspnet_regsql.exe`. Commencez par ouvrir l’Explorateur Windows et accédez au répertoire .NET Framework version 2,0 sur votre ordinateur,% WINDIR% \ Microsoft. NET\Framework\v2.0.50727. Vous devez trouver l’outil `aspnet_regsql.exe`. Cet outil peut être utilisé à partir de la ligne de commande, mais il comprend également une interface utilisateur graphique. double-cliquez sur le fichier `aspnet_regsql.exe` pour lancer son composant graphique.

L’outil démarre en affichant un écran de démarrage expliquant son rôle. Cliquez sur suivant pour passer à l’écran « Sélectionner une option d’installation », comme illustré à la figure 1. À partir de là, vous pouvez choisir d’ajouter les objets de base de données des services d’application ou de les supprimer d’une base de données. Étant donné que nous voulons ajouter ces objets à la base de données de production, sélectionnez l’option « configurer SQL Server pour les services d’application », puis cliquez sur suivant.

[![choisir de configurer SQL Server pour Services d’application](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Figure 1**: choisir de configurer SQL Server pour services d’application ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))

Dans l’écran « Sélectionner le serveur et la base de données », vous invite à entrer des informations pour vous connecter à la base de données. Entrez le serveur de base de données, les informations d’identification de sécurité et le nom de la base de données qui vous sont fournis par votre société d’hébergement Web, puis cliquez sur suivant.

> [!NOTE]
> Après avoir entré votre serveur de base de données et les informations d’identification, vous pouvez recevoir une erreur lors du développement de la liste déroulante de la base de données. L’outil `aspnet_regsql.exe` interroge la table système `sysdatabases` pour récupérer une liste de bases de données sur le serveur, mais certaines sociétés hébergeant le Web verrouillent leurs serveurs de base de données afin que ces informations ne soient pas disponibles publiquement. Si vous recevez cette erreur, vous pouvez taper le nom de la base de données directement dans la liste déroulante.

[![fournir à l’outil les informations de connexion de votre base de données](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Figure 2**: fournir l’outil avec les informations de connexion de votre base de données ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))

L’écran suivant résume les actions qui sont sur le point d’être effectuées, à savoir que les objets de base de données des services d’application vont être ajoutés à la base de données spécifiée. Cliquez sur suivant pour terminer cette action. Après quelques instants, l’écran final s’affiche, en notant que les objets de base de données ont été ajoutés (voir la figure 3).

[![réussie ! Les objets de base de données Services d’application ont été ajoutés à la base de données de production](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Figure 3**: réussite ! Les objets de base de données Services d’application ont été ajoutés à la base de données de production ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))

Pour vérifier que les objets de base de données des services d’application ont été correctement ajoutés à la base de données de production, ouvrez SQL Server Management Studio et connectez-vous à votre base de données de production. Comme le montre la figure 4, vous devez maintenant voir les tables de base de données des services d’application dans votre base de données, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`, etc.

[![confirmer que les objets de base de données ont été ajoutés à la base de données de production](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Figure 4**: confirmer que les objets de base de données ont été ajoutés à la base de données de production ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))

Vous devez uniquement utiliser l’outil `aspnet_regsql.exe` lors du déploiement de votre application Web pour la première fois ou pour la première fois après avoir commencé à utiliser les services d’application. Une fois que ces objets de base de données se trouvent dans la base de données de production, ils n’ont pas besoin d’être rajoutés ou modifiés.

### <a name="copying-user-accounts-from-development-to-production"></a>Copie de comptes d’utilisateurs du développement à la production

Lorsque vous utilisez les classes de fournisseur `SqlMembershipProvider` et `SqlRoleProvider` pour stocker les informations des services d’application dans une base de données SQL Server, le compte d’utilisateur et les informations de rôle sont stockés dans diverses tables de base de données, notamment `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`et `aspnet_UsersInRoles`, entre autres. Si, au cours du développement, vous créez des comptes d’utilisateur dans l’environnement de développement, vous pouvez répliquer ces comptes d’utilisateur en production en copiant les enregistrements correspondants à partir des tables de base de données applicables. Si vous avez utilisé l’Assistant Publication de base de données pour déployer les objets de base de données des services d’application, vous avez peut-être également choisi de copier les enregistrements, ce qui a pour effet que les comptes d’utilisateur créés dans le développement soient également en production. Toutefois, en fonction de vos paramètres de configuration, vous constaterez peut-être que les utilisateurs dont les comptes ont été créés en développement et copiés en production ne sont pas en mesure de se connecter à partir du site Web de production. Quelle en est la raison ?

Les classes de fournisseur `SqlMembershipProvider` et `SqlRoleProvider` ont été conçues de telle sorte qu’une base de données unique pouvait servir de magasin d’utilisateurs pour plusieurs applications, où chaque application pourrait, en théorie, avoir des utilisateurs avec des noms d’utilisateur et des rôles qui se chevauchent avec le même nom. Pour permettre cette flexibilité, la base de données gère une liste d’applications dans la table `aspnet_Applications`, et chaque utilisateur est associé à l’une de ces applications. Plus précisément, la table `aspnet_Users` contient une colonne `ApplicationId` qui relie chaque utilisateur à un enregistrement de la table `aspnet_Applications`.

En plus de la colonne `ApplicationId`, le tableau `aspnet_Applications` inclut également une colonne `ApplicationName`, qui fournit un nom plus convivial pour l’application. Lorsqu’un site Web tente d’utiliser un compte d’utilisateur, tel que la validation des informations d’identification d’un utilisateur à partir de la page de connexion, il doit indiquer à la classe `SqlMembershipProvider` l’application à utiliser. Pour ce faire, il fournit généralement le nom de l’application, et cette valeur provient de la configuration du fournisseur dans `Web.config`, spécifiquement via l’attribut `applicationName`.

Mais que se passe-t-il si l’attribut `applicationName` n’est pas spécifié dans `Web.config`? Dans ce cas, le système d’appartenance utilise le chemin d’accès racine de l’application comme valeur de `applicationName`. Si l’attribut `applicationName` n’est pas explicitement défini dans `Web.config`, il est possible que l’environnement de développement et l’environnement de production utilisent une racine d’application différente. ils seront donc associés à différents noms d’application dans les services d’application. Si une telle incompatibilité se produit, alors les utilisateurs créés dans l’environnement de développement auront une valeur `ApplicationId` qui ne correspond pas à la valeur `ApplicationId` pour l’environnement de production. Le résultat net est que ces utilisateurs ne pourront pas se connecter.

> [!NOTE]
> Si vous vous trouvez dans cette situation, avec des comptes d’utilisateur copiés en production avec une valeur de `ApplicationId` incompatible, vous pouvez écrire une requête pour mettre à jour ces valeurs de `ApplicationId` incorrectes sur le `ApplicationId` utilisé en production. Une fois mis à jour, les utilisateurs dont les comptes ont été créés dans l’environnement de développement peuvent désormais se connecter à l’application Web en production.

La bonne nouvelle, c’est qu’il existe une étape simple que vous pouvez suivre pour vous assurer que les deux environnements utilisent le même `ApplicationId`-explicitement définir l’attribut `applicationName` dans `Web.config` pour tous vos fournisseurs de services d’application. Je définis explicitement l’attribut `applicationName` sur « BookReviews » dans le `<membership>` et `<roleManager>` éléments comme cet extrait de code `Web.config`.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Pour plus d’informations sur la définition de l’attribut `applicationName` et le raisonnement qui le concerne, reportez-vous au billet de blog de [*Scott Guthrie*](https://weblogs.asp.net/scottgu/) , [*définir toujours la propriété ApplicationName lors de la configuration de l’appartenance ASP.net et d’autres fournisseurs*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Gestion des comptes d’utilisateur dans l’environnement de production

L’outil d’administration de site Web ASP.NET (WSAT) facilite la création et la gestion de comptes d’utilisateurs, la définition et l’application de rôles, ainsi que l’utilisation de règles d’autorisation basées sur les utilisateurs et les rôles. Vous pouvez lancer le WSAT à partir de Visual Studio en accédant au Explorateur de solutions et en cliquant sur l’icône de configuration ASP.NET ou en accédant aux menus du site Web ou du projet et en sélectionnant l’élément de menu Configuration ASP.NET. Malheureusement, les WSAT peuvent uniquement fonctionner avec des sites Web locaux. Par conséquent, vous ne pouvez pas utiliser le WSAT à partir de votre station de travail pour gérer le site Web dans l’environnement de production.

La bonne nouvelle, c’est que toutes les fonctionnalités exposées par le WSAT sont disponibles par programme via les API d’appartenance et de rôles. en outre, un grand nombre des écrans WSAT utilisent les contrôles de connexion ASP.NET standard. En bref, vous pouvez ajouter des pages ASP.NET à votre site Web, qui offrent les fonctionnalités de gestion nécessaires.

Rappelez-vous qu’un didacticiel précédent a mis à jour l’application Web de révisions de livres pour inclure un dossier `~/Admin`, et que ce dossier a été configuré pour autoriser uniquement les utilisateurs du rôle d’administrateur. J’ai ajouté une page à ce dossier nommé `CreateAccount.aspx` à partir duquel un administrateur peut créer un nouveau compte d’utilisateur. Cette page utilise le contrôle CreateUserWizard pour afficher l’interface utilisateur et la logique backend pour créer un nouveau compte d’utilisateur. De plus, j’ai personnalisé le contrôle pour inclure une case à cocher qui indique si le nouvel utilisateur doit également être ajouté au rôle d’administrateur (voir figure 5). Avec un peu de travail, vous pouvez créer un ensemble personnalisé de pages qui implémente les tâches liées à l’utilisateur et à la gestion des rôles qui seraient autrement fournies par WSAT.

> [!NOTE]
> Pour plus d’informations sur l’utilisation des API d’appartenance et de rôles, ainsi que sur les contrôles Web ASP.NET liés aux connexions, consultez les didacticiels sur la sécurité de votre [*site Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Pour plus d’informations sur la personnalisation du contrôle CreateUserWizard, reportez-vous à l’article [*création de comptes d’utilisateurs*](../../older-versions-security/membership/creating-user-accounts-vb.md) et stockage d' [*informations utilisateur supplémentaires*](../../older-versions-security/membership/storing-additional-user-information-vb.md) , ou consultez l’article [*Erich Peterson*](http://www.erichpeterson.com/) s, [*Personnalisation du contrôle CreateUserWizard*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).

[Les administrateurs de ![peuvent créer des comptes d’utilisateur](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Figure 5**: les administrateurs peuvent créer des comptes d’utilisateur ([cliquez pour afficher l’image en taille réelle](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))

Si vous avez besoin de toutes les fonctionnalités de la WSAT, Découvrez comment [*déployer votre propre outil d’administration de site Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), dans lequel l’auteur Dan Clem parcourt le processus de création d’un outil personnalisé de type WSAT. Dan partage son code source de l’application ( C#en) et fournit des instructions pas à pas pour l’ajouter à votre site Web hébergé.

## <a name="summary"></a>Récapitulatif

Lors du déploiement d’une application Web qui utilise l’implémentation de la base de données des services d’application, vous devez d’abord vous assurer que la base de données de production contient les objets de base de données requis. Ces objets peuvent être ajoutés à l’aide des techniques décrites dans le didacticiel sur le *déploiement d’une base de données* . vous pouvez également utiliser l’outil `aspnet_regsql.exe`, comme nous l’avons vu dans ce didacticiel. Autres défis que nous avons évoqué sur la synchronisation du nom d’application utilisé dans les environnements de développement et de production (ce qui est important si vous souhaitez que les utilisateurs et les rôles créés dans l’environnement de développement soient valides en production) et les techniques de gestion des utilisateurs et des rôles dans l’environnement de production.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [*Outil d’inscription de SQL Server ASP.NET (aspnet_regsql. exe)* ](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Création de la base de données Services d’application pour SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Création du schéma d’appartenance dans SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Examen de l’appartenance, des rôles et du profil ASP.NET s*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Déploiement de votre propre outil d’administration de site Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Didacticiels sur la sécurité de site Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Vue d’ensemble de l’outil Administration de site Web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Précédent](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [Suivant](strategies-for-database-development-and-deployment-vb.md)
