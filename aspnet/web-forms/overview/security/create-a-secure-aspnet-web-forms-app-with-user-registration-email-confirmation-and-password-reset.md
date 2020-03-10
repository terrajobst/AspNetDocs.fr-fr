---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Créer une application ASP.NET Web Forms sécurisée avec inscription de l’utilisateur, confirmation par e-mailC#et réinitialisation du mot de passe () | Microsoft Docs
author: Erikre
description: Ce didacticiel vous montre comment créer une application ASP.NET Web Forms avec l’inscription de l’utilisateur, la confirmation par courrier électronique et la réinitialisation du mot de passe à l’aide du membre ASP.NET Identity...
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: af3653bc164810126bc3bf8f1b1794d75642d807
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625152"
---
# <a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Créer une application Web Forms ASP.NET sécurisée avec inscription de l’utilisateur, confirmation par e-mail et réinitialisation du mot de passe (C#)

par [Erik Reitan](https://github.com/Erikre)

> Ce didacticiel vous montre comment créer une application ASP.NET Web Forms avec l’inscription de l’utilisateur, la confirmation par courrier électronique et la réinitialisation du mot de passe à l’aide du système d’appartenance ASP.NET Identity. Ce didacticiel est basé sur le [didacticiel MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)de Rick Anderson.

## <a name="introduction"></a>Introduction

Ce didacticiel vous guide tout au long des étapes nécessaires à la création d’une application ASP.NET Web Forms à l’aide de Visual Studio et ASP.NET 4,5 pour créer une application Web Forms sécurisée avec l’inscription de l’utilisateur, la confirmation par e-mail et la réinitialisation du mot de passe.

### <a name="tutorial-tasks-and-information"></a>Tâches et informations sur le didacticiel :

- [Créer une application de Web Forms ASP.NET](#createWebForms)
- [Raccorder SendGrid](#SG)
- [Exiger une confirmation par courrier électronique avant la connexion](#require)
- [Récupération et réinitialisation du mot de passe](#reset)
- [Renvoyer le lien de confirmation par E-mail](#rsend)
- [Résolution des problèmes de l’application](#dbg)
- [Ressources supplémentaires pour MSBuild](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Créer une application de Web Forms ASP.NET

Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installez [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure.

> [!NOTE]
> AVERTISSEMENT : vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou version ultérieure pour suivre ce didacticiel.

1. Créez un projet (**fichier** -&gt; **nouveau projet**), puis sélectionnez le modèle d' **application Web ASP.net** et la dernière version de .NET Framework dans la boîte de dialogue **nouveau projet** .
2. Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **Web Forms** . Laissez l’authentification par défaut en tant que **comptes d’utilisateur individuels**. Si vous souhaitez héberger l’application dans Azure, activez la case à cocher **hôte dans le Cloud** .   
 Cliquez ensuite sur **OK** pour créer le nouveau projet.  
    ![Boîte de dialogue New ASP.NET Project](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Activez protocole SSL (SSL) pour le projet. Suivez les étapes disponibles dans la section **activer SSL pour le projet** de la [série prise en main avec Web Forms didacticiel](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Exécutez l’application, cliquez sur le lien **Register** et inscrivez un nouvel utilisateur. À ce stade, la seule validation sur l’e-mail est basée sur l’attribut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) pour garantir que l’adresse de messagerie est bien formée. Vous allez modifier le code pour ajouter une confirmation par courrier électronique. Fermez la fenêtre du navigateur.
5. Dans **Explorateur de serveurs** de Visual Studio (**afficher** -&gt; **Explorateur de serveurs**), accédez à **Data Connections\DefaultConnection\Tables\AspNetUsers**, cliquez avec le bouton droit et sélectionnez **ouvrir la définition de table**. 

    L’illustration suivante montre le schéma de la table `AspNetUsers` :

    ![Schéma de la table AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. Dans **Explorateur de serveurs**, cliquez avec le bouton droit sur la table **AspNetUsers** et sélectionnez **afficher les données**de la table.  
  
    ![Données de la table AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 À ce stade, l’e-mail de l’utilisateur inscrit n’a pas été confirmé.
7. Cliquez sur la ligne et sélectionnez Supprimer pour supprimer l’utilisateur. Vous ajouterez à nouveau cet e-mail à l’étape suivante et envoyerez un message de confirmation à l’adresse de messagerie.

## <a name="email-confirmation"></a>Confirmation par e-mail

Il est recommandé de confirmer l’e-mail lors de l’inscription d’un nouvel utilisateur afin de vérifier qu’il n’emprunte pas l’identité d’un autre utilisateur (autrement dit, s’il ne s’est pas inscrit auprès de l’e-mail de quelqu’un d’autre). Supposons que vous disposiez d’un forum de discussion, vous souhaitiez empêcher `"bob@cpandl.com"` de vous inscrire en tant que `"joe@contoso.com"`. Sans confirmation par courrier électronique, `"joe@contoso.com"` pouvez obtenir un courrier indésirable à partir de votre application. Supposons que Bob s’est inscrit accidentellement comme `"bib@cpandl.com"` et n’avait pas remarqué qu’il ne serait pas en mesure d’utiliser la récupération de mot de passe parce que l’application n’a pas son adresse e-mail correcte. La confirmation par courrier électronique fournit uniquement une protection limitée des robots et ne fournit pas de protection contre les spammeurs déterminés.

En général, vous souhaitez empêcher les nouveaux utilisateurs de publier des données sur votre site Web avant qu’ils n’aient été confirmés par e-mail, par courrier électronique ou par un autre mécanisme. Dans les sections ci-dessous, nous allons activer la confirmation par courrier électronique et modifier le code pour empêcher les utilisateurs nouvellement inscrits de se connecter jusqu’à ce que leur adresse de messagerie soit confirmée. Vous utiliserez le service de messagerie SendGrid dans ce didacticiel.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Se connecter à SendGrid

SendGrid a changé son API depuis que ce didacticiel a été écrit. Pour obtenir les instructions SendGrid actuelles, consultez [SendGrid](http://sendgrid.com/) ou [activer la confirmation de compte et la récupération de mot de passe](xref:security/authentication/accconfirm#enable-account-confirmation-and-password-recovery).

Bien que ce didacticiel montre uniquement comment ajouter des notifications par courrier électronique via [SendGrid](http://sendgrid.com/), vous pouvez envoyer des e-mails à l’aide de SMTP et d’autres mécanismes (voir [ressources supplémentaires](#addRes)).

1. Dans Visual Studio, ouvrez la **console du gestionnaire de package** (**Outils** -&gt; gestionnaire de **package NuGet** -&gt; **console du gestionnaire de package**), puis entrez la commande suivante :  
    `Install-Package SendGrid`
2. Accédez à la [page d’inscription à Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) et inscrivez-vous pour obtenir un compte SendGrid gratuit. Vous pouvez également vous inscrire pour obtenir un compte SendGrid gratuit directement sur le [site de SendGrid](http://www.sendgrid.com).
3. À partir de **Explorateur de solutions** Ouvrez le fichier *IdentityConfig.cs* dans le dossier de démarrage de l' *application\_* et ajoutez le code suivant mis en surbrillance en jaune à la classe `EmailService` pour configurer **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Ajoutez également les instructions `using` suivantes au début du fichier *IdentityConfig.cs* : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Pour simplifier cet exemple, vous allez stocker les valeurs du compte de service de messagerie dans la section `appSettings` du fichier *Web. config* . Ajoutez le code XML suivant mis en surbrillance en jaune au fichier *Web. config* à la racine de votre projet :

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Sécurité - Ne jamais stocker de données sensibles dans votre code source. Dans cet exemple, le compte et les informations d’identification sont stockés dans la section **appSetting** du fichier *Web. config* . Sur Azure, vous pouvez stocker ces valeurs en toute sécurité sous l’onglet **[configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** de la portail Azure. Pour obtenir des informations connexes, consultez la rubrique de Rick Anderson intitulée [meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles dans ASP.net et Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Ajoutez les valeurs du service de messagerie pour refléter vos valeurs d’authentification SendGrid (nom d’utilisateur et mot de passe) afin de pouvoir envoyer des messages électroniques à partir de votre application. Veillez à utiliser le nom de votre compte SendGrid plutôt que l’adresse de messagerie que vous avez fournie SendGrid.

### <a name="enable-email-confirmation"></a>Activer la confirmation par courrier électronique

 Pour activer la confirmation par courrier électronique, vous allez modifier le code d’inscription en procédant comme suit.  

1. Dans le dossier *Account* , ouvrez le code-behind *Register.aspx.cs* et mettez à jour la méthode `CreateUser_Click` pour activer les modifications en surbrillance suivantes : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *default. aspx* , puis sélectionnez **définir comme page de démarrage**.
3. Exécutez l’application en appuyant sur **F5.** Une fois la page affichée, cliquez sur le lien **Register** pour afficher la page Register.
4. Entrez votre adresse de messagerie et votre mot de passe, puis cliquez sur le bouton **Register** pour envoyer un message électronique via SendGrid.  
   L’état actuel de votre projet et du code permettra à l’utilisateur de se connecter une fois qu’il aura terminé le formulaire d’inscription, même s’il n’a pas confirmé son compte.
5. Vérifiez votre compte de messagerie et cliquez sur le lien pour confirmer votre adresse de messagerie.  
   Une fois que vous avez envoyé le formulaire d’inscription, vous êtes connecté.  
    ![Exemple de site Web connecté](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Exiger une confirmation par courrier électronique avant la connexion

Bien que vous ayez confirmé le compte de messagerie, à ce stade, il n’est pas nécessaire de cliquer sur le lien contenu dans l’e-mail de vérification pour être entièrement connecté. Dans la section suivante, vous allez modifier le code nécessitant que les nouveaux utilisateurs disposent d’un e-mail confirmé avant qu’ils ne soient connectés (authentifié).

1. Dans **Explorateur de solutions** de Visual Studio, mettez à jour l’événement `CreateUser_Click` dans le code-behind *Register.aspx.cs* contenu dans le dossier *Accounts* avec les modifications en surbrillance suivantes : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Mettez à jour la méthode `LogIn` dans le code-behind *login.aspx.cs* avec les modifications en surbrillance suivantes : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Exécution de l'application

 Maintenant que vous avez implémenté le code pour vérifier si l’adresse de messagerie d’un utilisateur a été confirmée, vous pouvez vérifier la fonctionnalité à la fois dans les pages **Register** et **login** . 

1. Supprimez les comptes de la table **AspNetUsers** qui contiennent l’alias de messagerie que vous souhaitez tester.
2. Exécutez l’application (**F5**) et vérifiez que vous ne pouvez pas vous inscrire en tant qu’utilisateur tant que vous n’avez pas confirmé votre adresse de messagerie.
3. Avant de confirmer votre nouveau compte via l’e-mail qui vient d’être envoyé, essayez de vous connecter avec le nouveau compte.  
 Vous verrez que vous ne pouvez pas vous connecter et que vous devez disposer d’un compte de messagerie confirmé.
4. Une fois que vous avez confirmé votre adresse de messagerie, connectez-vous à l’application.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Récupération et réinitialisation du mot de passe

1. Dans Visual Studio, supprimez les caractères de commentaire de la méthode `Forgot` dans le code-behind *Forgot.aspx.cs* contenu dans le dossier *Account* , afin que la méthode apparaisse comme suit : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Ouvrez la page *login. aspx* . Remplacez le balisage près de la fin de la section **loginForm** comme indiqué ci-dessous : 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Ouvrez le code-behind *login.aspx.cs* et supprimez les marques de commentaire de la ligne de code suivante mise en surbrillance en jaune à partir du gestionnaire d’événements `Page_Load` : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Exécutez l’application en appuyant sur **F5.** Une fois la page affichée, cliquez sur le lien **se connecter** .
5. Cliquez sur le lien **vous avez oublié votre mot de passe ?** pour afficher la page **mot de passe oublié** .
6. Entrez votre adresse de messagerie, puis cliquez sur le bouton **Envoyer** pour envoyer un e-mail à votre adresse, ce qui vous permettra de réinitialiser votre mot de passe.   
   Vérifiez votre compte de messagerie et cliquez sur le lien pour afficher la page **Réinitialiser le mot de passe** .
7. Dans la page **Réinitialiser le mot de passe** , entrez votre adresse e-mail, le mot de passe et le mot de passe confirmé. Ensuite, appuyez sur le bouton **Réinitialiser** .  
   Lorsque vous avez correctement réinitialisé votre mot de passe, la page **modification du mot de passe** s’affiche. Vous pouvez maintenant vous connecter avec votre nouveau mot de passe.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Renvoyer le lien de confirmation par E-mail

Une fois qu’un utilisateur crée un compte local, un lien de confirmation lui est envoyé par e-mail, qu’il doit utiliser avant de pouvoir se connecter. Si l’utilisateur supprime accidentellement l’e-mail de confirmation ou si l’e-mail n’arrive jamais, il a besoin du lien de confirmation renvoyé. Les modifications suivantes du code montrent comment faire cela.

1. Dans Visual Studio, ouvrez le code-behind **login.aspx.cs** et ajoutez le gestionnaire d’événements suivant après le `LogIn` gestionnaire d’événements :   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modifiez le gestionnaire d’événements `LogIn` dans le code-behind *login.aspx.cs* en modifiant le code mis en surbrillance en jaune comme suit : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Mettez à jour la page *login. aspx* en ajoutant le code mis en surbrillance en jaune comme suit : 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Supprimez les comptes de la table **AspNetUsers** qui contiennent l’alias de messagerie que vous souhaitez tester.
5. Exécutez l’application (**F5**) et inscrivez votre adresse de messagerie.
6. Avant de confirmer votre nouveau compte via l’e-mail qui vient d’être envoyé, essayez de vous connecter avec le nouveau compte.  
   Vous verrez que vous ne pouvez pas vous connecter et que vous devez disposer d’un compte de messagerie confirmé. En outre, vous pouvez à présent renvoyer un message de confirmation à votre compte de messagerie.
7. Entrez votre adresse de messagerie et votre mot de passe, puis appuyez sur le bouton de **confirmation de renvoi** .
8. Une fois que vous avez confirmé votre adresse de messagerie en fonction du message électronique que vous venez d’envoyer, connectez-vous à l’application.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Résolution des problèmes de l’application

Si vous ne recevez pas de courrier électronique contenant le lien permettant de vérifier vos informations d’identification :

- Vérifiez votre dossier des courriers indésirables.
- Connectez-vous à votre compte SendGrid et cliquez sur le [lien activité de messagerie](https://sendgrid.com/logs/index).
- Assurez-vous que vous avez utilisé le nom de votre compte d’utilisateur SendGrid en tant que valeur *Web. config* plutôt que votre adresse de messagerie de compte SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Liens vers les ressources ASP.NET Identity recommandées](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmation de compte et récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Série de didacticiels Web Forms ASP.NET-ajouter un fournisseur OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Déployer une application ASP.NET Web Forms sécurisée avec appartenance, OAuth et SQL Database à Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Série de didacticiels ASP.NET Web Forms-activer SSL pour le projet](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
