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
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="8ecd9-104">Application ASP.NET MVC 5 avec authentification à deux facteurs par SMS et e-mail</span><span class="sxs-lookup"><span data-stu-id="8ecd9-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>

<span data-ttu-id="8ecd9-105">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8ecd9-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="8ecd9-106">Ce didacticiel vous montre comment créer une application Web ASP.NET MVC 5 avec authentification à deux facteurs.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="8ecd9-107">Avant de continuer, vous devez effectuer [la procédure création d’une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation du mot de passe](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) .</span><span class="sxs-lookup"><span data-stu-id="8ecd9-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="8ecd9-108">Vous pouvez télécharger l’application terminée [ici](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="8ecd9-109">Le téléchargement contient les helpers de débogage qui vous permettent de tester une confirmation par e-mail et par SMS sans configurer un fournisseur de messagerie ou de SMS.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="8ecd9-110">Ce didacticiel a été rédigé par [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter : [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

- [<span data-ttu-id="8ecd9-111">Créer une application MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ecd9-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="8ecd9-112">Configurer SMS pour l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="8ecd9-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="8ecd9-113">Activer l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="8ecd9-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="8ecd9-114">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8ecd9-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="8ecd9-115">Créer une application MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ecd9-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="8ecd9-116">Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="8ecd9-117">Installez [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="8ecd9-118">AVERTISSEMENT : vous devez effectuer [la procédure créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation du mot de passe](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="8ecd9-119">Pour suivre ce didacticiel, vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="8ecd9-120">Créez un projet Web ASP.NET et sélectionnez le modèle MVC.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="8ecd9-121">Web Forms prend également en charge ASP.NET Identity : vous pouvez donc suivre des étapes similaires dans une application Web Forms.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="8ecd9-122">Laissez l’authentification par défaut en tant que **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="8ecd9-123">Si vous souhaitez héberger l’application dans Azure, laissez la case à cocher activée.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="8ecd9-124">Plus loin dans ce didacticiel, nous déploierons sur Azure.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="8ecd9-125">Vous pouvez [ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="8ecd9-126">Définissez le [projet pour utiliser SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="8ecd9-127">Configurer SMS pour l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="8ecd9-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="8ecd9-128">Ce didacticiel fournit des instructions sur l’utilisation de Twilio ou de ASPSMS, mais vous pouvez utiliser n’importe quel autre fournisseur SMS.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="8ecd9-129">**Création d’un compte d’utilisateur avec un fournisseur SMS**</span><span class="sxs-lookup"><span data-stu-id="8ecd9-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="8ecd9-130">Créez un compte [Twilio](https://www.twilio.com/try-twilio) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .</span><span class="sxs-lookup"><span data-stu-id="8ecd9-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="8ecd9-131">**Installation de packages supplémentaires ou ajout de références de service**</span><span class="sxs-lookup"><span data-stu-id="8ecd9-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="8ecd9-132">Twilio</span><span class="sxs-lookup"><span data-stu-id="8ecd9-132">Twilio:</span></span>  
   <span data-ttu-id="8ecd9-133">Dans la Console du gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="8ecd9-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="8ecd9-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="8ecd9-134">ASPSMS:</span></span>  
   <span data-ttu-id="8ecd9-135">Vous devez ajouter la référence de service suivante :</span><span class="sxs-lookup"><span data-stu-id="8ecd9-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="8ecd9-136">Adresse :</span><span class="sxs-lookup"><span data-stu-id="8ecd9-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="8ecd9-137">Espace de noms :</span><span class="sxs-lookup"><span data-stu-id="8ecd9-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="8ecd9-138">**Identification des informations d’identification de l’utilisateur du fournisseur SMS**</span><span class="sxs-lookup"><span data-stu-id="8ecd9-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="8ecd9-139">Twilio</span><span class="sxs-lookup"><span data-stu-id="8ecd9-139">Twilio:</span></span>  
   <span data-ttu-id="8ecd9-140">À partir de l’onglet **tableau de bord** de votre compte Twilio, copiez le **SID de compte** et le jeton d' **authentification**.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="8ecd9-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="8ecd9-141">ASPSMS:</span></span>  
   <span data-ttu-id="8ecd9-142">Dans les paramètres de votre compte, accédez à **sensibles** et copiez-le avec votre **mot de passe**auto-défini.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="8ecd9-143">Nous stockerons ces valeurs ultérieurement dans le fichier *Web. config* dans les clés `"SMSAccountIdentification"` et `"SMSAccountPassword"`.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="8ecd9-144">**Spécification de SenderID/Originator**</span><span class="sxs-lookup"><span data-stu-id="8ecd9-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="8ecd9-145">Twilio</span><span class="sxs-lookup"><span data-stu-id="8ecd9-145">Twilio:</span></span>  
   <span data-ttu-id="8ecd9-146">Dans l’onglet **nombres** , copiez votre numéro de téléphone Twilio.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="8ecd9-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="8ecd9-147">ASPSMS:</span></span>  
   <span data-ttu-id="8ecd9-148">Dans le menu **déverrouiller** les expéditeurs, déverrouillez un ou plusieurs expéditeurs ou choisissez un expéditeur alphanumérique (non pris en charge par tous les réseaux).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="8ecd9-149">Nous stockerons ultérieurement cette valeur dans le fichier *Web. config* au sein de la `"SMSAccountFrom"` de clé.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="8ecd9-150">**Transfert des informations d’identification du fournisseur SMS vers l’application**</span><span class="sxs-lookup"><span data-stu-id="8ecd9-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="8ecd9-151">Mettez les informations d’identification et le numéro de téléphone de l’expéditeur à la disposition de l’application.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="8ecd9-152">Pour simplifier les choses, nous stockons ces valeurs dans le fichier *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="8ecd9-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="8ecd9-153">Lorsque nous déployons vers Azure, vous pouvez stocker les valeurs en toute sécurité dans la section paramètres de l' **application** sous l’onglet configurer du site Web.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="8ecd9-154">Sécurité - Ne jamais stocker de données sensibles dans votre code source.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="8ecd9-155">Le compte et les informations d’identification sont ajoutés au code ci-dessus pour que l’exemple reste simple.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="8ecd9-156">Consultez [meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles dans ASP.net et Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="8ecd9-157">**Implémentation du transfert de données vers le fournisseur SMS**</span><span class="sxs-lookup"><span data-stu-id="8ecd9-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="8ecd9-158">Configurez la classe `SmsService` dans le fichier *App\_Start\IdentityConfig.cs* .</span><span class="sxs-lookup"><span data-stu-id="8ecd9-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="8ecd9-159">Selon le fournisseur SMS utilisé, activez la section **Twilio** ou **ASPSMS** :</span><span class="sxs-lookup"><span data-stu-id="8ecd9-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="8ecd9-160">Mettre à jour la vue Razor *Views\Manage\Index.cshtml* : (Remarque : ne supprimez pas uniquement les commentaires dans le code de sortie, utilisez le code ci-dessous.)</span><span class="sxs-lookup"><span data-stu-id="8ecd9-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="8ecd9-161">Vérifiez que les méthodes d’action `EnableTwoFactorAuthentication` et `DisableTwoFactorAuthentication` dans la `ManageController` possèdent l’attribut[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :</span><span class="sxs-lookup"><span data-stu-id="8ecd9-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="8ecd9-162">Exécutez l’application et connectez-vous avec le compte que vous avez enregistré précédemment.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="8ecd9-163">Cliquez sur votre ID d’utilisateur, ce qui active la méthode d’action `Index` dans `Manage` contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="8ecd9-164">Cliquez sur Ajouter.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="8ecd9-165">La méthode d’action `AddPhoneNumber` affiche une boîte de dialogue qui permet d’entrer un numéro de téléphone qui peut recevoir des messages SMS.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="8ecd9-166">En quelques secondes, vous recevrez un message texte contenant le code de vérification.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="8ecd9-167">Entrez-le et appuyez sur **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="8ecd9-168">La vue gérer indique que votre numéro de téléphone a été ajouté.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="8ecd9-169">Activer l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="8ecd9-169">Enable two-factor authentication</span></span>

<span data-ttu-id="8ecd9-170">Dans l’application générée par le modèle, vous devez utiliser l’interface utilisateur pour activer l’authentification à deux facteurs (2FA).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="8ecd9-171">Pour activer 2FA, cliquez sur votre ID d’utilisateur (alias de messagerie) dans la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="8ecd9-172">Cliquez sur Enable 2FA.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="8ecd9-173">Déconnectez-vous, puis reconnectez-vous.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-173">Logout, then log back in.</span></span> <span data-ttu-id="8ecd9-174">Si vous avez activé la messagerie électronique (voir mon [didacticiel précédent](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), vous pouvez sélectionner le SMS ou l’E-mail pour 2FA.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="8ecd9-175">La page vérifier le code s’affiche, dans laquelle vous pouvez entrer le code (à partir de SMS ou de courrier électronique).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="8ecd9-176">Si vous cliquez sur la case à cocher **mémoriser ce navigateur** , vous ne devez pas utiliser 2FA pour vous connecter lorsque vous utilisez le navigateur et l’appareil dans lesquels vous avez coché la case.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="8ecd9-177">Tant que les utilisateurs malveillants ne peuvent pas accéder à votre appareil, l’activation de 2FA et un clic sur le bouton **mémoriser ce navigateur** vous fourniront un accès au mot de passe en une seule étape, tout en conservant une protection renforcée 2FA pour tous les accès à partir d’appareils non approuvés.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="8ecd9-178">Vous pouvez effectuer cela sur n’importe quel appareil privé que vous utilisez régulièrement.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="8ecd9-179">Ce didacticiel fournit une brève introduction à l’activation de 2FA sur une nouvelle application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="8ecd9-180">Mon didacticiel sur l' [authentification à deux facteurs à l’aide de SMS et la messagerie électronique avec ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) passe en détail dans le code de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="8ecd9-181">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8ecd9-181">Additional Resources</span></span>

- <span data-ttu-id="8ecd9-182">[Authentification à deux facteurs à l’aide de SMS et e-mail avec ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Passe en détail sur l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="8ecd9-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="8ecd9-183">Liens vers les ressources ASP.NET Identity recommandées</span><span class="sxs-lookup"><span data-stu-id="8ecd9-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="8ecd9-184">[Confirmation de compte et récupération de mot de passe avec ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Pour plus d’informations sur la récupération du mot de passe et la confirmation du compte.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="8ecd9-185">[Application MVC 5 avec l’authentification Facebook, Twitter, LinkedIn et Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ce didacticiel vous montre comment écrire une application ASP.NET MVC 5 avec l’autorisation Facebook et Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="8ecd9-186">Il montre également comment ajouter des données supplémentaires à la base de données Identity.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="8ecd9-187">[Déployez une application ASP.NET MVC sécurisée avec appartenance, OAuth et SQL Database sur Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="8ecd9-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="8ecd9-188">Ce tutoriel ajoute le déploiement Azure, la sécurisation de votre application avec des rôles, l’utilisation de l’API d’appartenance pour ajouter des utilisateurs et des rôles, et des fonctionnalités de sécurité supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="8ecd9-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="8ecd9-189">Création d’une application Google pour OAuth 2 et connexion de l’application au projet</span><span class="sxs-lookup"><span data-stu-id="8ecd9-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="8ecd9-190">Création de l’application dans Facebook et connexion de l’application au projet</span><span class="sxs-lookup"><span data-stu-id="8ecd9-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="8ecd9-191">Configuration de SSL dans le projet</span><span class="sxs-lookup"><span data-stu-id="8ecd9-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [<span data-ttu-id="8ecd9-192">Comment configurer votre C# environnement de développement ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8ecd9-192">How to set up your C# and ASP.NET MVC development environment</span></span>](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
