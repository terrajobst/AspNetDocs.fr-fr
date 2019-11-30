---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: Récupération et modification des mots de passe (VB) | Microsoft Docs
author: rick-anderson
description: ASP.NET comprend deux contrôles Web pour faciliter la récupération et la modification des mots de passe. Le contrôle PasswordRecovery permet à un visiteur de récupérer son PA perdu...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ed1df9455af94a86ce59ecc06c55846a4880596
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618752"
---
# <a name="recovering-and-changing-passwords-vb"></a>Récupération et changement des mots de passe (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET comprend deux contrôles Web pour faciliter la récupération et la modification des mots de passe. Le contrôle PasswordRecovery permet à un visiteur de récupérer son mot de passe perdu. Le contrôle ChangePassword permet à l’utilisateur de mettre à jour son mot de passe. Comme les autres contrôles Web relatifs aux connexions que nous avons vus dans cette série de didacticiels, les contrôles PasswordRecovery et ChangePassword fonctionnent avec l’infrastructure d’appartenance en arrière-plan pour réinitialiser ou modifier les mots de passe des utilisateurs.

## <a name="introduction"></a>Introduction

Entre les sites Web de ma banque, d’une société utilitaire, d’une société de téléphone, de comptes de messagerie et de portails Web personnalisés, j’ai, comme la plupart des gens, des dizaines de mots de passe à mémoriser. Avec un grand nombre d’informations d’identification pour mémoriser ces journées, il n’est pas rare que les gens oublient leur mot de passe. Pour tenir compte de cela, les sites Web qui offrent des comptes d’utilisateur doivent inclure une méthode permettant à un utilisateur de récupérer son mot de passe. Ce processus implique généralement la génération d’un nouveau mot de passe aléatoire et son envoi à l’adresse de messagerie de l’utilisateur dans le fichier. Après avoir reçu son nouveau mot de passe, la plupart des utilisateurs retournent sur le site et modifient leur mot de passe de l’un à l’autre de façon aléatoire.

ASP.NET comprend deux contrôles Web pour faciliter la récupération et la modification des mots de passe. Le contrôle PasswordRecovery permet à un visiteur de récupérer son mot de passe perdu. Le contrôle ChangePassword permet à l’utilisateur de mettre à jour son mot de passe. Comme les autres contrôles Web relatifs aux connexions que nous avons vus dans cette série de didacticiels, les contrôles PasswordRecovery et ChangePassword fonctionnent avec l’infrastructure d’appartenance en arrière-plan pour réinitialiser ou modifier les mots de passe des utilisateurs.

Dans ce didacticiel, nous allons examiner l’utilisation de ces deux contrôles. Nous verrons également comment modifier et réinitialiser par programmation le mot de passe d’un utilisateur via les méthodes `ChangePassword` et `ResetPassword` de la classe `MembershipUser`.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Étape 1 : aider les utilisateurs à récupérer les mots de passe perdus

Tous les sites Web qui prennent en charge les comptes d’utilisateur doivent fournir aux utilisateurs un mécanisme permettant de récupérer leurs mots de passe oubliés. La bonne nouvelle, c’est que l’implémentation de telles fonctionnalités dans ASP.NET est un simple avantage grâce au contrôle Web PasswordRecovery. Le contrôle PasswordRecovery restitue une interface qui invite l’utilisateur à entrer son nom d’utilisateur et, si nécessaire, la réponse à la question de sécurité. Il envoie ensuite son mot de passe à l’utilisateur.

> [!NOTE]
> Étant donné que les messages électroniques sont transmis sur le réseau en texte brut, il existe des risques de sécurité liés à l’envoi du mot de passe d’un utilisateur par courrier électronique.

Le contrôle PasswordRecovery se compose de trois vues :

- **Nom d’utilisateur** : invite le visiteur à entrer son nom d’utilisateur. Il s’agit de la vue initiale.
- **Question**: affiche le nom d’utilisateur et la question de sécurité de l’utilisateur sous forme de texte, ainsi qu’une zone de texte permettant à l’utilisateur d’entrer la réponse à sa question de sécurité.
- **Success**: affiche un message indiquant à l’utilisateur que son mot de passe a été envoyé par courrier électronique.

Les vues affichées et les actions effectuées par le contrôle PasswordRecovery dépendent des paramètres de configuration d’appartenance suivants :

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Le paramètre `RequiresQuestionAndAnswer` de l’infrastructure d’appartenance indique si les utilisateurs doivent spécifier une question et une réponse de sécurité lors de l’inscription d’un compte. Comme nous l’avons vu <a id="_msoanchor_1"> </a>dans le didacticiel [*création de comptes d’utilisateur*](../membership/creating-user-accounts-vb.md) , si `RequiresQuestionAndAnswer` a la valeur true (valeur par défaut), l’interface de CreateUserWizard comprend des contrôles TextBox pour la question et la réponse de sécurité du nouvel utilisateur. Si `RequiresQuestionAndAnswer` a la valeur false, aucune information n’est collectée. De même, si `RequiresQuestionAndAnswer` a la valeur true, le contrôle PasswordRecovery affiche la vue de la question une fois que l’utilisateur a entré son nom d’utilisateur ; le mot de passe est récupéré uniquement si l’utilisateur entre la bonne réponse de sécurité. Toutefois, si `RequiresQuestionAndAnswer` a la valeur false, le contrôle PasswordRecovery se déplace directement de la vue du nom d’utilisateur vers la vue opération réussie.

Une fois que l’utilisateur a fourni son nom d’utilisateur, ou son nom d’utilisateur et sa réponse de sécurité, si `RequiresQuestionAndAnswer` est vrai, le PasswordRecovery envoie par e-mail à l’utilisateur son mot de passe. Si l’option `EnablePasswordRetrieval` a la valeur true, l’utilisateur est envoyé par courrier électronique à son mot de passe actuel. S’il est défini sur false et que `EnablePasswordReset` a la valeur true, le contrôle PasswordRecovery génère un nouveau mot de passe aléatoire pour l’utilisateur et lui envoie par courrier électronique ce nouveau mot de passe. Si `EnablePasswordRetrieval` et `EnablePasswordReset` ont la valeur false, le contrôle PasswordRecovery lève une exception.

> [!NOTE]
> Rappelez-vous que le `SqlMembershipProvider` stocke les mots de passe des utilisateurs dans l’un des trois formats suivants : Clear, haché (valeur par défaut) ou chiffré. Le mécanisme de stockage utilisé dépend des paramètres de configuration de l’appartenance. l’application de démonstration utilise le format de mot de passe haché. Lorsque vous utilisez le format de mot de passe haché, l’option `EnablePasswordRetrieval` doit avoir la valeur false, car le système ne peut pas déterminer le mot de passe réel de l’utilisateur à partir de la version hachée stockée dans la base de données.

La figure 1 illustre la façon dont l’interface et le comportement du PasswordRecovery sont influencés par la configuration d’appartenance.

[![RequiresQuestionAndAnswer, EnablePasswordRetrieval et EnablePasswordReset influencent l’apparence et le comportement du contrôle PasswordRecovery](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Figure 1**: `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`et `EnablePasswordReset` influencer l’apparence et le comportement du contrôle PasswordRecovery ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image3.png))

> [!NOTE]
> Dans le <a id="_msoanchor_2"> </a>didacticiel [*création du schéma d’appartenance dans SQL Server*](../membership/creating-the-membership-schema-in-sql-server-vb.md) , nous avons configuré le fournisseur d’appartenances en affectant à `RequiresQuestionAndAnswer` la valeur true, `EnablePasswordRetrieval` la valeur false et `EnablePasswordReset` la valeur true.

### <a name="using-the-passwordrecovery-control"></a>Utilisation du contrôle PasswordRecovery

Examinons l’utilisation du contrôle PasswordRecovery dans une page ASP.NET. Ouvrez `RecoverPassword.aspx`, puis faites glisser et déposez un contrôle PasswordRecovery de la boîte à outils vers le concepteur. définissez ses `ID` sur `RecoverPwd`. Comme les contrôles Web login et CreateUserWizard, les vues du contrôle PasswordRecovery restituent une interface composite riche qui comprend des étiquettes, des zones de texte, des boutons et des contrôles de validation. Vous pouvez personnaliser l’apparence des vues par le biais des propriétés de style du contrôle ou en convertissant les vues en modèles. Je laisse cela en tant qu’exercice pour le lecteur intéressé.

Lorsqu’un utilisateur visite cette page, il entre son nom d’utilisateur et clique sur le bouton Envoyer. Étant donné que nous avons défini la propriété `RequiresQuestionAndAnswer` sur true dans nos paramètres de configuration d’appartenance, le contrôle PasswordRecovery affiche la vue de la question. Une fois que l’utilisateur a entré sa réponse de sécurité correcte et clique sur envoyer, le contrôle PasswordRecovery met à jour le mot de passe de l’utilisateur vers un mot de passe généré de manière aléatoire et envoie par courrier électronique ce mot de passe à l’adresse de messagerie du fichier. Tout cela était possible sans que nous n’ayez à écrire une seule ligne de code !

Avant de tester cette page, il y a un dernier élément de configuration à faire : nous devons spécifier les paramètres de remise du courrier dans `Web.config`. Le contrôle PasswordRecovery s’appuie sur ces paramètres pour l’envoi de l’e-mail.

La configuration de la remise du courrier est spécifiée par le biais de l' [élément`<mailSettings>`](https://msdn.microsoft.com/library/w355a94k.aspx)de l' [élément`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx). Utilisez l' [élément`<smtp>`](https://msdn.microsoft.com/library/ms164240.aspx) pour indiquer la méthode de remise et l’adresse par défaut. La balise suivante configure les paramètres de messagerie pour utiliser un serveur SMTP réseau nommé `smtp.example.com` sur le port 25 et avec les informations d’identification nom d’utilisateur/mot de passe du nom d’utilisateur et du mot de passe.

> [!NOTE]
> `<system.net>` est un élément enfant de l’élément racine `<configuration>` et un frère de `<system.web>`. Par conséquent, ne placez pas l’élément `<system.net>` dans l’élément `<system.web>` ; au lieu de cela, placez-le au même niveau.

[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

Outre l’utilisation d’un serveur SMTP sur le réseau, vous pouvez également spécifier un répertoire de collecte dans lequel les messages électroniques à envoyer doivent être déposés.

Une fois que vous avez configuré les paramètres SMTP, accédez à la page `RecoverPassword.aspx` via un navigateur. Essayez d’abord d’entrer un nom d’utilisateur qui n’existe pas dans le magasin de l’utilisateur. Comme le montre la figure 2, le contrôle PasswordRecovery affiche un message indiquant que les informations de l’utilisateur n’ont pas pu être consultées. Le texte du message peut être personnalisé via la [propriété`UserNameFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)du contrôle.

[![un message d’erreur s’affiche si un nom d’utilisateur non valide est entré](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Figure 2**: un message d’erreur s’affiche si un nom d’utilisateur non valide est entré ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image6.png))

Entrez maintenant un nom d’utilisateur. Utilisez le nom d’utilisateur d’un compte dans le système avec une adresse de messagerie à laquelle vous pouvez accéder et dont vous connaissez la réponse de sécurité. Après avoir entré le nom d’utilisateur et cliqué sur envoyer, le contrôle PasswordRecovery affiche la vue de la question. Comme pour la vue de nom d’utilisateur, si vous entrez une réponse incorrecte, le contrôle PasswordRecovery affiche un message d’erreur (voir figure 3). Utilisez la [propriété`QuestionFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) pour personnaliser ce message d’erreur.

[![un message d’erreur s’affiche si l’utilisateur entre une réponse de sécurité non valide](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Figure 3**: un message d’erreur s’affiche si l’utilisateur entre une réponse de sécurité non valide ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image9.png))

Enfin, entrez la réponse de sécurité correcte, puis cliquez sur Envoyer. En arrière-plan, le contrôle PasswordRecovery génère un mot de passe aléatoire, l’attribue au compte d’utilisateur, envoie un message électronique informant l’utilisateur de son nouveau mot de passe (voir figure 4), puis affiche la vue réussite.

[![l’utilisateur reçoit un E-mail avec son nouveau mot de passe](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Figure 4**: l’utilisateur reçoit un E-mail avec son nouveau mot de passe ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image12.png))

### <a name="customizing-the-email"></a>Personnalisation de l’E-mail

L’e-mail par défaut envoyé par le contrôle PasswordRecovery est plutôt terne (voir la figure 4). Le message est envoyé à partir du compte spécifié dans l’attribut `from` de l’élément `<smtp>` avec le mot de passe de l’objet et le corps de texte brut :

Revenez au site et connectez-vous à l’aide des informations suivantes.

Nom d’utilisateur : nom *d'* utilisateur

Mot de passe : *mot de passe*

Ce message peut être personnalisé par programme à l’aide d’un gestionnaire d’événements pour l' [événement`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)du contrôle PasswordRecovery, ou de façon déclarative via la [propriété`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Étudions ces deux options.

L’événement `SendingMail` est déclenché juste avant l’envoi du message électronique. il s’agit de la dernière chance d’ajuster par programmation le message électronique. Lorsque cet événement est déclenché, un objet de type [`MailMessageEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx)est passé au gestionnaire d’événements, dont la propriété `Message` contient une référence à l’e-mail à envoyer.

Créez un gestionnaire d’événements pour l’événement `SendingMail` et ajoutez le code suivant, qui ajoute par programmation `webmaster@example.com` à la liste CC.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

Le message électronique peut également être configuré par le biais de moyens déclaratifs. La propriété `MailDefinition` du PasswordRecovery est un objet de type [`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). La classe `MailDefinition` offre un hôte de propriétés liées à la messagerie, notamment `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`et d’autres. Pour commencer, définissez la [propriété`Subject`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) sur un nom plus descriptif que celui utilisé par défaut (mot de passe), tel que votre mot de passe a été réinitialisé...

Pour personnaliser le corps du message électronique, nous devons créer un fichier de modèle d’e-mail distinct qui contient le contenu du corps. Commencez par créer un nouveau dossier dans le site Web nommé `EmailTemplates`. Ajoutez ensuite un nouveau fichier texte à ce dossier nommé `PasswordRecovery.txt` et ajoutez le contenu suivant :

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Notez l’utilisation des espaces réservés `<%UserName%>` et `<%Password%>`. Le contrôle PasswordRecovery remplace automatiquement ces deux espaces réservés par le nom d’utilisateur et le mot de passe récupérés avant l’envoi de l’e-mail.

Enfin, faites pointer la [propriété`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) de l' `MailDefinition`vers le modèle de message électronique que nous venons de créer (`~/EmailTemplates/PasswordRecovery.txt`).

Après avoir apporté ces modifications, revisitez la page `RecoverPassword.aspx` et entrez votre nom d’utilisateur et votre réponse de sécurité. Vous recevez un message d’e-mail similaire à celui de la figure 5. Notez que `webmaster@example.com` a été de CC et que l’objet et le corps ont été mis à jour.

[![les listes objet, corps et CC ont été mises à jour](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Figure 5**: la liste objet, corps et CC a été mise à jour ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image15.png))

Pour envoyer un ensemble de messages au format HTML [`IsBodyHtml`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) la valeur true (la valeur par défaut est false) et mettre à jour le modèle de message électronique pour inclure le code html.

La propriété `MailDefinition` n’est pas unique à la classe PasswordRecovery. Comme nous le verrons à l’étape 2, le contrôle ChangePassword offre également une propriété `MailDefinition`. En outre, le contrôle CreateUserWizard comprend une telle propriété que vous pouvez configurer pour envoyer automatiquement un message électronique de bienvenue aux nouveaux utilisateurs.

> [!NOTE]
> Il n’existe actuellement aucun lien dans le volet de navigation de gauche pour atteindre la page de `RecoverPassword.aspx`. Un utilisateur souhaitera uniquement visiter cette page s’il n’a pas réussi à se connecter au site. Par conséquent, mettez à jour la page de `Login.aspx` pour inclure un lien vers la page de `RecoverPassword.aspx`.

### <a name="programmatically-resetting-a-users-password"></a>Réinitialisation par programmation du mot de passe d’un utilisateur

Lors de la réinitialisation du mot de passe d’un utilisateur, le contrôle PasswordRecovery appelle la [méthode`ResetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)de l’objet `MembershipUser`. Cette méthode a deux surcharges :

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** : réinitialise le mot de passe d’un utilisateur. Utilisez cette surcharge si `RequiresQuestionAndAnswer` a la valeur false.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -réinitialise le mot de passe d’un utilisateur uniquement si le *securityAnswer* fourni est correct. Utilisez cette surcharge si `RequiresQuestionAndAnswer` a la valeur true.

Les deux surcharges retournent le nouveau mot de passe généré de façon aléatoire.

Comme avec les autres méthodes de l’infrastructure d’appartenance, la méthode `ResetPassword` délègue au fournisseur configuré. Le `SqlMembershipProvider` appelle la procédure stockée `aspnet_Membership_ResetPassword`, en passant le nom d’utilisateur, le nouveau mot de passe et la réponse de mot de passe fournis, entre autres champs. La procédure stockée garantit que la réponse de mot de passe correspond, puis met à jour le mot de passe de l’utilisateur.

Voici quelques remarques sur l’implémentation de bas niveau :

- Un utilisateur verrouillé ne peut pas réinitialiser son mot de passe. Toutefois, un utilisateur non approuvé peut le faire. Nous aborderons les États verrouillés et approuvés plus en détail dans le <a id="_msoanchor_3"> </a>didacticiel sur le [*déverrouillage et l’approbation*](unlocking-and-approving-user-accounts-vb.md) des comptes d’utilisateur.
- Si la réponse du mot de passe est incorrecte, le nombre d’échecs de tentatives de réponse de mot de passe de l’utilisateur est incrémenté. Si un nombre spécifié de tentatives de réponse de sécurité non valides se produisent dans une fenêtre de temps spécifiée, l’utilisateur est verrouillé.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Un mot sur la façon dont les mots de passe aléatoires sont générés

Les mots de passe générés de manière aléatoire affichés dans les messages électroniques des figures 4 et 5 sont créés par la [méthode`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)de la classe d’appartenance. Cette méthode accepte deux valeurs d’entrée entières- *Length* et *numberOfNonAlphanumericCharacters* -et retourne une chaîne d’une *longueur* minimale de caractères avec au moins *numberOfNonAlphanumericCharacters* nombre de caractères non alphanumériques. Lorsque cette méthode est appelée à partir des classes d’appartenance ou des contrôles Web relatifs aux connexions, les valeurs de ces deux paramètres sont déterminées par les propriétés `MinRequiredPasswordLength` et `MinRequiredNonalphanumericCharacters` de la configuration d’appartenance, que nous définissons respectivement sur 7 et 1.

La méthode `GeneratePassword` utilise un générateur de nombres aléatoires forts du point de vue du chiffrement pour s’assurer qu’il n’existe aucun écart dans les caractères aléatoires sélectionnés. En outre, `GeneratePassword` est `Public`, ce qui signifie que vous pouvez l’utiliser directement à partir de votre application ASP.NET si vous avez besoin de générer des chaînes ou des mots de passe aléatoires.

> [!NOTE]
> La classe `SqlMembershipProvider` génère toujours un mot de passe aléatoire d’au moins 14 caractères. par conséquent, si `MinRequiredPasswordLength` est inférieur à 14, sa valeur est ignorée.

## <a name="step-2-changing-passwords"></a>Étape 2 : modification des mots de passe

Les mots de passe générés de manière aléatoire sont difficiles à mémoriser. Prenons le mot de passe illustré à la figure 4 : `WWGUZv(f2yM:Bd`. Essayez de la valider en mémoire ! Inutile de préciser qu’une fois qu’un utilisateur reçoit un mot de passe généré de manière aléatoire de ce type, il souhaite modifier le mot de passe en un nom plus mémorable.

Utilisez le contrôle ChangePassword pour créer une interface permettant à un utilisateur de modifier son mot de passe. À l’instar du contrôle PasswordRecovery, le contrôle ChangePassword se compose de deux vues : modifier le mot de passe et la réussite. La vue modifier le mot de passe invite l’utilisateur à entrer son ancien mot de passe et son nouveau mot de passe. Lorsque vous fournissez l’ancien mot de passe et un nouveau mot de passe qui répondent aux exigences de longueur minimale et de caractères non alphanumériques, le contrôle ChangePassword met à jour le mot de passe de l’utilisateur et affiche la vue réussite.

> [!NOTE]
> Le contrôle ChangePassword modifie le mot de passe de l’utilisateur en appelant la [méthode`ChangePassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)de l’objet `MembershipUser`. La méthode ChangePassword accepte deux `String` paramètres d’entrée- *oldPassword* et *newPassword*-et met à jour le compte de l’utilisateur avec *newPassword*, en supposant que le *oldPassword* fourni est correct.

Ouvrez la page `ChangePassword.aspx` et ajoutez un contrôle ChangePassword à la page, en lui attribuant un nom `ChangePwd`. À ce stade, le Mode Création doit afficher la vue modifier le mot de passe (voir figure 6). Comme avec le contrôle PasswordRecovery, vous pouvez basculer entre les vues via la balise active du contrôle. En outre, ces affichages sont personnalisables par le biais des propriétés de style assorties ou en les convertissant en un modèle.

[![ajouter un contrôle ChangePassword à la page](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Figure 6**: ajouter un contrôle ChangePassword à la page ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image18.png))

Le contrôle ChangePassword peut mettre à jour le mot de passe de l’utilisateur actuellement connecté *ou* le mot de passe d’un autre utilisateur spécifié. Comme le montre la figure 6, la vue modifier le mot de passe par défaut affiche uniquement trois contrôles TextBox : un pour l’ancien mot de passe et deux pour le nouveau mot de passe. Cette interface par défaut est utilisée pour mettre à jour le mot de passe de l’utilisateur actuellement connecté.

Pour utiliser le contrôle ChangePassword afin de mettre à jour le mot de passe d’un autre utilisateur, affectez la valeur true à la [propriété`DisplayUserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) du contrôle. Cela ajoute une quatrième zone de texte à la page, qui vous invite à entrer le nom d’utilisateur de l’utilisateur dont le mot de passe doit être modifié.

L’affectation de la valeur true à `DisplayUserName` est utile si vous souhaitez permettre à un utilisateur connecté de modifier son mot de passe sans avoir à se connecter. Personnellement, je pense qu’il n’y a rien de mal à demander à un utilisateur de se connecter avant de lui autoriser à modifier son mot de passe. Par conséquent, laissez `DisplayUserName` défini sur false (valeur par défaut). Toutefois, pour prendre cette décision, il est essentiel que les utilisateurs anonymes n’atteignent pas cette page. Mettez à jour les règles d’autorisation d’URL du site pour empêcher les utilisateurs anonymes de se rendre `ChangePassword.aspx`. Si vous devez actualiser votre mémoire sur la syntaxe de la règle d’autorisation d’URL, <a id="_msoanchor_4"> </a>reportez-vous au didacticiel sur les [*autorisations basées*](../membership/user-based-authorization-vb.md) sur l’utilisateur.

> [!NOTE]
> Il peut paraître évident que la propriété `DisplayUserName` est utile pour permettre aux administrateurs de modifier les mots de passe d’autres utilisateurs. Toutefois, même lorsque `DisplayUserName` est défini sur true, l’ancien mot de passe doit être connu et entré. Nous allons parler des techniques permettant aux administrateurs de modifier les mots de passe des utilisateurs à l’étape 3.

Accédez à la page de `ChangePassword.aspx` via un navigateur et modifiez votre mot de passe. Notez qu’un message d’erreur s’affiche si vous entrez un nouveau mot de passe qui ne répond pas aux exigences de longueur de mot de passe et de caractères non alphanumériques spécifiés dans la configuration d’appartenance (voir figure 7).

[![ajouter un contrôle ChangePassword à la page](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Figure 7**: ajouter un contrôle ChangePassword à la page ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image21.png))

Lorsque vous entrez l’ancien mot de passe et un nouveau mot de passe valides, le mot de passe de l’utilisateur connecté est modifié et la vue réussite s’affiche.

### <a name="sending-a-confirmation-email"></a>Envoi d’un E-mail de confirmation

Par défaut, le contrôle ChangePassword n’envoie pas de message électronique à l’utilisateur dont le mot de passe vient d’être mis à jour. Si vous souhaitez envoyer un message électronique, il vous suffit de configurer la propriété `MailDefinition` du contrôle. Configurons le contrôle ChangePassword afin que l’utilisateur envoie un message électronique au format HTML contenant son nouveau mot de passe.

Commencez par créer un nouveau fichier dans le dossier `EmailTemplates` nommé `ChangePassword.htm`. Ajoutez le balisage suivant :

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

Ensuite, définissez les propriétés de `BodyFileName`, `IsBodyHtml`et `Subject` de la propriété de `MailDefinition` du contrôle ChangePassword sur ~/EmailTemplates/ChangePassword.htm, true et votre mot de passe a changé !, respectivement.

Après avoir apporté ces modifications, réexaminez la page et modifiez à nouveau votre mot de passe. Cette fois-ci, le contrôle ChangePassword envoie un e-mail personnalisé au format HTML à l’adresse de messagerie de l’utilisateur dans le fichier (voir figure 8).

[![un message électronique informe l’utilisateur que son mot de passe a été modifié](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Figure 8**: un message électronique informe l’utilisateur que son mot de passe a changé ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image24.png))

## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Étape 3 : permettre aux administrateurs de modifier les mots de passe des utilisateurs

Une fonctionnalité courante dans les applications qui prennent en charge les comptes d’utilisateur est la possibilité pour un utilisateur administratif de modifier les mots de passe d’autres utilisateurs. Cette fonctionnalité est parfois nécessaire, car le système ne dispose pas de la possibilité pour les utilisateurs de modifier leurs mots de passe. Dans ce cas, la seule façon pour un utilisateur de récupérer son mot de passe oublié est que l’administrateur lui affecte un nouveau mot de passe. Avec les contrôles PasswordRecovery et ChangePassword, toutefois, les utilisateurs administratifs n’ont pas besoin d’être eux-mêmes occupés par la modification des mots de passe des utilisateurs, car ils sont en mesure de les faire eux-mêmes.

Mais que se passe-t-il si votre client insiste sur le fait que les utilisateurs administratifs doivent être en mesure de modifier les mots de passe d’autres utilisateurs ? Malheureusement, l’ajout de cette fonctionnalité peut être un peu de travail. Pour modifier le mot de passe d’un utilisateur, l’ancien et le nouveau mot de passe doivent être fournis à la méthode `ChangePassword` de l’objet `MembershipUser`, mais un administrateur ne doit pas avoir à connaître le mot de passe d’un utilisateur pour pouvoir le modifier.

Une solution de contournement consiste à réinitialiser le mot de passe de l’utilisateur, puis à le remplacer par le nouveau mot de passe à l’aide d’un code similaire à ce qui suit :

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Ce code commence par récupérer des informations sur le *nom d'* utilisateur, qui est l’utilisateur dont le mot de passe est l’administrateur souhaite changer. Ensuite, la méthode `ResetPassword` est appelée, ce qui affecte à l’utilisateur un nouveau mot de passe aléatoire. Ce mot de passe généré de façon aléatoire est retourné par la méthode et stocké dans la variable `resetPwd`. Maintenant que nous connaissons le mot de passe de l’utilisateur, nous pouvons le modifier via un appel à `ChangePassword`.

Le problème est que ce code ne fonctionne que si la configuration du système d’appartenance est définie de telle sorte que `RequiresQuestionAndAnswer` a la valeur false. Si `RequiresQuestionAndAnswer` a la valeur true, comme c’est le cas avec notre application, la méthode `ResetPassword` doit être passée à la réponse de sécurité, sinon une exception est levée.

Si l’infrastructure d’appartenance est configurée pour exiger une question et une réponse de sécurité, et que votre client insiste sur le fait que les administrateurs sont en mesure de modifier les mots de passe des utilisateurs, vous avez trois options :

- Levez les mains de l’air et dites à votre client qu’il ne s’agit là que d’une chose qui ne peut pas être effectuée.
- Affectez à `RequiresQuestionAndAnswer` la valeur false. Cela génère une application moins sécurisée. Imaginez qu’un utilisateur être mal intentionné a obtenu l’accès à la boîte de réception d’un autre utilisateur. Peut-être que l’utilisateur compromis a laissé son bureau au déjeuner et n’a pas verrouillé son poste de travail, ou peut-être qu’il a accédé à son adresse de messagerie à partir d’un terminal public et ne s’est pas déconnecté. Dans les deux cas, l’utilisateur être mal intentionné peut accéder à la page `RecoverPassword.aspx` et entrer le nom d’utilisateur de l’utilisateur. Le système enverra ensuite le mot de passe récupéré sans demander la réponse de sécurité.
- Contournez la couche d’abstraction créée par l’infrastructure d’appartenance et travaillez directement avec la base de données SQL Server. Le schéma d’appartenance comprend une procédure stockée nommée `aspnet_Membership_SetPassword` qui définit le mot de passe d’un utilisateur et ne nécessite pas la réponse de sécurité ou l’ancien mot de passe pour accomplir sa tâche.

Aucune de ces options n’est particulièrement attrayante, mais c’est comme cela que la vie d’un développeur se passe parfois.

J’ai mis en œuvre la troisième approche, en écrivant du code qui contourne les classes `Membership` et `MembershipUser` et opère directement sur la base de données `SecurityTutorials`.

> [!NOTE]
> En travaillant directement avec la base de données, l’encapsulation fournie par l’infrastructure d’appartenance est éclatée. Cette décision nous lie au `SqlMembershipProvider`, rendant notre code moins portable. En outre, ce code peut ne pas fonctionner comme prévu dans les futures versions de ASP.NET si le schéma d’appartenance change. Cette approche est une solution de contournement et, comme la plupart des solutions de contournement, n’est pas un exemple de meilleures pratiques.

Le code comporte des bits non attrayants et est assez long. Par conséquent, je ne souhaite pas encombrer ce didacticiel avec un examen approfondi de l’informatique. Si vous souhaitez en savoir plus, téléchargez le code de ce didacticiel et visitez la page `~/Administration/ManageUsers.aspx`. Cette page, que nous avons créée dans <a id="_msoanchor_5"> </a>le [didacticiel précédent](building-an-interface-to-select-one-user-account-from-many-vb.md), répertorie chaque utilisateur. J’ai mis à jour le GridView pour inclure un lien vers la page `UserInformation.aspx`, en transmettant le nom d’utilisateur de l’utilisateur sélectionné via QueryString. La page `UserInformation.aspx` affiche des informations sur l’utilisateur sélectionné et les zones de texte pour modifier son mot de passe (voir figure 9).

Après avoir entré le nouveau mot de passe, en le confirmant dans la deuxième zone de texte, puis en cliquant sur le bouton mettre à jour l’utilisateur, une publication est effectuée et la procédure stockée `aspnet_Membership_SetPassword` est appelée, ce qui met à jour le mot de passe de l’utilisateur. J’encourage les lecteurs intéressés par cette fonctionnalité à se familiariser avec le code et à essayer d’étendre les fonctionnalités pour inclure l’envoi d’un e-mail à l’utilisateur dont le mot de passe a été modifié.

[![un administrateur peut modifier le mot de passe d’un utilisateur](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Figure 9**: un administrateur peut modifier le mot de passe d’un utilisateur ([cliquez pour afficher l’image en taille réelle](recovering-and-changing-passwords-vb/_static/image27.png))

> [!NOTE]
> La page `UserInformation.aspx` ne fonctionne actuellement que si l’infrastructure d’appartenance est configurée pour stocker les mots de passe dans un format clair ou haché. Il ne dispose pas du code pour chiffrer le nouveau mot de passe, bien que vous soyez invité à ajouter cette fonctionnalité. La méthode recommandée pour ajouter le code nécessaire consiste à utiliser un décompilateur comme [réflecteur](http://www.aisto.com/roeder/dotnet/) pour examiner le code source des méthodes dans le .NET Framework ; Commencez par examiner la méthode `ChangePassword` de la classe `SqlMembershipProvider`. Il s’agit de la technique que j’ai utilisée pour écrire le code permettant de créer un hachage du mot de passe.

## <a name="summary"></a>Récapitulatif

ASP.NET propose deux contrôles pour aider les utilisateurs à gérer leur mot de passe. Le contrôle PasswordRecovery est utile pour les personnes qui ont oublié leur mot de passe. Selon la configuration de l’infrastructure d’appartenance, l’utilisateur est envoyé par courrier électronique à son mot de passe existant ou à un nouveau mot de passe généré de manière aléatoire. Le contrôle ChangePassword permet à un utilisateur de mettre à jour son mot de passe.

À l’instar des contrôles Login et CreateUserWizard, les contrôles PasswordRecovery et ChangePassword affichent une interface utilisateur riche sans avoir à écrire une seule ligne de code ou de balise déclarative. Si l’interface utilisateur par défaut ne répond pas à vos besoins, vous pouvez la personnaliser par le biais de diverses propriétés de style. Les interfaces des contrôles peuvent également être converties en modèles, pour un degré de contrôle encore plus fin. En arrière-plan, ces contrôles utilisent l’API d’appartenance, en appelant les méthodes de `ResetPassword` et de `ChangePassword` de l’objet `MembershipUser`.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Démarrages rapides du contrôle ChangePassword](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Démarrages rapides du contrôle PasswordRecovery](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Envoi de courrier électronique dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` FAQ](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel incluent Michael Emmings et Banerjee. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [Suivant](unlocking-and-approving-user-accounts-vb.md)
