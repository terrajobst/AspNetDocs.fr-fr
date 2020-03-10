---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: Utilisateurs et rôles sur le site Web de production (VB) | Microsoft Docs
author: rick-anderson
description: L’outil d’administration de site Web ASP.NET (WSAT) fournit une interface utilisateur Web pour la configuration des paramètres d’appartenance et de rôles, ainsi que pour la création, la modification, une...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: d4ce8b278322684be2d44faefd6e69fc524bbe18
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528265"
---
# <a name="users-and-roles-on-the-production-website-vb"></a>Utilisateurs et rôles sur le site Web de production (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> L’outil d’administration de site Web ASP.NET (WSAT) fournit une interface utilisateur Web pour la configuration des paramètres d’appartenance et de rôles, ainsi que pour la création, la modification et la suppression d’utilisateurs et de rôles. Malheureusement, le WSAT fonctionne uniquement lorsqu’il est visité depuis localhost, ce qui signifie que vous ne pouvez pas accéder à l’outil d’administration du site Web de production via votre navigateur. La bonne nouvelle, c’est qu’il existe des solutions de contournement qui permettent de gérer les utilisateurs et les rôles en production. Ce didacticiel examine ces solutions de contournement et d’autres.

## <a name="introduction"></a>Introduction

ASP.NET 2,0 a introduit un certain nombre de services d' *application*, qui sont une suite de services de bloc de construction que vous pouvez ajouter à votre application Web. Nous avons ajouté les services d’appartenance et de rôles au site Web de révisions de livres dans le [didacticiel *configuration d’un site Web qui utilise services d’application* ](configuring-a-website-that-uses-application-services-vb.md). Le service d’appartenance facilite la création et la gestion des comptes d’utilisateur. le service de rôles offre une API permettant de classer les utilisateurs dans des groupes. Le site de révisions de livres comporte trois comptes d’utilisateur : Scott, Jisun et Alice, ainsi qu’un rôle unique, admin, avec Scott et Jisun dans le rôle d’administrateur.

ASP. Les services d’application du réseau ne sont pas liés à une implémentation spécifique. Au lieu de cela, vous indiquez aux services d’application d’utiliser un *fournisseur*particulier, et ce fournisseur implémente le service à l’aide d’une technologie particulière. Nous avons configuré l’application Web de révisions de livres pour utiliser les fournisseurs `SqlMembershipProvider` et `SqlRoleProvider` pour les services d’appartenance et de rôles. Ces deux fournisseurs stockent les informations de compte d’utilisateur et de rôle dans une base de données SQL Server et sont les fournisseurs les plus couramment utilisés pour les applications Web basées sur Internet hébergées dans une société d’hébergement Web.

Un défi courant pour les développeurs qui utilisent les services d’appartenance et de rôles est de gérer les utilisateurs et les rôles dans l’environnement de production. Comment supprimer un compte d’utilisateur du site Web de production, ajouter un nouveau rôle ou ajouter un utilisateur existant à un rôle existant ? Ce didacticiel explore différentes techniques permettant de gérer les utilisateurs et les rôles sur le site Web de production.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Utilisation de l’outil Administration de site Web ASP.NET

ASP.NET comprend un [outil d’administration de site Web](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) qui facilite la création et la gestion des comptes et des rôles d’utilisateur, ainsi que la spécification de règles d’autorisation basées sur les utilisateurs et les rôles. Pour utiliser WSAT, cliquez sur l’icône de configuration ASP.NET dans le Explorateur de solutions ou accédez au menu site Web ou projet et choisissez l’option de configuration ASP.NET. L’une ou l’autre approche lance un navigateur Web et le pointe vers le WSAT à une adresse comme : `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

Le WSAT est divisé en trois sections :

- **Sécurité** : gérez les utilisateurs, les rôles et les règles d’autorisation.
- **ApplicationConfiguration** -gérer &lt;les paramètres&gt; APPSETTINGS et SMTP à partir d’ici. Vous pouvez également mettre l’application hors connexion et gérer les paramètres de débogage et de suivi à partir de cet emplacement, ainsi que spécifier la page d’erreurs personnalisée par défaut.
- **ProviderConfiguration** : configurez les fournisseurs utilisés par les services d’application.

La section sécurité (illustrée dans la **figure 1**) comprend des liens permettant de créer des utilisateurs, de gérer des utilisateurs, de créer et de gérer des rôles, ainsi que de créer et de gérer des règles d’accès. À partir de là, vous pouvez ajouter un nouveau rôle au système, supprimer un utilisateur existant ou ajouter ou supprimer des rôles d’un compte d’utilisateur particulier.

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**Figure 1**: la section sécurité WSAT comprend des options pour la gestion des utilisateurs et des rôles  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image3.png))

Malheureusement, le WSAT est uniquement accessible localement. Vous ne pouvez pas visiter le WSAT sur votre site Web de production distant. Si vous visitez `www.yoursite.com/asp.netwebadminfiles/default.aspx` vous recevez une réponse 404 introuvable. Le code qui alimente le WSAT utilise les classes `Membership` et `Roles` dans le .NET Framework pour créer, modifier et supprimer des utilisateurs et des rôles. Ces classes consultent les informations de configuration de l’application Web pour déterminer le fournisseur à utiliser ; dans le didacticiel [ *configuration d’un site Web qui utilise services d’application* ](configuring-a-website-that-uses-application-services-vb.md) , nous avons configuré le site Web de révisions de livres pour utiliser les fournisseurs de `SqlMembershipProvider` et de `SqlRoleProvider`. Cela implique l’ajout de `<membership>` et de sections `<roleManager>` à `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

Notez que les sections `<membership>` et `<roleManager>` référencent respectivement les fournisseurs `SqlMembershipProvider` et `SqlRoleProvider` dans leur attribut `type`. Ces fournisseurs stockent les informations d’utilisateur et de rôle dans une base de données SQL Server spécifiée. La base de données utilisée par ces fournisseurs est spécifiée par l’attribut `connectionStringName`, `ReviewsConnectionString`, qui est défini dans le fichier `~/ConfigSections/databaseConnectionStrings.config`. Rappelez-vous que le fichier `databaseConnectionStrings.config` dans l’environnement de développement contient la chaîne de connexion à la base de données de développement, tandis que le fichier `databaseConnectionStrings.config` en production contient la chaîne de connexion à la base de données de production.

En résumé, le WSAT doit être accessible localement par le biais de l’environnement de développement, et il fonctionne avec les informations d’utilisateur et de rôle dans la base de données spécifiée dans le fichier `databaseConnectionStrings.config`. Par conséquent, si nous modifions les informations de chaîne de connexion dans le fichier `databaseConnectionStrings.config` sur l’environnement de développement, nous pouvons utiliser le WSAT localement pour gérer les utilisateurs et les rôles dans l’environnement de production.

Pour illustrer cette fonctionnalité, ouvrez le fichier `databaseConnectionStrings.config` dans Visual Studio sur l’environnement de développement et remplacez la chaîne de connexion de la base de données de développement par la chaîne de connexion de la base de données de production. Ensuite, lancez le WSAT, accédez à l’onglet sécurité, puis ajoutez un nouvel utilisateur nommé Sam avec le mot de passe « password ! ». (moins les guillemets). La **figure 2** illustre l’écran WSAT lors de la création de ce compte.

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**Figure 2**: créer un nouvel utilisateur nommé Sam dans l’environnement de production  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image6.png))

Étant donné que nous avons modifié la chaîne de connexion dans `databaseConnectionStrings.config` pour qu’elle pointe vers le serveur de base de données de production, Sam a été ajouté en tant qu’utilisateur dans l’environnement de production. Pour vérifier cela, remplacez la chaîne de connexion dans le fichier `databaseConnectionStrings.config` par la base de données de développement, puis accédez à la page `Login.aspx` de l’environnement de développement. Essayez de vous connecter en tant que Sam (voir **figure 3**).

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**Figure 3**: vous ne pouvez pas vous connecter en tant que Sam dans l’environnement de développement  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image9.png))

Vous ne pouvez pas vous connecter en tant que Sam dans l’environnement de développement, car les informations de compte d’utilisateur n’existent pas dans la base de données locale. Au lieu de cela, a été ajouté à la base de données de production. Pour vérifier cela, affichez le contenu de la table `aspnet_Users` dans les bases de données de développement et de production. Dans l’environnement de développement, il ne doit y avoir que trois enregistrements pour les utilisateurs Scott, Jisun et Alice. Toutefois, la table `aspnet_Users` de la base de données de production comporte quatre enregistrements : Scott, Jisun, Alice et Sam. Par conséquent, Sam peut se connecter par le biais du site Web en production, mais pas par le biais de l’environnement de développement.

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**Figure 4**: Sam peut se connecter au site Web de production  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> N’oubliez pas de remplacer la chaîne de connexion dans le fichier `databaseConnectionStrings.config` par la chaîne de connexion de la base de données de développement lorsque vous avez fini d’utiliser le WSAT, sinon, vous utiliserez les données de production lors du test du site dans l’environnement de développement. Gardez également à l’esprit que, bien que la technique que nous venons d’évoquer nous permette d’utiliser le WSAT pour gérer à distance les utilisateurs et les rôles, les modifications apportées à l’une des autres options de configuration WSAT (règles d’accès, paramètres SMTP, débogage et paramètres de suivi, etc.) modifient le fichier `Web.config`. Par conséquent, toutes les modifications apportées aux paramètres s’appliquent à l’environnement de développement et non à l’environnement de production.

## <a name="creating-custom-user-and-role-management-web-pages"></a>Création de pages Web personnalisées de gestion des utilisateurs et des rôles

WSAT fournit un système prêt à l’emploi pour la gestion des utilisateurs et des rôles, mais il ne peut être lancé que localement et nécessite d’apporter des modifications aux informations de la chaîne de connexion afin de gérer les utilisateurs et les rôles en production. La plupart des sites Web qui prennent en charge les comptes d’utilisateur incluent également un certain nombre de pages Web d’administration des utilisateurs et des rôles qui permettent aux administrateurs de gérer les utilisateurs et les rôles à partir des pages du site. Ces pages d’administration Web facilitent grandement la gestion des utilisateurs et des rôles. elles sont essentielles pour les sites où il peut y avoir de nombreux administrateurs ou administrateurs qui n’ont pas accès à ou l’arrière-plan technique pour utiliser Visual Studio pour lancer le WSAT.

ASP.NET comprend un certain nombre de contrôles Web intégrés de connexion qui facilitent l’implémentation d’un grand nombre de ces pages Web d’administration. Par exemple, vous pouvez créer une page permettant aux administrateurs de créer un nouveau compte d’utilisateur en faisant glisser le contrôle CreateUserWizard sur la page et en définissant quelques propriétés. En fait, la page permettant de créer des utilisateurs dans le WSAT illustré à la **figure 2** utilise le même contrôle CreateUserWizard que vous pouvez ajouter à vos pages. En outre, les fonctionnalités des services d’appartenance et de rôles sont disponibles par programme par le biais des classes `Membership` et `Roles` dans le .NET Framework. Avec ces classes, vous pouvez écrire du code pour créer, modifier et supprimer des utilisateurs et des rôles, ainsi que pour ajouter ou supprimer des utilisateurs dans des rôles, pour déterminer quels sont les utilisateurs dans quels rôles et pour effectuer d’autres tâches liées aux utilisateurs et aux rôles.

Dans le [didacticiel *configuration d’un site Web qui utilise services d’application* ](configuring-a-website-that-uses-application-services-vb.md) , j’ai ajouté une page au dossier `Admin` nommé `CreateAccount.aspx`. Cette page permet à un administrateur d’ajouter un nouveau compte d’utilisateur au site et de spécifier si l’utilisateur nouvellement créé se trouve dans le rôle d’administrateur (voir la **figure 5**).

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**Figure 5**: les administrateurs peuvent créer des comptes d’utilisateur  
([Cliquez pour afficher l’image en taille réelle](users-and-roles-on-the-production-website-vb/_static/image15.png))

Pour plus d’informations sur la création de pages d’administration des utilisateurs et des rôles, ainsi que des instructions pas à pas sur l’utilisation des classes `Membership` et `Roles` et les contrôles Web ASP.NET liés aux connexions, consultez les didacticiels sur la sécurité de votre [site Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Vous y trouverez des conseils sur la façon de créer des pages Web pour créer des comptes, créer et gérer des rôles, assigner des utilisateurs à des rôles et effectuer d’autres tâches d’administration courantes.

Pour implémenter une fonctionnalité de type WSAT sur le site Web de production, vous pouvez toujours créer votre propre série de pages Web qui implémentent les fonctionnalités de WSAT. Pour vous aider à commencer, consultez le code source WSAT, qui se trouve dans le dossier `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Une autre option consiste à utiliser l’alternative WSAT de Dan Clem, qu’il partage dans son article, en [déployant votre propre outil d’administration de site Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan guide les lecteurs à travers le processus de création d’un outil WSAT personnalisé, inclut le code source de son application pour le C#téléchargement (dans), et fournit des instructions pas à pas pour ajouter son WSAT personnalisé à un site Web hébergé.

## <a name="summary"></a>Récapitulatif

L’outil d’administration de site Web ASP.NET (WSAT) peut être utilisé conjointement avec les services d’application d’appartenance et de rôles pour gérer les informations d’utilisateur et de rôle de votre site Web. Malheureusement, la WSAT est uniquement accessible localement et ne peut pas être consultée à partir de votre site Web de production. Toutefois, en modifiant la chaîne de connexion dans l’environnement de développement de façon à ce qu’elle pointe vers la base de données de production, vous pouvez utiliser le WSAT pour gérer les utilisateurs et les rôles sur le site Web de production.

Bien que l’approche WSAT offre un moyen simple et rapide de gérer les utilisateurs et les rôles, elle nécessite le lancement de WSAT à partir de Visual Studio, ainsi que la modification temporaire des informations de la chaîne de connexion. Le WSAT offre un moyen rapide de gérer les utilisateurs et les rôles en production, mais il est fastidieux et ne fonctionne pas correctement pour les sites Web avec plusieurs administrateurs ou avec les administrateurs qui n’ont pas ou ne sont pas familiarisés avec Visual Studio et WSAT. Pour ces raisons, la plupart des sites Web qui prennent en charge les comptes d’utilisateur incluent un ensemble de pages Web d’administration. Un tel ensemble de pages Web élimine le besoin de WSAT et est utilisé par différents utilisateurs administratifs à partir de n’importe quel ordinateur.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Examen d’ASP. Appartenance, rôles et profil du réseau](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Déploiement de votre propre outil d’administration de site Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Vue d’ensemble de l’outil Administration de site Web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Didacticiels sur la sécurité de site Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Précédent](precompiling-your-website-vb.md)
