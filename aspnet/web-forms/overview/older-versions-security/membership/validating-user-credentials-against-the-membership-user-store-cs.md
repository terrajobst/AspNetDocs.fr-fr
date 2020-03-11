---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: Validation des informations d’identification de l’utilisateur par rapport auC#magasin de l’utilisateur d’appartenance () | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner comment valider les informations d’identification d’un utilisateur par rapport au magasin d’utilisateurs d’appartenance à l’aide des deux méthodes de programmation et du contrôle de connexion....
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: aaf6df6f52253ef0f7369a7e77211b6786b97db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523827"
---
# <a name="validating-user-credentials-against-the-membership-user-store-c"></a>Validation des informations d’identification de l’utilisateur par rapport au magasin d’utilisateurs d’appartenance (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> Dans ce didacticiel, nous allons examiner comment valider les informations d’identification d’un utilisateur par rapport au magasin de l’utilisateur d’appartenance à l’aide des deux méthodes de programmation et du contrôle de connexion. Nous verrons également comment personnaliser l’apparence et le comportement du contrôle de connexion.

## <a name="introduction"></a>Introduction

Dans le <a id="Tutorial05"> </a> [didacticiel précédent](creating-user-accounts-cs.md) , nous avons vu comment créer un nouveau compte d’utilisateur dans l’infrastructure d’appartenance. Nous avons d’abord examiné la création par programmation des comptes d’utilisateur via la méthode `CreateUser` de la classe `Membership`, puis examinés à l’aide du contrôle Web CreateUserWizard. Toutefois, la page de connexion valide actuellement les informations d’identification fournies par rapport à une liste codée en dur de paires nom d’utilisateur/mot de passe. Nous devons mettre à jour la logique de la page de connexion afin qu’elle valide les informations d’identification par rapport au magasin de l’utilisateur de l’infrastructure d’appartenance.

À l’instar de la création de comptes d’utilisateur, les informations d’identification peuvent être validées par programme ou de façon déclarative. L’API d’appartenance comprend une méthode permettant de valider par programmation les informations d’identification d’un utilisateur par rapport au magasin de l’utilisateur. Et ASP.NET est fourni avec le contrôle Web login, qui restitue une interface utilisateur avec des zones de texte pour le nom d’utilisateur et le mot de passe, ainsi qu’un bouton pour la connexion.

Dans ce didacticiel, nous allons examiner comment valider les informations d’identification d’un utilisateur par rapport au magasin de l’utilisateur d’appartenance à l’aide des deux méthodes de programmation et du contrôle de connexion. Nous verrons également comment personnaliser l’apparence et le comportement du contrôle de connexion. C’est parti !

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Étape 1 : validation des informations d’identification par rapport au magasin d’utilisateurs d’appartenance

Pour les sites Web qui utilisent l’authentification par formulaire, un utilisateur se connecte au site Web en visitant une page de connexion et en entrant ses informations d’identification. Ces informations d’identification sont ensuite comparées à celles du magasin de l’utilisateur. S’ils sont valides, l’utilisateur reçoit un ticket d’authentification par formulaire, qui est un jeton de sécurité qui indique l’identité et l’authenticité du visiteur.

Pour valider un utilisateur par rapport à l’infrastructure d’appartenance, utilisez la [méthode`ValidateUser`](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)de la classe `Membership`. La méthode `ValidateUser` accepte deux paramètres d’entrée : *`username`* et *`password`* -et retourne une valeur booléenne indiquant si les informations d’identification sont valides. Comme avec la méthode `CreateUser` que nous avons examinée dans le didacticiel précédent, la méthode `ValidateUser` délègue la validation réelle au fournisseur d’appartenances configuré.

Le `SqlMembershipProvider` valide les informations d’identification fournies en obtenant le mot de passe de l’utilisateur spécifié via la procédure stockée `aspnet_Membership_GetPasswordWithFormat`. Rappelez-vous que le `SqlMembershipProvider` stocke les mots de passe des utilisateurs à l’aide de l’un des trois formats suivants : Clear, Encrypted ou Hashed. La procédure stockée `aspnet_Membership_GetPasswordWithFormat` retourne le mot de passe dans son format brut. Pour les mots de passe chiffrés ou hachés, le `SqlMembershipProvider` transforme la valeur *`password`* transmise dans la méthode `ValidateUser` en son état chiffré ou haché équivalent, puis la compare à ce qui a été retourné par la base de données. Si le mot de passe stocké dans la base de données correspond au mot de passe mis en forme entré par l’utilisateur, les informations d’identification sont valides.

Nous allons mettre à jour notre page de connexion (~/`Login.aspx`) pour qu’elle valide les informations d’identification fournies par rapport au magasin d’utilisateurs de l’infrastructure d’appartenance. Nous avons créé cette page de connexion dans <a id="Tutorial02"> </a>le didacticiel d' [*authentification des formulaires*](../introduction/an-overview-of-forms-authentication-cs.md) , qui crée une interface avec deux zones de texte pour le nom d’utilisateur et le mot de passe, une case à cocher mémoriser le mot de passe et un bouton de connexion (voir figure 1). Le code valide les informations d’identification entrées par rapport à une liste codée en dur de paires nom d’utilisateur/mot de passe (Scott/mot de passe, Jisun/mot de passe et Sam/mot de passe). Dans le <a id="Tutorial03"> </a>didacticiel sur la configuration de l' [*authentification par formulaire et les rubriques avancées,* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) nous avons mis à jour le code de la page de connexion pour stocker des informations supplémentaires dans la propriété `UserData` du ticket d’authentification par formulaire.

[![l’interface de la page de connexion comprend deux zones de texte, un CheckBoxList et un bouton](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Figure 1**: l’interface de la page de connexion comprend deux zones de texte, un CheckBoxList et un bouton ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))

L’interface utilisateur de la page de connexion peut rester inchangée, mais nous devons remplacer le gestionnaire d’événements `Click` du bouton de connexion par du code qui valide l’utilisateur par rapport au magasin d’utilisateurs de l’infrastructure d’appartenance. Mettez à jour le gestionnaire d’événements afin que son code apparaisse comme suit :

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Ce code est remarquablement simple. Nous commençons par appeler la méthode `Membership.ValidateUser`, en transmettant le nom d’utilisateur et le mot de passe fournis. Si cette méthode retourne la valeur true, l’utilisateur est connecté au site via la méthode RedirectFromLoginPage de la classe `FormsAuthentication`. (Comme nous l’avons vu <a id="Tutorial2"> </a>dans le didacticiel sur l' [*authentification par formulaire*](../introduction/an-overview-of-forms-authentication-cs.md) , le `FormsAuthentication.RedirectFromLoginPage` crée le ticket d’authentification par formulaire, puis redirige l’utilisateur vers la page appropriée.) Toutefois, si les informations d’identification ne sont pas valides, le `InvalidCredentialsMessage` étiquette est affiché, informant l’utilisateur que son nom d’utilisateur ou son mot de passe est incorrect.

C’est aussi simple que cela !

Pour vérifier que la page de connexion fonctionne comme prévu, essayez de vous connecter avec l’un des comptes d’utilisateur que vous avez créés dans le didacticiel précédent. Ou, si vous n’avez pas encore créé de compte, créez-en un à partir de la page `~/Membership/CreatingUserAccounts.aspx`.

> [!NOTE]
> Lorsque l’utilisateur entre ses informations d’identification et envoie le formulaire de page de connexion, les informations d’identification, y compris son mot de passe, sont transmises via Internet au serveur Web en *texte brut*. Cela signifie que tout pirate reniflant le trafic réseau peut voir le nom d’utilisateur et le mot de passe. Pour éviter cela, il est essentiel de chiffrer le trafic réseau à l’aide de [SSL (Secure Socket Layer)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Cela permet de s’assurer que les informations d’identification (ainsi que le balisage HTML de la page entière) sont chiffrées à partir du moment où elles sortent le navigateur jusqu’à ce qu’elles soient reçues par le serveur Web.

### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Comment l’infrastructure d’appartenance gère les tentatives de connexion non valides

Lorsqu’un visiteur atteint la page de connexion et envoie ses informations d’identification, son navigateur envoie une requête HTTP à la page de connexion. Si les informations d’identification sont valides, la réponse HTTP comprend le ticket d’authentification dans un cookie. Par conséquent, un pirate tentant de pénétrer dans votre site peut créer un programme qui envoie de manière exhaustive des requêtes HTTP à la page de connexion avec un nom d’utilisateur valide et une estimation du mot de passe. Si la demande de mot de passe est correcte, la page de connexion renvoie le cookie de ticket d’authentification, auquel cas le programme sait qu’il s’est arrêté sur une paire nom d’utilisateur/mot de passe valide. À l’aide de la force brute, un tel programme peut tombés sur le mot de passe d’un utilisateur, en particulier si le mot de passe est faible.

Pour empêcher ces attaques par force brute, l’infrastructure d’appartenance verrouille un utilisateur s’il existe un certain nombre de tentatives de connexion infructueuses dans un laps de temps donné. Les paramètres exacts sont configurables via les deux paramètres de configuration de fournisseur d’appartenances suivants :

- `maxInvalidPasswordAttempts` : spécifie le nombre de tentatives de mot de passe non valides autorisées pour l’utilisateur pendant la période de temps avant que le compte ne soit verrouillé. La valeur par défaut est 5.
- `passwordAttemptWindow` : indique l’intervalle de temps (en minutes) au cours duquel le nombre spécifié de tentatives de connexion non valides entraîne le verrouillage du compte. La valeur par défaut est 10.

Si un utilisateur a été verrouillé, il ne peut pas se connecter jusqu’à ce qu’un administrateur déverrouille son compte. Lorsqu’un utilisateur est verrouillé, la méthode `ValidateUser` retourne *toujours* `false`, même si des informations d’identification valides sont fournies. Bien que ce comportement réduise la probabilité qu’un pirate s’arrête sur votre site par le biais de méthodes de force brute, il peut finir par verrouiller un utilisateur valide qui a simplement oublié son mot de passe ou qui a un verrou en majuscules ou dont le jour de frappe est incorrect.

Malheureusement, il n’existe pas d’outil intégré pour le déverrouillage d’un compte d’utilisateur. Pour déverrouiller un compte, vous pouvez modifier la base de données directement-modifier le champ `IsLockedOut` dans la table `aspnet_Membership` pour le compte d’utilisateur approprié, ou créer une interface basée sur le Web qui répertorie les comptes verrouillés avec les options permettant de les déverrouiller. Nous examinerons la création d’interfaces d’administration pour accomplir des tâches courantes liées aux rôles et aux comptes d’utilisateur dans un prochain didacticiel.

> [!NOTE]
> L’un des inconvénients de la méthode `ValidateUser` est que lorsque les informations d’identification fournies ne sont pas valides, elle ne fournit aucune explication quant à la raison. Les informations d’identification ne sont peut-être pas valides, car il n’existe aucune paire nom d’utilisateur/mot de passe correspondante dans le magasin de l’utilisateur, ou parce que l’utilisateur n’a pas encore été approuvé ou que l’utilisateur a été verrouillé. À l’étape 4, nous verrons comment afficher un message plus détaillé à l’utilisateur lorsque la tentative de connexion échoue.

## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Étape 2 : collecte des informations d’identification via le contrôle Web de connexion

Le [contrôle Web de connexion](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) restitue une interface utilisateur par défaut très similaire à celle que nous avons créée <a id="SKM5"> </a>dans le didacticiel d' [*authentification des formulaires*](../introduction/an-overview-of-forms-authentication-cs.md) . L’utilisation du contrôle de connexion nous évite d’avoir à créer l’interface pour collecter les informations d’identification du visiteur. En outre, le contrôle de connexion connecte automatiquement l’utilisateur (en supposant que les informations d’identification envoyées sont valides), ce qui nous évite d’avoir à écrire du code.

Nous mettons à jour `Login.aspx`, en remplaçant l’interface créée manuellement et le code par un contrôle de connexion. Commencez par supprimer la balise et le code existants dans `Login.aspx`. Vous pouvez le supprimer, ou simplement la commenter. Pour commenter le balisage déclaratif, entourez-le des délimiteurs `<%--` et `--%>`. Vous pouvez entrer ces délimiteurs manuellement ou, comme le montre la figure 2, vous pouvez sélectionner le texte à commenter, puis cliquer sur l’icône commenter les lignes sélectionnées dans la barre d’outils. De même, vous pouvez utiliser l’icône commenter les lignes sélectionnées pour commenter le code sélectionné dans la classe code-behind.

[![commenter le balisage déclaratif et le code source existants dans login. aspx](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Figure 2**: commenter le balisage déclaratif et le code source existants dans `Login.aspx` ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))

> [!NOTE]
> L’icône de commentaire des lignes sélectionnées n’est pas disponible lors de l’affichage du balisage déclaratif dans Visual Studio 2005. Si vous n’utilisez pas Visual Studio 2008, vous devrez ajouter manuellement les délimiteurs `<%--` et `--%>`.

Ensuite, faites glisser un contrôle Login de la boîte à outils vers la page et définissez sa propriété `ID` sur `myLogin`. À ce stade, votre écran doit ressembler à la figure 3. Notez que l’interface par défaut du contrôle de connexion comprend des contrôles TextBox pour le nom d’utilisateur et le mot de passe, une case à cocher mémoriser la durée suivante et un bouton se connecter. Il existe également des contrôles de `RequiredFieldValidator` pour les deux zones de texte.

[![ajouter un contrôle de connexion à la page](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Figure 3**: ajouter un contrôle de connexion à la page ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))

Et c’est fini ! Quand l’utilisateur clique sur le bouton de connexion du contrôle de connexion, une publication est effectuée et le contrôle de connexion appelle la méthode `Membership.ValidateUser`, en passant le nom d’utilisateur et le mot de passe entrés. Si les informations d’identification ne sont pas valides, le contrôle de connexion affiche un message indiquant que tel est le cas. Toutefois, si les informations d’identification sont valides, le contrôle de connexion crée le ticket d’authentification par formulaire et redirige l’utilisateur vers la page appropriée.

Le contrôle de connexion utilise quatre facteurs pour déterminer la page appropriée pour rediriger l’utilisateur vers une connexion réussie :

- Indique si le contrôle de connexion se trouve sur la page de connexion, comme défini par `loginUrl` paramètre dans la configuration de l’authentification par formulaire. la valeur par défaut de ce paramètre est `Login.aspx`
- Présence d’un paramètre `ReturnUrl` QueryString
- Valeur de la [propriété`DestinationUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx) du contrôle de connexion
- Valeur `defaultUrl` spécifiée dans les paramètres de configuration de l’authentification par formulaire ; la valeur par défaut de ce paramètre est `Default.aspx`

La figure 4 illustre la manière dont le contrôle de connexion utilise ces quatre paramètres pour arriver à la décision de page appropriée.

[![ajouter un contrôle de connexion à la page](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Figure 4**: ajouter un contrôle de connexion à la page ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))

Prenez un moment pour tester le contrôle de connexion en visitant le site via un navigateur et en vous connectant en tant qu’utilisateur existant dans l’infrastructure d’appartenance.

L’interface rendue du contrôle de connexion est hautement configurable. Il existe un certain nombre de propriétés qui influencent son apparence. de plus, le contrôle de connexion peut être converti en un modèle pour un contrôle précis de la disposition des éléments de l’interface utilisateur. Le reste de cette étape explique comment personnaliser l’apparence et la mise en page.

### <a name="customizing-the-login-controls-appearance"></a>Personnalisation de l’apparence du contrôle de connexion

Les paramètres de propriété par défaut du contrôle de connexion affichent une interface utilisateur avec un titre (connexion), des contrôles de zone de texte et d’étiquette pour les entrées de nom d’utilisateur et de mot de passe, une case à cocher mémoriser la prochaine fois et un bouton se connecter. Les apparences de ces éléments sont toutes configurables par le biais des nombreuses propriétés du contrôle de connexion. En outre, des éléments d’interface utilisateur supplémentaires, tels qu’un lien vers une page pour créer un nouveau compte d’utilisateur, peuvent être ajoutés en définissant une ou deux propriétés.

Passons à quelques instants pour Gussy l’apparence de notre contrôle de connexion. Étant donné que la page `Login.aspx` contient déjà du texte en haut de la page qui indique la connexion, le titre du contrôle de connexion est superflu. Par conséquent, effacez la valeur de la [propriété`TitleText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) pour supprimer le titre du contrôle de connexion.

Les étiquettes nom d’utilisateur : et mot de passe à gauche des deux contrôles TextBox peuvent être personnalisées par le biais des propriétés [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) et [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx), respectivement. Nous allons modifier le nom d’utilisateur : label pour lire le nom d’utilisateur :. Les styles des étiquettes et des zones de texte peuvent être configurés par le biais des propriétés [`LabelStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) et [`TextBoxStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx), respectivement.

La propriété de texte mémoriser le nom suivant peut être définie via le [`RememberMeText property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)du contrôle de connexion, et son état activé par défaut est configurable par le biais de la [`RememberMeSet property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (valeur par défaut false). Poursuivez et affectez la valeur true à la propriété `RememberMeSet` pour que la case à cocher mémoriser la date et l’heure suivante soit activée par défaut.

Le contrôle de connexion offre deux propriétés permettant d’ajuster la disposition de ses contrôles d’interface utilisateur. La [`TextLayout property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) indique si les étiquettes username : et Password : apparaissent à gauche de leurs zones de texte correspondantes (la valeur par défaut) ou au-dessus. Le [`Orientation property`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) indique si les entrées de nom d’utilisateur et de mot de passe sont situées verticalement (l’une au-dessus de l’autre) ou horizontalement. Je vais conserver les valeurs par défaut de ces deux propriétés, mais je vous encourage à essayer de définir ces deux propriétés sur leurs valeurs autres que celles par défaut pour voir l’effet obtenu.

> [!NOTE]
> Dans la section suivante, configuration de la disposition du contrôle de connexion, nous examinerons l’utilisation de modèles pour définir la disposition précise des éléments de l’interface utilisateur du contrôle de disposition.

Encapsulez les paramètres de propriété du contrôle de connexion en définissant les propriétés [`CreateUserText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) et [`CreateUserUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) sur pas encore enregistrées. Créez un compte. et `~/Membership/CreatingUserAccounts.aspx`, respectivement. Cela ajoute un lien hypertexte à l’interface du contrôle de connexion qui pointe vers la page que <a id="SKM6"> </a>nous avons créée dans le [didacticiel précédent](creating-user-accounts-cs.md). Les propriétés [`HelpPageText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) et [`HelpPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) du contrôle de connexion, ainsi que les propriétés [`PasswordRecoveryText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) et [`PasswordRecoveryUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) fonctionnent de la même manière, le rendu des liens vers une page d’aide et une page de récupération du mot de passe.

Après avoir apporté ces modifications de propriété, le balisage déclaratif et l’apparence de votre contrôle de connexion doivent ressembler à ce qui est illustré à la figure 5.

[![les valeurs des propriétés du contrôle de connexion déterminent leur apparence](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Figure 5**: les valeurs des propriétés du contrôle de connexion déterminent leur apparence ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))

### <a name="configuring-the-login-controls-layout"></a>Configuration de la disposition du contrôle de connexion

L’interface utilisateur par défaut du contrôle Web login dispose l’interface dans un `<table>`HTML. Mais que se passe-t-il si nous avons besoin d’un contrôle plus fin sur la sortie rendue ? Peut-être souhaitez-vous remplacer le `<table>` par une série de balises `<div>`. Que se passe-t-il si notre application requiert des informations d’identification supplémentaires pour l’authentification ? De nombreux sites Web financiers, par exemple, requièrent que les utilisateurs fournissent non seulement un nom d’utilisateur et un mot de passe, mais également un numéro d’identification personnel (PIN) ou d’autres informations d’identification. Quelle que soit la raison, il est possible de convertir le contrôle de connexion en un modèle, à partir duquel nous pouvons définir explicitement le balisage déclaratif de l’interface.

Nous devons effectuer deux opérations pour mettre à jour le contrôle de connexion afin de collecter des informations d’identification supplémentaires :

1. Mettez à jour l’interface du contrôle de connexion pour inclure le ou les contrôles Web pour collecter les informations d’identification supplémentaires.
2. Remplacez la logique d’authentification interne du contrôle de connexion afin qu’un utilisateur soit authentifié uniquement si son nom d’utilisateur et son mot de passe sont valides et si leurs informations d’identification supplémentaires sont également valides.

Pour accomplir la première tâche, nous devons convertir le contrôle de connexion en un modèle et ajouter les contrôles Web nécessaires. Comme pour la deuxième tâche, la logique d’authentification du contrôle de connexion peut être remplacée par la création d’un gestionnaire d’événements pour l' [événement`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)du contrôle.

Nous allons mettre à jour le contrôle de connexion afin qu’il invite les utilisateurs à entrer leur nom d’utilisateur, leur mot de passe et leur adresse de messagerie et n’authentifie l’utilisateur que si l’adresse de messagerie indiquée correspond à son adresse de messagerie. Nous devons tout d’abord convertir l’interface du contrôle de connexion en modèle. À partir de la balise active du contrôle de connexion, choisissez l’option convertir en modèle.

[![convertir le contrôle de connexion en modèle](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Figure 6**: convertir le contrôle de connexion en modèle ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))

> [!NOTE]
> Pour rétablir le contrôle de connexion à sa version de modèle prédéfinie, cliquez sur le lien de réinitialisation à partir de la balise active du contrôle.

La conversion du contrôle de connexion en modèle ajoute un `LayoutTemplate` au balisage déclaratif du contrôle avec des éléments HTML et des contrôles Web qui définissent l’interface utilisateur. Comme le montre la figure 7, la conversion du contrôle en modèle supprime un certain nombre de propriétés de la Fenêtre Propriétés, comme `TitleText`, `CreateUserUrl`, etc., puisque ces valeurs de propriété sont ignorées lors de l’utilisation d’un modèle.

[![moins de propriétés sont disponibles lorsque le contrôle de connexion est converti en modèle](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Figure 7**: moins de propriétés sont disponibles lorsque le contrôle de connexion est converti en modèle ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))

Le balisage HTML dans le `LayoutTemplate` peut être modifié en fonction des besoins. De même, n’hésitez pas à ajouter des contrôles Web au modèle. Toutefois, il est important que les contrôles Web principaux du contrôle de connexion restent dans le modèle et maintiennent leurs valeurs d' `ID` assignées. En particulier, ne supprimez pas ou ne renommez pas les zones de texte `UserName` ou `Password`, la case à cocher `RememberMe`, le bouton `LoginButton`, l’étiquette `FailureText` ou les contrôles `RequiredFieldValidator`.

Pour collecter l’adresse de messagerie du visiteur, nous devons ajouter une zone de texte au modèle. Ajoutez le balisage déclaratif suivant entre la ligne de table (`<tr>`) qui contient la zone de texte `Password` et la ligne de table qui contient la case à cocher mémoriser la date et l’heure suivante :

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Après avoir ajouté la zone de texte `Email`, accédez à la page via un navigateur. Comme le montre la figure 8, l’interface utilisateur du contrôle de connexion comprend désormais une troisième zone de texte.

[![le contrôle de connexion comprend désormais une zone de texte pour l’adresse de messagerie de l’utilisateur](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Figure 8**: le contrôle de connexion comprend maintenant une zone de texte pour l’adresse de messagerie de l’utilisateur ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))

À ce stade, le contrôle de connexion utilise toujours la méthode `Membership.ValidateUser` pour valider les informations d’identification fournies. En conséquence, la valeur entrée dans la zone de texte `Email` n’a aucun impact sur le fait que l’utilisateur puisse se connecter. À l’étape 3, nous verrons comment remplacer la logique d’authentification du contrôle de connexion afin que les informations d’identification ne soient considérées comme valides que si le nom d’utilisateur et le mot de passe sont valides et que l’adresse e-mail fournie correspond à l’adresse e-mail indiquée dans le fichier.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Étape 3 : modification de la logique d’authentification du contrôle de connexion

Lorsqu’un visiteur fournit ses informations d’identification et clique sur le bouton se connecter, une publication est effectuée et le contrôle de connexion progresse dans son flux de travail d’authentification. Le flux de travail démarre en déclenchant l' [événement`LoggingIn`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Tous les gestionnaires d’événements associés à cet événement peuvent annuler l’opération de connexion en affectant à la propriété `e.Cancel` la valeur `true`.

Si l’opération de connexion n’est pas annulée, le flux de travail progresse en déclenchant l' [événement`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). S’il existe un gestionnaire d’événements pour l’événement `Authenticate`, il est chargé de déterminer si les informations d’identification fournies sont valides ou non. Si aucun gestionnaire d’événements n’est spécifié, le contrôle de connexion utilise la méthode `Membership.ValidateUser` pour déterminer la validité des informations d’identification.

Si les informations d’identification fournies sont valides, le ticket d’authentification par formulaire est créé, l' [événement`LoggedIn`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) est déclenché et l’utilisateur est redirigé vers la page appropriée. Toutefois, si les informations d’identification sont considérées comme non valides, l' [événement`LoginError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) est déclenché et un message s’affiche pour informer l’utilisateur que ses informations d’identification ne sont pas valides. Par défaut, en cas d’échec, le contrôle de connexion définit simplement sa propriété Text du contrôle Label `FailureText` sur un message d’échec (votre tentative de connexion a échoué. Veuillez réessayer). Toutefois, si la [propriété`FailureAction`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) du contrôle de connexion est définie sur `RedirectToLoginPage`, le contrôle de connexion émet un `Response.Redirect` à la page de connexion en ajoutant le paramètre QueryString `loginfailure=1` (ce qui amène le contrôle de connexion à afficher le message d’échec).

La figure 9 présente un organigramme du flux de travail d’authentification.

[![le flux de travail d’authentification du contrôle de connexion](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Figure 9**: flux de travail d’authentification du contrôle de connexion ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))

> [!NOTE]
> Si vous vous demandez quand vous utiliserez l’option de page `RedirectToLogin` de `FailureAction`, tenez compte du scénario suivant. Pour le moment, notre page maître `Site.master` contient actuellement le texte Hello, étranger affiché dans la colonne de gauche lorsqu’il est visité par un utilisateur anonyme, mais imaginez que nous voulions remplacer ce texte par un contrôle de connexion. Cela permettrait à un utilisateur anonyme de se connecter à partir de n’importe quelle page du site, au lieu de lui demander d’accéder directement à la page de connexion. Toutefois, si un utilisateur n’a pas pu se connecter via le contrôle de connexion rendu par la page maître, il peut être judicieux de le rediriger vers la page de connexion (`Login.aspx`), car cette page comprend probablement des instructions supplémentaires, des liens et d’autres informations, telles que des liens permettant de créer un nouveau compte ou de récupérer un mot de

### <a name="creating-theauthenticateevent-handler"></a>Création du gestionnaire d’événements`Authenticate`

Pour pouvoir intégrer notre logique d’authentification personnalisée, nous devons créer un gestionnaire d’événements pour l’événement `Authenticate` du contrôle de connexion. La création d’un gestionnaire d’événements pour l’événement `Authenticate` génère la définition de gestionnaire d’événements suivante :

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Comme vous pouvez le voir, le gestionnaire d’événements `Authenticate` reçoit un objet de type [`AuthenticateEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) en tant que second paramètre d’entrée. La classe `AuthenticateEventArgs` contient une propriété booléenne nommée `Authenticated` qui est utilisée pour spécifier si les informations d’identification fournies sont valides. Notre tâche, puis, consiste à écrire du code qui détermine si les informations d’identification fournies sont valides ou non, et à définir la propriété `e.Authenticate` en conséquence.

### <a name="determining-and-validating-the-supplied-credentials"></a>Détermination et validation des informations d’identification fournies

Utilisez les propriétés [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) et [`Password`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) du contrôle Login pour déterminer les informations d’identification de nom d’utilisateur et de mot de passe entrées par l’utilisateur. Pour déterminer les valeurs entrées dans des contrôles Web supplémentaires (tels que la zone de texte `Email` que nous avons ajoutée à l’étape précédente), utilisez *`LoginControlID`* `.FindControl`(" *`controlID`* ") pour obtenir une référence par programmation au contrôle Web dans le modèle dont la propriété `ID` est égale à *`controlID`* . Par exemple, pour obtenir une référence à la zone de texte `Email`, utilisez le code suivant :

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Pour valider les informations d’identification de l’utilisateur, vous devez effectuer deux opérations :

1. Vérifier que le nom d’utilisateur et le mot de passe fournis sont valides
2. S’assurer que l’adresse de messagerie entrée correspond à l’adresse de messagerie de l’utilisateur qui tente de se connecter

Pour effectuer la première vérification, nous pouvons simplement utiliser la méthode `Membership.ValidateUser` comme nous l’avons vu à l’étape 1. Pour la deuxième vérification, nous devons déterminer l’adresse de messagerie de l’utilisateur afin de pouvoir le comparer à l’adresse de messagerie qu’il a entrée dans le contrôle TextBox. Pour obtenir des informations sur un utilisateur particulier, utilisez la [méthode`GetUser`](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)de la classe `Membership`.

La méthode `GetUser` a un certain nombre de surcharges. Si elle est utilisée sans passer de paramètres, elle retourne des informations sur l’utilisateur actuellement connecté. Pour obtenir des informations sur un utilisateur particulier, appelez `GetUser` en passant son nom d’utilisateur. Dans les deux cas, `GetUser` retourne un [objet`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), qui a des propriétés telles que `UserName`, `Email`, `IsApproved`, `IsOnline`, et ainsi de suite.

Le code suivant implémente ces deux contrôles. Si les deux tests réussissent, `e.Authenticate` est défini sur `true`, sinon, il est affecté `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Une fois ce code en place, essayez de vous connecter en tant qu’utilisateur valide, en saisissant le nom d’utilisateur, le mot de passe et l’adresse de messagerie appropriés. Essayez à nouveau, mais cette fois-ci, utilisez une adresse e-mail incorrecte (voir figure 10). Enfin, essayez une troisième fois en utilisant un nom d’utilisateur inexistant. Dans le premier cas, vous devez avoir ouvert une session sur le site, mais dans les deux derniers cas, vous devriez voir le message d’informations d’identification non valides du contrôle de connexion.

[![Tito ne peut pas se connecter lors de la saisie d’une adresse E-mail incorrecte](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Figure 10**: Tito ne peut pas se connecter lors de la saisie d’une adresse E-mail incorrecte ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))

> [!NOTE]
> Comme indiqué dans la section Comment l’infrastructure d’appartenance gère les tentatives de connexion non valides à l’étape 1, lorsque la méthode `Membership.ValidateUser` est appelée et qu’elle reçoit des informations d’identification non valides, elle effectue le suivi de la tentative de connexion non valide et verrouille l’utilisateur si elle dépasse un certain seuil de tentatives non valides dans une fenêtre de temps spécifiée. Étant donné que notre logique d’authentification personnalisée appelle la méthode `ValidateUser`, un mot de passe incorrect pour un nom d’utilisateur valide incrémente le compteur de tentatives de connexion non valides, mais ce compteur n’est pas incrémenté lorsque le nom d’utilisateur et le mot de passe sont valides, mais que l’adresse de messagerie est incorrecte. Il y a de fortes chances que ce comportement soit approprié, car il est peu probable qu’un pirate connaisse le nom d’utilisateur et le mot de passe, mais qu’il doit utiliser des techniques de force brute pour déterminer l’adresse de messagerie de l’utilisateur.

## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Étape 4 : amélioration du message d’informations d’identification non valides du contrôle de connexion

Lorsqu’un utilisateur tente de se connecter avec des informations d’identification non valides, le contrôle de connexion affiche un message indiquant que la tentative de connexion a échoué. En particulier, le contrôle affiche le message spécifié par sa [propriété`FailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), qui a une valeur par défaut de votre tentative de connexion a échoué. Réessayez.

Rappelez-vous qu’il existe de nombreuses raisons pour lesquelles les informations d’identification d’un utilisateur ne sont pas valides :

- Le nom d’utilisateur n’existe peut-être pas
- Le nom d’utilisateur existe, mais le mot de passe n’est pas valide
- Le nom d’utilisateur et le mot de passe sont valides, mais l’utilisateur n’est pas encore approuvé
- Le nom d’utilisateur et le mot de passe sont valides, mais l’utilisateur est verrouillé (probablement parce qu’il a dépassé le nombre de tentatives de connexion non valides dans le laps de temps spécifié)

Et il peut y avoir d’autres raisons d’utiliser la logique d’authentification personnalisée. Par exemple, avec le code que nous avons écrit à l’étape 3, le nom d’utilisateur et le mot de passe peuvent être valides, mais l’adresse de messagerie peut être incorrecte.

Quelle que soit la raison pour laquelle les informations d’identification ne sont pas valides, le contrôle de connexion affiche le même message d’erreur. Ce manque de commentaires peut être confus pour un utilisateur dont le compte n’a pas encore été approuvé ou qui a été verrouillé. Avec un peu de travail, toutefois, nous pouvons faire en sorte que le contrôle de connexion affiche un message plus approprié.

Chaque fois qu’un utilisateur tente de se connecter avec des informations d’identification non valides, le contrôle de connexion déclenche son événement `LoginError`. Continuez et créez un gestionnaire d’événements pour cet événement, puis ajoutez le code suivant :

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

Le code ci-dessus commence par définir la propriété `FailureText` du contrôle de connexion sur la valeur par défaut (votre tentative de connexion a échoué. Veuillez réessayer). Il vérifie ensuite si le nom d’utilisateur fourni est mappé à un compte d’utilisateur existant. Dans ce cas, il consulte les propriétés `IsLockedOut` et `IsApproved` de l’objet `MembershipUser` qui en résultent pour déterminer si le compte a été verrouillé ou n’a pas encore été approuvé. Dans les deux cas, la propriété `FailureText` est mise à jour avec une valeur correspondante.

Pour tester ce code, essayez de vous connecter intentionnellement en tant qu’utilisateur existant, mais utilisez un mot de passe incorrect. Procédez à cette opération cinq fois sur une ligne dans un laps de temps de 10 minutes et le compte sera verrouillé. Comme le montre la figure 11, les tentatives de connexion suivantes échouent toujours (même avec le mot de passe approprié), mais affichent désormais plus de description que votre compte a été verrouillé en raison d’un trop grand nombre de tentatives de connexion non valides. Veuillez contacter l’administrateur pour que votre compte soit déverrouillé.

[![Tito a effectué un trop grand nombre de tentatives de connexion non valides et a été verrouillé](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Figure 11**: Tito a effectué un trop grand nombre de tentatives de connexion non valides et a été verrouillé ([cliquez pour afficher l’image en taille réelle](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))

## <a name="summary"></a>Récapitulatif

Avant ce didacticiel, notre page de connexion validait les informations d’identification fournies par rapport à une liste codée en dur de paires nom d’utilisateur/mot de passe. Dans ce didacticiel, nous avons mis à jour la page pour valider les informations d’identification par rapport à l’infrastructure d’appartenance. À l’étape 1, nous avons examiné l’utilisation de la méthode `Membership.ValidateUser` par programmation. À l’étape 2, nous avons remplacé notre interface utilisateur créée manuellement et le code par le contrôle Login.

Le contrôle de connexion restitue une interface utilisateur de connexion standard et valide automatiquement les informations d’identification de l’utilisateur par rapport à l’infrastructure d’appartenance. En outre, dans le cas d’informations d’identification valides, le contrôle de connexion connecte l’utilisateur via l’authentification par formulaire. En bref, une expérience utilisateur de connexion entièrement fonctionnelle est disponible en faisant simplement glisser le contrôle de connexion sur une page, aucun balisage déclaratif ou code supplémentaire n’est nécessaire. De plus, le contrôle de connexion est hautement personnalisable, ce qui permet de contrôler précisément l’interface utilisateur et la logique d’authentification rendues.

À ce stade, les visiteurs du site Web peuvent créer un nouveau compte d’utilisateur et se connecter au site, mais nous n’avons pas encore consulté la restriction de l’accès aux pages en fonction de l’utilisateur authentifié. À l’heure actuelle, tout utilisateur, authentifié ou anonyme, peut afficher n’importe quelle page de notre site. En plus de contrôler l’accès aux pages de nos sites par utilisateur, nous pouvons avoir certaines pages dont la fonctionnalité dépend de l’utilisateur. Le didacticiel suivant explique comment limiter l’accès et les fonctionnalités dans la page en fonction de l’utilisateur connecté.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Affichage de messages personnalisés pour les utilisateurs verrouillés et non approuvés](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Examen de l’appartenance, des rôles et du profil de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procédure : créer une page de connexion ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Documentation technique sur le contrôle de connexion](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Utilisation des contrôles de connexion](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Teresa Murphy et Michael Olivero. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Précédent](creating-user-accounts-cs.md)
> [Suivant](user-based-authorization-cs.md)
