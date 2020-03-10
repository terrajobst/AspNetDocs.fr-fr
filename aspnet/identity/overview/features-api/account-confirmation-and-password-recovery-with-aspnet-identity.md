---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Confirmation de compte & récupération du mot deC#passe-ASP.net Identity ()-ASP.net 4. x
author: HaoK
description: Avant de suivre ce didacticiel, vous devez d’abord créer une application Web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation du mot de passe. Ce didacticiel...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4b2c88280df39aa81d60f9508910e8fe5d6db6b8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616815"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Confirmation de compte et récupération de mot deC#passe avec ASP.net Identity ()

> Avant de suivre ce didacticiel, vous devez [d’abord créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation du mot de passe](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Ce didacticiel contient plus de détails et vous indique comment configurer la messagerie électronique pour la confirmation du compte local et permettre aux utilisateurs de réinitialiser leur mot de passe oublié dans ASP.NET Identity.

Un compte d’utilisateur local oblige l’utilisateur à créer un mot de passe pour le compte et ce mot de passe est stocké (en toute sécurité) dans l’application Web. ASP.NET Identity prend également en charge les comptes sociaux, qui ne nécessitent pas que l’utilisateur crée un mot de passe pour l’application. Les [comptes sociaux](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) utilisent un tiers (par exemple, Google, Twitter, Facebook ou Microsoft) pour authentifier les utilisateurs. Cette rubrique couvre les sujets suivants :

- [Créez une application MVC ASP.net](#createMvc) et explorez les fonctionnalités de ASP.net Identity.
- [Générer l’exemple Identity](#build)
- [Configurer la confirmation par e-mail](#email)

Les nouveaux utilisateurs inscrivent leur alias de messagerie, ce qui crée un compte local.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Le fait de sélectionner le bouton inscrire envoie un e-mail de confirmation contenant un jeton de validation à son adresse de messagerie.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

L’utilisateur reçoit un e-mail avec un jeton de confirmation pour son compte.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

La sélection du lien confirme le compte.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Récupération/réinitialisation du mot de passe

Les utilisateurs locaux qui oublient leur mot de passe peuvent avoir un jeton de sécurité envoyé à leur compte de messagerie, ce qui leur permet de réinitialiser leur mot de passe.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
L’utilisateur recevra bientôt un e-mail contenant un lien leur permettant de réinitialiser leur mot de passe.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Si vous sélectionnez le lien, la page de réinitialisation est redéfinie.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Le fait de sélectionner le bouton **Réinitialiser** confirme que le mot de passe a été réinitialisé.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Créer une application web ASP.NET

Commencez par installer et exécuter [Visual Studio 2017](https://visualstudio.microsoft.com/).

1. Créez un projet Web ASP.NET et sélectionnez le modèle MVC. Web Forms également prendre en charge ASP.NET Identity, vous pouvez suivre des étapes similaires dans une application Web Forms.
2. Modifiez l’authentification en **comptes d’utilisateur individuels**.
3. Exécutez l’application, sélectionnez le lien **Register** et inscrivez un utilisateur. À ce stade, la seule validation sur l’e-mail est avec l’attribut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
4. Dans Explorateur de serveurs, accédez à **Data Connections\DefaultConnection\Tables\AspNetUsers**, cliquez avec le bouton droit et sélectionnez **ouvrir la définition de table**.

    L’illustration suivante montre le schéma de `AspNetUsers` :

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Cliquez avec le bouton droit sur la table **AspNetUsers** et sélectionnez **afficher les données**de la table.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   À ce stade, l’adresse de messagerie n’a pas été confirmée.

Le magasin de données par défaut pour ASP.NET Identity est Entity Framework, mais vous pouvez le configurer pour utiliser d’autres magasins de données et ajouter des champs supplémentaires. Consultez la section [ressources supplémentaires](#addRes) à la fin de ce didacticiel.

La [classe de démarrage OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) est appelée lorsque l’application démarre et appelle la méthode `ConfigureAuth` dans l' *application\_Start\Startup.auth.cs*, qui configure le pipeline OWIN et initialise ASP.net Identity. Examinez la méthode `ConfigureAuth`. Chaque appel de `CreatePerOwinContext` inscrit un rappel (enregistré dans le `OwinContext`) qui sera appelé une fois par demande pour créer une instance du type spécifié. Vous pouvez définir un point d’arrêt dans le constructeur et `Create` méthode de chaque type (`ApplicationDbContext, ApplicationUserManager`) et vérifier qu’ils sont appelés à chaque requête. Une instance de `ApplicationDbContext` et `ApplicationUserManager` est stockée dans le contexte OWIN, accessible dans l’ensemble de l’application. ASP.NET Identity des hooks dans le pipeline OWIN via l’intergiciel de cookies. Pour plus d’informations, consultez Gestion de la [durée de vie des demandes pour la classe usermanager dans ASP.net Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Lorsque vous modifiez votre profil de sécurité, un nouveau tampon de sécurité est généré et stocké dans le champ `SecurityStamp` de la table *AspNetUsers* . Notez que le champ `SecurityStamp` est différent du cookie de sécurité. Le cookie de sécurité n’est pas stocké dans la table `AspNetUsers` (ou n’importe où ailleurs dans la base de l’identité). Le jeton de cookie de sécurité est auto-signé à l’aide de [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) et est créé avec les informations de `UserId, SecurityStamp` et d’heure d’expiration.

L’intergiciel de cookies vérifie le cookie à chaque requête. La méthode `SecurityStampValidator` de la classe `Startup` atteint la base de donnée et vérifie périodiquement la présence d’un tampon de sécurité, comme spécifié avec le `validateInterval`. Cela se produit toutes les 30 minutes (dans notre exemple), sauf si vous modifiez votre profil de sécurité. L’intervalle de 30 minutes a été choisi pour limiter les allers-retours vers la base de données. Pour plus d’informations, consultez [le didacticiel sur l’authentification à deux facteurs](index.md) .

En fonction des commentaires du code, la méthode `UseCookieAuthentication` prend en charge l’authentification par cookie. Le champ `SecurityStamp` et le code associé fournissent une couche supplémentaire de sécurité à votre application. quand vous modifiez votre mot de passe, vous êtes déconnecté du navigateur avec lequel vous vous êtes connecté. La méthode `SecurityStampValidator.OnValidateIdentity` permet à l’application de valider le jeton de sécurité lorsque l’utilisateur se connecte, ce qui est utilisé lorsque vous modifiez un mot de passe ou utilisez la connexion externe. Cela est nécessaire pour garantir que tous les jetons (cookies) générés avec l’ancien mot de passe sont invalidés. Dans l’exemple de projet, si vous modifiez le mot de passe des utilisateurs, un nouveau jeton est généré pour l’utilisateur, tous les jetons précédents sont invalidés et le champ `SecurityStamp` est mis à jour.

Le système d’identité vous permet de configurer votre application de sorte que le profil de sécurité des utilisateurs change (par exemple, lorsque l’utilisateur modifie son mot de passe ou modifie la connexion associée (par exemple, à partir de Facebook, Google, compte Microsoft, etc.), l’utilisateur se déconnecte de tous les instances de navigateur. Par exemple, l’image ci-dessous montre l’exemple d’application [SignOut unique](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) , qui permet à l’utilisateur de se déconnecter de toutes les instances de navigateur (dans ce cas, IE, Firefox et chrome) en sélectionnant un bouton. L’exemple vous permet également de vous déconnecter d’une instance de navigateur spécifique.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

L' [exemple d’application SignOut unique](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) montre comment ASP.net Identity vous permet de régénérer le jeton de sécurité. Cela est nécessaire pour garantir que tous les jetons (cookies) générés avec l’ancien mot de passe sont invalidés. Cette fonctionnalité fournit une couche supplémentaire de sécurité à votre application. Lorsque vous modifiez votre mot de passe, vous êtes déconnecté de l’emplacement où vous vous êtes connecté à cette application.

Le fichier *App\_Start\IdentityConfig.cs* contient les classes `ApplicationUserManager`, `EmailService` et `SmsService`. Les classes `EmailService` et `SmsService` implémentent chacune l’interface `IIdentityMessageService`. par conséquent, vous disposez de méthodes communes pour chaque classe afin de configurer la messagerie électronique et SMS. Bien que ce didacticiel montre uniquement comment ajouter des notifications par courrier électronique via [SendGrid](http://sendgrid.com/), vous pouvez envoyer des e-mails à l’aide de SMTP et d’autres mécanismes.

La classe `Startup` contient également une plaque de chaudière pour ajouter des connexions sociales (Facebook, Twitter, etc.). pour plus d’informations, consultez mon didacticiel [application MVC 5 avec Facebook, Twitter, LinkedIn et l’authentification Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) .

Examinez la classe `ApplicationUserManager`, qui contient les informations d’identité des utilisateurs et configure les fonctionnalités suivantes :

- Exigences de la force du mot de passe.
- Verrouillage de l’utilisateur (tentatives et temps).
- Authentification à deux facteurs (2FA). Je vais aborder 2FA et SMS dans un autre didacticiel.
- Raccordement de l’e-mail et des services SMS. (Je couvrirai SMS dans un autre didacticiel).

La classe `ApplicationUserManager` dérive de la classe `UserManager<ApplicationUser>` générique. `ApplicationUser` dérive de [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` dérive de la classe `IdentityUser` générique :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Les propriétés ci-dessus coïncident avec les propriétés de la table `AspNetUsers`, comme indiqué ci-dessus.

Les arguments génériques sur les `IUser` vous permettent de dériver une classe à l’aide de différents types pour la clé primaire. Consultez l’exemple [ChangePK](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) qui montre comment modifier la clé primaire de String en int ou de GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) est défini dans *Models\IdentityModels.cs* comme suit :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Le code en surbrillance ci-dessus génère un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity et l’authentification des cookies OWIN sont basées sur les revendications. par conséquent, l’infrastructure requiert que l’application génère un `ClaimsIdentity` pour l’utilisateur. `ClaimsIdentity` contient des informations sur toutes les revendications pour l’utilisateur, telles que le nom de l’utilisateur, l’âge et les rôles auxquels l’utilisateur appartient. Vous pouvez également ajouter d’autres revendications pour l’utilisateur à ce niveau.

La méthode OWIN `AuthenticationManager.SignIn` transmet le `ClaimsIdentity` et se connecte à l’utilisateur :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

L' [application MVC 5 avec Facebook, Twitter, LinkedIn et l’authentification Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) montre comment vous pouvez ajouter des propriétés supplémentaires à la classe `ApplicationUser`.

## <a name="email-confirmation"></a>Confirmation par e-mail

Il est judicieux de confirmer l’adresse de messagerie à laquelle un nouvel utilisateur s’inscrit afin de vérifier qu’il n’emprunte pas l’identité d’un autre utilisateur (autrement dit, s’il ne s’est pas inscrit auprès de l’e-mail de quelqu’un d’autre). Supposons que vous disposiez d’un forum de discussion, vous souhaitiez empêcher `"bob@example.com"` de vous inscrire en tant que `"joe@contoso.com"`. Sans confirmation par courrier électronique, `"joe@contoso.com"` pouvez obtenir un courrier indésirable à partir de votre application. Supposons que Bob s’est inscrit accidentellement comme `"bib@example.com"` et n’avait pas remarqué qu’il ne serait pas en mesure d’utiliser la récupération de mot de passe parce que l’application n’a pas son adresse e-mail correcte. La confirmation par courrier électronique fournit uniquement une protection limitée des robots et ne fournit pas de protection contre les spammeurs déterminés. ils ont de nombreux alias de messagerie de travail qu’ils peuvent utiliser pour s’inscrire. Dans l’exemple ci-dessous, l’utilisateur ne peut pas modifier son mot de passe tant que son compte n’a pas été confirmé (en sélectionnant un lien de confirmation reçu sur le compte de messagerie qu’il a inscrit avec). Vous pouvez appliquer ce processus à d’autres scénarios, par exemple en envoyant un lien pour confirmer et réinitialiser le mot de passe sur les nouveaux comptes créés par l’administrateur, en envoyant un e-mail à l’utilisateur lorsqu’il a modifié son profil, et ainsi de suite. En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web avant qu’ils soient confirmés par courrier électronique, par SMS ou via un autre mécanisme. <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>Créer un exemple plus complet

Dans cette section, vous allez utiliser NuGet pour télécharger un exemple plus complet avec lequel nous allons travailler.

1. Créez un projet Web ASP.NET ***vide*** .
2. Dans la console du gestionnaire de package, entrez les commandes suivantes : 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   Dans ce didacticiel, nous allons utiliser [SendGrid](http://sendgrid.com/) pour envoyer des courriers électroniques. Le package `Identity.Samples` installe le code que nous allons utiliser.
3. Définissez le [projet pour utiliser SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Testez la création du compte local en exécutant l’application, en sélectionnant le lien **Register** et en publiant le formulaire d’inscription.
5. Sélectionnez le lien e-mail de démonstration, qui simule la confirmation de l’e-mail.
6. Supprimez le code de confirmation de la liaison par e-mail de démonstration de l’exemple (le code `ViewBag.Link` dans le contrôleur de compte. Consultez les méthodes d’action `DisplayEmail` et `ForgotPasswordConfirmation` et les vues Razor).

> [!WARNING]
> Si vous modifiez l’un des paramètres de sécurité de cet exemple, les applications de production doivent subir un audit de sécurité qui appelle explicitement les modifications apportées.

## <a name="examine-the-code-in-app_startidentityconfigcs"></a>Examinez le code dans l’application\_Start\IdentityConfig.cs

L’exemple montre comment créer un compte et l’ajouter au rôle d' *administrateur* . Vous devez remplacer l’adresse de messagerie dans l’exemple par le courrier électronique que vous allez utiliser pour le compte d’administrateur. La méthode la plus simple pour créer un compte d’administrateur est par programmation dans la méthode `Seed`. Nous espérons avoir un outil à l’avenir qui vous permettra de créer et d’administrer des utilisateurs et des rôles. L’exemple de code vous permet de créer et de gérer des utilisateurs et des rôles, mais vous devez d’abord disposer d’un compte administrateurs pour exécuter les pages rôles et administrateur d’utilisateur. Dans cet exemple, le compte d’administrateur est créé lorsque la base de code est amorcée.

Modifiez le mot de passe et remplacez le nom par un compte dans lequel vous pouvez recevoir des notifications par courrier électronique.

> [!WARNING]
> Sécurité - Ne jamais stocker de données sensibles dans votre code source.

Comme mentionné précédemment, l’appel de `app.CreatePerOwinContext` dans la classe Startup ajoute des rappels à la méthode `Create` du contenu de la base de données de l’application, au gestionnaire des utilisateurs et aux classes de gestionnaires de rôles. Le pipeline OWIN appelle la méthode `Create` sur ces classes pour chaque requête et stocke le contexte pour chaque classe. Le contrôleur de compte expose le gestionnaire des utilisateurs à partir du contexte HTTP (qui contient le contexte OWIN) :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Lorsqu’un utilisateur inscrit un compte local, la méthode `HTTP Post Register` est appelée :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Le code ci-dessus utilise les données du modèle pour créer un nouveau compte d’utilisateur à l’aide de l’adresse de messagerie et du mot de passe entrés. Si l’alias de messagerie se trouve dans le magasin de données, la création du compte échoue et le formulaire s’affiche à nouveau. La méthode `GenerateEmailConfirmationTokenAsync` crée un jeton de confirmation sécurisé et le stocke dans le magasin de données ASP.NET Identity. La méthode [URL. action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) crée un lien contenant le `UserId` et le jeton de confirmation. Ce lien est ensuite envoyé par courrier électronique à l’utilisateur, l’utilisateur peut sélectionner le lien dans son application de messagerie pour confirmer son compte.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Configurer la confirmation par e-mail

Accédez à la [page d’inscription à Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) et inscrivez-vous pour obtenir un compte gratuit. Ajoutez du code semblable au suivant pour configurer SendGrid :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Les clients de messagerie acceptent souvent uniquement les messages texte (pas de code HTML). Vous devez fournir le message au format texte et HTML. Dans l’exemple SendGrid ci-dessus, cette opération est effectuée avec le `myMessage.Text` et `myMessage.Html` code indiqué ci-dessus.

Le code suivant montre comment envoyer un e-mail à l’aide de la classe [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) où `message.Body` retourne uniquement le lien.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Sécurité - Ne jamais stocker de données sensibles dans votre code source. Le compte et les informations d’identification sont stockés dans l’appSetting. Sur Azure, vous pouvez stocker ces valeurs en toute sécurité sous l’onglet **[configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** de la portail Azure. Consultez [meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles dans ASP.net et Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

Entrez vos informations d’identification SendGrid, exécutez l’application, inscrivez-vous avec un alias de messagerie vous pouvez sélectionner le lien confirmer dans votre e-mail. Pour savoir comment faire avec votre compte de messagerie [Outlook.com](http://outlook.com) , consultez Configuration SMTP de John atten [ C# pour l’hôte SMTP Outlook.com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) et son[ASP.net Identity 2,0 : configuration de la validation de compte et des publications d’autorisation à deux facteurs](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) .

Une fois qu’un utilisateur a sélectionné le bouton **inscrire** , un e-mail de confirmation contenant un jeton de validation est envoyé à son adresse de messagerie.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

L’utilisateur reçoit un e-mail avec un jeton de confirmation pour son compte.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Examiner le code

Le code suivant montre la méthode `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

La méthode échoue en silence si la messagerie de l’utilisateur n’a pas été confirmée. Si une erreur a été publiée pour une adresse de messagerie non valide, des utilisateurs malveillants pouvaient utiliser ces informations pour trouver des ID d’utilisateur (alias de messagerie) valides pour attaquer.

Le code suivant montre la méthode `ConfirmEmail` dans le contrôleur de compte qui est appelée lorsque l’utilisateur sélectionne le lien de confirmation dans le message électronique qui lui est envoyé :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Une fois qu’un jeton de mot de passe oublié a été utilisé, il est invalidé. La modification de code suivante dans la méthode `Create` (dans le fichier *App\_Start\IdentityConfig.cs* ) définit les jetons pour qu’ils expirent dans 3 heures.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Avec le code ci-dessus, le mot de passe oublié et les jetons de confirmation par courrier électronique expirent dans 3 heures. La `TokenLifespan` par défaut est d’un jour.

Le code suivant illustre la méthode de confirmation par courrier électronique :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Pour renforcer la sécurité de votre application, ASP.NET Identity prend en charge l’authentification à deux facteurs (2FA). Consultez [ASP.NET Identity 2,0 : configuration de la validation de compte et de l’autorisation à deux facteurs](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) par John atten. Bien que vous puissiez définir le verrouillage de compte lors de tentatives de mot de passe de connexion, cette approche rend votre connexion vulnérable aux verrouillages [dos](http://en.wikipedia.org/wiki/Denial-of-service_attack) . Nous vous recommandons d’utiliser le verrouillage de compte uniquement avec 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Ressources supplémentaires

- [Vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- L' [application MVC 5 avec Facebook, Twitter, LinkedIn et l’authentification Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) montre également comment ajouter des informations de profil au tableau utilisateurs.
- [ASP.NET MVC et identity 2,0 : présentation des concepts de base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) de John atten.
- [Introduction à ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Annonce de la version RTM de ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) par Pranav Rastogi.
