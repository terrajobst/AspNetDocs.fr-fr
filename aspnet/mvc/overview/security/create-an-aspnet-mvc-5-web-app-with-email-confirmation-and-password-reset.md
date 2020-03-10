---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation de mot de passe (C#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 avec confirmation par courrier électronique et réinitialisation de mot de passe en utilisant le système d’appartenance d'ASP.NET Identity. Ca...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 6169c972ad0f4ee2079d3638c54a5accc4b8b3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538345"
---
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par courrier électronique et réinitialisation de mot de passe (C#)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 avec confirmation par courrier électronique et réinitialisation de mot de passe en utilisant le système d’appartenance d'ASP.NET Identity.

Pour obtenir une version mise à jour de ce didacticiel qui utilise .NET Core, consultez [confirmation de compte et récupération de mot de passe dans ASP.net Core](/aspnet/core/security/authentication/accconfirm).

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Créer une application MVC ASP.NET

Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installez [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure.

> [!NOTE]
> AVERTISSEMENT : vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou version ultérieure pour suivre ce didacticiel.

1. Créez un projet Web ASP.NET et sélectionnez le modèle MVC. Web Forms prend également en charge ASP.NET Identity : vous pouvez donc suivre des étapes similaires dans une application Web Forms.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Laissez l’authentification par défaut en tant que **comptes d’utilisateur individuels**. Si vous souhaitez héberger l’application dans Azure, laissez la case à cocher activée. Plus loin dans ce didacticiel, nous déploierons sur Azure. Vous pouvez [ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Définissez le [projet pour utiliser SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Exécutez l’application, cliquez sur le lien **Register** et inscrivez un utilisateur. À ce stade, la seule validation sur l’e-mail est avec l’attribut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
5. Dans Explorateur de serveurs, accédez à **Data Connections\DefaultConnection\Tables\AspNetUsers**, cliquez avec le bouton droit et sélectionnez **ouvrir la définition de table**.

    L’illustration suivante montre le schéma de `AspNetUsers` :

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Cliquez avec le bouton droit sur la table **AspNetUsers** et sélectionnez **afficher les données**de la table.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 À ce stade, l’adresse de messagerie n’a pas été confirmée.
7. Cliquez sur la ligne et sélectionnez Supprimer. Vous allez ajouter cet e-mail à l’étape suivante et envoyer un e-mail de confirmation.

## <a name="email-confirmation"></a>Confirmation par e-mail

Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur pour vérifier qu’ils n’empruntent pas l'identité d'une autre personne (autrement dit, qu'ils ne se sont pas inscrits avec par l'adresse de messagerie de quelqu'un d’autre). Supposons que vous disposiez d’un forum de discussion, vous souhaitiez empêcher `"bob@example.com"` de vous inscrire en tant que `"joe@contoso.com"`. Sans confirmation par courrier électronique, `"joe@contoso.com"` pouvez obtenir un courrier indésirable à partir de votre application. Supposons que Bob s’est inscrit accidentellement comme `"bib@example.com"` et n’avait pas remarqué qu’il ne serait pas en mesure d’utiliser la récupération de mot de passe parce que l’application n’a pas son adresse e-mail correcte. La confirmation par courrier électronique fournit uniquement une protection limitée des robots et ne fournit pas de protection contre les spammeurs déterminés. ils ont de nombreux alias de messagerie de travail qu’ils peuvent utiliser pour s’inscrire.

En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web avant qu’ils soient confirmés par courrier électronique, par SMS ou via un autre mécanisme. <a id="build"></a>Dans les sections ci-dessous, nous allons activer la confirmation par courrier électronique et modifier le code pour empêcher les utilisateurs nouvellement inscrits de se connecter jusqu’à ce que leur adresse de messagerie soit confirmée.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Se connecter à SendGrid

Les instructions de cette section ne sont pas à jour. Pour obtenir des instructions mises à jour, consultez [configurer le fournisseur de messagerie SendGrid](/aspnet/core/security/authentication/accconfirm#configure-email-provider) .

Bien que ce didacticiel montre uniquement comment ajouter des notifications par courrier électronique via [SendGrid](http://sendgrid.com/), vous pouvez envoyer des e-mails à l’aide de SMTP et d’autres mécanismes (voir [ressources supplémentaires](#addRes)).

1. Dans la Console du Gestionnaire de Package, entrez la commande suivante : 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Accédez à la [page d’inscription à Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) et inscrivez-vous pour obtenir un compte SendGrid gratuit. Configurez SendGrid en ajoutant un code semblable au suivant dans *App_Start/identityconfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Vous devrez ajouter les inclusions suivantes :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Pour simplifier cet exemple, nous allons stocker les paramètres de l’application dans le fichier *Web. config* :

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Sécurité - Ne jamais stocker de données sensibles dans votre code source. Le compte et les informations d’identification sont stockés dans l’appSetting. Sur Azure, vous pouvez stocker ces valeurs en toute sécurité sous l’onglet **[configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** de la portail Azure. Consultez [meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles dans ASP.net et Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

### <a name="enable-email-confirmation-in-the-account-controller"></a>Activer la confirmation par courrier électronique dans le contrôleur de compte

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Vérifiez que le fichier *Views\Account\ConfirmEmail.cshtml* a une syntaxe Razor correcte. (Le caractère @ dans la première ligne est peut-être manquant. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Exécutez l’application et cliquez sur le lien S’inscrire. Une fois que vous avez envoyé le formulaire d’inscription, vous êtes connecté.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Vérifiez votre compte de messagerie et cliquez sur le lien pour confirmer votre adresse de messagerie.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Demander une confirmation par courrier électronique avant la connexion

Actuellement, une fois qu’un utilisateur complète le formulaire d’inscription, il est connecté. En règle générale, vous voulez vérifier son adresse e-mail avant de le connecter. Dans la section ci-dessous, nous modifions le code pour demander aux nouveaux utilisateurs un e-mail vérifié avant de les connecter (les authentifier). Mettez à jour la méthode `HttpPost Register` avec les modifications en surbrillance suivantes :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

En commentant la méthode `SignInAsync`, l’utilisateur n’est pas connecté par l’inscription. La ligne de `TempData["ViewBagLink"] = callbackUrl;` peut être utilisée pour [déboguer l’inscription de l’application](#dbg) et tester l’inscription sans envoyer de courrier électronique. `ViewBag.Message` est utilisé pour afficher les instructions Confirm. L' [exemple de téléchargement](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contient du code pour tester la confirmation de l’e-mail sans configurer la messagerie électronique et peut également être utilisé pour déboguer l’application.

Créez un fichier `Views\Shared\Info.cshtml` et ajoutez le balisage Razor suivant :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Ajoutez l' [attribut Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) à la méthode d’action `Contact` du contrôleur d’hébergement. Vous pouvez cliquer sur le lien de **contact** pour vérifier que les utilisateurs anonymes n’ont pas d’accès et que les utilisateurs authentifiés ont accès.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Vous devez également mettre à jour le `HttpPost Login` méthode d’action :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Mettez à jour la vue *Views\Shared\Error.cshtml* pour afficher le message d’erreur :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Supprimez les comptes de la table **AspNetUsers** qui contiennent l’alias de messagerie avec lequel vous souhaitez effectuer le test. Exécutez l’application et vérifiez que vous ne pouvez pas vous connecter avant d’avoir confirmé votre adresse e-mail. Une fois que vous avez confirmé votre adresse de messagerie, cliquez sur le lien du **contact** .

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Récupération/réinitialisation du mot de passe

Supprimez les caractères de commentaire de la méthode d’action `HttpPost ForgotPassword` dans le contrôleur de compte :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Supprimez les caractères de commentaire du `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) dans le fichier de vue Razor *Views\Account\Login.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

La page de connexion affiche alors un lien pour réinitialiser le mot de passe.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Renvoyer le lien de confirmation par e-mail

Une fois qu’un utilisateur crée un compte local, un lien de confirmation lui est envoyé par e-mail, qu’il doit utiliser avant de pouvoir se connecter. Si l’utilisateur supprime accidentellement l’e-mail de confirmation ou si l’e-mail n’arrive jamais, il a besoin du lien de confirmation renvoyé. Les modifications suivantes du code montrent comment faire cela.

Ajoutez la méthode d’assistance suivante au bas du fichier *Controllers\AccountController.cs* :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Mettez à jour la méthode Register de façon à utiliser le nouveau helper :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Mettez à jour la méthode Login de façon à renvoyer le mot de passe si le compte d’utilisateur n’a pas été confirmé :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combiner les comptes de connexion sociale et locale

Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre e-mail. Dans l’ordre suivant **RickAndMSFT@gmail.com** est créé en tant que connexion locale, mais vous pouvez créer le compte en tant que journal des réseaux sociaux dans un premier temps, puis ajouter une connexion locale.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Cliquez sur le lien **gérer** . Notez les **connexions externes : 0** associées à ce compte.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application. Les deux comptes ont été combinés : vous pourrez donc vous connecter avec l'un ou l'autre compte. Vous pouvez permettre à vos utilisateurs d’ajouter des comptes locaux au cas où leur service d’authentification de connexion de réseau social serait indisponible, ou au cas plus probable où ils auraient perdu l’accès à leur compte social.

Dans l’image suivante, Tom est une connexion sociale (que vous pouvez voir à partir des **connexions externes : 1** apparaissant sur la page).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

En cliquant sur **choisir un mot de passe** , vous pouvez ajouter un journal local associé au même compte.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Confirmation de l’e-mail plus en détail

La confirmation de mon [compte et la récupération de mot de passe avec ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) vont plus en détail dans cette rubrique.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Débogage de l’application

Si vous ne recevez pas d’e-mail contenant le lien :

- Vérifiez votre dossier des courriers indésirables.
- Connectez-vous à votre compte SendGrid et cliquez sur le [lien activité de messagerie](https://sendgrid.com/logs/index).

Pour tester le lien de vérification sans courrier électronique, téléchargez l' [exemple terminé](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Le lien de confirmation et les codes de confirmation seront affichés dans la page.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Liens vers les ressources ASP.NET Identity recommandées](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmation de compte et récupération de mot de passe avec ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Pour plus d’informations sur la récupération du mot de passe et la confirmation du compte.
- [Application MVC 5 avec l’authentification Facebook, Twitter, LinkedIn et Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ce didacticiel vous montre comment écrire une application ASP.NET MVC 5 avec l’autorisation Facebook et Google OAuth 2. Il montre également comment ajouter des données supplémentaires à la base de données Identity.
- [Déployez une application ASP.NET MVC sécurisée avec appartenance, OAuth et SQL Database à Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ce tutoriel ajoute le déploiement Azure, la sécurisation de votre application avec des rôles, l’utilisation de l’API d’appartenance pour ajouter des utilisateurs et des rôles, et des fonctionnalités de sécurité supplémentaires.
- [Création d’une application Google pour OAuth 2 et connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Création de l’application dans Facebook et connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configuration de SSL dans le projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
