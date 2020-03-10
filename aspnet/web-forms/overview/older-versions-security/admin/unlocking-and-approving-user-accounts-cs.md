---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Déverrouillage et approbation des comptes d’utilisateur (C#) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel montre comment créer une page Web permettant aux administrateurs de gérer les États verrouillés et approuvés des utilisateurs. Nous verrons également comment approuver de nouveaux utilisateurs...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: c19f7dfac0ddd12c2b4f3388a71a8ca0f71cbb18
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78545520"
---
# <a name="unlocking-and-approving-user-accounts-c"></a>Déblocage et approbation des comptes d’utilisateur (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> Ce didacticiel montre comment créer une page Web permettant aux administrateurs de gérer les États verrouillés et approuvés des utilisateurs. Nous verrons également comment approuver de nouveaux utilisateurs uniquement après avoir vérifié leur adresse de messagerie.

## <a name="introduction"></a>Introduction

Avec un nom d’utilisateur, un mot de passe et un e-mail, chaque compte d’utilisateur a deux champs d’État qui déterminent si l’utilisateur peut se connecter au site : verrouillé et approuvé. Un utilisateur est automatiquement verrouillé s’il fournit des informations d’identification non valides un nombre spécifié de fois dans un nombre de minutes spécifié (les paramètres par défaut verrouillent un utilisateur après 5 tentatives de connexion non valides dans un délai de 10 minutes). L’État approuvé est utile dans les scénarios où certaines actions doivent s’écouler avant qu’un nouvel utilisateur soit en mesure de se connecter au site. Par exemple, un utilisateur peut avoir besoin de vérifier son adresse de messagerie ou d’être approuvé par un administrateur avant de pouvoir se connecter.

Étant donné qu’un utilisateur verrouillé ou non approuvé ne peut pas se connecter, il est seulement naturel de se demander comment ces États peuvent être réinitialisés. ASP.NET n’inclut pas de fonctionnalités intégrées ni de contrôles Web pour la gestion des États verrouillés et approuvés des utilisateurs, en partie parce que ces décisions doivent être gérées pour chaque site. Certains sites peuvent approuver automatiquement tous les nouveaux comptes d’utilisateur (comportement par défaut). D’autres personnes ont un administrateur approuver les nouveaux comptes ou n’approuvent pas les utilisateurs tant qu’ils n’ont pas visité un lien envoyé à l’adresse de messagerie fournie lors de leur inscription. De même, certains sites peuvent verrouiller les utilisateurs jusqu’à ce qu’un administrateur réinitialise leur état, tandis que les autres sites envoient un message électronique à l’utilisateur verrouillé avec une URL à laquelle ils peuvent accéder pour déverrouiller leur compte.

Ce didacticiel montre comment créer une page Web permettant aux administrateurs de gérer les États verrouillés et approuvés des utilisateurs. Nous verrons également comment approuver de nouveaux utilisateurs uniquement après avoir vérifié leur adresse de messagerie.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Étape 1 : gestion des États verrouillés et approuvés des utilisateurs

Dans le <a id="Tutorial12"> </a>didacticiel [*création d’une interface pour sélectionner un compte d’utilisateur dans de nombreux*](building-an-interface-to-select-one-user-account-from-many-cs.md) didacticiels, nous avons créé une page qui répertorie chaque compte d’utilisateur dans un GridView filtré et paginé. La grille répertorie le nom et l’adresse de messagerie de chaque utilisateur, leurs États approuvés et verrouillés, s’ils sont actuellement en ligne et les commentaires relatifs à l’utilisateur. Pour gérer les États approuvés et verrouillés des utilisateurs, nous pouvons rendre cette grille modifiable. Pour modifier l’État approuvé d’un utilisateur, l’administrateur doit d’abord Rechercher le compte d’utilisateur, puis modifier la ligne GridView correspondante, en cochant ou en désélectionnant la case à cocher approuvé. Vous pouvez également gérer les États approuvés et verrouillés via une page de ASP.NET distincte.

Pour ce didacticiel, nous allons utiliser deux pages ASP.NET : `ManageUsers.aspx` et `UserInformation.aspx`. L’idée est que `ManageUsers.aspx` répertorie les comptes d’utilisateur dans le système, tandis que `UserInformation.aspx` permet à l’administrateur de gérer les États approuvés et verrouillés pour un utilisateur spécifique. Notre premier ordre d’activité consiste à compléter le contrôle GridView dans `ManageUsers.aspx` pour inclure un HyperLinkField, qui est restitué sous la forme d’une colonne de liens. Nous souhaitons que chaque lien pointe vers `UserInformation.aspx?user=UserName`, où *username* est le nom de l’utilisateur à modifier.

> [!NOTE]
> Si vous avez téléchargé le code pour <a id="Tutorial13"> </a>le didacticiel sur la [*récupération et la modification des mots de passe*](recovering-and-changing-passwords-cs.md) , vous avez peut-être remarqué que la page `ManageUsers.aspx` contient déjà un ensemble de liens « gérer » et que la page `UserInformation.aspx` fournit une interface permettant de modifier le mot de passe de l’utilisateur sélectionné. J’ai décidé de ne pas répliquer cette fonctionnalité dans le code associé à ce didacticiel, car elle fonctionnait en contournant l’API d’appartenance et en opérant directement avec la base de données SQL Server pour modifier le mot de passe d’un utilisateur. Ce didacticiel commence à partir de zéro avec la page `UserInformation.aspx`.

### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Ajout de liens « Manage » à la`UserAccounts`GridView

Ouvrez la page `ManageUsers.aspx` et ajoutez un HyperLinkField au `UserAccounts` GridView. Affectez à la propriété `Text` de HyperLinkField la valeur « Manage » et à ses propriétés `DataNavigateUrlFields` et `DataNavigateUrlFormatString` la valeur `UserName` et « UserInformation. aspx ? User ={0}», respectivement. Ces paramètres configurent les HyperLinkField de manière à ce que tous les liens hypertexte affichent le texte « Manage », mais chaque lien transmet la valeur de *nom d’utilisateur* appropriée dans la chaîne de chaîne.

Après avoir ajouté HyperLinkField au GridView, prenez un moment pour afficher la page `ManageUsers.aspx` dans un navigateur. Comme le montre la figure 1, chaque ligne GridView comprend maintenant un lien « gérer ». Le lien « gérer » pour Bruce pointe vers `UserInformation.aspx?user=Bruce`, tandis que le lien « gérer » pour Dave pointe vers `UserInformation.aspx?user=Dave`.

[![HyperLinkField ajoute un](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Figure 1**: le HyperLinkField ajoute un lien « gérer » pour chaque compte d’utilisateur ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-cs/_static/image3.png))

Nous allons créer l’interface utilisateur et le code de la page `UserInformation.aspx` dans un moment, mais voyons tout d’abord comment modifier par programmation les États verrouillés et approuvés d’un utilisateur. La [classe`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) a des propriétés [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) et [`IsApproved`](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). La propriété `IsLockedOut` est en lecture seule. Il n’existe aucun mécanisme permettant de verrouiller un utilisateur par programmation ; pour déverrouiller un utilisateur, utilisez la [méthode`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)de la classe `MembershipUser`. La propriété `IsApproved` est accessible en lecture et en écriture. Pour enregistrer les modifications apportées à cette propriété, vous devez appeler la [méthode`UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)de la classe `Membership`, en passant l’objet `MembershipUser` modifié.

Étant donné que la propriété `IsApproved` est lisible et accessible en écriture, un contrôle CheckBox est probablement le meilleur élément d’interface utilisateur pour la configuration de cette propriété. Toutefois, une case à cocher ne fonctionne pas pour la propriété `IsLockedOut`, car un administrateur ne peut pas verrouiller un utilisateur, elle peut uniquement déverrouiller un utilisateur. Une interface utilisateur appropriée pour la propriété `IsLockedOut` est un bouton qui, lorsque vous cliquez dessus, déverrouille le compte d’utilisateur. Ce bouton ne doit être activé que si l’utilisateur est verrouillé.

### <a name="creating-theuserinformationaspxpage"></a>Création de la page`UserInformation.aspx`

Nous sommes maintenant prêts à implémenter l’interface utilisateur dans `UserInformation.aspx`. Ouvrez cette page et ajoutez les contrôles Web suivants :

- Contrôle de lien hypertexte qui, lorsque vous cliquez dessus, retourne l’administrateur à la page de `ManageUsers.aspx`.
- Contrôle Web d’étiquette permettant d’afficher le nom de l’utilisateur sélectionné. Définissez la `ID` de cette étiquette sur `UserNameLabel` et effacez sa propriété `Text`.
- Contrôle CheckBox nommé `IsApproved`. Affectez à sa propriété `AutoPostBack` la valeur `true`.
- Contrôle d’étiquette pour afficher la date du dernier verrouillage verrouillé de l’utilisateur. Nommez cette étiquette `LastLockedOutDateLabel` et effacez sa propriété `Text`.
- Bouton permettant de déverrouiller l’utilisateur. Nommez ce bouton `UnlockUserButton` et définissez sa propriété `Text` sur « Unlock User ».
- Contrôle d’étiquette pour l’affichage des messages d’État, tel que « l’État approuvé de l’utilisateur a été mis à jour ». Nommez ce contrôle `StatusMessage`, effacez sa propriété `Text` et définissez sa propriété `CssClass` sur `Important`. (La classe CSS `Important` est définie dans le fichier de feuille de style `Styles.css`. elle affiche le texte correspondant dans une police large, rouge).

Après avoir ajouté ces contrôles, le Mode Création dans Visual Studio doit ressembler à la capture d’écran de la figure 2.

[![créer l’interface utilisateur pour UserInformation. aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Figure 2**: créer l’interface utilisateur pour `UserInformation.aspx` ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-cs/_static/image6.png))

Une fois l’interface utilisateur terminée, la tâche suivante consiste à définir la case à cocher `IsApproved` et d’autres contrôles en fonction des informations de l’utilisateur sélectionné. Créez un gestionnaire d’événements pour l’événement `Load` de la page et ajoutez le code suivant :

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Le code ci-dessus commence par s’assurer qu’il s’agit de la première visite de la page et non d’une publication (postback) suivante. Il lit ensuite le nom d’utilisateur transmis par le champ `user` QueryString et récupère des informations sur ce compte d’utilisateur via la méthode `Membership.GetUser(username)`. Si aucun nom d’utilisateur n’a été fourni via QueryString, ou si l’utilisateur spécifié est introuvable, l’administrateur est renvoyé à la page de `ManageUsers.aspx`.

La valeur de `UserName` de l’objet `MembershipUser` est ensuite affichée dans le `UserNameLabel` et la case à cocher `IsApproved` est activée en fonction de la valeur de la propriété `IsApproved`.

La [propriété`LastLockoutDate`](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) de l’objet `MembershipUser` retourne une valeur de `DateTime` indiquant à quel moment l’utilisateur a été verrouillé pour la dernière fois. Si l’utilisateur n’a jamais été verrouillé, la valeur retournée dépend du fournisseur d’appartenances. Lorsqu’un nouveau compte est créé, le `SqlMembershipProvider` affecte la valeur `1754-01-01 12:00:00 AM`au champ `LastLockoutDate` de la table `aspnet_Membership`. Le code ci-dessus affiche une chaîne vide dans le `LastLockoutDateLabel` si la propriété `LastLockoutDate` se produit avant l’année 2000 ; dans le cas contraire, la partie Date de la propriété `LastLockoutDate` s’affiche dans l’étiquette. La propriété `UnlockUserButton'` s `Enabled` est définie sur l’état verrouillé de l’utilisateur, ce qui signifie que ce bouton est activé uniquement si l’utilisateur est verrouillé.

Prenez un moment pour tester la page de `UserInformation.aspx` par le biais d’un navigateur. Bien entendu, vous devez commencer à `ManageUsers.aspx` et sélectionner un compte d’utilisateur à gérer. Lors de l’arrivée à `UserInformation.aspx`, Notez que la case à cocher `IsApproved` n’est activée que si l’utilisateur est approuvé. Si l’utilisateur a déjà été verrouillé, la date de son dernier verrouillage est affichée. Le bouton déverrouiller l’utilisateur est activé uniquement si l’utilisateur est actuellement verrouillé. L’activation ou la désactivation de la case à cocher `IsApproved` ou un clic sur le bouton déverrouiller l’utilisateur entraîne une publication, mais aucune modification n’est apportée au compte d’utilisateur, car nous n’avons pas encore créé de gestionnaires d’événements pour ces événements.

Revenez à Visual Studio et créez des gestionnaires d’événements pour le `IsApproved` `CheckedChanged` événement de la case à cocher et l’événement `Click` du bouton `UnlockUser`. Dans le gestionnaire d’événements `CheckedChanged`, définissez la propriété `IsApproved` de l’utilisateur sur la propriété `Checked` de la case à cocher, puis enregistrez les modifications via un appel à `Membership.UpdateUser`. Dans le gestionnaire d’événements `Click`, appelez simplement la méthode `UnlockUser` de l’objet `MembershipUser`. Dans les deux gestionnaires d’événements, affichez un message approprié dans l’étiquette `StatusMessage`.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Test de la page de`UserInformation.aspx`

Une fois ces gestionnaires d’événements en place, réexaminez la page et désapprouvez un utilisateur. Comme le montre la figure 3, vous devriez voir un bref message sur la page indiquant que la propriété de `IsApproved` de l’utilisateur a été correctement modifiée.

[![Chris n’a pas été approuvé](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Figure 3**: Chris n’a pas été approuvé ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-cs/_static/image9.png))

Ensuite, déconnectez-vous, puis essayez de vous connecter en tant qu’utilisateur dont le compte n’a pas été approuvé. Étant donné que l’utilisateur n’est pas approuvé, il ne peut pas se connecter. Par défaut, le contrôle de connexion affiche le même message si l’utilisateur ne peut pas se connecter, quelle que soit la raison. Mais dans le <a id="Tutorial6"> </a>didacticiel [*validation des informations d’identification de l’utilisateur par rapport au magasin de l’utilisateur d’appartenance*](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) , nous avons examiné comment améliorer le contrôle de connexion pour afficher un message plus approprié. Comme le montre la figure 4, Chris affiche un message expliquant qu’il ne peut pas se connecter, car son compte n’est pas encore approuvé.

[![Chris ne peut pas se connecter, car son compte n’est pas approuvé](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Figure 4**: Chris ne peut pas se connecter, car son compte n’est pas approuvé ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-cs/_static/image12.png))

Pour tester la fonctionnalité verrouillée, essayez de vous connecter en tant qu’utilisateur approuvé, mais utilisez un mot de passe incorrect. Répétez ce processus le nombre de fois nécessaire jusqu’à ce que le compte de l’utilisateur soit verrouillé. Le contrôle de connexion a également été mis à jour pour afficher un message personnalisé en cas de tentative de connexion à partir d’un compte verrouillé. Vous savez qu’un compte a été verrouillé une fois que vous avez commencé à voir le message suivant sur la page de connexion : «votre compte a été verrouillé en raison d’un trop grand nombre de tentatives de connexion non valides. Veuillez contacter l’administrateur pour déverrouiller votre compte.»

Revenez à la page `ManageUsers.aspx`, puis cliquez sur le lien gérer pour l’utilisateur verrouillé. Comme le montre la figure 5, une valeur doit s’afficher dans la `LastLockedOutDateLabel` le bouton déverrouiller l’utilisateur doit être activé. Cliquez sur le bouton déverrouiller l’utilisateur pour déverrouiller le compte d’utilisateur. Une fois que vous avez déverrouillé l’utilisateur, celui-ci peut se reconnecter.

[![Dave a été verrouillé à partir du système](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Figure 5**: Dave a été verrouillé hors du système ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-cs/_static/image15.png))

## <a name="step-2-specifying-new-users-approved-status"></a>Étape 2 : spécification de l’état d’approbation du nouvel utilisateur

L’État approuvé est utile dans les scénarios où vous souhaitez que des actions soient effectuées avant qu’un nouvel utilisateur puisse se connecter et accéder aux fonctionnalités spécifiques à l’utilisateur du site. Par exemple, vous utilisez peut-être un site Web privé dans lequel toutes les pages, à l’exception des pages de connexion et d’abonnement, sont accessibles uniquement aux utilisateurs authentifiés. Mais que se passe-t-il si un étranger atteint votre site Web, trouve la page d’inscription et crée un compte ? Pour éviter cela, vous pouvez déplacer la page d’inscription vers un dossier `Administration` et demander à un administrateur de créer manuellement chaque compte. Vous pouvez également autoriser quiconque à s’inscrire, mais interdire l’accès au site jusqu’à ce qu’un administrateur approuve le compte d’utilisateur.

Par défaut, le contrôle CreateUserWizard approuve les nouveaux comptes. Vous pouvez configurer ce comportement à l’aide de la [propriété`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)du contrôle. Définissez cette propriété sur `true` pour ne pas approuver les nouveaux comptes d’utilisateur.

> [!NOTE]
> Par défaut, le contrôle CreateUserWizard ouvre automatiquement une session sur le nouveau compte d’utilisateur. Ce comportement est dicté par la [propriété`LoginCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)du contrôle. Étant donné que les utilisateurs non approuvés ne peuvent pas se connecter au site, lorsque `DisableCreatedUser` est `true` le nouveau compte d’utilisateur n’est pas connecté au site, quelle que soit la valeur de la propriété `LoginCreatedUser`.

Si vous créez par programme des comptes d’utilisateur à l’aide de la méthode `Membership.CreateUser`, pour créer un compte d’utilisateur non approuvé, utilisez l’une des surcharges qui acceptent la valeur de la propriété `IsApproved` du nouvel utilisateur en tant que paramètre d’entrée.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Étape 3 : approbation des utilisateurs en vérifiant leur adresse de messagerie

De nombreux sites Web qui prennent en charge les comptes d’utilisateur n’approuvent pas les nouveaux utilisateurs jusqu’à ce qu’ils vérifient l’adresse de messagerie fournie lors de l’inscription. Ce processus de vérification est couramment utilisé pour contrecarrer les robots, les spammeurs et les autres ne’er, car il nécessite une adresse e-mail vérifiée unique et ajoute une étape supplémentaire dans le processus d’inscription. Avec ce modèle, quand un nouvel utilisateur s’inscrit, il reçoit un message électronique contenant un lien vers une page de vérification. En visitant le lien, l’utilisateur a prouvé qu’il a reçu l’e-mail et, par conséquent, que l’adresse de messagerie fournie est valide. La page vérification est chargée de l’approbation de l’utilisateur. Cela peut se produire automatiquement, en approuvant tout utilisateur qui atteint cette page, ou uniquement après que l’utilisateur a fourni des informations supplémentaires, telles qu’un [captcha](http://en.wikipedia.org/wiki/Captcha).

Pour prendre en charge ce flux de travail, nous devons tout d’abord mettre à jour la page de création de compte afin que les nouveaux utilisateurs ne soient pas approuvés. Ouvrez la page `EnhancedCreateUserWizard.aspx` dans le dossier `Membership` et affectez à la propriété `DisableCreatedUser` du contrôle CreateUserWizard la valeur `true`.

Ensuite, nous devons configurer le contrôle CreateUserWizard pour envoyer un message électronique au nouvel utilisateur, avec des instructions sur la façon de vérifier son compte. En particulier, nous inclurons un lien dans le message électronique sur la page `Verification.aspx` (que nous avons créée), en transmettant le `UserId` de l’utilisateur via la chaîne de chaîne. La page `Verification.aspx` recherchera l’utilisateur spécifié et marquera qu’il sera approuvé.

### <a name="sending-a-verification-email-to-new-users"></a>Envoi d’un E-mail de vérification aux nouveaux utilisateurs

Pour envoyer un message électronique à partir du contrôle CreateUserWizard, configurez sa propriété `MailDefinition` de manière appropriée. Comme indiqué dans le <a id="Tutorial13"> </a> [didacticiel précédent](recovering-and-changing-passwords-cs.md), les contrôles ChangePassword et PasswordRecovery incluent une [propriété`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) qui fonctionne de la même façon que le du contrôle CreateUserWizard.

> [!NOTE]
> Pour utiliser la propriété `MailDefinition`, vous devez spécifier les options de remise des messages dans `Web.config`. Pour plus d’informations, consultez [Envoyer un message électronique dans ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

Commencez par créer un modèle d’e-mail nommé `CreateUserWizard.txt` dans le dossier `EmailTemplates`. Utilisez le texte suivant pour le modèle :

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Définissez la propriété `MailDefinition'` s `BodyFileName` sur « ~/EmailTemplates/CreateUserWizard.txt » et sa propriété `Subject` sur «Bienvenue dans mon site Web ! Veuillez activer votre compte.»

Notez que le modèle de courrier électronique `CreateUserWizard.txt` comprend un espace réservé `<%VerificationUrl%>`. C’est ici que l’URL de la page de `Verification.aspx` sera placée. CreateUserWizard remplace automatiquement les espaces réservés `<%UserName%>` et `<%Password%>` par le nom d’utilisateur et le mot de passe du nouveau compte, mais il n’existe pas d’espace réservé `<%VerificationUrl%>` intégré. Nous devons la remplacer manuellement par l’URL de vérification appropriée.

Pour ce faire, créez un gestionnaire d’événements pour l' [événement`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) de CreateUserWizard et ajoutez le code suivant :

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

L’événement `SendingMail` se déclenche après l’événement `CreatedUser`, ce qui signifie que lors de l’exécution du gestionnaire d’événements ci-dessus, le nouveau compte d’utilisateur a déjà été créé. Nous pouvons accéder à la valeur de `UserId` du nouvel utilisateur en appelant la méthode `Membership.GetUser`, en transmettant le `UserName` entré dans le contrôle CreateUserWizard. Ensuite, l’URL de vérification est formée. L’instruction `Request.Url.GetLeftPart(UriPartial.Authority)` retourne la partie `http://yourserver.com` de l’URL ; `Request.ApplicationPath` retourne le chemin d’accès où l’application est enracinée. L’URL de vérification est ensuite définie en tant que `Verification.aspx?ID=userId`. Ces deux chaînes sont ensuite concaténées pour former l’URL complète. Enfin, le corps du message électronique (`e.Message.Body`) a toutes les occurrences de `<%VerificationUrl%>` remplacées par l’URL complète.

L’effet net est que les nouveaux utilisateurs ne sont pas approuvés, ce qui signifie qu’ils ne peuvent pas se connecter au site. En outre, ils reçoivent automatiquement un e-mail contenant un lien vers l’URL de vérification (voir figure 6).

[![le nouvel utilisateur reçoit un message électronique contenant un lien vers l’URL de vérification](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Figure 6**: le nouvel utilisateur reçoit un message électronique contenant un lien vers l’URL de vérification ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-cs/_static/image18.png))

> [!NOTE]
> L’étape CreateUserWizard par défaut du contrôle CreateUserWizard affiche un message indiquant à l’utilisateur que son compte a été créé et affiche un bouton continuer. Le fait de cliquer sur cette option amène l’utilisateur à l’URL spécifiée par la propriété `ContinueDestinationPageUrl` du contrôle. Le CreateUserWizard dans `EnhancedCreateUserWizard.aspx` est configuré pour envoyer de nouveaux utilisateurs à l' `~/Membership/AdditionalUserInfo.aspx`, qui invite l’utilisateur à entrer son ville natale, son URL de page d’accueil et sa signature. Étant donné que ces informations ne peuvent être ajoutées que par des utilisateurs connectés, il est judicieux de mettre à jour cette propriété pour renvoyer les utilisateurs vers la page d’accueil du site (`~/Default.aspx`). En outre, la page de `EnhancedCreateUserWizard.aspx` ou l’étape CreateUserWizard doit être augmentée pour informer l’utilisateur qu’un message de vérification a été envoyé et que son compte n’est pas activé tant qu’il n’a pas suivi les instructions contenues dans cet e-mail. Je laisse ces modifications en tant qu’exercice pour le lecteur.

### <a name="creating-the-verification-page"></a>Création de la page de vérification

La dernière tâche consiste à créer la page de `Verification.aspx`. Ajoutez cette page au dossier racine, en l’associant à la page maître `Site.master`. Comme nous l’avons fait avec la plupart des pages de contenu précédentes ajoutées au site, supprimez le contrôle de contenu qui référence le `LoginContent` ContentPlaceHolder afin que la page de contenu utilise le contenu par défaut de la page maître.

Ajoutez un contrôle Web Label à la page `Verification.aspx`, affectez à son `ID` la valeur `StatusMessage` et effacez sa propriété Text. Ensuite, créez le gestionnaire d’événements `Page_Load` et ajoutez le code suivant :

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

La majeure partie du code ci-dessus vérifie que le `UserId` fourni par le QueryString existe, qu’il s’agit d’une valeur de `Guid` valide et qu’il fait référence à un compte d’utilisateur existant. Si toutes ces vérifications réussissent, le compte d’utilisateur est approuvé ; dans le cas contraire, un message d’état approprié est affiché.

La figure 7 illustre la page de `Verification.aspx` lorsqu’elle est visitée via un navigateur.

[![le nouveau compte d’utilisateur est désormais approuvé](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Figure 7**: le compte de nouvel utilisateur est maintenant approuvé ([cliquez pour afficher l’image en taille réelle](unlocking-and-approving-user-accounts-cs/_static/image21.png))

## <a name="summary"></a>Récapitulatif

Tous les comptes d’utilisateur d’appartenance ont deux États qui déterminent si l’utilisateur peut se connecter au site : `IsLockedOut` et `IsApproved`. Ces deux propriétés doivent être `true` pour que l’utilisateur se connecte.

L’état verrouillé de l’utilisateur est utilisé comme mesure de sécurité afin de réduire la probabilité qu’un pirate passe dans un site par le biais de méthodes en force brute. Plus précisément, un utilisateur est verrouillé s’il existe un certain nombre de tentatives de connexion non valides dans une certaine fenêtre de temps. Ces limites sont configurables via les paramètres du fournisseur d’appartenances dans `Web.config`.

L’État approuvé est couramment utilisé pour empêcher les nouveaux utilisateurs de se connecter jusqu’à ce qu’une action soit dépassée. Le site nécessite peut-être que les nouveaux comptes soient approuvés par l’administrateur ou, comme nous l’avons vu à l’étape 3, en vérifiant leur adresse de messagerie.

Bonne programmation !

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à...

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](recovering-and-changing-passwords-cs.md)
> [Suivant](building-an-interface-to-select-one-user-account-from-many-vb.md)
