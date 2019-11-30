---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Notions de base de la sécuritéC#et prise en charge de ASP.net () | Microsoft Docs
author: rick-anderson
description: Il s’agit du premier didacticiel d’une série de didacticiels qui explorent les techniques d’authentification des visiteurs via un formulaire Web, autorisant l’accès à une partie...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ccaac101a83d0e28b07b220b8b7b61a9039227e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642449"
---
# <a name="security-basics-and-aspnet-support-c"></a>Concepts de base et prise en charge de la sécurité par ASP.NET (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Il s’agit du premier didacticiel d’une série de didacticiels qui explorent les techniques d’authentification des visiteurs via un formulaire Web, qui autorisent l’accès à des pages et des fonctionnalités particulières et la gestion des comptes d’utilisateur dans une application ASP.NET.

## <a name="introduction"></a>Introduction

Quelles sont les forums, les sites de commerce électronique, les sites Web de messagerie en ligne, les sites Web du portail et les sites de réseaux sociaux en commun ? Ils proposent tous des *comptes d’utilisateur*. Les sites qui proposent des comptes d’utilisateur doivent fournir un certain nombre de services. Au minimum, les nouveaux visiteurs doivent être en mesure de créer un compte et de renvoyer des visiteurs doivent être en mesure de se connecter. De telles applications Web peuvent prendre des décisions en fonction de l’utilisateur connecté : certaines pages ou actions peuvent être limitées uniquement aux utilisateurs connectés, ou à un certain sous-ensemble d’utilisateurs. d’autres pages peuvent afficher des informations spécifiques à l’utilisateur connecté, ou peuvent afficher plus ou moins d’informations, en fonction de l’utilisateur qui consulte la page.

Il s’agit du premier didacticiel d’une série de didacticiels qui explorent les techniques d’authentification des visiteurs via un formulaire Web, qui autorisent l’accès à des pages et des fonctionnalités particulières et la gestion des comptes d’utilisateur dans une application ASP.NET. Au cours de ces didacticiels, nous allons examiner comment :

- Identifier et connecter des utilisateurs à un site Web
- Utilisez ASP. Infrastructure d’appartenance NET pour gérer les comptes d’utilisateur
- Créer, mettre à jour et supprimer des comptes d’utilisateur
- Limiter l’accès à une page Web, à un répertoire ou à des fonctionnalités spécifiques basées sur l’utilisateur connecté
- Utilisez ASP. Infrastructure des rôles du réseau pour associer des comptes d’utilisateur à des rôles
- Gérer les rôles d’utilisateur
- Limiter l’accès à une page Web, à un répertoire ou à des fonctionnalités spécifiques en fonction du rôle de l’utilisateur connecté
- Personnaliser et étendre ASP. Contrôles Web de sécurité du réseau

Ces didacticiels sont destinés à être concis et fournissent des instructions pas à pas avec de nombreuses captures d’écran pour vous guider tout au long du processus. Chaque Didacticiel est disponible dans C# et Visual Basic versions et comprend un téléchargement du code complet utilisé. (Ce premier didacticiel se concentre sur les concepts de sécurité d’un point de vue de haut niveau et ne contient donc pas de code associé.)

Dans ce didacticiel, nous aborderons les concepts de sécurité importants et les fonctionnalités disponibles dans ASP.NET pour vous aider à implémenter l’authentification par formulaire, l’autorisation, les comptes d’utilisateurs et les rôles. Commençons !

> [!NOTE]
> La sécurité est un aspect important de toute application qui s’étend aux décisions physiques, technologiques et de stratégie, et qui requiert un degré élevé de connaissance de la planification et du domaine. Cette série de didacticiels n’est pas conçue comme un guide pour le développement d’applications Web sécurisées. Au lieu de cela, elle se concentre spécifiquement sur l’authentification par formulaire, l’autorisation, les comptes d’utilisateurs et les rôles. Bien que certains concepts de sécurité en rapport avec ces problèmes soient abordés dans cette série, d’autres ne sont pas explorés.

## <a name="authentication-authorization-user-accounts-and-roles"></a>Authentification, autorisation, comptes d’utilisateurs et rôles

L’authentification, l’autorisation, les comptes d’utilisateurs et les rôles sont quatre termes qui seront très souvent utilisés dans cette série de didacticiels. J’aimerais donc prendre un moment rapide pour définir ces termes dans le contexte de la sécurité Web. Dans un modèle client-serveur, tel qu’Internet, il existe de nombreux scénarios dans lesquels le serveur doit identifier le client qui effectue la demande. L' *authentification* est le processus qui consiste à déterminer l’identité du client. Un client qui a été identifié avec succès est dit *authentifié*. Un client non identifié est dit non *authentifié* ou *anonyme*.

Les systèmes d’authentification sécurisés impliquent au moins l’une des trois facettes suivantes : [un nom que vous connaissez, un nom de votre choix ou un autre](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html)outil. La plupart des applications Web s’appuient sur un élément connu du client, tel qu’un mot de passe ou un code PIN. Les informations utilisées pour identifier un utilisateur, son nom d’utilisateur et son mot de passe, par exemple, sont appelées *informations d’identification*. Cette série de didacticiels se concentre sur *l’authentification par formulaire*, qui est un modèle d’authentification dans lequel les utilisateurs se connectent au site en fournissant leurs informations d’identification dans un formulaire de page Web. Nous avons tous connu ce type d’authentification auparavant. Accédez à n’importe quel site de commerce électronique. Lorsque vous êtes prêt à consulter, vous êtes invité à vous connecter en entrant votre nom d’utilisateur et votre mot de passe dans les zones de texte d’une page Web.

En plus d’identifier les clients, un serveur peut avoir besoin de limiter les ressources ou les fonctionnalités accessibles en fonction du client qui effectue la demande. L' *autorisation* est le processus qui consiste à déterminer si un utilisateur particulier a l’autorité nécessaire pour accéder à une ressource ou à une fonctionnalité spécifique.

Un *compte d’utilisateur* est un magasin pour la conservation des informations relatives à un utilisateur particulier. Les comptes d’utilisateurs doivent inclure au minimum des informations qui identifient de façon unique l’utilisateur, telles que le nom de connexion et le mot de passe de l’utilisateur. Outre ces informations essentielles, les comptes d’utilisateurs peuvent inclure des éléments tels que l’adresse de messagerie de l’utilisateur. Date et heure de création du compte ; Date et heure de la dernière connexion ; prénom et nom ; Numéro de téléphone ; et adresse postale. Lors de l’utilisation de l’authentification par formulaire, les informations de compte d’utilisateur sont généralement stockées dans une base de données relationnelle comme Microsoft SQL Server.

Les applications Web qui prennent en charge les comptes d’utilisateur peuvent éventuellement regrouper des utilisateurs dans des *rôles*. Un rôle est simplement une étiquette qui est appliquée à un utilisateur et fournit une abstraction pour la définition des règles d’autorisation et des fonctionnalités au niveau de la page. Par exemple, un site Web peut inclure un rôle d’administrateur avec des règles d’autorisation qui interdisent à quiconque, mais également à un administrateur d’accéder à un ensemble spécifique de pages Web. En outre, toute une série de pages accessibles à tous les utilisateurs (y compris les non-administrateurs) peut afficher des données supplémentaires ou offrir des fonctionnalités supplémentaires lorsqu’ils sont visités par les utilisateurs du rôle Administrateurs. À l’aide de rôles, nous pouvons définir ces règles d’autorisation sur la base d’un rôle par rôle plutôt que d’un utilisateur par utilisateur.

## <a name="authenticating-users-in-an-aspnet-application"></a>Authentification des utilisateurs dans une application ASP.NET

Lorsqu’un utilisateur entre une URL dans la fenêtre d’adresse de son navigateur ou clique sur un lien, le navigateur envoie une requête [http (Hypertext Transfer Protocol)](http://en.wikipedia.org/wiki/HTTP) au serveur Web pour le contenu spécifié, qu’il s’agisse d’une page ASP.net, d’une image, d’un fichier JavaScript ou de tout autre type de contenu. Le serveur Web est chargé de retourner le contenu demandé. Dans ce cas, il doit déterminer un certain nombre de choses sur la demande, notamment la personne qui a effectué la demande et si l’identité est autorisée à récupérer le contenu demandé.

Par défaut, les navigateurs envoient des requêtes HTTP qui n’ont pas de type d’informations d’identification. Toutefois, si le navigateur inclut des informations d’authentification, le serveur Web démarre le flux de travail d’authentification, qui tente d’identifier le client à l’origine de la demande. Les étapes du flux de travail d’authentification dépendent du type d’authentification utilisé par l’application Web. ASP.NET prend en charge trois types d’authentification : Windows, Passport et formulaires. Cette série de didacticiels se concentre sur l’authentification par formulaire, mais nous allons prendre quelques minutes pour comparer les magasins d’utilisateurs et le flux de travail d’authentification Windows.

### <a name="authentication-via-windows-authentication"></a>Authentification via l’authentification Windows

Le flux de travail d’authentification Windows utilise l’une des techniques d’authentification suivantes :

- Authentification de base
- Authentification Digest
- authentification Windows intégrée

Les trois techniques fonctionnent à peu près de la même façon : lorsqu’une demande anonyme non autorisée arrive, le serveur Web renvoie une réponse HTTP indiquant que l’autorisation est requise pour continuer. Le navigateur affiche ensuite une boîte de dialogue modale qui invite l’utilisateur à entrer son nom d’utilisateur et son mot de passe (voir figure 1). Ces informations sont ensuite renvoyées au serveur Web via un en-tête HTTP.

![Une boîte de dialogue modale invite l’utilisateur à entrer ses informations d’identification](security-basics-and-asp-net-support-cs/_static/image1.png)

**Figure 1**: une boîte de dialogue modale invite l’utilisateur à entrer ses informations d’identification

Les informations d’identification fournies sont validées par rapport au magasin d’utilisateurs Windows du serveur Web. Cela signifie que chaque utilisateur authentifié dans votre application Web doit avoir un compte Windows dans votre organisation. Cette situation est courante dans les scénarios intranet. En fait, lors de l’utilisation de l’authentification intégrée de Windows dans un paramètre intranet, le navigateur fournit automatiquement au serveur Web les informations d’identification utilisées pour se connecter au réseau, supprimant ainsi la boîte de dialogue illustrée dans la figure 1. Bien que l’authentification Windows soit idéale pour les applications intranet, elle est généralement irréalisable pour les applications Internet, car vous ne souhaitez pas créer de comptes Windows pour chaque utilisateur qui se connecte à votre site.

### <a name="authentication-via-forms-authentication"></a>Authentification via l’authentification par formulaire

L’authentification par formulaire, en revanche, est idéale pour les applications Web Internet. Rappelez-vous que l’authentification par formulaire identifie l’utilisateur en l’invitant à entrer ses informations d’identification via un formulaire Web. Par conséquent, lorsqu’un utilisateur tente d’accéder à une ressource non autorisée, il est automatiquement redirigé vers la page de connexion où il peut entrer ses informations d’identification. Les informations d’identification envoyées sont ensuite validées par rapport à un magasin d’utilisateurs personnalisé (généralement une base de données).

Après avoir vérifié les informations d’identification soumises, un *ticket d’authentification par formulaire* est créé pour l’utilisateur. Ce ticket indique que l’utilisateur a été authentifié et inclut des informations d’identification, telles que le nom d’utilisateur. Le ticket d’authentification par formulaire est (généralement) stocké en tant que cookie sur l’ordinateur client. Par conséquent, les visites suivantes du site Web incluent le ticket d’authentification par formulaire dans la requête HTTP, ce qui permet à l’application Web d’identifier l’utilisateur une fois qu’il s’est connecté.

La figure 2 illustre le flux de travail d’authentification par formulaire à partir d’un point bourré de haut niveau. Notez que les éléments d’authentification et d’autorisation dans ASP.NET agissent comme deux entités distinctes. Le système d’authentification par formulaire identifie l’utilisateur (ou signale qu’il est anonyme). Le système d’autorisation détermine si l’utilisateur a accès à la ressource demandée. Si l’utilisateur n’est pas autorisé (comme c’est le cas dans la figure 2 lors d’une tentative de visite anonyme de ProtectedPage. aspx), le système d’autorisation signale que l’utilisateur est refusé, ce qui amène le système d’authentification des formulaires à rediriger automatiquement l’utilisateur vers la page de connexion.

Une fois que l’utilisateur s’est connecté, les demandes HTTP suivantes incluent le ticket d’authentification par formulaire. Le système d’authentification par formulaire identifie simplement l’utilisateur : il s’agit du système d’autorisation qui détermine si l’utilisateur peut accéder à la ressource demandée.

![Flux de travail d’authentification par formulaire](security-basics-and-asp-net-support-cs/_static/image2.png)

**Figure 2**: flux de travail d’authentification par formulaire

Nous étudierons l’authentification par formulaire plus en détail dans les deux didacticiels suivants,[une vue d’ensemble de](an-overview-of-forms-authentication-cs.md) l’authentification par formulaire et de la configuration de l' [authentification par formulaire et des rubriques avancées](forms-authentication-configuration-and-advanced-topics-cs.md). Pour plus d’informations sur ASP. Les options d’authentification du réseau, consultez [authentification ASP.net](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limitation de l’accès aux pages Web, aux répertoires et aux fonctionnalités de page

ASP.NET propose deux méthodes pour déterminer si un utilisateur particulier a l’autorité nécessaire pour accéder à un fichier ou à un répertoire spécifique :

- **Autorisation de fichier** : étant donné que les pages ASP.net et les services Web sont implémentés en tant que fichiers résidant sur le système de fichiers du serveur Web, l’accès à ces fichiers peut être spécifié par le biais de listes de Access Control (ACL). L’autorisation de fichier est couramment utilisée avec l’authentification Windows, car les listes de contrôle d’accès sont des autorisations qui s’appliquent aux comptes Windows. Lors de l’utilisation de l’authentification par formulaire, toutes les demandes au niveau du système d’exploitation et du système de fichiers sont exécutées par le même compte Windows, quel que soit l’utilisateur visitant le site.
- **Autorisation d’URL**: avec l’autorisation d’URL, le développeur de pages spécifie les règles d’autorisation dans Web. config. Ces règles d’autorisation spécifient les utilisateurs ou les rôles autorisés ou non à accéder à certaines pages ou répertoires de l’application.

Autorisation de fichier et autorisation d’URL définissez des règles d’autorisation pour accéder à une page ASP.NET particulière ou à toutes les pages ASP.NET dans un répertoire particulier. À l’aide de ces techniques, nous pouvons demander à ASP.NET de refuser des demandes à une page particulière pour un utilisateur particulier, ou d’autoriser l’accès à un ensemble d’utilisateurs et de refuser l’accès à tous les autres utilisateurs. Qu’en est-il des scénarios dans lesquels tous les utilisateurs peuvent accéder à la page, mais les fonctionnalités de la page dépendent de l’utilisateur ? Par exemple, de nombreux sites qui prennent en charge des comptes d’utilisateur ont des pages qui affichent du contenu ou des données différents pour les utilisateurs authentifiés par rapport aux utilisateurs anonymes. Un utilisateur anonyme peut voir un lien pour se connecter au site, tandis qu’un utilisateur authentifié voit à la place un message de bienvenue, de bienvenue et un *nom* d’utilisateur, ainsi qu’un lien pour se déconnecter. Autre exemple : lorsque vous affichez un article sur un site d’enchères, vous obtenez des informations différentes selon que vous êtes un enchérisseur ou l’un de ces enchères.

Ces ajustements au niveau des pages peuvent être effectués de façon déclarative ou par programme. Pour afficher un contenu différent pour les utilisateurs authentifiés, il vous suffit de faire glisser un [contrôle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) sur votre page et d’entrer le contenu approprié dans ses modèles AnonymousTemplate et LoggedInTemplate. Vous pouvez également déterminer par programmation si la requête actuelle est authentifiée, qui est l’utilisateur, et à quels rôles ils appartiennent (le cas échéant). Vous pouvez utiliser ces informations pour afficher ou masquer des colonnes dans une grille ou des panneaux sur la page.

Cette série comprend trois didacticiels qui se concentrent sur l’autorisation. L' ***autorisation basée sur l’utilisateur***examine comment limiter l’accès à une page ou aux pages d’un répertoire pour des comptes d’utilisateur spécifiques. L' ***autorisation basée sur les rôles recherche des*** règles d’autorisation au niveau du rôle. Enfin, l' ***affichage du contenu basé sur le didacticiel utilisateur actuellement connecté*** explore la modification du contenu et des fonctionnalités d’une page particulière en fonction de l’utilisateur visitant la page. Pour plus d’informations sur ASP. Les options d’autorisation du réseau, consultez [ASP.net Authorization](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Comptes d’utilisateur et rôles

ASP. L’authentification par formulaire de .NET fournit une infrastructure permettant aux utilisateurs de se connecter à un site et de faire en sorte que leur état authentifié soit mémorisé au fil des visites de page. Et l’autorisation d’URL offre une infrastructure pour limiter l’accès à des fichiers ou des dossiers spécifiques dans une application ASP.NET. Toutefois, aucune fonctionnalité ne fournit un moyen de stocker les informations de compte d’utilisateur ou de gérer les rôles.

Avant ASP.NET 2,0, les développeurs étaient responsables de la création de leurs propres magasins d’utilisateurs et de rôles. Ils étaient également sur le crochet pour la conception des interfaces utilisateur et l’écriture du code pour les pages essentielles relatives au compte d’utilisateur, comme la page de connexion et la page pour créer un nouveau compte, entre autres. Sans aucune infrastructure de compte d’utilisateur intégrée dans ASP.NET, chaque développeur qui implémente des comptes d’utilisateur devait parvenir à des décisions de conception sur des questions comme, Comment faire stocker des mots de passe ou d’autres informations sensibles ? et quelles directives dois-je imposer en ce qui concerne la longueur et la force du mot de passe ?

Aujourd’hui, l’implémentation de comptes d’utilisateur dans une application ASP.NET est beaucoup plus simple grâce à l' *infrastructure d’appartenance* et aux contrôles Web de connexion intégrés. L’infrastructure d’appartenance est un petit nombre de classes dans l' [espace de noms System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) qui fournissent des fonctionnalités permettant d’effectuer des tâches essentielles relatives au compte d’utilisateur. La classe de clé dans l’infrastructure d’appartenance est la [classe d’appartenance](https://msdn.microsoft.com/library/system.web.security.membership.aspx), qui a des méthodes telles que :

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

L’infrastructure d’appartenance utilise le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), qui sépare correctement l’API de l’infrastructure d’appartenance de son implémentation. Cela permet aux développeurs d’utiliser une API commune, mais leur permet d’utiliser une implémentation qui répond aux besoins personnalisés de leur application. En bref, la classe d’appartenance définit la fonctionnalité essentielle de l’infrastructure (les méthodes, les propriétés et les événements), mais ne fournit pas réellement de détails d’implémentation. Au lieu de cela, les méthodes de la classe d’appartenance appellent le fournisseur configuré, ce qui effectue le travail réel. Par exemple, lorsque la méthode CreateUser de la classe d’appartenance est appelée, la classe d’appartenance ne connaît pas les détails du magasin de l’utilisateur. Il ne sait pas si les utilisateurs sont maintenus dans une base de données ou dans un fichier XML ou dans un autre magasin. La classe Membership examine la configuration de l’application Web pour déterminer quel fournisseur délègue l’appel à, et cette classe de fournisseur est chargée de créer le nouveau compte d’utilisateur dans le magasin d’utilisateurs approprié. Cette interaction est illustrée à la figure 3.

Microsoft expédie deux classes de fournisseur d’appartenance dans le .NET Framework :

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) : implémente l’API d’appartenance dans les serveurs Active Directory et Active Directory en mode application (Adam).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) : implémente l’API d’appartenance dans une base de données SQL Server.

Cette série de didacticiels porte exclusivement sur SqlMembershipProvider.

[![le modèle de fournisseur permet aux différentes implémentations d’être connectées en toute transparence au Framework&lt;/Strong&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Figure 03**: le modèle de fournisseur permet à différentes implémentations d’être connectées en toute transparence à l’infrastructure ([cliquez pour afficher l’image en taille réelle](security-basics-and-asp-net-support-cs/_static/image5.png))

L’avantage du modèle de fournisseur est que les autres implémentations peuvent être développées par Microsoft, par des fournisseurs tiers ou par des développeurs individuels, et être connectées en toute transparence au Framework d’appartenance. Par exemple, Microsoft a publié [un fournisseur d’appartenances pour les bases de données Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Pour plus d’informations sur les fournisseurs d’appartenances, reportez-vous à la [boîte à outils du fournisseur](https://msdn.microsoft.com/asp.net/aa336558.aspx), qui comprend une procédure pas à pas des fournisseurs d’appartenances, des exemples de fournisseurs personnalisés, plus de 100 pages de documentation sur le modèle de fournisseur et le code source complet pour les fournisseurs d’appartenances intégrés (à savoir, ActiveDirectoryMembershipProvider et SqlMembershipProvider).

ASP.NET 2,0 a également introduit l’infrastructure des rôles. À l’instar de l’infrastructure d’appartenance, l’infrastructure de rôles est générée au-dessus du modèle de fournisseur. Son API est exposée via la [classe Roles](https://msdn.microsoft.com/library/system.web.security.roles.aspx) et le .NET Framework est fourni avec trois classes de fournisseur :

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -gère les informations de rôle dans un magasin de stratégies du gestionnaire d’autorisations, tel que Active Directory ou Adam.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) : implémente des rôles dans une base de données SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) : associe les informations de rôle en fonction du groupe Windows du visiteur. Cette méthode est généralement utilisée avec l’authentification Windows.

Cette série de didacticiels porte exclusivement sur SqlRoleProvider.

Étant donné que le modèle de fournisseur comprend une API de transfert unique (classes d’appartenance et de rôles), il est possible de générer des fonctionnalités autour de cette API sans avoir à vous soucier des détails d’implémentation. ceux-ci sont gérés par les fournisseurs sélectionnés par la page. Revel. Cette API unifiée permet à Microsoft et aux fournisseurs tiers de créer des contrôles Web qui s’interfacent avec les infrastructures d’appartenance et de rôles. ASP.NET est fourni avec un certain nombre de [contrôles Web de connexion](https://msdn.microsoft.com/library/ms178329.aspx) pour l’implémentation d’interfaces utilisateur de compte d’utilisateur courantes. Par exemple, le [contrôle de connexion](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) invite l’utilisateur à entrer ses informations d’identification, les valide, puis les enregistre dans via l’authentification par formulaire. Le [contrôle LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) offre des modèles permettant d’afficher des balises différentes pour les utilisateurs anonymes par rapport aux utilisateurs authentifiés, ou un autre balisage basé sur le rôle de l’utilisateur. Et le [contrôle CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) fournit une interface utilisateur pas à pas pour créer un nouveau compte d’utilisateur.

Sous le couvre les différents contrôles de connexion interagissent avec les infrastructures d’appartenance et de rôles. La plupart des contrôles de connexion peuvent être implémentés sans qu’il soit nécessaire d’écrire une seule ligne de code. Nous examinerons ces contrôles plus en détail dans les prochains didacticiels, y compris les techniques permettant d’étendre et de personnaliser leurs fonctionnalités.

## <a name="summary"></a>Récapitulatif

Toutes les applications Web qui prennent en charge les comptes d’utilisateur nécessitent des fonctionnalités similaires, telles que : la possibilité pour les utilisateurs de se connecter et dont l’état de connexion est mémorisé sur les visites de page. une page Web pour les nouveaux visiteurs qui créent un compte ; et la possibilité pour le développeur de pages de spécifier la ressource, les données et les fonctionnalités disponibles pour les utilisateurs ou les rôles. Les tâches d’authentification et d’autorisation des utilisateurs et de gestion des comptes d’utilisateur et des rôles sont remarquablement faciles à accomplir dans les applications ASP.NET grâce à l’authentification par formulaire, à l’autorisation d’URL et aux infrastructures d’appartenance et de rôles.

Au cours des prochains didacticiels, nous examinerons ces aspects en créant une application Web de travail de bout en bout, de manière pas à pas. Dans les deux didacticiels suivants, nous explorerons l’authentification par formulaire en détail. Nous allons voir le flux de travail d’authentification des formulaires en action, étudier en détail le ticket d’authentification par formulaire, discuter des problèmes de sécurité et voir comment configurer le système d’authentification par formulaire-tout lors de la création d’une application Web qui permet aux visiteurs de se connecter et de se déconnecter.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [ASP.NET 2,0 appartenance, rôles, authentification par formulaire et ressources de sécurité](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Instructions de sécurité de ASP.NET 2,0](https://msdn.microsoft.com/library/ms998258.aspx)
- [Authentification ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorisation ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Vue d’ensemble des contrôles de connexion ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Examen de l’appartenance, des rôles et du profil de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Comment faire : sécuriser mon site à l’aide de l’appartenance et des rôles ?](https://asp.net/learn/videos/video-45.aspx) Vidéosurveillance
- [Présentation de l’appartenance](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Centre de développement de la sécurité MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Gestion de la sécurité, de l’appartenance et des rôles de ASP.net Professional 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN : 978-0-7645-9698-8)
- [Boîte à outils du fournisseur](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel a été révisé par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel incluent Alicja maziarz, John SURU et Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Suivant](an-overview-of-forms-authentication-cs.md)
