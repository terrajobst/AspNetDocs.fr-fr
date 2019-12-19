---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
title: Création de comptes d'C#utilisateur () | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons explorer l’utilisation de l’infrastructure d’appartenance (via SqlMembershipProvider) pour créer des comptes d’utilisateur. Nous verrons comment créer de nouveaux États-Unis...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: f175278c-6079-4d91-b9b4-2493ed43d9ec
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 955592320e7d36c7ae3b9c03a361bee2183f1776
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74625477"
---
# <a name="creating-user-accounts-c"></a>Création de comptes d’utilisateurs (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_cs.pdf)

> Dans ce didacticiel, nous allons explorer l’utilisation de l’infrastructure d’appartenance (via SqlMembershipProvider) pour créer des comptes d’utilisateur. Nous verrons comment créer des utilisateurs par programme et par le biais d’ASP. Contrôle CreateUserWizard intégré au réseau.

## <a name="introduction"></a>Présentation

Dans le <a id="_msoanchor_1"> </a> [didacticiel précédent](creating-the-membership-schema-in-sql-server-cs.md) , nous avons installé le schéma des services d’application dans une base de données, qui a ajouté les tables, les vues et les procédures stockées nécessaires à la `SqlMembershipProvider` et `SqlRoleProvider`. Cela a créé l’infrastructure dont nous aurons besoin pour le reste des didacticiels de cette série. Dans ce didacticiel, nous allons explorer l’utilisation de l’infrastructure d’appartenance (via le `SqlMembershipProvider`) pour créer des comptes d’utilisateur. Nous verrons comment créer des utilisateurs par programme et par le biais d’ASP. Contrôle CreateUserWizard intégré au réseau.

En plus d’apprendre à créer des comptes d’utilisateur, nous devons également mettre à jour le site Web de démonstration que nous avons créé dans le *<a id="_msoanchor_2"></a>[didacticiel d’authentification des formulaires](../introduction/an-overview-of-forms-authentication-cs.md)* , puis amélioré dans le didacticiel sur la configuration de l' *<a id="https://www.asp.net/learn/security/tutorial-03-cs.aspx"></a> authentification par formulaire et les rubriques avancées*. Notre application Web de démonstration a une page de connexion qui valide les informations d’identification des utilisateurs par rapport à des paires nom d’utilisateur/mot de passe codées en dur. En outre, `Global.asax` comprend du code qui crée des objets `IPrincipal` et `IIdentity` personnalisés pour les utilisateurs authentifiés. Nous allons mettre à jour la page de connexion pour valider les informations d’identification des utilisateurs par rapport à l’infrastructure d’appartenance et supprimer le principal personnalisé et la logique d’identité.

Commençons !

## <a name="the-forms-authentication-and-membership-checklist"></a>Liste de vérification de l’authentification et de l’appartenance aux formulaires

Avant de commencer à travailler avec l’infrastructure d’appartenance, nous allons prendre un moment pour passer en revue les étapes importantes que nous avons suivies pour atteindre ce point. Lors de l’utilisation de l’infrastructure d’appartenance avec l' `SqlMembershipProvider` dans un scénario d’authentification basé sur les formulaires, les étapes suivantes doivent être effectuées avant d’implémenter la fonctionnalité d’appartenance dans votre application Web :

1. **Activez l’authentification basée sur les formulaires.** Comme nous l’avons vu dans *<a id="_msoanchor_4"></a>[une vue d’ensemble de l’authentification par formulaire](../introduction/an-overview-of-forms-authentication-cs.md)* , l’authentification par formulaire est activée en modifiant `Web.config` et en affectant à l’attribut `mode` de l’élément `<authentication>` la valeur `Forms`. Lorsque l’authentification par formulaire est activée, chaque demande entrante est examinée pour obtenir un *ticket d’authentification par formulaire*, qui, le cas échéant, identifie le demandeur.
2. **Ajoutez le schéma des services d’application à la base de données appropriée.** Lors de l’utilisation de la `SqlMembershipProvider` vous devez installer le schéma des services d’application dans une base de données. En général, ce schéma est ajouté à la base de données qui contient le modèle de données de l’application. Le didacticiel *<a id="_msoanchor_5"></a>[Création du schéma d’appartenance dans SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* a été abordé à l’aide de l’outil `aspnet_regsql.exe` pour y parvenir.
3. **Personnalisez les paramètres de l’application Web pour faire référence à la base de données de l’étape 2.** Le didacticiel *création du schéma d’appartenance dans SQL Server* a montré deux façons de configurer l’application Web afin que le `SqlMembershipProvider` utilise la base de données sélectionnée à l’étape 2 : en modifiant le nom de la chaîne de connexion `LocalSqlServer` ; ou en ajoutant un nouveau fournisseur inscrit à la liste des fournisseurs d’infrastructure d’appartenance et en personnalisant ce nouveau fournisseur pour utiliser la base de données de l’étape 2.

Lors de la création d’une application Web qui utilise la `SqlMembershipProvider` et l’authentification basée sur les formulaires, vous devez effectuer ces trois étapes avant d’utiliser la classe `Membership` ou les contrôles Web de connexion ASP.NET. Étant donné que nous avons déjà effectué ces étapes dans les didacticiels précédents, nous sommes prêts à commencer à utiliser l’infrastructure d’appartenance !

## <a name="step-1-adding-new-aspnet-pages"></a>Étape 1 : Ajout de nouvelles pages ASP.NET

Dans ce didacticiel et les trois suivants, nous examinerons diverses fonctions et fonctionnalités liées à l’appartenance. Nous aurons besoin d’une série de pages ASP.NET pour implémenter les rubriques examinées dans ces didacticiels. Nous allons créer ces pages, puis un fichier de plan de site `(Web.sitemap)`.

Commencez par créer un nouveau dossier dans le projet nommé `Membership`. Ensuite, ajoutez cinq nouvelles pages ASP.NET au dossier `Membership`, en liant chaque page à la page maître `Site.master`. Nommer les pages :

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

À ce stade, le Explorateur de solutions de votre projet doit ressembler à la capture d’écran illustrée à la figure 1.

[![cinq nouvelles pages ont été ajoutées au dossier d’appartenance](creating-user-accounts-cs/_static/image2.png)](creating-user-accounts-cs/_static/image1.png)

**Figure 1**: Cinq nouvelles pages ont été ajoutées au dossier `Membership` ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image3.png))

À ce stade, chaque page doit avoir les deux contrôles de contenu, un pour chaque ContentPlaceHolders : `MainContent` et `LoginContent`de la page maître.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample1.aspx)]

N’oubliez pas que le balisage par défaut de `LoginContent` ContentPlaceHolder affiche un lien permettant d’ouvrir une session ou de se déconnecter du site, selon que l’utilisateur est authentifié ou non. Toutefois, la présence du contrôle de contenu `Content2` remplace le balisage par défaut de la page maître. Comme nous l’avons vu dans *<a id="_msoanchor_6"></a>[ une vue d’ensemble du didacticiel sur l’authentification par formulaire](../introduction/an-overview-of-forms-authentication-cs.md)* , cela s’avère utile dans les pages où vous ne souhaitez pas afficher les options relatives à la connexion dans la colonne de gauche.

Toutefois, pour ces cinq pages, nous souhaitons afficher le balisage par défaut de la page maître pour le `LoginContent` ContentPlaceHolder. Par conséquent, supprimez le balisage déclaratif pour le contrôle de contenu `Content2`. Après cela, chacun des balises de la page de cinq pages ne doit contenir qu’un seul contrôle de contenu.

## <a name="step-2-creating-the-site-map"></a>Étape 2 : Création du plan de site

Tous les sites Web, à l’exception des plus simples, doivent implémenter une forme d’interface utilisateur de navigation. L’interface utilisateur de navigation peut être une simple liste de liens vers les différentes sections du site. Ces liens peuvent également être organisés dans des menus ou des arborescences. En tant que développeurs de pages, la création de l’interface utilisateur de navigation n’est que la moitié de l’histoire. Nous avons également besoin d’un moyen de définir la structure logique du site dans un mode gérable et modifiable. À mesure que de nouvelles pages sont ajoutées ou que des pages existantes ont été supprimées, nous voulons être en mesure de mettre à jour une seule source (plan du site) et de faire en sorte que ces modifications soient reflétées dans l’interface utilisateur de navigation du site.

Ces deux tâches, telles que la définition du plan de site et l’implémentation d’une interface utilisateur de navigation basée sur le plan de site, sont faciles à accomplir grâce à l’infrastructure de plan de site et aux contrôles Web de navigation ajoutés à la version 2,0 de ASP.NET. L’infrastructure de plan de site permet à un développeur de définir un plan de site, puis d’y accéder par le biais d’une API de programmation (la [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Les contrôles Web de navigation intégrés incluent un [contrôle de menu](https://msdn.microsoft.com/library/bz09dy46.aspx), le [contrôle TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)et le [contrôle SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

À l’instar des frameworks d’appartenance et de rôles, le Framework de plan de site est créé au-dessus du [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). La tâche de la classe de fournisseur de plan de site consiste à générer la structure en mémoire utilisée par la classe `SiteMap` à partir d’un magasin de données persistant, tel qu’un fichier XML ou une table de base de données. Le .NET Framework est fourni avec un fournisseur de plan de site par défaut qui lit les données de plan de site à partir d’un fichier XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), et il s’agit du fournisseur que nous allons utiliser dans ce didacticiel. Pour d’autres implémentations de fournisseur de plan de site, reportez-vous à la section lectures supplémentaires à la fin de ce didacticiel.

Le fournisseur de plan de site par défaut attend un fichier XML correctement mis en forme nommé `Web.sitemap` pour qu’il existe dans le répertoire racine. Étant donné que nous utilisons ce fournisseur par défaut, nous devons ajouter un fichier de ce type et définir la structure du plan de site dans le format XML approprié. Pour ajouter le fichier, cliquez avec le bouton droit sur le nom du projet dans Explorateur de solutions et choisissez Ajouter un nouvel élément. Dans la boîte de dialogue, choisissez d’ajouter un fichier de type plan du site nommé `Web.sitemap`.

[![ajouter un fichier nommé Web. sitemap au répertoire racine du projet](creating-user-accounts-cs/_static/image5.png)](creating-user-accounts-cs/_static/image4.png)

**Figure 2**: Ajoutez un fichier nommé `Web.sitemap` au répertoire racine du projet ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image6.png))

Le fichier de plan de site XML définit la structure du site Web en tant que hiérarchie. Cette relation hiérarchique est modélisée dans le fichier XML via le ascendance des éléments de `<siteMapNode>`. Le `Web.sitemap` doit commencer par un nœud parent `<siteMap>` qui a précisément un `<siteMapNode>` enfant. Cet élément de `<siteMapNode>` de niveau supérieur représente la racine de la hiérarchie et peut avoir un nombre arbitraire de nœuds descendants. Chaque élément `<siteMapNode>` doit inclure un attribut `title` et éventuellement inclure des attributs `url` et `description`, entre autres ; chaque attribut `url` non vide doit être unique.

Entrez le code XML suivant dans le fichier `Web.sitemap` :

[!code-xml[Main](creating-user-accounts-cs/samples/sample2.xml)]

Le balisage de plan de site ci-dessus définit la hiérarchie illustrée à la figure 3.

[![le plan de site représente une structure de navigation hiérarchique](creating-user-accounts-cs/_static/image8.png)](creating-user-accounts-cs/_static/image7.png)

**Figure 3**: Le plan de site représente une structure de navigation hiérarchique ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image9.png))

## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Étape 3 : Mise à jour de la page maître pour inclure une interface utilisateur de navigation

ASP.NET comprend un certain nombre de contrôles Web relatifs à la navigation pour la conception d’une interface utilisateur. Celles-ci incluent les contrôles menu, TreeView et SiteMapPath. Les contrôles menu et TreeView affichent la structure de plan de site dans un menu ou une arborescence, respectivement, tandis que SiteMapPath affiche un fil d’Ariane qui montre le nœud actuel visité ainsi que ses ancêtres. Les données de plan de site peuvent être liées à d’autres contrôles Web de données à l’aide du SiteMapDataSource et sont accessibles par programme par le biais de la classe `SiteMap`.

Dans la mesure où une présentation approfondie de l’infrastructure de plan de site et des contrôles de navigation dépasse le cadre de cette série de didacticiels, plutôt que de consacrer du temps à la création de notre propre interface utilisateur de navigation, nous allons emprunter celle utilisée dans la série de didacticiels *[utilisation de données dans ASP.NET 2,0](../../data-access/index.md)* , qui utilise un contrôle Repeater pour afficher une liste à puces en deux points,

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Ajout d’une liste de liens à deux niveaux dans la colonne de gauche

Pour créer cette interface, ajoutez la balise déclarative suivante à la colonne de gauche de la page maître `Site.master` où le texte «TODO : Le menu s’affiche...» réside actuellement.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample3.aspx)]

Le balisage ci-dessus lie un contrôle Repeater nommé `menu` à un SiteMapDataSource, qui retourne la hiérarchie de plan de site définie dans `Web.sitemap`. Dans la mesure où la [propriété`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) du contrôle SiteMapDataSource est définie sur false, elle commence à retourner la hiérarchie du plan de site en commençant par les descendants du nœud « démarrage ». Le Repeater affiche chacun de ces nœuds (actuellement « membership ») dans un élément `<li>`. Un autre répéteur interne affiche ensuite les enfants du nœud actuel dans une liste non triée imbriquée.

La figure 4 illustre la sortie rendue du balisage ci-dessus avec la structure de plan de site créée à l’étape 2. Le Repeater restitue le balisage de liste non triée vanille ; les règles de feuille de style en cascade définies dans `Styles.css` sont responsables de la disposition esthétiquement agréable. Pour obtenir une description plus détaillée du fonctionnement du balisage ci-dessus, reportez-vous au didacticiel sur les [pages maîtres et la navigation](https://asp.net/learn/data-access/tutorial-03-cs.aspx) dans les sites.

[![l’interface utilisateur de navigation est rendue à l’aide de listes non triées imbriquées](creating-user-accounts-cs/_static/image11.png)](creating-user-accounts-cs/_static/image10.png)

**Figure 4**: L’interface utilisateur de navigation est rendue à l’aide de listes imbriquées non triées ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image12.png))

### <a name="adding-breadcrumb-navigation"></a>Ajout d’une navigation de navigation

En plus de la liste de liens dans la colonne de gauche, nous avons également chaque page qui affiche un [fil d’Ariane](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Un fil d’Ariane est un élément d’interface utilisateur de navigation qui affiche rapidement les utilisateurs à leur position actuelle dans la hiérarchie de site. Le contrôle SiteMapPath utilise l’infrastructure de plan de site pour déterminer l’emplacement de la page actuelle dans le plan de site, puis affiche un fil d’Ariane sur la base de ces informations.

Plus précisément, ajoutez un élément `<span>` à l’élément d’en-tête `<div>` de la page maître, puis affectez la valeur « Breadcrumb » à l’attribut `class` de l’élément nouvel `<span>`. (La classe `Styles.css` contient une règle pour une classe « Breadcrumb ».) Ensuite, ajoutez un SiteMapPath à ce nouvel élément `<span>`.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample4.aspx)]

La figure 5 illustre la sortie du SiteMapPath lorsque vous vous rendez `~/Membership/CreatingUserAccounts.aspx`.

[![le Breadcrumb affiche la page actuelle et ses ancêtres dans le plan du site](creating-user-accounts-cs/_static/image14.png)](creating-user-accounts-cs/_static/image13.png)

**Figure 5**: Le Breadcrumb affiche la page actuelle et ses ancêtres dans le plan du site ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image15.png))

## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Étape 4 : Suppression de la logique d’identité et du principal personnalisé

Dans le didacticiel sur la *<a id="_msoanchor_7"></a>[configuration de l’authentification par formulaire et les rubriques avancées](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)* , nous avons vu comment associer des objets principal et Identity personnalisés à l’utilisateur authentifié. Pour ce faire, nous avons créé un gestionnaire d’événements dans `Global.asax` pour l’événement `PostAuthenticateRequest` de l’application, qui se déclenche après que le `FormsAuthenticationModule` a authentifié l’utilisateur. Dans ce gestionnaire d’événements, nous avons remplacé les objets `GenericPrincipal` et `FormsIdentity` ajoutés par le `FormsAuthenticationModule` par les objets `CustomPrincipal` et `CustomIdentity` que nous avons créés dans ce didacticiel.

Bien que les objets principal et Identity personnalisés soient utiles dans certains scénarios, dans la plupart des cas, les objets `GenericPrincipal` et `FormsIdentity` sont suffisants. Par conséquent, je pense qu’il serait utile de revenir au comportement par défaut. Apportez cette modification en supprimant ou en commentant le gestionnaire d’événements `PostAuthenticateRequest` ou en supprimant entièrement le fichier `Global.asax`.

## <a name="step-5-programmatically-creating-a-new-user"></a>Étape 5 : Création d’un nouvel utilisateur par programmation

Pour créer un nouveau compte d’utilisateur par le biais de l’infrastructure d’appartenance, utilisez la [méthode`CreateUser`](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)de la classe `Membership`. Cette méthode a des paramètres d’entrée pour le nom d’utilisateur, le mot de passe et d’autres champs liés à l’utilisateur. Lors de l’appel, il délègue la création du nouveau compte d’utilisateur au fournisseur d’appartenances configuré, puis retourne un [objet`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) représentant le compte d’utilisateur créé juste.

La méthode `CreateUser` a quatre surcharges, chacune acceptant un nombre différent de paramètres d’entrée :

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Ces quatre surcharges diffèrent de la quantité d’informations collectées. La première surcharge, par exemple, nécessite simplement le nom d’utilisateur et le mot de passe du nouveau compte d’utilisateur, tandis que la deuxième nécessite également l’adresse de messagerie de l’utilisateur.

Ces surcharges existent parce que les informations nécessaires à la création d’un nouveau compte d’utilisateur dépendent des paramètres de configuration du fournisseur d’appartenances. Dans le didacticiel *<a id="_msoanchor_8"></a>[Création du schéma d’appartenance dans SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* , nous avons examiné la spécification des paramètres de configuration du fournisseur d’appartenance dans `Web.config`. Le tableau 2 inclut une liste complète des paramètres de configuration.

L’un des paramètres de configuration de ce fournisseur d’appartenances ayant un impact sur les surcharges de `CreateUser` peut être utilisé est le paramètre `requiresQuestionAndAnswer`. Si `requiresQuestionAndAnswer` est défini sur `true` (valeur par défaut), lorsque vous créez un nouveau compte d’utilisateur, nous devons spécifier une question et une réponse de sécurité. Ces informations sont utilisées ultérieurement si l’utilisateur doit réinitialiser ou modifier son mot de passe. Plus précisément, à ce moment-là, la question de sécurité s’affiche et les utilisateurs doivent entrer la bonne réponse pour réinitialiser ou modifier leur mot de passe. Par conséquent, si le `requiresQuestionAndAnswer` a la valeur `true` alors l’appel de l’une des deux premières surcharges `CreateUser` entraîne une exception, car la question de sécurité et la réponse sont manquantes. Étant donné que notre application est actuellement configurée pour exiger une question et une réponse de sécurité, nous devons utiliser l’une des deux surcharges lors de la création par programme de l’utilisateur.

Pour illustrer l’utilisation de la méthode `CreateUser`, nous allons créer une interface utilisateur qui invite l’utilisateur à entrer son nom, son mot de passe, son adresse de messagerie et une réponse à une question de sécurité prédéfinie. Ouvrez la page `CreatingUserAccounts.aspx` dans le dossier `Membership` et ajoutez les contrôles Web suivants au contrôle de contenu :

- Une zone de texte nommée `Username`
- Une zone de texte nommée `Password`, dont la propriété `TextMode` a la valeur `Password`
- Une zone de texte nommée `Email`
- Une étiquette nommée `SecurityQuestion` avec sa propriété `Text` supprimée
- Une zone de texte nommée `SecurityAnswer`
- Un bouton nommé `CreateAccountButton` dont la propriété Text est définie sur « créer le compte d’utilisateur »
- Contrôle Label nommé `CreateAccountResults` avec sa propriété `Text` supprimée

À ce stade, votre écran doit ressembler à la capture d’écran illustrée à la figure 6.

[![ajouter les différents contrôles Web à la page CreatingUserAccounts. aspx](creating-user-accounts-cs/_static/image17.png)](creating-user-accounts-cs/_static/image16.png)

**Figure 6**: Ajouter les différents contrôles Web à la page `CreatingUserAccounts.aspx` ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image18.png))

Les `SecurityQuestion` étiquette et `SecurityAnswer` zone de texte sont destinées à afficher une question de sécurité prédéfinie et à collecter la réponse de l’utilisateur. Notez que la question de sécurité et la réponse sont stockées par utilisateur, par conséquent, il est possible d’autoriser chaque utilisateur à définir sa propre question de sécurité. Toutefois, pour cet exemple, j’ai décidé d’utiliser une question de sécurité universelle, à savoir : « Quelle est votre couleur préférée ? »

Pour implémenter cette question de sécurité prédéfinie, ajoutez une constante à la classe code-behind de la page nommée `passwordQuestion`, en lui assignant la question de sécurité. Ensuite, dans le gestionnaire d’événements `Page_Load`, assignez cette constante à la propriété `Text` de l’étiquette `SecurityQuestion` :

[!code-csharp[Main](creating-user-accounts-cs/samples/sample5.cs)]

Ensuite, créez un gestionnaire d’événements pour l’événement `Click` de l' `CreateAccountButton`et ajoutez le code suivant :

[!code-csharp[Main](creating-user-accounts-cs/samples/sample6.cs)]

Le gestionnaire d’événements `Click` commence par la définition d’une variable nommée `createStatus` de type [`MembershipCreateStatus`](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` est une énumération qui indique l’état de l’opération de `CreateUser`. Par exemple, si le compte d’utilisateur est créé avec succès, l’instance de `MembershipCreateStatus` résultante est définie sur la valeur `Success`; en revanche, si l’opération échoue parce qu’il existe déjà un utilisateur avec le même nom d’utilisateur, elle est définie sur la valeur `DuplicateUserName`. Dans la `CreateUser` surcharge que nous utilisons, nous devons passer une instance `MembershipCreateStatus` dans la méthode en tant que paramètre `out`. Ce paramètre est défini sur la valeur appropriée dans la méthode `CreateUser`, et nous pouvons examiner sa valeur après l’appel de la méthode pour déterminer si le compte d’utilisateur a été créé avec succès.

Après l’appel de `CreateUser`, en passant `createStatus`, une instruction `switch` est utilisée pour générer un message approprié en fonction de la valeur assignée à `createStatus`. Les figures 7 affichent la sortie lorsqu’un nouvel utilisateur a été créé avec succès. Les figures 8 et 9 affichent la sortie lorsque le compte d’utilisateur n’est pas créé. Dans la figure 8, le visiteur a entré un mot de passe de cinq lettres, qui ne répond pas aux exigences de la force de mot de passe dans les paramètres de configuration du fournisseur d’appartenances. Dans la figure 9, le visiteur tente de créer un compte d’utilisateur avec un nom d’utilisateur existant (celui créé à la figure 7).

[![un nouveau compte d’utilisateur a été créé](creating-user-accounts-cs/_static/image20.png)](creating-user-accounts-cs/_static/image19.png)

**Figure 7**: Un nouveau compte d’utilisateur a été créé avec succès ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image21.png))

[![le compte d’utilisateur n’est pas créé, car le mot de passe fourni est trop faible](creating-user-accounts-cs/_static/image23.png)](creating-user-accounts-cs/_static/image22.png)

**Figure 8**: Le compte d’utilisateur n’a pas été créé, car le mot de passe fourni est trop faible ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image24.png))

[![le compte d’utilisateur n’est pas créé, car le nom d’utilisateur est déjà utilisé](creating-user-accounts-cs/_static/image26.png)](creating-user-accounts-cs/_static/image25.png)

**Figure 9**: Le compte d’utilisateur n’est pas créé, car le nom d’utilisateur est déjà utilisé ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image27.png))

> [!NOTE]
> Vous vous demandez peut-être comment déterminer la réussite ou l’échec lors de l’utilisation de l’une des deux premières surcharges de méthode `CreateUser`, qui ne possèdent pas de paramètre de type `MembershipCreateStatus`. Ces deux premières surcharges lèvent une [exception`MembershipCreateUserException`](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) en cas d’échec, qui comprend une [propriété`StatusCode`](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) de type `MembershipCreateStatus`.

Après avoir créé quelques comptes d’utilisateur, vérifiez que les comptes ont été créés en répertoriant le contenu des tables `aspnet_Users` et `aspnet_Membership` dans la base de données `SecurityTutorials.mdf`. Comme le montre la figure 10, j’ai ajouté deux utilisateurs via la page `CreatingUserAccounts.aspx` : Tito et Bruce.

[![deux utilisateurs se trouvent dans le magasin de l’utilisateur d’appartenance : Tito et Bruce](creating-user-accounts-cs/_static/image29.png)](creating-user-accounts-cs/_static/image28.png)

**Figure 10**: Il y a deux utilisateurs dans le magasin d’utilisateurs d’appartenance : Tito et Bruce ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image30.png))

Alors que le magasin de l’utilisateur d’appartenance comprend désormais les informations de compte Bruce et Tito, nous n’avons pas encore implémenté les fonctionnalités permettant à Bruce ou Tito de se connecter au site. Actuellement, `Login.aspx` valide les informations d’identification de l’utilisateur par rapport à un ensemble codé en dur de paires nom d’utilisateur/mot de passe : il ne valide *pas* les informations d’identification fournies par rapport à l’infrastructure d’appartenance. Pour l’instant, l’affichage des nouveaux comptes d’utilisateur dans les tables `aspnet_Users` et `aspnet_Membership` doit suffire. Dans le didacticiel suivant, *<a id="_msoanchor_9"></a>[validation des informations d’identification de l’utilisateur par rapport au magasinde l’utilisateur d’appartenance](validating-user-credentials-against-the-membership-user-store-cs.md)* , nous allons mettre à jour la page de connexion pour effectuer une validation par rapport au magasin d’appartenance.

> [!NOTE]
> Si vous ne voyez aucun utilisateur dans votre base de données `SecurityTutorials.mdf`, cela peut être dû au fait que votre application Web utilise le fournisseur d’appartenances par défaut, `AspNetSqlMembershipProvider`, qui utilise la base de données `ASPNETDB.mdf` comme magasin de l’utilisateur. Pour déterminer s’il s’agit du problème, cliquez sur le bouton Actualiser dans la Explorateur de solutions. Si une base de données nommée `ASPNETDB.mdf` a été ajoutée au dossier `App_Data`, c’est le problème. Revenez à l'étape 4 du didacticiel *<a id="_msoanchor_10"></a>[Création du schéma d’appartenance dans SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* pour obtenir des instructions sur la façon de configurer correctement le fournisseur d’appartenances.

Dans la plupart des scénarios de création de compte d’utilisateur, le visiteur reçoit une interface permettant d’entrer son nom d’utilisateur, son mot de passe, son adresse de messagerie et d’autres informations essentielles, à partir duquel un nouveau compte est créé. Au cours de cette étape, nous avons abordé la création d’une interface à la main, puis vu comment utiliser la méthode `Membership.CreateUser` pour ajouter par programmation le nouveau compte d’utilisateur en fonction des entrées de l’utilisateur. Notre code, cependant, vient de créer le nouveau compte d’utilisateur. Il n’a pas effectué d’actions de suivi, telles que la connexion de l’utilisateur au site sous le compte d’utilisateur juste créé, ou l’envoi d’un e-mail de confirmation à l’utilisateur. Ces étapes supplémentaires nécessiteraient un code supplémentaire dans le gestionnaire d’événements `Click` du bouton.

ASP.NET est fourni avec le contrôle CreateUserWizard, qui est conçu pour gérer le processus de création de compte d’utilisateur, du rendu d’une interface utilisateur pour la création de nouveaux comptes d’utilisateur, à la création du compte dans l’infrastructure d’appartenance et à l’exécution de l’opération de publication de compte. des tâches de création, telles que l’envoi d’un e-mail de confirmation et l’enregistrement de l’utilisateur créé dans le site. L’utilisation du contrôle CreateUserWizard est aussi simple que le fait de faire glisser le contrôle CreateUserWizard de la boîte à outils vers une page, puis de définir certaines propriétés. Dans la plupart des cas, vous n’aurez pas besoin d’écrire une seule ligne de code. Nous allons explorer ce contrôle génial en détail à l’étape 6.

Si de nouveaux comptes d’utilisateur sont créés uniquement par le biais d’une page Web de création de compte classique, il est peu probable que vous deviez écrire du code qui utilise la méthode `CreateUser`, car le contrôle CreateUserWizard répondra probablement à vos besoins. Toutefois, la méthode `CreateUser` est pratique dans les scénarios où vous avez besoin d’une expérience utilisateur de création de compte hautement personnalisée ou lorsque vous devez créer des comptes d’utilisateur par programmation par le biais d’une autre interface. Par exemple, vous pouvez avoir une page qui permet à un utilisateur de télécharger un fichier XML qui contient des informations utilisateur à partir d’une autre application. La page peut analyser le contenu du fichier XML téléchargé et créer un nouveau compte pour chaque utilisateur représenté dans le code XML en appelant la méthode `CreateUser`.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Étape 6 : Création d’un utilisateur avec le contrôle CreateUserWizard

ASP.NET est fourni avec un certain nombre de contrôles Web de connexion. Ces contrôles aident dans de nombreux scénarios courants liés au compte d’utilisateur et à la connexion. Le [contrôle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) est un contrôle qui est conçu pour présenter une interface utilisateur qui permet d’ajouter un nouveau compte d’utilisateur à l’infrastructure d’appartenance.

Comme la plupart des autres contrôles Web liés à la connexion, le contrôle CreateUserWizard peut être utilisé sans écrire une seule ligne de code. Il fournit intuitivement une interface utilisateur basée sur les paramètres de configuration du fournisseur d’appartenance et appelle en interne la méthode `CreateUser` de la classe `Membership` une fois que l’utilisateur a entré les informations nécessaires et clique sur le bouton « créer un utilisateur ». Le contrôle CreateUserWizard est extrêmement personnalisable. Il existe un hôte d’événements qui se déclenchent au cours des différentes étapes du processus de création du compte. Nous pouvons créer des gestionnaires d’événements, si nécessaire, pour injecter une logique personnalisée dans le flux de travail de création de compte. En outre, l’apparence de CreateUserWizard est très flexible. Il existe un certain nombre de propriétés qui définissent l’apparence de l’interface par défaut ; Si nécessaire, le contrôle peut être converti en modèle ou des « étapes » supplémentaires peuvent être ajoutées.

Commençons par examiner l’utilisation de l’interface et du comportement par défaut du contrôle CreateUserWizard. Nous allons ensuite étudier comment personnaliser l’apparence à l’aide des propriétés et des événements du contrôle.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Examen de l’interface et du comportement par défaut de CreateUserWizard

Revenez à la page `CreatingUserAccounts.aspx` dans le dossier `Membership`, basculez en mode création ou fractionnement, puis ajoutez un contrôle CreateUserWizard en haut de la page. Le contrôle CreateUserWizard est classé dans la section des contrôles de connexion de la boîte à outils. Après avoir ajouté le contrôle, définissez sa propriété `ID` sur `RegisterUser`. Comme le montre la capture d’écran de la figure 11, le contrôle CreateUserWizard affiche une interface avec des zones de texte pour le nom d’utilisateur, le mot de passe, l’adresse de messagerie et la question de sécurité du nouvel utilisateur.

[![le contrôle CreateUserWizard restitue une interface utilisateur de création générique](creating-user-accounts-cs/_static/image32.png)](creating-user-accounts-cs/_static/image31.png)

**Figure 11**: Le contrôle CreateUserWizard restitue une interface utilisateur Create générique ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image33.png))

Prenons un moment pour comparer l’interface utilisateur par défaut générée par le contrôle CreateUserWizard avec l’interface que nous avons créée à l’étape 5. Pour commencer, le contrôle CreateUserWizard permet au visiteur de spécifier à la fois la question et la réponse de sécurité, tandis que notre interface créée manuellement a utilisé une question de sécurité prédéfinie. L’interface du contrôle CreateUserWizard comprend également des contrôles de validation, tandis que nous n’avions pas encore implémenté la validation sur les champs de formulaire de l’interface. Et l’interface de contrôle CreateUserWizard comprend une zone de texte « confirmer le mot de passe » avec un CompareValidator pour garantir que le texte entré dans les zones de texte « mot de passe » et « comparer le mot de passe » est égal à.

Ce qui est intéressant, c’est que le contrôle CreateUserWizard consulte les paramètres de configuration du fournisseur d’appartenance lors du rendu de son interface utilisateur. Par exemple, les zones de texte question de sécurité et réponse ne s’affichent que si `requiresQuestionAndAnswer` a la valeur true. De même, CreateUserWizard ajoute automatiquement un contrôle RegularExpressionValidator pour s’assurer que les exigences de la force de mot de passe sont respectées et définit ses propriétés `ErrorMessage` et `ValidationExpression` en fonction des paramètres de configuration `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`et `passwordStrengthRegularExpression`.

Le contrôle CreateUserWizard, comme son nom l’indique, est dérivé du [contrôle](https://msdn.microsoft.com/library/s2etd1ek.aspx)de l’Assistant. Les contrôles de l’Assistant sont conçus pour fournir une interface permettant d’effectuer des tâches en plusieurs étapes. Un contrôle Wizard peut avoir un nombre arbitraire de `WizardSteps`, chacun d’eux étant un modèle qui définit les contrôles HTML et Web pour cette étape. Le contrôle Wizard affiche initialement la première `WizardStep`, ainsi que des contrôles de navigation qui permettent à l’utilisateur de passer d’une étape à l’autre, ou de revenir aux étapes précédentes.

Comme le montre le balisage déclaratif de la figure 11, l’interface par défaut du contrôle CreateUserWizard comprend deux `WizardSteps:`

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) : effectue le rendu de l’interface pour collecter des informations pour la création du nouveau compte d’utilisateur. Il s’agit de l’étape illustrée dans la figure 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) : affiche un message indiquant que le compte a été créé avec succès.

L’apparence et le comportement de CreateUserWizard peuvent être modifiés en convertissant l’une de ces étapes en modèles, ou en ajoutant votre propre `WizardSteps`. Nous allons examiner l’ajout d’un `WizardStep` à l’interface d’inscription dans le didacticiel *sur le stockage des informations utilisateur supplémentaires* .

Voyons le contrôle CreateUserWizard en action. Visitez la page `CreatingUserAccounts.aspx` via un navigateur. Commencez par entrer des valeurs non valides dans l’interface de CreateUserWizard. Essayez d’entrer un mot de passe qui n’est pas conforme aux exigences de la force de mot de passe, ou laissez la zone de texte « nom d’utilisateur » vide. CreateUserWizard affiche un message d’erreur approprié. La figure 12 illustre la sortie lors d’une tentative de création d’un utilisateur avec un mot de passe fort insuffisant.

[![que CreateUserWizard injecte automatiquement les contrôles de validation](creating-user-accounts-cs/_static/image35.png)](creating-user-accounts-cs/_static/image34.png)

**Figure 12**: CreateUserWizard injecte automatiquement les contrôles de validation ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image36.png))

Ensuite, entrez les valeurs appropriées dans CreateUserWizard, puis cliquez sur le bouton « créer un utilisateur ». En supposant que les champs requis ont été entrés et que la force du mot de passe soit suffisante, CreateUserWizard crée un nouveau compte d’utilisateur via l’infrastructure d’appartenance, puis affiche l’interface du `CompleteWizardStep`(voir figure 13). En arrière-plan, CreateUserWizard appelle la méthode `Membership.CreateUser`, comme nous l’avons fait à l’étape 5.

[![un nouveau compte d’utilisateur a été créé avec succès](creating-user-accounts-cs/_static/image38.png)](creating-user-accounts-cs/_static/image37.png)

**Figure 13**: Un nouveau compte d’utilisateur a été créé avec succès ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image39.png))

> [!NOTE]
> Comme le montre la figure 13, l’interface de `CompleteWizardStep`comprend un bouton continuer. Toutefois, à ce stade, il suffit de cliquer dessus pour effectuer une publication, ce qui laisse le visiteur sur la même page. Dans la section « personnalisation de l’apparence et du comportement de CreateUserWizard par le biais de ses propriétés », nous allons voir comment vous pouvez faire en sorte que ce bouton envoie le visiteur à `Default.aspx` (ou à une autre page).

Après avoir créé un nouveau compte d’utilisateur, revenez à Visual Studio et examinez les `aspnet_Users` et `aspnet_Membership` tables comme nous l’avons fait à la figure 10 pour vérifier que le compte a bien été créé.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Personnalisation du comportement et de l’apparence de CreateUserWizard par le biais de ses propriétés

L’CreateUserWizard peut être personnalisé de plusieurs façons, via des propriétés, des `WizardSteps`et des gestionnaires d’événements. Dans cette section, nous allons voir comment personnaliser l’apparence du contrôle par le biais de ses propriétés. la section suivante examine l’extension du comportement du contrôle par le biais de gestionnaires d’événements.

Presque tout le texte affiché dans l’interface utilisateur par défaut du contrôle CreateUserWizard peut être personnalisé par le biais de sa multitude de propriétés. Par exemple, les étiquettes « nom d’utilisateur », « mot de passe », « confirmer le mot de passe », « adresse de messagerie », « question de sécurité » et « réponse de sécurité » affichées à gauche des zones de texte peuvent être personnalisées respectivement par les propriétés [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [`ConfirmPasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [`EmailLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [`QuestionLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)et [`AnswerLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) . De même, il existe des propriétés permettant de spécifier le texte des boutons « CREATE USER » et « continue » dans le `CreateUserWizardStep` et `CompleteWizardStep`, ainsi que si ces boutons sont rendus sous forme de boutons, LinkButtons ou ImageButtons.

Les couleurs, les bordures, les polices et d’autres éléments visuels peuvent être configurés via un hôte de propriétés de style. Le contrôle CreateUserWizard lui-même possède les propriétés de style de contrôle Web communes (`BackColor`, `BorderStyle`, `CssClass`, `Font`, etc.) et il existe un certain nombre de propriétés de style pour définir l’apparence des sections particulières de l’interface de CreateUserWizard. La [propriété`TextBoxStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), par exemple, définit les styles des zones de texte dans le `CreateUserWizardStep`, tandis que la [propriété`TitleTextStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) définit le style du titre (« s’inscrire pour votre nouveau compte »).

Outre les propriétés relatives à l’apparence, il existe un certain nombre de propriétés qui affectent le comportement du contrôle CreateUserWizard. La [propriété`DisplayCancelButton`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), si elle est définie sur true, affiche un bouton Annuler en regard du bouton « Create User » (la valeur par défaut est false). Si vous affichez le bouton Annuler, veillez à définir également la [propriété`CancelDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), qui spécifie la page à laquelle l’utilisateur est envoyé après avoir cliqué sur Annuler. Comme indiqué dans la section précédente, le bouton continuer de l’interface de `CompleteWizardStep`entraîne une publication (postback), mais laisse le visiteur sur la même page. Pour envoyer le visiteur à une autre page après avoir cliqué sur le bouton continuer, spécifiez simplement l’URL dans la [propriété`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Nous allons mettre à jour le contrôle `RegisterUser` CreateUserWizard pour afficher un bouton Annuler et pour envoyer le visiteur à `Default.aspx` lorsque l’utilisateur clique sur les boutons annuler ou continuer. Pour ce faire, affectez la valeur true à la propriété `DisplayCancelButton` et les propriétés `CancelDestinationPageUrl` et `ContinueDestinationPageUrl` à « ~/default.aspx ». La figure 14 illustre la mise à jour de CreateUserWizard quand vous l’affichez dans un navigateur.

[![CreateUserWizardStep contient un bouton Annuler](creating-user-accounts-cs/_static/image41.png)](creating-user-accounts-cs/_static/image40.png)

**Figure 14**: Le `CreateUserWizardStep` comprend un bouton Annuler ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image42.png))

Lorsqu’un visiteur entre un nom d’utilisateur, un mot de passe, une adresse de messagerie et une question de sécurité et répond, puis clique sur « créer un utilisateur », un nouveau compte d’utilisateur est créé et le visiteur est connecté en tant que l’utilisateur nouvellement créé. En supposant que la personne visitant la page crée un nouveau compte pour lui-même, il s’agit probablement du comportement souhaité. Toutefois, vous souhaiterez peut-être autoriser les administrateurs à ajouter de nouveaux comptes d’utilisateur. Dans ce cas, le compte d’utilisateur est créé, mais l’administrateur reste connecté en tant qu’administrateur (et non en tant que compte nouvellement créé). Ce comportement peut être modifié à l’aide de la [propriété booléenne`LoginCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Les comptes d’utilisateur dans l’infrastructure d’appartenance contiennent un indicateur approuvé ; les utilisateurs qui ne sont pas approuvés ne sont pas en mesure de se connecter au site. Par défaut, un compte nouvellement créé est marqué comme approuvé, ce qui permet à l’utilisateur de se connecter immédiatement au site. Toutefois, il est possible que de nouveaux comptes d’utilisateur soient marqués comme non approuvés. Peut-être souhaitez-vous qu’un administrateur approuve manuellement les nouveaux utilisateurs avant qu’ils ne puissent se connecter ; ou vous souhaitez peut-être vérifier que l’adresse e-mail entrée lors de l’inscription est valide avant d’autoriser un utilisateur à se connecter. Quel que soit le cas, vous pouvez faire en sorte que le compte d’utilisateur nouvellement créé soit marqué comme non approuvé en affectant à la [propriété`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) du contrôle CreateUserWizard la valeur true (la valeur par défaut est false).

D’autres propriétés liées au comportement de note incluent `AutoGeneratePassword` et `MailDefinition`. Si la [propriété`AutoGeneratePassword`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) est définie sur true, le `CreateUserWizardStep` n’affiche pas les zones de texte « mot de passe » et « confirmer le mot de passe ». au lieu de cela, le mot de passe de l’utilisateur nouvellement créé est automatiquement généré à l’aide de la [méthode`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)de la classe `Membership`. La méthode `GeneratePassword` construit un mot de passe d’une longueur spécifiée et avec un nombre suffisant de caractères non alphanumériques pour satisfaire les exigences de force de mot de passe configurées.

La [propriété`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) est utile si vous souhaitez envoyer un e-mail à l’adresse de messagerie spécifiée pendant le processus de création du compte. La propriété `MailDefinition` comprend une série de sous-propriétés permettant de définir les informations relatives au message électronique construit. Ces sous-propriétés incluent des options telles que `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`et `BodyFileName`. La [propriété`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) pointe vers un fichier texte ou HTML qui contient le corps du message électronique. Le corps prend en charge deux espaces réservés prédéfinis : `<%UserName%>` et `<%Password%>`. Ces espaces réservés, s’ils sont présents dans le fichier `BodyFileName`, seront remplacés par le nom et le mot de passe de l’utilisateur que vous venez de créer.

> [!NOTE]
> La propriété `MailDefinition` du contrôle `CreateUserWizard` spécifie simplement des détails sur le message électronique envoyé lorsqu’un nouveau compte est créé. Il n’inclut pas de détails sur la façon dont le message électronique est réellement envoyé (c’est-à-dire, si un répertoire de dépôt de messagerie ou de serveur SMTP est utilisé, toutes les informations d’authentification, etc.). Ces détails de bas niveau doivent être définis dans la section `<system.net>` de `Web.config`. Pour plus d’informations sur ces paramètres de configuration et sur l’envoi de courrier électronique à partir de ASP.NET 2,0 en général, reportez-vous aux [questions fréquentes sur SystemNetMail.com](http://www.systemnetmail.com/) et mon article, [envoi de courrier électronique dans ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Extension du comportement de CreateUserWizard à l’aide de gestionnaires d’événements

Le contrôle CreateUserWizard déclenche un certain nombre d’événements pendant son flux de travail. Par exemple, une fois qu’un visiteur entre son nom d’utilisateur, son mot de passe et d’autres informations pertinentes et clique sur le bouton « créer un utilisateur », le contrôle CreateUserWizard déclenche son [événement`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Si un problème survient au cours du processus de création, l' [événement`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) est déclenché ; Toutefois, si l’utilisateur est correctement créé, l' [événement`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) est déclenché. Des événements de contrôle CreateUserWizard supplémentaires sont déclenchés, mais ce sont les trois les plus allemands.

Dans certains scénarios, nous pouvons souhaiter exploiter le workflow CreateUserWizard, que nous pouvons faire en créant un gestionnaire d’événements pour l’événement approprié. Pour illustrer cela, nous allons améliorer le contrôle `RegisterUser` CreateUserWizard pour inclure une validation personnalisée sur le nom d’utilisateur et le mot de passe. En particulier, nous allons améliorer CreateUserWizard afin que les noms d’utilisateur ne puissent pas contenir d’espaces de début ou de fin, et le nom d’utilisateur ne peut pas apparaître n’importe où dans le mot de passe. En bref, nous voulons empêcher une personne de créer un nom d’utilisateur tel que « Scott », ou d’avoir une combinaison nom d’utilisateur/mot de passe comme « Scott » et « Scott. 1234 ».

Pour ce faire, nous allons créer un gestionnaire d’événements pour l’événement `CreatingUser` pour effectuer nos contrôles de validation supplémentaires. Si les données fournies ne sont pas valides, nous devons annuler le processus de création. Nous devons également ajouter un contrôle Web Label à la page pour afficher un message expliquant que le nom d’utilisateur ou le mot de passe n’est pas valide. Commencez par ajouter un contrôle Label sous le contrôle CreateUserWizard, en affectant à sa propriété `ID` la valeur `InvalidUserNameOrPasswordMessage` et à sa propriété `ForeColor` la valeur `Red`. Effacez sa propriété `Text` et affectez à ses propriétés `EnableViewState` et `Visible` la valeur false.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample7.aspx)]

Ensuite, créez un gestionnaire d’événements pour l’événement `CreatingUser` du contrôle CreateUserWizard. Pour créer un gestionnaire d’événements, sélectionnez le contrôle dans le concepteur, puis accédez au Fenêtre Propriétés. À partir de là, cliquez sur l’icône représentant un éclair, puis double-cliquez sur l’événement approprié pour créer le gestionnaire d’événements.

Ajoutez le code suivant au gestionnaire d'événements `CreatingUser` :

[!code-csharp[Main](creating-user-accounts-cs/samples/sample8.cs)]

Notez que le nom d’utilisateur et le mot de passe entrés dans le contrôle CreateUserWizard sont disponibles par le biais de ses propriétés [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) et [`Password`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx), respectivement. Nous utilisons ces propriétés dans le gestionnaire d’événements ci-dessus pour déterminer si le nom d’utilisateur fourni contient des espaces de début ou de fin et si le nom d’utilisateur se trouve dans le mot de passe. Si l’une de ces conditions est remplie, un message d’erreur s’affiche dans l’étiquette `InvalidUserNameOrPasswordMessage` et la propriété `e.Cancel` du gestionnaire d’événements est définie sur `true`. Si `e.Cancel` est défini sur `true`, le flux de travail de la gestion des comptes de compte CreateUserWizard réduit en fait le processus de création du compte d’utilisateur.

La figure 15 illustre une capture d’écran de `CreatingUserAccounts.aspx` lorsque l’utilisateur entre un nom d’utilisateur avec des espaces de début.

[![les noms d’utilisateur avec des espaces de début ou de fin ne sont pas autorisés](creating-user-accounts-cs/_static/image44.png)](creating-user-accounts-cs/_static/image43.png)

**Figure 15**: Les noms d’utilisateur avec des espaces de début ou de fin ne sont pas autorisés ([cliquez pour afficher l’image en taille réelle](creating-user-accounts-cs/_static/image45.png))

> [!NOTE]
> Nous verrons un exemple d’utilisation de l’événement du contrôle `CreatedUser` CreateUserWizard dans le didacticiel *<a id="_msoanchor_11"></a>[stockage d’informations supplémentaires sur l’utilisateur](storing-additional-user-information-cs.md)* .

## <a name="summary"></a>Récapitulatif

La méthode `CreateUser` de la classe `Membership` crée un nouveau compte d’utilisateur dans l’infrastructure d’appartenance. Pour ce faire, il délègue l’appel au fournisseur d’appartenances configuré. Dans le cas de la `SqlMembershipProvider`, la méthode `CreateUser` ajoute un enregistrement aux tables de base de données `aspnet_Users` et `aspnet_Membership`.

Alors que de nouveaux comptes d’utilisateur peuvent être créés par programme (comme nous l’avons vu à l’étape 5), une approche plus rapide et plus facile consiste à utiliser le contrôle CreateUserWizard. Ce contrôle restitue une interface utilisateur à plusieurs étapes pour la collecte d’informations utilisateur et la création d’un nouvel utilisateur dans l’infrastructure d’appartenance. En coulisses, ce contrôle utilise la même méthode de `Membership.CreateUser` que celle examinée à l’étape 5, mais le contrôle crée l’interface utilisateur, les contrôles de validation et répond aux erreurs de création de compte d’utilisateur sans avoir à écrire de code.

À ce stade, nous avons la fonctionnalité en place pour créer des comptes d’utilisateur. Toutefois, la page de connexion est toujours validée par rapport aux informations d’identification codées en dur que nous avons spécifiées dans le deuxième didacticiel. Dans le <a id="_msoanchor_12"> </a> [didacticiel suivant](validating-user-credentials-against-the-membership-user-store-cs.md) , nous allons mettre à jour `Login.aspx` pour valider les informations d’identification fournies par l’utilisateur par rapport à l’infrastructure d’appartenance.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Documentation technique `CreateUser`](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Vue d’ensemble du contrôle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Création d’un fournisseur de plan de site basé sur un système de fichiers](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Création d’une interface utilisateur pas à pas avec le contrôle de l’Assistant ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Examen de la navigation de site de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Pages maîtres et navigation dans les sites](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Le fournisseur de plan de site SQL que vous attendiez](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à...

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](creating-the-membership-schema-in-sql-server-cs.md)
> [Suivant](validating-user-credentials-against-the-membership-user-store-cs.md)
