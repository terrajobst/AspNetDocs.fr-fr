---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Application ASP.NET MVC 5 avec SMS et e-mail pour l’authentification à deux facteurs | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application Web ASP.NET MVC 5 avec authentification à deux facteurs. Vous devez terminer la création d’une application Web ASP.NET MVC 5 sécurisée avec...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c14149d802bfc0a227a839a2981dc3e8a3849c25
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457594"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Application ASP.NET MVC 5 avec authentification à deux facteurs par SMS et e-mail

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous montre comment créer une application Web ASP.NET MVC 5 avec authentification à deux facteurs. Avant de continuer, vous devez effectuer [la procédure création d’une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation du mot de passe](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) . Vous pouvez télécharger l’application terminée [ici](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Le téléchargement contient les helpers de débogage qui vous permettent de tester une confirmation par e-mail et par SMS sans configurer un fournisseur de messagerie ou de SMS.
> 
> Ce didacticiel a été rédigé par [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter : [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

- [Créer une application MVC ASP.NET](#createMvc)
- [Configurer SMS pour l’authentification à deux facteurs](#SMS)
- [Activer l’authentification à deux facteurs](#enable2)
- [Ressources supplémentaires](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Créer une application MVC ASP.NET

Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installez [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure.

> [!NOTE]
> AVERTISSEMENT : vous devez effectuer [la procédure créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation du mot de passe](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) avant de continuer. Pour suivre ce didacticiel, vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou version ultérieure.

1. Créez un projet Web ASP.NET et sélectionnez le modèle MVC. Web Forms prend également en charge ASP.NET Identity : vous pouvez donc suivre des étapes similaires dans une application Web Forms.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Laissez l’authentification par défaut en tant que **comptes d’utilisateur individuels**. Si vous souhaitez héberger l’application dans Azure, laissez la case à cocher activée. Plus loin dans ce didacticiel, nous déploierons sur Azure. Vous pouvez [ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Définissez le [projet pour utiliser SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adresse :  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Espace de noms :  
    `ASPSMSX2`
3. **Identification des informations d’identification de l’utilisateur du fournisseur SMS**  
  
   Twilio  
   À partir de l’onglet **tableau de bord** de votre compte Twilio, copiez le **SID de compte** et le jeton d' **authentification**.  
  
   ASPSMS:  
   Dans les paramètres de votre compte, accédez à **sensibles** et copiez-le avec votre **mot de passe**auto-défini.  
  
   Nous stockerons ces valeurs ultérieurement dans le fichier *Web. config* dans les clés `"SMSAccountIdentification"` et `"SMSAccountPassword"`.
4. **Spécification de SenderID/Originator**  
  
   Twilio  
   Dans l’onglet **nombres** , copiez votre numéro de téléphone Twilio.  
  
   ASPSMS:  
   Dans le menu **déverrouiller** les expéditeurs, déverrouillez un ou plusieurs expéditeurs ou choisissez un expéditeur alphanumérique (non pris en charge par tous les réseaux).  
  
   Nous stockerons ultérieurement cette valeur dans le fichier *Web. config* au sein de la `"SMSAccountFrom"` de clé.
5. **Transfert des informations d’identification du fournisseur SMS vers l’application**  
  
   Mettez les informations d’identification et le numéro de téléphone de l’expéditeur à la disposition de l’application. Pour simplifier les choses, nous stockons ces valeurs dans le fichier *Web. config* . Lorsque nous déployons vers Azure, vous pouvez stocker les valeurs en toute sécurité dans la section paramètres de l' **application** sous l’onglet configurer du site Web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Sécurité - Ne jamais stocker de données sensibles dans votre code source. Le compte et les informations d’identification sont ajoutés au code ci-dessus pour que l’exemple reste simple. Consultez [meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles dans ASP.net et Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implémentation du transfert de données vers le fournisseur SMS**  
  
   Configurez la classe `SmsService` dans le fichier *App\_Start\IdentityConfig.cs* .  
  
   Selon le fournisseur SMS utilisé, activez la section **Twilio** ou **ASPSMS** : 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Mettre à jour la vue Razor *Views\Manage\Index.cshtml* : (Remarque : ne supprimez pas uniquement les commentaires dans le code de sortie, utilisez le code ci-dessous.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Vérifiez que les méthodes d’action `EnableTwoFactorAuthentication` et `DisableTwoFactorAuthentication` dans la `ManageController` possèdent l’attribut[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Exécutez l’application et connectez-vous avec le compte que vous avez enregistré précédemment.
10. Cliquez sur votre ID d’utilisateur, ce qui active la méthode d’action `Index` dans `Manage` contrôleur.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Cliquez sur Ajouter.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. La méthode d’action `AddPhoneNumber` affiche une boîte de dialogue qui permet d’entrer un numéro de téléphone qui peut recevoir des messages SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. En quelques secondes, vous recevrez un message texte contenant le code de vérification. Entrez-le et appuyez sur **Envoyer**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. La vue gérer indique que votre numéro de téléphone a été ajouté.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Activer l’authentification à deux facteurs

Dans l’application générée par le modèle, vous devez utiliser l’interface utilisateur pour activer l’authentification à deux facteurs (2FA). Pour activer 2FA, cliquez sur votre ID d’utilisateur (alias de messagerie) dans la barre de navigation.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Cliquez sur Enable 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Déconnectez-vous, puis reconnectez-vous. Si vous avez activé la messagerie électronique (voir mon [didacticiel précédent](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), vous pouvez sélectionner le SMS ou l’E-mail pour 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

La page vérifier le code s’affiche, dans laquelle vous pouvez entrer le code (à partir de SMS ou de courrier électronique).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Si vous cliquez sur la case à cocher **mémoriser ce navigateur** , vous ne devez pas utiliser 2FA pour vous connecter lorsque vous utilisez le navigateur et l’appareil dans lesquels vous avez coché la case. Tant que les utilisateurs malveillants ne peuvent pas accéder à votre appareil, l’activation de 2FA et un clic sur le bouton **mémoriser ce navigateur** vous fourniront un accès au mot de passe en une seule étape, tout en conservant une protection renforcée 2FA pour tous les accès à partir d’appareils non approuvés. Vous pouvez effectuer cela sur n’importe quel appareil privé que vous utilisez régulièrement.

Ce didacticiel fournit une brève introduction à l’activation de 2FA sur une nouvelle application ASP.NET MVC. Mon didacticiel sur l' [authentification à deux facteurs à l’aide de SMS et la messagerie électronique avec ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) passe en détail dans le code de l’exemple.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Authentification à deux facteurs à l’aide de SMS et e-mail avec ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Passe en détail sur l’authentification à deux facteurs
- [Liens vers les ressources ASP.NET Identity recommandées](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmation de compte et récupération de mot de passe avec ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Pour plus d’informations sur la récupération du mot de passe et la confirmation du compte.
- [Application MVC 5 avec l’authentification Facebook, Twitter, LinkedIn et Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ce didacticiel vous montre comment écrire une application ASP.NET MVC 5 avec l’autorisation Facebook et Google OAuth 2. Il montre également comment ajouter des données supplémentaires à la base de données Identity.
- [Déployez une application ASP.NET MVC sécurisée avec appartenance, OAuth et SQL Database sur Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ce tutoriel ajoute le déploiement Azure, la sécurisation de votre application avec des rôles, l’utilisation de l’API d’appartenance pour ajouter des utilisateurs et des rôles, et des fonctionnalités de sécurité supplémentaires.
- [Création d’une application Google pour OAuth 2 et connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Création de l’application dans Facebook et connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configuration de SSL dans le projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [Comment configurer votre C# environnement de développement ASP.NET MVC](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
