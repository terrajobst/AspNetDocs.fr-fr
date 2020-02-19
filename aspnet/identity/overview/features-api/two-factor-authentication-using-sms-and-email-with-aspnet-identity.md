---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Authentification à deux facteurs à l’aide de SMS et e-mail avec ASP.NET Identity-ASP.NET 4. x
author: HaoK
description: Ce didacticiel vous montre comment configurer l’authentification à deux facteurs (2FA) à l’aide de SMS et de la messagerie électronique. Cet article a été rédigé par Rick Anderson (@RickAndMSFT), PR...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 527b4392846e60dae0b216fdeabf21fd6618e4d7
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456736"
---
# <a name="two-factorauthentication-using-sms-and-email-with-aspnet-identity"></a>Authentification à deux facteurs à l’aide de SMS et e-mail avec ASP.NET Identity

par [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> Ce didacticiel vous montre comment configurer l’authentification à deux facteurs (2FA) à l’aide de SMS et de la messagerie électronique.
> 
> Cet article a été rédigé par Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung et Suhas Joshi. L’exemple NuGet a été écrit principalement par Hao Kung.

Cette rubrique couvre les sujets suivants :

- [Génération de l’exemple Identity](#build)
- [Configurer SMS pour l’authentification à deux facteurs](#SMS)
- [Activer l’authentification à deux facteurs](#enable2)
- [Comment inscrire un fournisseur d’authentification à deux facteurs](#reg)
- [Combiner les comptes de connexion sociale et locale](#combine)
- [Verrouillage de compte contre les attaques par force brute](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Génération de l’exemple Identity

Dans cette section, vous allez utiliser NuGet pour télécharger un exemple avec lequel nous allons travailler. Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installez Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) ou une version ultérieure.

> [!NOTE]
> AVERTISSEMENT : vous devez installer Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) pour suivre ce didacticiel.

1. Créez un projet Web ASP.NET ***vide*** .
2. Dans la console du gestionnaire de package, entrez les commandes suivantes :  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   Dans ce didacticiel, nous allons utiliser [SendGrid](http://sendgrid.com/) pour envoyer des messages électroniques et [Twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) pour le texte SMS. Le package `Identity.Samples` installe le code que nous allons utiliser.
3. Définissez le [projet pour utiliser SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Facultatif*: suivez les instructions du didacticiel de confirmation de mon [adresse de messagerie](account-confirmation-and-password-recovery-with-aspnet-identity.md) pour raccorder SendGrid, puis exécutez l’application et inscrivez un compte de messagerie.
5. *Facultatif :* Supprimez le code de confirmation de la liaison par e-mail de démonstration de l’exemple (le code `ViewBag.Link` dans le contrôleur de compte. Consultez les méthodes d’action `DisplayEmail` et `ForgotPasswordConfirmation` et les vues Razor).
6. *Facultatif :* Supprimez le code `ViewBag.Status` des contrôleurs de gestion et de compte et des vues Razor *Views\Account\VerifyCode.cshtml* et *Views\Manage\VerifyPhoneNumber.cshtml* . Vous pouvez également conserver l’affichage de `ViewBag.Status` pour tester le fonctionnement de cette application localement sans avoir à raccorder et à envoyer des messages électroniques et SMS.

> [!NOTE]
> AVERTISSEMENT : Si vous modifiez l’un des paramètres de sécurité de cet exemple, les applications de production doivent subir un audit de sécurité qui appelle explicitement les modifications apportées.

<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Configurer SMS pour l’authentification à deux facteurs

Ce didacticiel fournit des instructions sur l’utilisation de Twilio ou de ASPSMS, mais vous pouvez utiliser n’importe quel autre fournisseur SMS.

1. **Création d’un compte d’utilisateur avec un fournisseur SMS**  
  
   Créez un compte [Twilio](https://www.twilio.com/try-twilio) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .
2. **Installation de packages supplémentaires ou ajout de références de service**  
  
   Twilio  
   Dans la Console du gestionnaire de package, entrez la commande suivante :  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Vous devez ajouter la référence de service suivante :  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Adresse :  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Espace de noms :  
    `ASPSMSX2`
3. **Identification des informations d’identification de l’utilisateur du fournisseur SMS**  
  
   Twilio  
   À partir de l’onglet **tableau de bord** de votre compte Twilio, copiez le **SID de compte** et le jeton d' **authentification**.  
  
   ASPSMS:  
   Dans les paramètres de votre compte, accédez à **sensibles** et copiez-le avec votre **mot de passe**auto-défini.  
  
   Nous stockerons ultérieurement ces valeurs dans les variables `SMSAccountIdentification` et `SMSAccountPassword`.
4. **Spécification de SenderID/Originator**  
  
   Twilio  
   Dans l’onglet **nombres** , copiez votre numéro de téléphone Twilio.  
  
   ASPSMS:  
   Dans le menu **déverrouiller** les expéditeurs, déverrouillez un ou plusieurs expéditeurs ou choisissez un expéditeur alphanumérique (non pris en charge par tous les réseaux).  
  
   Nous stockerons cette valeur plus tard dans la variable `SMSAccountFrom`.
5. **Transfert des informations d’identification du fournisseur SMS vers l’application**  
  
   Mettez les informations d’identification et le numéro de téléphone de l’expéditeur à la disposition de l’application :

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Sécurité - Ne jamais stocker de données sensibles dans votre code source. Le compte et les informations d’identification sont ajoutés au code ci-dessus pour que l’exemple reste simple. Consultez ASP.NET MVC de Jon atten [: conserver les paramètres privés en dehors du contrôle de code source](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implémentation du transfert de données vers le fournisseur SMS**  
  
   Configurez la classe `SmsService` dans le fichier *App\_Start\IdentityConfig.cs* .  
  
   Selon le fournisseur SMS utilisé, activez la section **Twilio** ou **ASPSMS** : 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Exécutez l’application et connectez-vous avec le compte que vous avez enregistré précédemment.
8. Cliquez sur votre ID d’utilisateur, ce qui active la méthode d’action `Index` dans `Manage` contrôleur.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Cliquez sur Ajouter.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. En quelques secondes, vous recevrez un message texte contenant le code de vérification. Entrez-le et appuyez sur **Envoyer**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. La vue gérer indique que votre numéro de téléphone a été ajouté.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Examiner le code

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

La méthode d’action `Index` dans `Manage` Controller définit le message d’État en fonction de votre action précédente et fournit des liens pour modifier votre mot de passe local ou ajouter un compte local. La méthode `Index` affiche également l’État ou votre numéro de téléphone 2FA, les connexions externes, 2FA est activé et mémoriser la méthode 2FA pour ce navigateur (expliquée plus loin). Le fait de cliquer sur votre ID d’utilisateur (e-mail) dans la barre de titre ne transmet pas de message. Cliquer sur le **numéro de téléphone : supprimer** le lien passe `Message=RemovePhoneSuccess` comme chaîne de requête.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

La méthode d’action `AddPhoneNumber` affiche une boîte de dialogue qui permet d’entrer un numéro de téléphone qui peut recevoir des messages SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

En cliquant sur le bouton **Envoyer le code de vérification** , le numéro de téléphone est publié dans la méthode d’action HTTP post `AddPhoneNumber`.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

La méthode `GenerateChangePhoneNumberTokenAsync` génère le jeton de sécurité qui sera défini dans le message SMS. Si le service SMS a été configuré, le jeton est envoyé en tant que chaîne &quot;votre code de sécurité est &lt;jeton&gt;&quot;. La méthode `SmsService.SendAsync` à est appelée de manière asynchrone, puis l’application est redirigée vers la méthode d’action `VerifyPhoneNumber` (qui affiche la boîte de dialogue suivante), où vous pouvez entrer le code de vérification.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Une fois que vous avez entré le code et cliqué sur envoyer, le code est publié dans la méthode d’action HTTP POST `VerifyPhoneNumber`.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

La méthode `ChangePhoneNumberAsync` vérifie le code de sécurité publié. Si le code est correct, le numéro de téléphone est ajouté au champ `PhoneNumber` de la table `AspNetUsers`. Si cet appel réussit, la méthode `SignInAsync` est appelée :

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

Le paramètre `isPersistent` définit si la session d’authentification est rendue persistante entre plusieurs demandes.

Lorsque vous modifiez votre profil de sécurité, un nouveau tampon de sécurité est généré et stocké dans le champ `SecurityStamp` de la table *AspNetUsers* . Notez que le champ `SecurityStamp` est différent du cookie de sécurité. Le cookie de sécurité n’est pas stocké dans la table `AspNetUsers` (ou n’importe où ailleurs dans la base de l’identité). Le jeton de cookie de sécurité est auto-signé à l’aide de [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) et est créé avec les informations de `UserId, SecurityStamp` et d’heure d’expiration.

L’intergiciel de cookies vérifie le cookie à chaque requête. La méthode `SecurityStampValidator` de la classe `Startup` atteint la base de donnée et vérifie périodiquement la présence d’un tampon de sécurité, comme spécifié avec le `validateInterval`. Cela se produit toutes les 30 minutes (dans notre exemple), sauf si vous modifiez votre profil de sécurité. L’intervalle de 30 minutes a été choisi pour limiter les allers-retours vers la base de données.

La méthode `SignInAsync` doit être appelée lorsqu’une modification est apportée au profil de sécurité. Lorsque le profil de sécurité change, la base de données met à jour le champ `SecurityStamp` et sans appeler la méthode `SignInAsync` vous n’êtes connecté *qu'* à la prochaine fois que le pipeline OWIN atteint la base de données (le `validateInterval`). Vous pouvez tester cela en modifiant la méthode `SignInAsync` pour qu’elle soit retournée immédiatement, et en définissant la propriété cookie `validateInterval` de 30 minutes à 5 secondes :

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Une fois le code ci-dessus modifié, vous pouvez modifier votre profil de sécurité (par exemple, en modifiant l’état de l' **option à deux facteurs activés**) et vous serez déconnecté en 5 secondes en cas d’échec de la méthode `SecurityStampValidator.OnValidateIdentity`. Supprimez la ligne de retour de la méthode `SignInAsync`, effectuez une autre modification du profil de sécurité et vous ne serez pas déconnecté. La méthode `SignInAsync` génère un nouveau cookie de sécurité.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Activer l’authentification à deux facteurs

Dans l’exemple d’application, vous devez utiliser l’interface utilisateur pour activer l’authentification à deux facteurs (2FA). Pour activer 2FA, cliquez sur votre ID d’utilisateur (alias de messagerie) dans la barre de navigation.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Cliquez sur Enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Déconnectez-vous, puis reconnectez-vous. Si vous avez activé la messagerie électronique (voir mon [didacticiel précédent](account-confirmation-and-password-recovery-with-aspnet-identity.md)), vous pouvez sélectionner le SMS ou l’E-mail pour 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) La page vérifier le code s’affiche, dans laquelle vous pouvez entrer le code (à partir de SMS ou de courrier électronique).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Si vous cliquez sur la case à cocher **mémoriser ce navigateur** , vous n’avez pas besoin d’utiliser 2FA pour ouvrir une session avec cet ordinateur et ce navigateur. L’activation de 2FA et le simple clic sur **ce navigateur** vous offriront une protection 2FA renforcée contre les utilisateurs malveillants qui essaient d’accéder à votre compte, à condition qu’ils n’aient pas accès à votre ordinateur. Vous pouvez le faire sur n’importe quel ordinateur privé que vous utilisez régulièrement. En définissant **mémoriser ce navigateur**, vous bénéficiez de la sécurité supplémentaire de 2FA à partir des ordinateurs que vous n’utilisez pas régulièrement, et vous avez l’avantage de ne pas devoir passer par 2FA sur vos propres ordinateurs. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Comment inscrire un fournisseur d’authentification à deux facteurs

Lorsque vous créez un projet MVC, le fichier *IdentityConfig.cs* contient le code suivant pour inscrire un fournisseur d’authentification à deux facteurs :

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Ajouter un numéro de téléphone pour 2FA

La méthode d’action `AddPhoneNumber` dans le contrôleur de `Manage` génère un jeton de sécurité et l’envoie au numéro de téléphone que vous avez fourni.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Une fois le jeton envoyé, il est redirigé vers la méthode d’action `VerifyPhoneNumber`, où vous pouvez entrer le code pour enregistrer SMS pour 2FA. SMS 2FA n’est pas utilisé tant que vous n’avez pas vérifié le numéro de téléphone.

## <a name="enabling-2fa"></a>Activation de 2FA

La méthode d’action `EnableTFA` active 2FA :

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Notez que la `SignInAsync` doit être appelée, car Enable 2FA est une modification apportée au profil de sécurité. Quand 2FA est activé, l’utilisateur doit utiliser 2FA pour se connecter, à l’aide des approches 2FA qu’il a inscrites (SMS et e-mail dans l’exemple).

Vous pouvez ajouter d’autres fournisseurs 2FA tels que des générateurs de code QR ou vous pouvez écrire vous-même (consultez [utilisation de Google Authenticator avec ASP.net Identity](https://www.jerriepelser.com//blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Les codes 2FA sont générés à l’aide d' [un algorithme de mot de passe](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) à usage unique et les codes sont valides pendant six minutes. Si vous prenez plus de six minutes pour entrer le code, vous obtiendrez un message d’erreur de code non valide.

<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combiner les comptes de connexion sociale et locale

Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre e-mail. Dans l’ordre suivant &quot;RickAndMSFT@gmail.com&quot; est d’abord créé en tant que connexion locale, mais vous pouvez créer le compte comme journal social dans un premier temps, puis ajouter une connexion locale.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Cliquez sur le lien **gérer** . Notez la valeur 0 externe (connexions sociales) associée à ce compte.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application. Les deux comptes ont été combinés : vous pourrez donc vous connecter avec l'un ou l'autre compte. Vous pouvez permettre à vos utilisateurs d’ajouter des comptes locaux au cas où leur service d’authentification de connexion de réseau social serait indisponible, ou au cas plus probable où ils auraient perdu l’accès à leur compte social.

Dans l’image suivante, Tom est une connexion sociale (que vous pouvez voir à partir des **connexions externes : 1** apparaissant sur la page).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

En cliquant sur **choisir un mot de passe** , vous pouvez ajouter un journal local associé au même compte.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Verrouillage de compte contre les attaques par force brute

Vous pouvez protéger les comptes de votre application contre les attaques par dictionnaire en activant le verrouillage de l’utilisateur. Le code suivant dans la méthode `ApplicationUserManager Create` configure le verrouillage :

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

Le code ci-dessus active le verrouillage pour l’authentification à deux facteurs uniquement. Si vous pouvez activer le verrouillage pour les connexions en remplaçant `shouldLockout` par true dans la méthode `Login` du contrôleur de compte, nous vous recommandons de ne pas activer le verrouillage pour les connexions, car cela rend le compte vulnérable aux attaques de connexion par [déni](http://en.wikipedia.org/wiki/Denial-of-service_attack) de service. Dans l’exemple de code, le verrouillage est désactivé pour le compte administrateur créé dans la méthode `ApplicationDbInitializer Seed` :

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Demander à un utilisateur de disposer d’un compte de messagerie validé

Le code suivant requiert qu’un utilisateur dispose d’un compte de messagerie validé pour pouvoir se connecter :

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Comment SignInManager vérifie la spécification 2FA

La connexion locale et le journal des réseaux sociaux dans vérifient si 2FA est activé. Si 2FA est activé, la méthode de connexion `SignInManager` retourne `SignInStatus.RequiresVerification`, et l’utilisateur est redirigé vers la méthode d’action `SendCode`, où il devra entrer le code pour terminer le journal dans l’ordre. Si l’utilisateur a RememberMe est défini sur le cookie local des utilisateurs, le `SignInManager` retourne `SignInStatus.Success` et il n’est pas obligé de passer par 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

Le code suivant illustre la méthode d’action `SendCode`. Un [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) est créé avec toutes les méthodes 2FA activées pour l’utilisateur. Le [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) est transmis à l’application auxiliaire [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) , qui permet à l’utilisateur de sélectionner l’approche 2FA (en général, la messagerie électronique et SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Une fois que l’utilisateur a publié l’approche 2FA, la méthode d’action `HTTP POST SendCode` est appelée, le `SignInManager` envoie le code 2FA, et l’utilisateur est redirigé vers la méthode d’action `VerifyCode` où il peut entrer le code pour terminer la connexion.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Verrouillage 2FA

Bien que vous puissiez définir le verrouillage de compte lors de tentatives de mot de passe de connexion, cette approche rend votre connexion vulnérable aux verrouillages [dos](http://en.wikipedia.org/wiki/Denial-of-service_attack) . Nous vous recommandons d’utiliser le verrouillage de compte uniquement avec 2FA. Lorsque le `ApplicationUserManager` est créé, l’exemple de code définit le verrouillage 2FA et `MaxFailedAccessAttemptsBeforeLockout` sur cinq. Une fois qu’un utilisateur se connecte (par le biais d’un compte local ou d’un compte social), chaque tentative ayant échoué sur 2FA est stockée, et si le nombre maximal de tentatives est atteint, l’utilisateur est verrouillé pendant cinq minutes (vous pouvez définir le délai de verrouillage avec `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Ressources supplémentaires

- [ASP.net Identity les ressources recommandées](../getting-started/aspnet-identity-recommended-resources.md) Liste complète des blogs, des vidéos, des didacticiels et des liens utiles pour les identités.
- L' [application MVC 5 avec Facebook, Twitter, LinkedIn et l’authentification Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) montre également comment ajouter des informations de profil au tableau utilisateurs.
- [ASP.NET MVC et identity 2,0 : présentation des concepts de base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) de John atten.
- [Confirmation de compte et récupération de mot de passe avec ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introduction à ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Annonce de la version RTM de ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) par Pranav Rastogi.
- [ASP.NET Identity 2,0 : configuration de la validation de compte et de l’autorisation à deux facteurs](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) par John atten.
