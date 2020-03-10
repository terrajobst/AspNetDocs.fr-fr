---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Créer une application ASP.NET Web Forms avec l’authentification à deux facteurs SMSC#() | Microsoft Docs
author: Erikre
description: Ce didacticiel vous montre comment créer une application ASP.NET Web Forms avec l’authentification à deux facteurs. Ce didacticiel a été conçu pour compléter le didacticiel intitulé CR...
ms.author: riande
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c9558aca8a655071c0c94ed66433cf721f26c011
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78565463"
---
# <a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Créer une application Web Forms ASP.NET avec authentification à deux facteurs par SMS (C#)

par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’application ASP.NET Web Forms avec la messagerie et l’authentification à deux facteurs SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Ce didacticiel vous montre comment créer une application ASP.NET Web Forms avec l’authentification à deux facteurs. Ce didacticiel a été conçu pour compléter le didacticiel intitulé [créer une application secure ASP.NET Web Forms avec l’inscription de l’utilisateur, la confirmation par e-mail et la réinitialisation du mot de passe](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). En outre, ce didacticiel est basé sur le [didacticiel MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)de Rick Anderson.

## <a name="introduction"></a>Introduction

Ce didacticiel vous guide tout au long des étapes nécessaires à la création d’une application de Web Forms ASP.NET qui prend en charge l’authentification à deux facteurs à l’aide de Visual Studio. L’authentification à deux facteurs est une étape d’authentification utilisateur supplémentaire. Cette étape supplémentaire génère un numéro d’identification personnel (PIN) unique lors de la connexion. Le code PIN est généralement envoyé à l’utilisateur sous forme de message électronique ou SMS. L’utilisateur de votre application entre alors le code confidentiel comme mesure d’authentification supplémentaire lors de la connexion.

### <a name="tutorial-tasks-and-information"></a>Tâches et informations sur le didacticiel :

- [Créer une application de Web Forms ASP.NET](#createWebForms)
- [Configurer SMS et l’authentification à deux facteurs](#SMS)
- [Activer l’authentification à deux facteurs pour l’utilisateur inscrit](#use2FA)
- [Ressources supplémentaires pour MSBuild](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Créer une application de Web Forms ASP.NET

Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installez [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure. En outre, vous devrez créer un compte [Twilio](https://www.twilio.com/try-twilio) , comme expliqué ci-dessous.

> [!NOTE]
> Important : vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou version ultérieure pour suivre ce didacticiel.

1. Créez un projet (**fichier** -&gt; **nouveau projet**), puis sélectionnez le modèle **Application Web ASP.net** avec le .NET Framework version 4.5.2 dans la boîte de dialogue **nouveau projet** .
2. Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **Web Forms** . Laissez l’authentification par défaut en tant que **comptes d’utilisateur individuels**. Cliquez ensuite sur **OK** pour créer le nouveau projet.  
    ![Boîte de dialogue New ASP.NET Project](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Activez protocole SSL (SSL) pour le projet. Suivez les étapes disponibles dans la section **activer SSL pour le projet** de la [série prise en main avec Web Forms didacticiel](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Dans Visual Studio, ouvrez la **console du gestionnaire de package** (**Outils** -&gt; gestionnaire de **package NuGet** -&gt; **console du gestionnaire de package**), puis entrez la commande suivante :  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Configurer SMS et l’authentification à deux facteurs

Ce didacticiel utilise Twilio, mais vous pouvez utiliser n’importe quel fournisseur SMS.

1. Créez un compte [Twilio](https://www.twilio.com/try-twilio) .
2. À partir de l’onglet **tableau de bord** de votre compte Twilio, copiez le **SID de compte** et le **jeton d’authentification.** Vous les ajouterez ultérieurement à votre application.
3. Dans l’onglet **nombres** , copiez également votre **numéro de téléphone** Twilio.
4. Rendez le **SID de compte**Twilio, le **jeton d’authentification** et le **numéro de téléphone** disponibles pour l’application. Pour simplifier les choses, vous allez stocker ces valeurs dans le fichier *Web. config* . Lorsque vous déployez dans Azure, vous pouvez stocker les valeurs en toute sécurité dans la section **appSettings** de l’onglet configurer du site Web. En outre, lorsque vous ajoutez le numéro de téléphone, utilisez uniquement des chiffres.   
   Notez que vous pouvez également ajouter des informations d’identification SendGrid. SendGrid est un service de notification par courrier électronique. Pour plus d’informations sur l’activation de SendGrid, consultez la section « raccorder SendGrid » du didacticiel intitulé [créer une application sécurisée de ASP.NET Web Forms avec inscription de l’utilisateur, confirmation par e-mail et réinitialisation du mot de passe.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Sécurité - Ne jamais stocker de données sensibles dans votre code source. Dans cet exemple, le compte et les informations d’identification sont stockés dans la section **appSettings** du fichier *Web. config* . Sur Azure, vous pouvez stocker ces valeurs en toute sécurité sous l’onglet **[configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** de la portail Azure. Pour obtenir des informations connexes, consultez la rubrique « [meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles dans ASP.net et Azure](/aspnet/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)» de Rick Anderson.
5. Configurez la classe `SmsService` dans le fichier *App\_Start\IdentityConfig.cs* en mettant en surbrillance les modifications suivantes en jaune : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Ajoutez les instructions `using` suivantes au début du fichier *IdentityConfig.cs* : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Mettez à jour le fichier *Account/Manage. aspx* en supprimant les lignes mises en surbrillance en jaune :  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. Dans le gestionnaire de `Page_Load` du code-behind *Manage.aspx.cs* , supprimez les marques de commentaire de la ligne de code mise en surbrillance en jaune afin qu’elle apparaisse comme suit : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. Dans le codebehind of *Account*/*TwoFactorAuthenticationSignIn.aspx.cs*, mettez à jour le gestionnaire de `Page_Load` en ajoutant le code suivant mis en surbrillance en jaune : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   En modifiant le code ci-dessus, le DropDownList « Providers » contenant les options d’authentification ne sera pas réinitialisé à la première valeur. Cela permettra à l’utilisateur de sélectionner avec succès toutes les options à utiliser lors de l’authentification, et pas seulement la première.
10. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *default. aspx* , puis sélectionnez **définir comme page de démarrage**.
11. En testant votre application, commencez par créer l’application (**Ctrl**+**Shift**+**B**), puis exécutez l’application (**F5**) et sélectionnez **inscrire** pour créer un nouveau compte d’utilisateur, ou sélectionnez **se connecter** si le compte d’utilisateur a déjà été inscrit.
12. Une fois que vous (en tant qu’utilisateur) êtes connecté, cliquez sur l’ID d’utilisateur (adresse de messagerie) dans la barre de navigation pour afficher la page **gérer le compte** (Manage. aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Cliquez sur **Ajouter** en regard de **numéro de téléphone** dans la page gérer le **compte** .  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Ajoutez le numéro de téléphone auquel vous (en tant qu’utilisateur) souhaiterez recevoir des messages SMS (messages texte), puis cliquez sur le bouton **Envoyer** .   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    À ce stade, l’application utilise les informations d’identification du *fichier Web. config* pour contacter Twilio. Un message SMS (message texte) sera envoyé au téléphone associé au compte d’utilisateur. Vous pouvez vérifier que le message Twilio a été envoyé en consultant le tableau de bord Twilio.
15. En quelques secondes, le téléphone associé au compte d’utilisateur recevra un message texte contenant le code de vérification. Entrez le code de vérification, puis appuyez sur **Envoyer**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Activer l’authentification à deux facteurs pour un utilisateur inscrit

À ce stade, vous avez activé l’authentification à deux facteurs pour votre application. Pour qu’un utilisateur puisse utiliser l’authentification à deux facteurs, il peut simplement modifier ses paramètres à l’aide de l’interface utilisateur. 

1. En tant qu’utilisateur de votre application, vous pouvez activer l’authentification à deux facteurs pour votre compte spécifique en cliquant sur l’ID d’utilisateur (alias de messagerie) dans la barre de navigation pour afficher la page **gérer le compte** . Ensuite, cliquez sur le lien **activer** pour activer l’authentification à deux facteurs pour le compte.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Déconnectez-vous, puis reconnectez-vous. Si vous avez activé la messagerie électronique, vous pouvez sélectionner SMS ou e-mail pour l’authentification à deux facteurs. Si vous n’avez pas activé la messagerie électronique, consultez le didacticiel intitulé [créer une application sécurisée ASP.NET Web Forms avec inscription de l’utilisateur, confirmation par e-mail et réinitialisation du mot de passe](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. La page authentification à deux facteurs s’affiche, dans laquelle vous pouvez entrer le code (à partir de SMS ou de courrier électronique).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Si vous cliquez sur la case à cocher **mémoriser ce navigateur** , vous ne devez pas utiliser l’authentification à deux facteurs pour vous connecter lorsque vous utilisez le navigateur et l’appareil où vous avez activé la case à cocher. Tant que les utilisateurs malveillants ne peuvent pas accéder à votre appareil, l’activation de l’authentification à deux facteurs et le **rappel de ce navigateur** vous fourniront un accès au mot de passe en une seule étape, tout en conservant une protection renforcée à deux facteurs pour tous les accès à partir d’appareils non approuvés. Vous pouvez effectuer cela sur n’importe quel appareil privé que vous utilisez régulièrement.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Authentification à deux facteurs par SMS et e-mail avec ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Liens vers les ressources ASP.NET Identity recommandées](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Déployer une application ASP.NET Web Forms sécurisée avec appartenance, OAuth et SQL Database sur un site Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Série de didacticiels Web Forms ASP.NET-ajouter un fournisseur OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Série de didacticiels ASP.NET Web Forms-activer SSL pour le projet](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Confirmation de compte et récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Création de l’application dans Facebook et connexion de l’application au projet](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
