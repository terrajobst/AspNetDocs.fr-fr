---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: Création d’une interface pour sélectionner un compte d’utilisateur àC#partir de nombreux () | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons créer une interface utilisateur avec une grille paginée et filtrable. En particulier, notre interface utilisateur se compose d’une série de LinkButtons pour...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 8057cfbcd33c74376076363bc27940cebd522c08
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642533"
---
# <a name="building-an-interface-to-select-one-user-account-from-many-c"></a>Création d’une interface pour sélectionner un compte d’utilisateur parmi de nombreux comptes (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> Dans ce didacticiel, nous allons créer une interface utilisateur avec une grille paginée et filtrable. En particulier, notre interface utilisateur se compose d’une série de LinkButtons pour filtrer les résultats en fonction de la lettre de départ du nom d’utilisateur, et d’un contrôle GridView pour afficher les utilisateurs correspondants. Nous allons commencer par répertorier tous les comptes d’utilisateur dans un GridView. Ensuite, à l’étape 3, nous ajouterons le filtre LinkButtons. L’étape 4 examine la pagination des résultats filtrés. L’interface construite dans les étapes 2 à 4 sera utilisée dans les didacticiels suivants pour effectuer des tâches d’administration pour un compte d’utilisateur particulier.

## <a name="introduction"></a>Introduction

Dans le <a id="_msoanchor_1"> </a>didacticiel [*attribution de rôles aux utilisateurs*](../roles/assigning-roles-to-users-cs.md) , nous avons créé une interface rudimentaire permettant à un administrateur de sélectionner un utilisateur et de gérer ses rôles. Plus précisément, l’interface a présenté à l’administrateur une liste déroulante de tous les utilisateurs. Une telle interface est appropriée lorsqu’il y a une douzaine de comptes d’utilisateur, mais qu’elle est difficile pour les sites avec des centaines ou des milliers de comptes. Une grille paginée et filtrable est une interface utilisateur plus appropriée pour les sites Web avec des bases d’utilisateurs volumineuses.

Dans ce didacticiel, nous allons créer une interface utilisateur de ce type. En particulier, notre interface utilisateur se compose d’une série de LinkButtons pour filtrer les résultats en fonction de la lettre de départ du nom d’utilisateur, et d’un contrôle GridView pour afficher les utilisateurs correspondants. Nous allons commencer par répertorier tous les comptes d’utilisateur dans un GridView. Ensuite, à l’étape 3, nous ajouterons le filtre LinkButtons. L’étape 4 examine la pagination des résultats filtrés. L’interface construite dans les étapes 2 à 4 sera utilisée dans les didacticiels suivants pour effectuer des tâches d’administration pour un compte d’utilisateur particulier.

C’est parti !

## <a name="step-1-adding-new-aspnet-pages"></a>Étape 1 : ajout de nouvelles pages ASP.NET

Dans ce didacticiel et les deux prochaines, nous examinerons diverses fonctions et fonctionnalités liées à l’administration. Nous aurons besoin d’une série de pages ASP.NET pour implémenter les rubriques examinées dans ces didacticiels. Nous allons créer ces pages et mettre à jour le plan du site.

Commencez par créer un nouveau dossier dans le projet nommé `Administration`. Ensuite, ajoutez deux nouvelles pages ASP.NET au dossier, en liant chaque page à la page maître `Site.master`. Nommer les pages :

- `ManageUsers.aspx`
- `UserInformation.aspx`

Ajoutez également deux pages dans le répertoire racine du site Web : `ChangePassword.aspx` et `RecoverPassword.aspx`.

À ce stade, ces quatre pages disposent de deux contrôles de contenu, un pour chaque ContentPlaceHolders : `MainContent` et `LoginContent`de la page maître.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

Nous souhaitons afficher le balisage par défaut de la page maître pour le `LoginContent` ContentPlaceHolder pour ces pages. Par conséquent, supprimez le balisage déclaratif pour le contrôle de contenu `Content2`. Après cela, le balisage des pages ne doit contenir qu’un seul contrôle de contenu.

Les pages ASP.NET dans le dossier `Administration` sont destinées uniquement aux utilisateurs administratifs. Nous avons ajouté un rôle administrateurs au système dans le <a id="_msoanchor_2"> </a>didacticiel [*création et gestion des rôles*](../roles/creating-and-managing-roles-cs.md) . Limitez l’accès à ces deux pages à ce rôle. Pour ce faire, ajoutez un fichier `Web.config` dans le dossier `Administration` et configurez son élément `<authorization>` pour admettre les utilisateurs du rôle administrateurs et pour refuser tous les autres.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

À ce stade, le Explorateur de solutions de votre projet doit ressembler à la capture d’écran illustrée à la figure 1.

[![quatre nouvelles pages et un fichier Web. config ont été ajoutés au site Web](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**Figure 1**: quatre nouvelles pages et un fichier de `Web.config` ont été ajoutés au site Web ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))

Enfin, mettez à jour le plan de site (`Web.sitemap`) pour inclure une entrée sur la page de `ManageUsers.aspx`. Ajoutez le code XML suivant après le `<siteMapNode>` que nous avons ajouté pour les didacticiels de rôles.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

Une fois le plan de site mis à jour, accédez au site via un navigateur. Comme le montre la figure 2, la navigation à gauche comprend maintenant des éléments pour les didacticiels d’administration.

[![le plan du site comprend un nœud intitulé Administration des utilisateurs](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**Figure 2**: le plan de site comprend un nœud intitulé Administration des utilisateurs ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))

## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Étape 2 : répertorier tous les comptes d’utilisateur dans un GridView

Notre objectif final pour ce didacticiel est de créer une grille paginée, filtrable, par l’intermédiaire de laquelle un administrateur peut sélectionner un compte d’utilisateur à gérer. Commençons par répertorier *tous* les utilisateurs dans un GridView. Une fois cette opération terminée, nous ajouterons les interfaces et les fonctionnalités de filtrage et de pagination.

Ouvrez la page `ManageUsers.aspx` dans le dossier `Administration` et ajoutez un GridView, en affectant à sa `ID` la valeur `UserAccounts`. Dans un moment, nous allons écrire du code pour lier l’ensemble des comptes d’utilisateur au GridView à l’aide de la méthode `GetAllUsers` de la classe `Membership`. Comme indiqué dans les didacticiels précédents, la méthode GetAllUsers retourne un objet `MembershipUserCollection`, qui est une collection d’objets `MembershipUser`. Chaque `MembershipUser` de la collection comprend des propriétés comme `UserName`, `Email`, `IsApproved`, etc.

Pour afficher les informations de compte d’utilisateur souhaitées dans le GridView, affectez la valeur false à la propriété `AutoGenerateColumns` de GridView et ajoutez BoundFields pour les propriétés `UserName`, `Email`et `Comment` et CheckBoxFields pour les propriétés `IsApproved`, `IsLockedOut`et `IsOnline`. Cette configuration peut être appliquée via le balisage déclaratif du contrôle ou via la boîte de dialogue champs. La figure 3 illustre une capture d’écran de la boîte de dialogue champs après la désactivation de la case à cocher générer automatiquement les champs et l’ajout et la configuration des BoundFields et des CheckBoxFields.

[![ajouter trois BoundFields et trois CheckBoxFields au GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**Figure 3**: ajouter trois BoundFields et trois CheckBoxFields au contrôle GridView ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))

Après avoir configuré votre GridView, assurez-vous que son balisage déclaratif ressemble à ce qui suit :

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

Ensuite, nous devons écrire du code qui lie les comptes d’utilisateur au GridView. Créez une méthode nommée `BindUserAccounts` pour effectuer cette tâche, puis appelez-la à partir du gestionnaire d’événements `Page_Load` sur la première page visitée.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

Prenez un moment pour tester la page via un navigateur. Comme le montre la figure 4, le `UserAccounts` GridView répertorie le nom d’utilisateur, l’adresse de messagerie et d’autres informations de compte pertinentes pour tous les utilisateurs du système.

[![les comptes d’utilisateur sont répertoriés dans le contrôle GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**Figure 4**: les comptes d’utilisateur sont répertoriés dans le contrôle GridView ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))

## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Étape 3 : filtrage des résultats selon la première lettre du nom d’utilisateur

Actuellement, le `UserAccounts` GridView affiche *tous* les comptes d’utilisateur. Pour les sites Web avec des centaines ou des milliers de comptes d’utilisateur, il est impératif que l’utilisateur soit en mesure de réduire rapidement les comptes affichés. Pour ce faire, vous pouvez ajouter le filtrage LinkButtons à la page. Nous allons ajouter 27 LinkButtons à la page : un intitulé tout avec un LinkButton pour chaque lettre de l’alphabet. Si un visiteur clique sur le LinkButton tout, le GridView affiche tous les utilisateurs. S’ils cliquent sur une lettre particulière, seuls les utilisateurs dont le nom d’utilisateur commence par la lettre sélectionnée seront affichés.

Notre première tâche consiste à ajouter les 27 contrôles LinkButton. Une option consisterait à créer les 27 LinkButtons de façon déclarative, un par un. Une approche plus souple consiste à utiliser un contrôle Repeater avec un `ItemTemplate` qui restitue un LinkButton, puis lie les options de filtrage au Repeater en tant que tableau `string`.

Commencez par ajouter un contrôle Repeater à la page au-dessus du `UserAccounts` GridView. Définissez la propriété `ID` du répétiteur sur `FilteringUI`. Configurez les modèles de Repeater afin que son `ItemTemplate` restitue un LinkButton dont les propriétés `Text` et `CommandName` sont liées à l’élément de tableau actuel. Comme nous l’avons vu <a id="_msoanchor_3"> </a>dans le didacticiel [*attribution de rôles aux utilisateurs*](../roles/assigning-roles-to-users-cs.md) , cela peut être accompli à l’aide de la syntaxe de liaison de liaison `Container.DataItem`. Utilisez la `SeparatorTemplate` du répéteur pour afficher une ligne verticale entre chaque lien.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

Pour remplir ce répéteur avec les options de filtrage souhaitées, créez une méthode nommée `BindFilteringUI`. Veillez à appeler cette méthode à partir du gestionnaire d’événements `Page_Load` lors du premier chargement de la page.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

Cette méthode spécifie les options de filtrage en tant qu’éléments dans le tableau de `string` `filterOptions`. Pour chaque élément du tableau, le Repeater restitue un LinkButton avec son `Text` et `CommandName` propriétés assignées à la valeur de l’élément de tableau.

La figure 5 illustre la page de `ManageUsers.aspx` lorsque vous l’affichez dans un navigateur.

[![les listes de répéteurs 27 LinkButtons de filtrage](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**Figure 5**: listes de répétitions 27 LinkButtons de filtrage ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))

> [!NOTE]
> Les noms d’utilisateur peuvent commencer par n’importe quel caractère, y compris des chiffres et des signes de ponctuation. Pour afficher ces comptes, l’administrateur doit utiliser l’option tous les LinkButtons. Vous pouvez également ajouter un LinkButton pour retourner tous les comptes d’utilisateur qui commencent par un chiffre. Je laisse cela en tant qu’exercice pour le lecteur.

Le fait de cliquer sur l’un des LinkButtons de filtrage provoque une publication et déclenche l’événement de `ItemCommand` du répéteur, mais il n’y a aucune modification dans la grille, car nous n’avons pas encore écrit de code pour filtrer les résultats. La classe `Membership` comprend une [méthode`FindUsersByName`](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) qui retourne les comptes d’utilisateurs dont le nom d’utilisateur correspond à un modèle de recherche spécifié. Nous pouvons utiliser cette méthode pour récupérer uniquement les comptes d’utilisateurs dont les noms d’utilisateur commencent par la lettre spécifiée par le `CommandName` du LinkButton filtré sur lequel l’utilisateur a cliqué.

Commencez par mettre à jour la classe code-behind de la page `ManageUser.aspx` pour qu’elle inclue une propriété nommée `UsernameToMatch`. Cette propriété rend persistante la chaîne de filtre de nom d’utilisateur entre les publications (postback) :

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

La propriété `UsernameToMatch` stocke sa valeur affectée dans la collection `ViewState` à l’aide de la clé UsernameToMatch. Lorsque la valeur de cette propriété est lue, elle vérifie si une valeur existe dans la collection `ViewState` ; Si ce n’est pas le cas, elle retourne la valeur par défaut, une chaîne vide. La propriété `UsernameToMatch` présente un modèle commun, à savoir la persistance d’une valeur à l’état d’affichage afin que toutes les modifications apportées à la propriété soient rendues persistantes entre les publications. Pour plus d’informations sur ce modèle, consultez [comprendre l’état d’affichage de ASP.net](https://msdn.microsoft.com/library/ms972976.aspx).

Ensuite, mettez à jour la méthode `BindUserAccounts` afin qu’au lieu d’appeler `Membership.GetAllUsers`, elle appelle `Membership.FindUsersByName`, en passant la valeur de la propriété `UsernameToMatch` ajoutée au caractère générique SQL,%.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

Pour afficher uniquement les utilisateurs dont le nom d’utilisateur commence par la lettre A, affectez à la propriété `UsernameToMatch` la valeur et appelez ensuite `BindUserAccounts`. Cela entraînerait un appel à `Membership.FindUsersByName("A%")`, qui renverra tous les utilisateurs dont le nom d’utilisateur commence par un. de la même manière, pour retourner *tous* les utilisateurs, assignez une chaîne vide à la propriété `UsernameToMatch` afin que la méthode `BindUserAccounts` appelle `Membership.FindUsersByName("%")`, renvoyant ainsi tous les comptes d’utilisateur.

Créez un gestionnaire d’événements pour l’événement `ItemCommand` du répéteur. Cet événement est déclenché chaque fois qu’un utilisateur clique sur l’un des LinkButtons de filtre ; la valeur `CommandName` du LinkButton cliqué est passée par le biais de l’objet `RepeaterCommandEventArgs`. Nous devons affecter la valeur appropriée à la propriété `UsernameToMatch`, puis appeler la méthode `BindUserAccounts`. Si le `CommandName` est All, assignez une chaîne vide à `UsernameToMatch` afin que tous les comptes d’utilisateur soient affichés. Sinon, assignez la valeur `CommandName` à `UsernameToMatch`.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

Avec ce code en place, testez la fonctionnalité de filtrage. Lorsque la page est visitée pour la première fois, tous les comptes d’utilisateur sont affichés (reportez-vous à la figure 5). Le fait de cliquer sur un LinkButton entraîne une publication (postback) et filtre les résultats, en affichant uniquement les comptes d’utilisateur qui commencent par un.

[![utiliser le filtre LinkButtons pour afficher les utilisateurs dont le nom d’utilisateur commence par une lettre donnée](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**Figure 6**: utiliser le filtre LinkButtons pour afficher les utilisateurs dont le nom d’utilisateur commence par une lettre ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))

## <a name="step-4-updating-the-gridview-to-use-paging"></a>Étape 4 : mise à jour de GridView pour utiliser la pagination

Le contrôle GridView présenté dans les figures 5 et 6 répertorie tous les enregistrements retournés par la méthode `FindUsersByName`. S’il y a des centaines ou des milliers de comptes d’utilisateur, cela peut entraîner une surcharge d’informations lors de l’affichage de tous les comptes (comme c’est le cas lorsque vous cliquez sur le bouton tout ou lorsque vous visitez initialement la page). Pour aider à présenter les comptes d’utilisateur dans des segments plus gérables, nous allons configurer le contrôle GridView pour afficher 10 comptes d’utilisateur à la fois.

Le contrôle GridView offre deux types de pagination :

- **Pagination par défaut** : facile à implémenter, mais inefficace. En résumé, avec la pagination par défaut, le GridView attend *tous* les enregistrements de sa source de données. Il affiche ensuite uniquement la page d’enregistrements appropriée.
- **Pagination personnalisée** : nécessite plus de travail à implémenter, mais elle est plus efficace que la pagination par défaut, car avec la pagination personnalisée, la source de données retourne uniquement le jeu d’enregistrements précis à afficher.

La différence de performances entre la pagination par défaut et la pagination personnalisée peut être très importante lors de la pagination de milliers d’enregistrements. Étant donné que nous créons cette interface en supposant qu’il peut y avoir des centaines ou des milliers de comptes d’utilisateur, utilisons la pagination personnalisée.

> [!NOTE]
> Pour une discussion plus approfondie sur les différences entre la pagination par défaut et la pagination personnalisée, ainsi que sur les défis liés à l’implémentation de la pagination personnalisée, reportez-vous à [pagination efficace de grandes quantités de données](https://asp.net/learn/data-access/tutorial-25-cs.aspx). Pour une analyse de la différence de performances entre la pagination par défaut et la pagination personnalisée, consultez [pagination personnalisée dans ASP.net avec SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).

Pour implémenter la pagination personnalisée, nous avons d’abord besoin d’un mécanisme permettant de récupérer le sous-ensemble précis d’enregistrements affichés par le GridView. La bonne nouvelle, c’est que la méthode `FindUsersByName` de la classe `Membership` a une surcharge qui nous permet de spécifier l’index de page et la taille de page, et de retourner uniquement les comptes d’utilisateur qui se trouvent dans cette plage d’enregistrements.

En particulier, cette surcharge a la signature suivante : [`FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)`](https://msdn.microsoft.com/library/fa5st8b2.aspx).

Le paramètre *pageIndex* spécifie la page de comptes d’utilisateur à retourner ; *pageSize* indique le nombre d’enregistrements à afficher par page. Le paramètre *totalRecords* est un paramètre `out` qui retourne le nombre total de comptes d’utilisateur dans le magasin de l’utilisateur.

> [!NOTE]
> Les données retournées par `FindUsersByName` sont triées par nom d’utilisateur ; les critères de tri ne peuvent pas être personnalisés.

Le GridView peut être configuré pour utiliser la pagination personnalisée, mais uniquement lorsqu’il est lié à un contrôle ObjectDataSource. Pour que le contrôle ObjectDataSource implémente la pagination personnalisée, il requiert deux méthodes : une qui reçoit un index de début de ligne et le nombre maximal d’enregistrements à afficher, et retourne le sous-ensemble précis des enregistrements qui se trouvent dans cette étendue. et une méthode qui retourne le nombre total d’enregistrements paginés. La surcharge `FindUsersByName` accepte un index de page et une taille de page, et retourne le nombre total d’enregistrements via un paramètre `out`. Il y a donc une incompatibilité d’interface ici.

Une option consiste à créer une classe proxy qui expose l’interface attendue par ObjectDataSource, puis appelle en interne la méthode `FindUsersByName`. Une autre option, et celle que nous allons utiliser pour cet article, consiste à créer votre propre interface de pagination et à l’utiliser à la place de l’interface de pagination intégrée de GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Création d’une première interface de pagination, précédent, suivant, dernière

Nous allons créer une interface de pagination avec les LinkButtons First, Previous, Next et Last. Le premier LinkButton, quand vous cliquez dessus, envoie l’utilisateur à la première page de données, tandis que précédent le retourne à la page précédente. De même, le suivant et le dernier déplace l’utilisateur vers la page suivante et la dernière page, respectivement. Ajoutez les quatre contrôles LinkButton sous le `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

Ensuite, créez un gestionnaire d’événements pour chaque événement `Click` du LinkButton.

La figure 7 illustre les quatre LinkButtons affichés dans Visual Web Developer Mode Création.

[![ajouter tout d’abord, précédent, suivant et dernier LinkButtons sous le contrôle GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**Figure 7**: ajouter en premier, précédent, suivant et dernier LinkButtons sous le contrôle GridView ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))

### <a name="keeping-track-of-the-current-page-index"></a>Suivi de l’index de la page active

Quand un utilisateur visite pour la première fois la page de `ManageUsers.aspx` ou clique sur l’un des boutons de filtrage, nous souhaitons afficher la première page de données dans le GridView. Toutefois, lorsque l’utilisateur clique sur l’un des LinkButtons de navigation, nous devons mettre à jour l’index de la page. Pour conserver l’index de page et le nombre d’enregistrements à afficher par page, ajoutez les deux propriétés suivantes à la classe code-behind de la page :

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

À l’instar de la propriété `UsernameToMatch`, la propriété `PageIndex` rend sa valeur persistante dans l’état d’affichage. La propriété `PageSize` en lecture seule retourne une valeur codée en dur, 10. J’invite le lecteur intéressé à mettre à jour cette propriété pour qu’elle utilise le même modèle que `PageIndex`, puis à augmenter la page `ManageUsers.aspx` de telle sorte que la personne visitant la page puisse spécifier le nombre de comptes d’utilisateur à afficher par page.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Récupération des enregistrements de la page actuelle uniquement, mise à jour de l’index de la page et activation et désactivation de l’interface de pagination LinkButtons

Une fois l’interface de pagination en place et les propriétés `PageIndex` et `PageSize` ajoutées, nous sommes prêts à mettre à jour la méthode `BindUserAccounts` afin qu’elle utilise la surcharge `FindUsersByName` appropriée. En outre, cette méthode doit activer ou désactiver l’interface de pagination en fonction de la page affichée. Lorsque vous affichez la première page de données, les liens précédents et précédents doivent être désactivés. Next et Last doivent être désactivés lors de l’affichage de la dernière page.

Mettez à jour la méthode `BindUserAccounts` avec le code suivant :

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

Notez que le nombre total d’enregistrements paginés est déterminé par le dernier paramètre de la méthode `FindUsersByName`. Il s’agit d’un paramètre `out`, donc nous devons d’abord déclarer une variable pour contenir cette valeur (`totalRecords`), puis la préfixer avec le mot clé `out`.

Une fois la page spécifiée de comptes d’utilisateur retournée, les quatre LinkButtons sont activés ou désactivés, selon que la première ou la dernière page de données est affichée.

La dernière étape consiste à écrire le code pour les quatre gestionnaires d’événements `Click` « LinkButtons ». Ces gestionnaires d’événements doivent mettre à jour la propriété `PageIndex`, puis relier les données au contrôle GridView via un appel à `BindUserAccounts`. Les gestionnaires d’événements First, Previous et Next sont très simples. Toutefois, le gestionnaire d’événements `Click` pour le dernier LinkButton est un peu plus complexe, car vous devez déterminer le nombre d’enregistrements affichés pour déterminer le dernier index de page.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

Les figures 8 et 9 montrent l’interface de pagination personnalisée en action. La figure 8 illustre la page de `ManageUsers.aspx` lors de l’affichage de la première page de données pour tous les comptes d’utilisateur. Notez que seuls 10 des 13 comptes sont affichés. Le fait de cliquer sur le lien suivant ou le dernier déclenche une publication (postback), met à jour la `PageIndex` à 1 et lie la deuxième page de comptes d’utilisateur à la grille (voir figure 9).

[![les 10 premiers comptes d’utilisateur sont affichés](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**Figure 8**: les 10 premiers comptes d’utilisateur sont affichés ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))

[![cliquant sur le lien suivant affiche la deuxième page des comptes d’utilisateur](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**Figure 9**: cliquer sur le lien suivant affiche la deuxième page de comptes d’utilisateur ([cliquez pour afficher l’image en taille réelle](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))

## <a name="summary"></a>Récapitulatif

Les administrateurs doivent souvent sélectionner un utilisateur dans la liste des comptes. Dans les didacticiels précédents, nous avons examiné à l’aide d’une liste déroulante remplie avec les utilisateurs, mais cette approche n’est pas très adaptée. Dans ce didacticiel, nous avons exploré une meilleure alternative : une interface filtrable dont les résultats sont affichés dans un GridView paginé. Avec cette interface utilisateur, les administrateurs peuvent localiser et sélectionner rapidement et efficacement un compte d’utilisateur parmi les milliers.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Pagination personnalisée dans ASP.NET avec SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Pagination efficace de grandes quantités de données](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [Déploiement de votre propre outil d’administration de site Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Alicja maziarz. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](recovering-and-changing-passwords-cs.md)
