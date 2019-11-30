---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: Attribution de rôles aux utilisateurs (C#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons créer deux pages ASP.NET pour faciliter la gestion de ce que les utilisateurs appartiennent aux rôles. La première page inclut des fonctionnalités pour voir ce qui est...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: 3346e47cf604ed1d4003ca83203116666e37cb1b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74634073"
---
# <a name="assigning-roles-to-users-c"></a>Attribution de rôles aux utilisateurs (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> Dans ce didacticiel, nous allons créer deux pages ASP.NET pour faciliter la gestion de ce que les utilisateurs appartiennent aux rôles. La première page inclut des fonctionnalités pour voir ce que les utilisateurs appartiennent à un rôle donné, les rôles auxquels un utilisateur particulier appartient et la possibilité d’affecter ou de supprimer un utilisateur spécifique d’un rôle particulier. Dans la deuxième page, nous augmenterons le contrôle CreateUserWizard afin qu’il inclue une étape pour spécifier les rôles auxquels l’utilisateur nouvellement créé appartient. Cela est utile dans les scénarios où un administrateur est en mesure de créer des comptes d’utilisateur.

## <a name="introduction"></a>Introduction

Le <a id="_msoanchor_1"> </a> [didacticiel précédent](creating-and-managing-roles-cs.md) a examiné l’infrastructure des rôles et le `SqlRoleProvider`; Nous avons vu comment utiliser la classe `Roles` pour créer, récupérer et supprimer des rôles. Outre la création et la suppression de rôles, nous devons être en mesure d’affecter ou de supprimer des utilisateurs d’un rôle. Malheureusement, ASP.NET n’est pas fourni avec les contrôles Web pour gérer ce que les utilisateurs appartiennent aux rôles. Au lieu de cela, nous devons créer nos propres pages ASP.NET pour gérer ces associations. La bonne nouvelle, c’est que l’ajout et la suppression d’utilisateurs à des rôles sont très faciles. La classe `Roles` contient un certain nombre de méthodes pour ajouter un ou plusieurs utilisateurs à un ou plusieurs rôles.

Dans ce didacticiel, nous allons créer deux pages ASP.NET pour faciliter la gestion de ce que les utilisateurs appartiennent aux rôles. La première page inclut des fonctionnalités pour voir ce que les utilisateurs appartiennent à un rôle donné, les rôles auxquels un utilisateur particulier appartient et la possibilité d’affecter ou de supprimer un utilisateur spécifique d’un rôle particulier. Dans la deuxième page, nous augmenterons le contrôle CreateUserWizard afin qu’il inclue une étape pour spécifier les rôles auxquels l’utilisateur nouvellement créé appartient. Cela est utile dans les scénarios où un administrateur est en mesure de créer des comptes d’utilisateur.

Commençons !

## <a name="listing-what-users-belong-to-what-roles"></a>Liste des utilisateurs appartenant aux rôles

Le premier ordre des activités pour ce didacticiel consiste à créer une page Web à partir de laquelle les utilisateurs peuvent être attribués à des rôles. Avant de nous soucier de l’affectation d’utilisateurs aux rôles, commençons par nous concentrer tout d’abord sur la manière de déterminer ce que les utilisateurs appartiennent aux rôles. Il existe deux façons d’afficher ces informations : « par rôle » ou « par utilisateur ». Nous pourrions autoriser le visiteur à sélectionner un rôle, puis à afficher tous les utilisateurs qui appartiennent au rôle (l’affichage « par rôle ») ou à inviter le visiteur à sélectionner un utilisateur, puis à afficher les rôles attribués à cet utilisateur (l’affichage « par l’utilisateur »).

La vue « par rôle » est utile dans les cas où le visiteur souhaite connaître l’ensemble des utilisateurs qui appartiennent à un rôle particulier. la vue « par l’utilisateur » est idéale quand le visiteur doit connaître le (s) rôle (s) d’un utilisateur particulier. Nous allons faire en sorte que notre page comprenne les interfaces « par rôle » et « par utilisateur ».

Nous allons commencer par créer l’interface « par utilisateur ». Cette interface se compose d’une liste déroulante et d’une liste de cases à cocher. La liste déroulante sera remplie avec l’ensemble des utilisateurs dans le système. les cases à cocher énumèrent les rôles. La sélection d’un utilisateur dans la liste déroulante permet de vérifier les rôles auxquels l’utilisateur appartient. La personne visitant la page peut ensuite cocher ou décocher les cases pour ajouter ou supprimer l’utilisateur sélectionné dans les rôles correspondants.

> [!NOTE]
> L’utilisation d’une liste déroulante pour répertorier les comptes d’utilisateur n’est pas un choix idéal pour les sites Web où il peut y avoir des centaines de comptes d’utilisateur. Une liste déroulante est conçue pour permettre à un utilisateur de sélectionner un élément dans une liste relativement brève d’options. Il devient rapidement difficile à mesure que le nombre d’éléments de liste augmente. Si vous créez un site Web qui aura potentiellement un grand nombre de comptes d’utilisateurs, vous souhaiterez peut-être utiliser une autre interface utilisateur, telle qu’un contrôle GridView paginable ou une interface filtrable qui indique à l’utilisateur de choisir une lettre, puis uniquement affiche les utilisateurs dont le nom d’utilisateur commence par la lettre sélectionnée.

## <a name="step-1-building-the-by-user-user-interface"></a>Étape 1 : création de l’interface utilisateur « par utilisateur »

Ouvrez la page `UsersAndRoles.aspx`. En haut de la page, ajoutez un contrôle Label Web nommé `ActionStatus` et effacez sa propriété `Text`. Nous allons utiliser cette étiquette pour fournir des commentaires sur les actions effectuées, en affichant des messages tels que « l’utilisateur Tito a été ajouté au rôle administrateurs » ou « l’utilisateur Jisun a été supprimé du rôle superviseurs ». Pour que ces messages soient mis en évidence, définissez la propriété `CssClass` de l’étiquette sur « important ».

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

Ensuite, ajoutez la définition de classe CSS suivante à la feuille de style `Styles.css` :

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

Cette définition CSS indique au navigateur d’afficher l’étiquette à l’aide d’une grande police rouge. La figure 1 illustre cet effet via le concepteur Visual Studio.

[![la propriété CssClass de l’étiquette donne une grande police rouge](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**Figure 1**: la propriété `CssClass` de l’étiquette produit une grande police rouge ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image3.png))

Ensuite, ajoutez un contrôle DropDownList à la page, définissez sa propriété `ID` sur `UserList`et définissez sa propriété `AutoPostBack` sur true. Nous utiliserons ce DropDownList pour répertorier tous les utilisateurs dans le système. Ce DropDownList sera lié à une collection d’objets MembershipUser. Comme nous voulons que le DropDownList affiche la propriété UserName de l’objet MembershipUser (et l’utilise comme valeur des éléments de liste), définissez les propriétés `DataTextField` et `DataValueField` de DropDownList sur « UserName ».

Sous le DropDownList, ajoutez un répéteur nommé `UsersRoleList`. Ce répéteur répertorie tous les rôles du système sous la forme d’une série de cases à cocher. Définissez le `ItemTemplate` du répéteur à l’aide de la balise déclarative suivante :

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

Le balisage `ItemTemplate` comprend un contrôle Web CheckBox unique nommé `RoleCheckBox`. La propriété `AutoPostBack` de la case à cocher a la valeur true et la propriété `Text` est liée à `Container.DataItem`. La syntaxe DataBinding est simplement `Container.DataItem` car le Framework de rôles retourne la liste des noms de rôles sous la forme d’un tableau de chaînes, et c’est ce tableau de chaînes que nous allons lier au Repeater. Une description détaillée de la raison pour laquelle cette syntaxe est utilisée pour afficher le contenu d’un tableau lié à un contrôle Web de données dépasse le cadre de ce didacticiel. Pour plus d’informations sur ce sujet, reportez-vous à la rubrique [liaison d’un tableau scalaire à un contrôle Web de données](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

À ce stade, le balisage déclaratif de l’interface « par l’utilisateur » doit ressembler à ce qui suit :

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

Nous sommes maintenant prêts à écrire le code pour lier l’ensemble de comptes d’utilisateur à la DropDownList et l’ensemble de rôles au répétiteur. Dans la classe code-behind de la page, ajoutez une méthode nommée `BindUsersToUserList` et une autre `BindRolesList`nommée, à l’aide du code suivant :

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

La méthode `BindUsersToUserList` récupère tous les comptes d’utilisateur dans le système via la [méthode`Membership.GetAllUsers`](https://msdn.microsoft.com/library/dy8swhya.aspx). Cela retourne un [objet`MembershipUserCollection`](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), qui est une collection d' [instances de`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Cette collection est ensuite liée au `UserList` DropDownList. Les instances de `MembershipUser` qui comportent la collection contiennent diverses propriétés, comme `UserName`, `Email`, `CreationDate`et `IsOnline`. Pour indiquer à la classe DropDownList d’afficher la valeur de la propriété `UserName`, assurez-vous que les propriétés `DataTextField` et `DataValueField` de l' `UserList` DropDownList ont été définies sur « UserName ».

> [!NOTE]
> La méthode `Membership.GetAllUsers` a deux surcharges : une qui n’accepte aucun paramètre d’entrée et retourne tous les utilisateurs, et une qui prend des valeurs entières pour l’index de la page et la taille de la page, et retourne uniquement le sous-ensemble spécifié des utilisateurs. Lorsqu’un grand nombre de comptes d’utilisateur sont affichés dans un élément d’interface utilisateur paginable, la deuxième surcharge peut être utilisée pour parcourir les utilisateurs de manière plus efficace, car elle retourne simplement le sous-ensemble précis de comptes d’utilisateur plutôt que tous.

La méthode `BindRolesToList` commence par appeler la [méthode`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)de la classe `Roles`, qui retourne un tableau de chaînes contenant les rôles dans le système. Ce tableau de chaînes est ensuite lié au Repeater.

Enfin, nous devons appeler ces deux méthodes lorsque la page est chargée pour la première fois. Ajoutez le code suivant au gestionnaire d'événements `Page_Load` :

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

Une fois ce code en place, prenez un moment pour visiter la page à travers un navigateur. votre écran doit ressembler à la figure 2. Tous les comptes d’utilisateur sont renseignés dans la liste déroulante et, en dessous, chaque rôle apparaît sous la forme d’une case à cocher. Étant donné que nous définissons les propriétés `AutoPostBack` des contrôles DropDownList et CheckBox sur true, la modification de l’utilisateur sélectionné ou l’activation ou la désactivation d’un rôle entraîne une publication (postback). Aucune action n’est effectuée, toutefois, parce que nous n’avons pas encore écrit de code pour gérer ces actions. Nous allons aborder ces tâches dans les deux sections suivantes.

[![la page affiche les utilisateurs et les rôles.](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**Figure 2**: la page affiche les utilisateurs et les rôles ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image6.png))

### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Vérification des rôles auxquels l’utilisateur sélectionné appartient

Lorsque la page est chargée pour la première fois, ou lorsque le visiteur sélectionne un nouvel utilisateur dans la liste déroulante, nous devons mettre à jour les cases à cocher de la `UsersRoleList`de manière à ce qu’une case à cocher rôle donnée soit vérifiée uniquement si l’utilisateur sélectionné appartient à ce rôle. Pour ce faire, créez une méthode nommée `CheckRolesForSelectedUser` avec le code suivant :

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

Le code ci-dessus commence par déterminer qui est l’utilisateur sélectionné. Il utilise ensuite la [méthode`GetRolesForUser(userName)`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) de la classe Roles pour retourner l’ensemble de rôles de l’utilisateur spécifié en tant que tableau de chaînes. Ensuite, les éléments de Repeater sont énumérés et le `RoleCheckBox` case à cocher de chaque élément est référencé par programmation. La case à cocher est activée uniquement si le rôle auquel elle correspond est contenu dans le tableau de chaînes `selectedUsersRoles`.

> [!NOTE]
> La syntaxe de `selectedUserRoles.Contains<string>(...)` ne se compile pas si vous utilisez la version 2,0 de ASP.NET. La méthode `Contains<string>` fait partie de la [bibliothèque LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), qui est une nouveauté de ASP.net 3,5. Si vous utilisez toujours la version 2,0 de ASP.NET, utilisez plutôt la [méthode`Array.IndexOf<string>`](https://msdn.microsoft.com/library/eha9t187.aspx) .

La méthode `CheckRolesForSelectedUser` doit être appelée dans deux cas : lorsque la page est chargée pour la première fois et chaque fois que l’index sélectionné `UserList` DropDownList est modifié. Par conséquent, appelez cette méthode à partir du gestionnaire d’événements `Page_Load` (après les appels à `BindUsersToUserList` et `BindRolesToList`). Créez également un gestionnaire d’événements pour l’événement `SelectedIndexChanged` de DropDownList et appelez cette méthode à partir de là.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

Une fois ce code en place, vous pouvez tester la page dans le navigateur. Toutefois, étant donné que la page `UsersAndRoles.aspx` ne dispose pas actuellement de la possibilité d’affecter des utilisateurs à des rôles, aucun utilisateur n’a de rôles. Nous allons créer l’interface pour affecter des utilisateurs à des rôles dans un moment. vous pouvez donc accepter le mot que ce code fonctionne et vérifier qu’il le fait ultérieurement, ou vous pouvez ajouter manuellement des utilisateurs aux rôles en insérant des enregistrements dans la table `aspnet_UsersInRoles` afin de tester cette fonctionnalité.

### <a name="assigning-and-removing-users-from-roles"></a>Affectation et suppression d’utilisateurs dans des rôles

Lorsque le visiteur active ou décoche une case à cocher dans le `UsersRoleList` répéteur, nous devons ajouter ou supprimer l’utilisateur sélectionné du rôle correspondant. La propriété `AutoPostBack` de la case à cocher est actuellement définie sur true, ce qui entraîne une publication à chaque fois qu’une case à cocher dans le Repeater est activée ou désactivée. En bref, nous devons créer un gestionnaire d’événements pour l’événement de `CheckChanged` de la case à cocher. Étant donné que la case à cocher se trouve dans un contrôle Repeater, nous devons ajouter manuellement la plomberie du gestionnaire d’événements. Commencez par ajouter le gestionnaire d’événements à la classe code-behind comme méthode `protected`, comme suit :

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

Nous allons revenir à écrire le code pour ce gestionnaire d’événements dans un moment. Mais commençons par effectuer les manipulations de gestion des événements. Dans la case à cocher de la `ItemTemplate`du répétiteur, ajoutez `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Cette syntaxe relie le gestionnaire d’événements `RoleCheckBox_CheckChanged` à l’événement `CheckedChanged` du `RoleCheckBox`.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

La dernière tâche consiste à terminer le gestionnaire d’événements `RoleCheckBox_CheckChanged`. Nous devons commencer par référencer le contrôle CheckBox qui a déclenché l’événement, car cette instance de case à cocher nous indique quel rôle a été activé ou désactivé via ses propriétés `Text` et `Checked`. À l’aide de ces informations, ainsi que du nom d’utilisateur de l’utilisateur sélectionné, nous ajoutons ou supprimons l’utilisateur du rôle via la méthode [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) ou [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)de la classe `Roles`.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

Le code ci-dessus commence par programmer en référençant la case à cocher qui a déclenché l’événement, qui est disponible via le paramètre d’entrée `sender`. Si la case à cocher est activée, l’utilisateur sélectionné est ajouté au rôle spécifié, sinon il est supprimé du rôle. Dans les deux cas, l’étiquette `ActionStatus` affiche un message résumant l’action qui vient d’être effectuée.

Prenez un moment pour tester cette page par le biais d’un navigateur. Sélectionnez User Tito, puis ajoutez Tito aux rôles administrateurs et superviseurs.

[![Tito a été ajouté aux rôles administrateurs et superviseurs](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**Figure 3**: Tito a été ajouté aux rôles administrateurs et superviseurs ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image9.png))

Ensuite, sélectionnez l’utilisateur Bruce dans la liste déroulante. Il existe une publication (postback) et les cases à cocher du répéteur sont mises à jour via le `CheckRolesForSelectedUser`. Dans la mesure où Bruce n’appartient pas encore à un rôle, les deux cases à cocher sont désactivées. Ensuite, ajoutez Bruce au rôle superviseurs.

[![Bruce a été ajouté au rôle superviseurs](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**Figure 4**: Bruce a été ajouté au rôle superviseurs ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image12.png))

Pour vérifier plus en détail les fonctionnalités de la méthode `CheckRolesForSelectedUser`, sélectionnez un utilisateur autre que Tito ou Bruce. Notez que les cases à cocher sont automatiquement désactivées, indiquant qu’elles n’appartiennent à aucun rôle. Revenez à Tito. Les cases à cocher administrateurs et superviseurs doivent être vérifiées.

## <a name="step-2-building-the-by-roles-user-interface"></a>Étape 2 : création de l’interface utilisateur « par rôles »

À ce stade, nous avons terminé l’interface « par les utilisateurs » et nous sommes prêts à prendre en main l’interface « par rôle ». L’interface « par rôle » invite l’utilisateur à sélectionner un rôle dans une liste déroulante, puis affiche l’ensemble des utilisateurs qui appartiennent à ce rôle dans un GridView.

Ajoutez un autre contrôle DropDownList à la page `UsersAndRoles.aspx`. Placez-le sous le contrôle Repeater, nommez-le `RoleList`et affectez la valeur true à sa propriété `AutoPostBack`. En dessous de cela, ajoutez un GridView et nommez-le `RolesUserList`. Ce GridView répertorie les utilisateurs qui appartiennent au rôle sélectionné. Affectez à la propriété `AutoGenerateColumns` de GridView la valeur false, ajoutez un TemplateField à la collection de `Columns` de la grille et affectez à sa propriété `HeaderText` la valeur « Users ». Définissez le `ItemTemplate` du TemplateField afin qu’il affiche la valeur de l’expression DataBinding `Container.DataItem` dans la propriété `Text` d’une étiquette nommée `UserNameLabel`.

Après avoir ajouté et configuré le contrôle GridView, le balisage déclaratif de l’interface « par rôle » doit ressembler à ce qui suit :

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

Nous devons remplir le `RoleList` DropDownList avec l’ensemble de rôles dans le système. Pour ce faire, mettez à jour la méthode `BindRolesToList` de manière à ce qu’elle lie le tableau de chaînes retourné par la méthode `Roles.GetAllRoles` à l' `RolesList` DropDownList (ainsi qu’au répéteur `UsersRoleList`).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

Les deux dernières lignes de la méthode `BindRolesToList` ont été ajoutées pour lier l’ensemble de rôles au contrôle `RoleList` DropDownList. La figure 5 illustre le résultat final lorsqu’il est affiché dans un navigateur : liste déroulante remplie avec les rôles du système.

[![les rôles sont affichés dans le DropDownList RoleList](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**Figure 5**: les rôles sont affichés dans le DropDownList `RoleList` ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image15.png))

### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Affichage des utilisateurs qui appartiennent au rôle sélectionné

Lorsque la page est chargée pour la première fois, ou lorsqu’un nouveau rôle est sélectionné à partir de la `RoleList` DropDownList, nous devons afficher la liste des utilisateurs qui appartiennent à ce rôle dans le GridView. Créez une méthode nommée `DisplayUsersBelongingToRole` à l’aide du code suivant :

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

Cette méthode commence par obtenir le rôle sélectionné à partir de la `RoleList` DropDownList. Il utilise ensuite la [méthode`Roles.GetUsersInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) pour récupérer un tableau de chaînes des noms d’utilisateur des utilisateurs qui appartiennent à ce rôle. Ce tableau est ensuite lié au `RolesUserList` GridView.

Cette méthode doit être appelée dans deux cas : lors du chargement initial de la page et lorsque le rôle sélectionné dans le `RoleList` DropDownList change. Par conséquent, mettez à jour le gestionnaire d’événements `Page_Load` pour que cette méthode soit appelée après l’appel à `CheckRolesForSelectedUser`. Ensuite, créez un gestionnaire d’événements pour l’événement `SelectedIndexChanged` de l' `RoleList`, puis appelez cette méthode à partir de là.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

Une fois ce code en place, le `RolesUserList` GridView doit afficher les utilisateurs qui appartiennent au rôle sélectionné. Comme le montre la figure 6, le rôle superviseurs se compose de deux membres : Bruce et Tito.

[![le contrôle GridView répertorie les utilisateurs qui appartiennent au rôle sélectionné](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**Figure 6**: le contrôle GridView répertorie les utilisateurs qui appartiennent au rôle sélectionné ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image18.png))

### <a name="removing-users-from-the-selected-role"></a>Suppression des utilisateurs du rôle sélectionné

Ajoutons le `RolesUserList` GridView afin qu’il comprenne une colonne de boutons « Remove ». En cliquant sur le bouton « Supprimer » pour un utilisateur particulier, vous le supprimez de ce rôle.

Commencez par ajouter un champ bouton de suppression au contrôle GridView. Faites en sorte que ce champ apparaisse comme le plus à gauche et modifiez sa propriété `DeleteText` de « Delete » (valeur par défaut) en « Remove ».

[![ajouter le](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**Figure 7**: ajouter le bouton « Supprimer » au contrôle GridView ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image21.png))

Quand l’utilisateur clique sur le bouton « supprimer », une publication (postback) est levée et l’événement de `RowDeleting` du GridView est déclenché. Nous devons créer un gestionnaire d’événements pour cet événement et écrire du code qui supprime l’utilisateur du rôle sélectionné. Créez le gestionnaire d’événements, puis ajoutez le code suivant :

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

Le code commence par déterminer le nom du rôle sélectionné. Il fait ensuite référence par programmation au contrôle `UserNameLabel` à partir de la ligne sur laquelle l’utilisateur a cliqué sur le bouton « supprimer » afin de déterminer le nom d’utilisateur de l’utilisateur à supprimer. L’utilisateur est ensuite supprimé du rôle via un appel à la méthode `Roles.RemoveUserFromRole`. Le `RolesUserList` GridView est ensuite actualisé et un message s’affiche via le contrôle d’étiquette `ActionStatus`.

> [!NOTE]
> Le bouton « supprimer » ne nécessite aucun type de confirmation de la part de l’utilisateur avant de supprimer l’utilisateur du rôle. Je vous invite à ajouter un niveau de confirmation de l’utilisateur. L’une des méthodes les plus simples pour confirmer une action consiste à utiliser une boîte de dialogue de confirmation côté client. Pour plus d’informations sur cette technique, consultez [Ajout d’une confirmation côté client lors de la suppression](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

La figure 8 illustre la page une fois que l’utilisateur Tito a été supprimé du groupe des superviseurs.

[![hélas, Tito n’est plus un superviseur](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**Figure 8**: hélas, Tito n’est plus un superviseur ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image24.png))

### <a name="adding-new-users-to-the-selected-role"></a>Ajout de nouveaux utilisateurs au rôle sélectionné

En plus de supprimer des utilisateurs du rôle sélectionné, le visiteur de cette page doit également être en mesure d’ajouter un utilisateur au rôle sélectionné. La meilleure interface pour ajouter un utilisateur au rôle sélectionné dépend du nombre de comptes d’utilisateur que vous prévoyez d’avoir. Si votre site Web héberge seulement quelques douzaines de comptes d’utilisateur ou moins, vous pouvez utiliser un DropDownList ici. S’il peut y avoir des milliers de comptes d’utilisateur, vous pouvez inclure une interface utilisateur qui permet au visiteur de parcourir les comptes, Rechercher un compte particulier ou filtrer les comptes d’utilisateur d’une autre manière.

Pour cette page, nous utilisons une interface très simple qui fonctionne quel que soit le nombre de comptes d’utilisateur dans le système. À savoir, nous allons utiliser une zone de texte, invitant le visiteur à saisir le nom d’utilisateur de l’utilisateur qu’il souhaite ajouter au rôle sélectionné. S’il n’existe aucun utilisateur portant ce nom, ou si l’utilisateur est déjà membre du rôle, nous affichons un message dans `ActionStatus` étiquette. Toutefois, si l’utilisateur existe et n’est pas membre du rôle, nous les ajouterons au rôle et actualiserons la grille.

Ajoutez une zone de texte et un bouton sous le contrôle GridView. Définissez le `ID` de la zone de texte sur `UserNameToAddToRole` et définissez les propriétés `ID` et `Text` du bouton sur `AddUserToRoleButton` et sur « Ajouter un utilisateur à un rôle », respectivement.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

Ensuite, créez un gestionnaire d’événements `Click` pour le `AddUserToRoleButton` et ajoutez le code suivant :

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

La majorité du code dans le gestionnaire d’événements `Click` effectue différents contrôles de validation. Il garantit que le visiteur a fourni un nom d’utilisateur dans la zone de texte `UserNameToAddToRole`, que l’utilisateur existe dans le système, et qu’il n’appartient pas déjà au rôle sélectionné. Si l’une de ces vérifications échoue, un message approprié s’affiche dans `ActionStatus` et le gestionnaire d’événements est fermé. Si toutes les vérifications réussissent, l’utilisateur est ajouté au rôle via la méthode `Roles.AddUserToRole`. À la suite de cela, la propriété `Text` de la zone de texte est supprimée, le GridView est actualisé et l’étiquette `ActionStatus` affiche un message indiquant que l’utilisateur spécifié a été correctement ajouté au rôle sélectionné.

> [!NOTE]
> Pour vous assurer que l’utilisateur spécifié n’appartient pas déjà au rôle sélectionné, nous utilisons la [méthode`Roles.IsUserInRole(userName, roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), qui retourne une valeur booléenne indiquant si le *nom d’utilisateur* est membre de *roleName*. Nous utiliserons de nouveau cette méthode dans <a id="_msoanchor_2"> </a>le [didacticiel suivant](role-based-authorization-cs.md) lorsque nous examinerons l’autorisation basée sur les rôles.

Accédez à la page via un navigateur et sélectionnez le rôle superviseurs dans la `RoleList` DropDownList. Essayez d’entrer un nom d’utilisateur non valide. vous devez voir un message indiquant que l’utilisateur n’existe pas dans le système.

[![vous ne pouvez pas ajouter d’utilisateur inexistant à un rôle](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**Figure 9**: vous ne pouvez pas ajouter un utilisateur inexistant à un rôle ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image27.png))

Essayez maintenant d’ajouter un utilisateur valide. Continuez et rajoutez Tito au rôle superviseurs.

[![Tito est à nouveau un superviseur.](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**Figure 10**: Tito est à nouveau un superviseur.  ([Cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image30.png))

## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Étape 3 : mise à jour croisée des interfaces « par utilisateur » et « par rôle »

La page `UsersAndRoles.aspx` offre deux interfaces distinctes pour la gestion des utilisateurs et des rôles. Actuellement, ces deux interfaces agissent indépendamment les unes des autres. il est donc possible qu’une modification apportée dans une interface ne soit pas reflétée immédiatement dans l’autre. Par exemple, imaginez que le visiteur de la page sélectionne le rôle superviseurs dans le `RoleList` DropDownList, qui répertorie Bruce et Tito en tant que membres. Ensuite, le visiteur sélectionne Tito dans la `UserList` DropDownList, qui vérifie les cases à cocher administrateurs et superviseurs dans le `UsersRoleList` Repeater. Si le visiteur décoche le rôle superviseur du répétiteur, Tito est supprimé du rôle superviseurs, mais cette modification n’est pas reflétée dans l’interface « par rôle ». Le contrôle GridView affiche toujours Tito comme membre du rôle superviseurs.

Pour résoudre ce problème, nous devons actualiser le contrôle GridView chaque fois qu’un rôle est activé ou désactivé à partir du répéteur `UsersRoleList`. De même, nous devons actualiser le répéteur chaque fois qu’un utilisateur est supprimé ou ajouté à un rôle à partir de l’interface « par rôle ».

Le Repeater dans l’interface « par l’utilisateur » est actualisé en appelant la méthode `CheckRolesForSelectedUser`. L’interface « par rôle » peut être modifiée dans le gestionnaire d’événements `RowDeleting` de `RolesUserList` GridView et le gestionnaire d’événements `Click` du bouton `AddUserToRoleButton`. Par conséquent, nous devons appeler la méthode `CheckRolesForSelectedUser` à partir de chacune de ces méthodes.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

De même, le contrôle GridView de l’interface « par rôle » est actualisé en appelant la méthode `DisplayUsersBelongingToRole` et l’interface « par utilisateur » est modifiée via le gestionnaire d’événements `RoleCheckBox_CheckChanged`. Par conséquent, nous devons appeler la méthode `DisplayUsersBelongingToRole` à partir de ce gestionnaire d’événements.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

Avec ces modifications de code mineures, les interfaces « par utilisateur » et « par rôle » sont désormais correctement mises à jour de façon croisée. Pour vérifier cela, accédez à la page via un navigateur et sélectionnez Tito et superviseurs dans le `UserList` et `RoleList` DropDownList, respectivement. Notez que, lorsque vous décochez le rôle superviseurs pour Tito du répéteur dans l’interface « par l’utilisateur », Tito est automatiquement supprimé du contrôle GridView dans l’interface « par rôle ». L’ajout de Tito au rôle superviseurs à partir de l’interface « par rôle » réactive automatiquement la case à cocher superviseurs dans l’interface « par utilisateur ».

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Étape 4 : personnalisation de CreateUserWizard pour inclure une étape « spécifier des rôles »

Dans le <a id="_msoanchor_3"> </a>didacticiel [*création de comptes d’utilisateur*](../membership/creating-user-accounts-cs.md) , nous avons vu comment utiliser le contrôle Web CreateUserWizard pour fournir une interface pour la création d’un nouveau compte d’utilisateur. Le contrôle CreateUserWizard peut être utilisé de deux manières :

- Comme un moyen pour les visiteurs de créer leur propre compte d’utilisateur sur le site, et
- Comme moyen pour les administrateurs de créer de nouveaux comptes

Dans le premier cas d’utilisation, un visiteur accède au site et remplit le CreateUserWizard, en saisissant ses informations afin de s’inscrire sur le site. Dans le deuxième cas, un administrateur crée un nouveau compte pour une autre personne.

Lorsqu’un compte est créé par un administrateur pour une autre personne, il peut être utile d’autoriser l’administrateur à spécifier les rôles auxquels le nouveau compte d’utilisateur appartient. Dans le <a id="_msoanchor_4"> </a>didacticiel sur le [ *stockage* d' *informations utilisateur supplémentaires* ](../membership/storing-additional-user-information-cs.md) , nous avons vu comment personnaliser CreateUserWizard en ajoutant des `WizardSteps`supplémentaires. Nous allons voir comment ajouter une étape supplémentaire à CreateUserWizard pour spécifier les rôles de nouvel utilisateur.

Ouvrez la page `CreateUserWizardWithRoles.aspx` et ajoutez un contrôle CreateUserWizard nommé `RegisterUserWithRoles`. Affectez à la propriété `ContinueDestinationPageUrl` du contrôle la valeur « ~/default.aspx ». Étant donné que l’idée est qu’un administrateur utilisera ce contrôle CreateUserWizard pour créer de nouveaux comptes d’utilisateur, affectez la valeur false à la propriété `LoginCreatedUser` du contrôle. Cette propriété de `LoginCreatedUser` spécifie si le visiteur est connecté automatiquement en tant qu’utilisateur qui vient d’être créé, et prend par défaut la valeur true. Nous définissons la valeur sur false, car lorsqu’un administrateur crée un compte, nous voulons lui rester connecté.

Ensuite, sélectionnez l’option « Ajouter/supprimer des `WizardSteps`... ». à partir de la balise active de CreateUserWizard et ajoutez une nouvelle `WizardStep`, en affectant à son `ID` la valeur `SpecifyRolesStep`. Déplacez le `SpecifyRolesStep WizardStep` pour qu’il se trouve après l’étape « s’inscrire à votre nouveau compte », mais avant l’étape « complète ». Affectez à la propriété `Title` de l' `WizardStep`la valeur « spécifier des rôles », à sa propriété `StepType` la valeur `Step`, et à sa propriété `AllowReturn` la valeur false.

[![ajouter le](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**Figure 11**: ajouter le `WizardStep` « spécifier des rôles » à CreateUserWizard ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image33.png))

Après cette modification, le balisage déclaratif de votre CreateUserWizard doit ressembler à ce qui suit :

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

Dans la `WizardStep`« spécifier des rôles », ajoutez un CheckBoxList nommé `RoleList`. Ce CheckBoxList répertorie les rôles disponibles, ce qui permet à la personne visitant la page de vérifier les rôles auxquels l’utilisateur nouvellement créé appartient.

Nous avons laissé deux tâches de codage : tout d’abord, nous devons remplir le `RoleList` CheckBoxList avec les rôles dans le système. Deuxièmement, nous devons ajouter l’utilisateur créé aux rôles sélectionnés lorsque l’utilisateur passe de l’étape « spécifier les rôles » à l’étape « terminé ». Nous pouvons effectuer la première tâche dans le gestionnaire d’événements `Page_Load`. Le code suivant fait référence par programmation à la case à cocher `RoleList` à la première visite de la page et lie les rôles du système à celle-ci.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

Le code ci-dessus doit vous paraître familier. Dans le <a id="_msoanchor_5"> </a>didacticiel sur le [ *stockage* d' *informations utilisateur supplémentaires* ](../membership/storing-additional-user-information-cs.md) , nous avons utilisé deux instructions `FindControl` pour référencer un contrôle Web à partir d’un `WizardStep`personnalisé. Et le code qui lie les rôles au CheckBoxList a été pris de plus tôt dans ce didacticiel.

Pour effectuer la deuxième tâche de programmation, nous avons besoin de savoir quand l’étape « spécifier des rôles » est terminée. N’oubliez pas que CreateUserWizard a un événement `ActiveStepChanged`, qui se déclenche chaque fois que le visiteur navigue d’une étape à l’autre. Ici, nous pouvons déterminer si l’utilisateur a atteint l’étape « complète ». dans ce cas, nous devons ajouter l’utilisateur aux rôles sélectionnés.

Créez un gestionnaire d’événements pour l’événement `ActiveStepChanged` et ajoutez le code suivant :

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

Si l’utilisateur vient d’accéder à l’étape « terminé », le gestionnaire d’événements énumère les éléments du `RoleList` CheckBoxList et l’utilisateur juste créé est affecté aux rôles sélectionnés.

Visitez cette page via un navigateur. La première étape de CreateUserWizard est l’étape standard « s’inscrire à votre nouveau compte », qui vous invite à entrer le nom d’utilisateur, le mot de passe, l’adresse de messagerie et d’autres informations clés du nouvel utilisateur. Entrez les informations pour créer un nouvel utilisateur nommé Wanda.

[![créer un utilisateur nommé Wanda](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**Figure 12**: créer un utilisateur nommé Wanda ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image36.png))

Cliquez sur le bouton « créer un utilisateur ». Le CreateUserWizard appelle en interne la méthode `Membership.CreateUser`, en créant le nouveau compte d’utilisateur, puis passe à l’étape suivante, « spécifier des rôles ». Les rôles système sont répertoriés ici. Activez la case à cocher superviseurs et cliquez sur suivant.

[![faire de Wanda un membre du rôle superviseurs](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**Figure 13**: faire de Wanda un membre du rôle superviseurs ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image39.png))

Le fait de cliquer sur suivant provoque une publication et met à jour la `ActiveStep` à l’étape « terminé ». Dans le gestionnaire d’événements `ActiveStepChanged`, le compte d’utilisateur créé récemment est affecté au rôle superviseurs. Pour vérifier cela, revenez à la page `UsersAndRoles.aspx` et sélectionnez superviseurs dans la `RoleList` DropDownList. Comme le montre la figure 14, les superviseurs sont désormais constitués de trois utilisateurs : Bruce, Tito et Wanda.

[![Bruce, Tito et Wanda sont tous des superviseurs](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**Figure 14**: Bruce, Tito et Wanda sont tous des superviseurs ([cliquez pour afficher l’image en taille réelle](assigning-roles-to-users-cs/_static/image42.png))

## <a name="summary"></a>Récapitulatif

L’infrastructure de rôles offre des méthodes permettant de récupérer des informations sur les rôles et les méthodes d’un utilisateur particulier afin de déterminer ce que les utilisateurs appartiennent à un rôle spécifié. En outre, il existe plusieurs méthodes pour ajouter et supprimer un ou plusieurs utilisateurs à un ou plusieurs rôles. Dans ce didacticiel, nous nous sommes concentrés sur deux méthodes : `AddUserToRole` et `RemoveUserFromRole`. Des variantes supplémentaires sont conçues pour ajouter plusieurs utilisateurs à un seul rôle et pour attribuer plusieurs rôles à un seul utilisateur.

Ce didacticiel a également inclus un aperçu de l’extension du contrôle CreateUserWizard pour inclure une `WizardStep` pour spécifier les rôles de l’utilisateur nouvellement créé. Une telle étape peut aider un administrateur à rationaliser le processus de création de comptes d’utilisateur pour les nouveaux utilisateurs.

À ce stade, nous avons vu comment créer et supprimer des rôles et comment ajouter et supprimer des utilisateurs dans des rôles. Mais nous n’avons pas encore vu à appliquer l’autorisation basée sur les rôles. Dans le <a id="_msoanchor_6"> </a> [didacticiel suivant](role-based-authorization-cs.md) , nous allons examiner la définition des règles d’autorisation d’URL pour chaque rôle, ainsi que la façon de limiter les fonctionnalités au niveau de la page en fonction des rôles de l’utilisateur actuellement connecté.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Vue d’ensemble de l’outil Administration de site Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Examen d’ASP. Appartenance, rôles et profil du réseau](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Déploiement de votre propre outil d’administration de site Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à...

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-and-managing-roles-cs.md)
> [Suivant](role-based-authorization-cs.md)
