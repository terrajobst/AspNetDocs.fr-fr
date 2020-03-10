---
uid: web-api/overview/security/external-authentication-services
title: Services d’authentification externes avec API Web ASP.NETC#() | Microsoft Docs
author: rmcmurray
description: Décrit l’utilisation des services d’authentification externes dans API Web ASP.NET.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555474"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="91adb-103">Services d’authentification externes avec API Web ASP.NETC#()</span><span class="sxs-lookup"><span data-stu-id="91adb-103">External Authentication Services with ASP.NET Web API (C#)</span></span>

<span data-ttu-id="91adb-104">Visual Studio 2017 et ASP.NET 4.7.2 développent les options de sécurité pour les [applications à page unique](../../../single-page-application/index.md) (Spa) et les services d' [API Web](../../index.md) pour s’intégrer aux services d’authentification externes, qui incluent plusieurs services d’authentification de réseaux sociaux et OAuth/OpenID : comptes Microsoft, Twitter, Facebook et Google.</span><span class="sxs-lookup"><span data-stu-id="91adb-104">Visual Studio 2017 and ASP.NET 4.7.2 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>  

### <a name="in-this-walkthrough"></a><span data-ttu-id="91adb-105">Dans cette procédure pas à pas</span><span class="sxs-lookup"><span data-stu-id="91adb-105">In this Walkthrough</span></span>

- [<span data-ttu-id="91adb-106">Utilisation des services d’authentification externes</span><span class="sxs-lookup"><span data-stu-id="91adb-106">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="91adb-107">Création de l’exemple d’application Web</span><span class="sxs-lookup"><span data-stu-id="91adb-107">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="91adb-108">Activation de l’authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="91adb-108">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="91adb-109">Activation de l’authentification Google</span><span class="sxs-lookup"><span data-stu-id="91adb-109">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="91adb-110">Activation de l’authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="91adb-110">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="91adb-111">Activation de l’authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="91adb-111">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="91adb-112">Informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="91adb-112">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="91adb-113">Combinaison de services d’authentification externes</span><span class="sxs-lookup"><span data-stu-id="91adb-113">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="91adb-114">Configuration de IIS Express pour utiliser un nom de domaine complet</span><span class="sxs-lookup"><span data-stu-id="91adb-114">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="91adb-115">Comment obtenir les paramètres de votre application pour l’authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="91adb-115">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="91adb-116">Facultatif : désactiver l’inscription locale</span><span class="sxs-lookup"><span data-stu-id="91adb-116">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="91adb-117">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="91adb-117">Prerequisites</span></span>

<span data-ttu-id="91adb-118">Pour suivre les exemples de cette procédure pas à pas, vous devez disposer des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="91adb-118">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="91adb-119">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="91adb-119">Visual Studio 2017</span></span>
- <span data-ttu-id="91adb-120">Un compte de développeur avec l’identificateur d’application et la clé secrète pour l’un des services d’authentification de réseaux sociaux suivants :</span><span class="sxs-lookup"><span data-stu-id="91adb-120">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

  - <span data-ttu-id="91adb-121">Comptes Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="91adb-121">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
  - <span data-ttu-id="91adb-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="91adb-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
  - <span data-ttu-id="91adb-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="91adb-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>
  - <span data-ttu-id="91adb-124">Google ([https://developers.google.com/](https://developers.google.com))</span><span class="sxs-lookup"><span data-stu-id="91adb-124">Google ([https://developers.google.com/](https://developers.google.com))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="91adb-125">Utilisation des services d’authentification externes</span><span class="sxs-lookup"><span data-stu-id="91adb-125">Using External Authentication Services</span></span>

<span data-ttu-id="91adb-126">L’abondance de services d’authentification externes actuellement disponibles pour les développeurs Web permet de réduire le temps de développement lors de la création de nouvelles applications Web.</span><span class="sxs-lookup"><span data-stu-id="91adb-126">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="91adb-127">Les utilisateurs Web ont généralement plusieurs comptes existants pour les services Web populaires et les sites Web des réseaux sociaux. par conséquent, lorsqu’une application Web implémente les services d’authentification à partir d’un service Web externe ou d’un site Web de réseaux sociaux, elle enregistre le temps de développement a été consacré à la création d’une implémentation d’authentification.</span><span class="sxs-lookup"><span data-stu-id="91adb-127">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="91adb-128">L’utilisation d’un service d’authentification externe évite aux utilisateurs finaux d’avoir à créer un autre compte pour votre application Web et à se souvenir également d’un autre nom d’utilisateur et d’un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="91adb-128">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="91adb-129">Par le passé, les développeurs avaient deux possibilités : créer leur propre implémentation d’authentification, ou apprendre à intégrer un service d’authentification externe dans leurs applications.</span><span class="sxs-lookup"><span data-stu-id="91adb-129">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="91adb-130">Au niveau le plus simple, le diagramme suivant illustre un simple workflow de requête pour un agent utilisateur (navigateur Web) qui demande des informations à partir d’une application Web configurée pour utiliser un service d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="91adb-130">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

<span data-ttu-id="91adb-131">Dans le schéma précédent, l’agent utilisateur (ou le navigateur Web dans cet exemple) effectue une demande à une application Web, qui redirige le navigateur Web vers un service d’authentification externe.</span><span class="sxs-lookup"><span data-stu-id="91adb-131">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="91adb-132">L’agent utilisateur envoie ses informations d’identification au service d’authentification externe, et si l’agent utilisateur s’est correctement authentifié, le service d’authentification externe redirige l’agent utilisateur vers l’application Web d’origine avec une forme de jeton que le l’agent utilisateur sera envoyé à l’application Web.</span><span class="sxs-lookup"><span data-stu-id="91adb-132">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="91adb-133">L’application Web utilise le jeton pour vérifier que l’agent utilisateur a été correctement authentifié par le service d’authentification externe, et l’application Web peut utiliser le jeton pour collecter plus d’informations sur l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="91adb-133">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="91adb-134">Une fois que l’application a terminé le traitement des informations de l’agent utilisateur, l’application Web renverra la réponse appropriée à l’agent utilisateur en fonction de ses paramètres d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="91adb-134">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="91adb-135">Dans ce deuxième exemple, l’agent utilisateur négocie avec l’application Web et le serveur d’autorisation externe, et l’application Web effectue une communication supplémentaire avec le serveur d’autorisation externe pour récupérer des informations supplémentaires sur l’utilisateur. agent</span><span class="sxs-lookup"><span data-stu-id="91adb-135">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

<span data-ttu-id="91adb-136">Visual Studio 2017 et ASP.NET 4.7.2 facilitent l’intégration des services d’authentification externes pour les développeurs en fournissant une intégration intégrée pour les services d’authentification suivants :</span><span class="sxs-lookup"><span data-stu-id="91adb-136">Visual Studio 2017 and ASP.NET 4.7.2 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="91adb-137">Facebook</span><span class="sxs-lookup"><span data-stu-id="91adb-137">Facebook</span></span>
- <span data-ttu-id="91adb-138">Google</span><span class="sxs-lookup"><span data-stu-id="91adb-138">Google</span></span>
- <span data-ttu-id="91adb-139">Comptes Microsoft (comptes Windows Live ID)</span><span class="sxs-lookup"><span data-stu-id="91adb-139">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="91adb-140">Twitter</span><span class="sxs-lookup"><span data-stu-id="91adb-140">Twitter</span></span>

<span data-ttu-id="91adb-141">Les exemples de cette procédure pas à pas montrent comment configurer chacun des services d’authentification externes pris en charge à l’aide du nouveau modèle d’application Web ASP.NET fourni avec Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="91adb-141">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2017.</span></span>

> [!NOTE]
> <span data-ttu-id="91adb-142">Si nécessaire, vous devrez peut-être ajouter votre nom de domaine complet aux paramètres de votre service d’authentification externe.</span><span class="sxs-lookup"><span data-stu-id="91adb-142">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="91adb-143">Cette exigence est basée sur des contraintes de sécurité pour certains services d’authentification externes qui nécessitent que le nom de domaine complet dans les paramètres de votre application corresponde au nom de domaine complet utilisé par vos clients.</span><span class="sxs-lookup"><span data-stu-id="91adb-143">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="91adb-144">(Les étapes de cette opération varient considérablement pour chaque service d’authentification externe ; vous devrez consulter la documentation de chaque service d’authentification externe pour voir si cela est nécessaire et comment configurer ces paramètres.) Si vous devez configurer IIS Express pour utiliser un nom de domaine complet pour tester cet environnement, consultez la section [configuration de IIS Express pour utiliser un nom de domaine complet](#FQDN) plus loin dans cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="91adb-144">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a><span data-ttu-id="91adb-145">Créer un exemple d’application Web</span><span class="sxs-lookup"><span data-stu-id="91adb-145">Create a Sample Web Application</span></span>

<span data-ttu-id="91adb-146">Les étapes suivantes vous guideront dans la création d’un exemple d’application à l’aide du modèle d’application Web ASP.NET, et vous utiliserez cet exemple d’application pour chacun des services d’authentification externes plus loin dans cette procédure pas à pas.</span><span class="sxs-lookup"><span data-stu-id="91adb-146">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="91adb-147">Démarrez Visual Studio 2017, puis sélectionnez **nouveau projet** dans la page de démarrage.</span><span class="sxs-lookup"><span data-stu-id="91adb-147">Start Visual Studio 2017 and select **New Project** from the Start page.</span></span> <span data-ttu-id="91adb-148">Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="91adb-148">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

<span data-ttu-id="91adb-149">Quand la boîte **de dialogue Nouveau projet** s’affiche, sélectionnez **installé** et développez **visuel C#** .</span><span class="sxs-lookup"><span data-stu-id="91adb-149">When the **New Project** dialog box is displayed, select **Installed** and expand **Visual C#**.</span></span> <span data-ttu-id="91adb-150">Sous **visuel C#** , sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="91adb-150">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="91adb-151">Dans la liste des modèles de projet, sélectionnez **application Web ASP.net (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="91adb-151">In the list of project templates, select **ASP.NET Web Application (.Net Framework)**.</span></span> <span data-ttu-id="91adb-152">Entrez un nom pour votre projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="91adb-152">Enter a name for your project and click **OK**.</span></span>

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

<span data-ttu-id="91adb-153">Lorsque le **nouveau projet ASP.net** s’affiche, sélectionnez le modèle **application à page unique** , puis cliquez sur **créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="91adb-153">When the **New ASP.NET Project** is displayed, select the **Single Page Application** template and click **Create Project**.</span></span>

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

<span data-ttu-id="91adb-154">Attendez que Visual Studio 2017 crée votre projet.</span><span class="sxs-lookup"><span data-stu-id="91adb-154">Wait as Visual Studio 2017 creates your project.</span></span>

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

<span data-ttu-id="91adb-155">Lorsque Visual Studio 2017 a terminé la création de votre projet, ouvrez le fichier *Startup.auth.cs* situé dans le dossier de démarrage de l' **application\_** .</span><span class="sxs-lookup"><span data-stu-id="91adb-155">When Visual Studio 2017 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="91adb-156">Lorsque vous créez le projet pour la première fois, aucun des services d’authentification externes n’est activé dans le fichier *Startup.auth.cs* ; l’exemple suivant illustre ce que votre code peut ressembler, avec les sections mises en surbrillance pour l’emplacement où vous activez un service d’authentification externe et tous les paramètres appropriés afin d’utiliser des comptes Microsoft, Twitter, Facebook ou l’authentification Google avec votre application ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="91adb-156">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="91adb-157">Quand vous appuyez sur F5 pour générer et déboguer votre application Web, un écran de connexion s’affiche, dans lequel vous verrez qu’aucun service d’authentification externe n’a été défini.</span><span class="sxs-lookup"><span data-stu-id="91adb-157">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

<span data-ttu-id="91adb-158">Dans les sections suivantes, vous allez apprendre à activer chacun des services d’authentification externes fournis avec ASP.NET dans Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="91adb-158">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2017.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="91adb-159">Activation de l’authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="91adb-159">Enabling Facebook authentication</span></span>

<span data-ttu-id="91adb-160">L’utilisation de l’authentification Facebook vous oblige à créer un compte de développeur Facebook, et votre projet aura besoin d’un ID d’application et d’une clé secrète de Facebook pour fonctionner.</span><span class="sxs-lookup"><span data-stu-id="91adb-160">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="91adb-161">Pour plus d’informations sur la création d’un compte de développeur Facebook et l’obtention de votre ID d’application et de votre clé secrète, consultez [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="91adb-161">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="91adb-162">Une fois que vous avez obtenu votre ID d’application et votre clé secrète, procédez comme suit pour activer l’authentification Facebook pour votre application Web :</span><span class="sxs-lookup"><span data-stu-id="91adb-162">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="91adb-163">Lorsque votre projet est ouvert dans Visual Studio 2017, ouvrez le fichier *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="91adb-163">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="91adb-164">Recherchez la section de code d’authentification Facebook :</span><span class="sxs-lookup"><span data-stu-id="91adb-164">Locate the Facebook authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="91adb-165">Supprimez le &quot;//&quot; caractères pour supprimer les marques de commentaire des lignes de code en surbrillance, puis ajoutez votre ID d’application et votre clé secrète.</span><span class="sxs-lookup"><span data-stu-id="91adb-165">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="91adb-166">Une fois que vous avez ajouté ces paramètres, vous pouvez recompiler votre projet :</span><span class="sxs-lookup"><span data-stu-id="91adb-166">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="91adb-167">Quand vous appuyez sur F5 pour ouvrir votre application Web dans votre navigateur Web, vous verrez que Facebook a été défini en tant que service d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="91adb-167">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. <span data-ttu-id="91adb-168">Lorsque vous cliquez sur le bouton **Facebook** , votre navigateur est redirigé vers la page de connexion à Facebook :</span><span class="sxs-lookup"><span data-stu-id="91adb-168">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. <span data-ttu-id="91adb-169">Une fois que vous avez entré vos informations d’identification Facebook et cliqué sur **se connecter**, votre navigateur Web est redirigé vers votre application Web, ce qui vous invite à entrer le **nom d’utilisateur** que vous souhaitez associer à votre compte Facebook :</span><span class="sxs-lookup"><span data-stu-id="91adb-169">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. <span data-ttu-id="91adb-170">Une fois que vous avez entré votre nom d’utilisateur et cliqué sur le bouton s’inscrire, votre application Web affiche la **page** d' **installation** par défaut de votre compte Facebook :</span><span class="sxs-lookup"><span data-stu-id="91adb-170">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="91adb-171">Activation de l’authentification Google</span><span class="sxs-lookup"><span data-stu-id="91adb-171">Enabling Google Authentication</span></span>

<span data-ttu-id="91adb-172">L’utilisation de l’authentification Google vous oblige à créer un compte de développeur Google et votre projet nécessitera un ID d’application et une clé secrète de Google pour fonctionner.</span><span class="sxs-lookup"><span data-stu-id="91adb-172">Using Google authentication requires you to create a Google developer account, and your project will require an application ID and secret key from Google in order to function.</span></span> <span data-ttu-id="91adb-173">Pour plus d’informations sur la création d’un compte de développeur Google et l’obtention de votre ID d’application et de votre clé secrète, consultez [https://developers.google.com](https://developers.google.com).</span><span class="sxs-lookup"><span data-stu-id="91adb-173">For information about creating a Google developer account and obtaining your application ID and secret key, see [https://developers.google.com](https://developers.google.com).</span></span>

<span data-ttu-id="91adb-174">Pour activer l’authentification Google pour votre application Web, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="91adb-174">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="91adb-175">Lorsque votre projet est ouvert dans Visual Studio 2017, ouvrez le fichier *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="91adb-175">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="91adb-176">Recherchez la section de code d’authentification Google :</span><span class="sxs-lookup"><span data-stu-id="91adb-176">Locate the Google authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="91adb-177">Supprimez le &quot;//&quot; caractères pour supprimer les marques de commentaire des lignes de code en surbrillance, puis ajoutez votre ID d’application et votre clé secrète.</span><span class="sxs-lookup"><span data-stu-id="91adb-177">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="91adb-178">Une fois que vous avez ajouté ces paramètres, vous pouvez recompiler votre projet :</span><span class="sxs-lookup"><span data-stu-id="91adb-178">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="91adb-179">Lorsque vous appuyez sur F5 pour ouvrir votre application Web dans votre navigateur Web, vous verrez que Google a été défini en tant que service d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="91adb-179">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. <span data-ttu-id="91adb-180">Lorsque vous cliquez sur le bouton **Google** , votre navigateur est redirigé vers la page de connexion Google :</span><span class="sxs-lookup"><span data-stu-id="91adb-180">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. <span data-ttu-id="91adb-181">Après avoir entré vos informations d’identification Google et cliqué sur **se connecter**, Google vous invite à vérifier que votre application Web dispose des autorisations d’accès à votre compte Google :</span><span class="sxs-lookup"><span data-stu-id="91adb-181">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. <span data-ttu-id="91adb-182">Lorsque vous cliquez sur **accepter**, votre navigateur Web est redirigé vers votre application Web, ce qui vous invite à entrer le **nom d’utilisateur** que vous souhaitez associer à votre compte Google :</span><span class="sxs-lookup"><span data-stu-id="91adb-182">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. <span data-ttu-id="91adb-183">Une fois que vous avez entré votre nom d’utilisateur et cliqué sur le bouton s’inscrire, votre application Web affiche la **page** d' **installation** par défaut de votre compte Google :</span><span class="sxs-lookup"><span data-stu-id="91adb-183">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="91adb-184">Activation de l’authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="91adb-184">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="91adb-185">L’authentification Microsoft vous oblige à créer un compte de développeur et nécessite un ID client et une clé secrète client pour fonctionner.</span><span class="sxs-lookup"><span data-stu-id="91adb-185">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="91adb-186">Pour plus d’informations sur la création d’un compte de développeur Microsoft et sur l’obtention de votre ID client et la clé secrète client, consultez [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span><span class="sxs-lookup"><span data-stu-id="91adb-186">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="91adb-187">Une fois que vous avez obtenu votre clé de consommateur et le secret de consommateur, procédez comme suit pour activer l’authentification Microsoft pour votre application Web :</span><span class="sxs-lookup"><span data-stu-id="91adb-187">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="91adb-188">Lorsque votre projet est ouvert dans Visual Studio 2017, ouvrez le fichier *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="91adb-188">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="91adb-189">Recherchez la section d’authentification Microsoft de code :</span><span class="sxs-lookup"><span data-stu-id="91adb-189">Locate the Microsoft authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="91adb-190">Supprimez le &quot;//&quot; caractères pour supprimer les marques de commentaire des lignes de code en surbrillance, puis ajoutez votre ID client et votre clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="91adb-190">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="91adb-191">Une fois que vous avez ajouté ces paramètres, vous pouvez recompiler votre projet :</span><span class="sxs-lookup"><span data-stu-id="91adb-191">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="91adb-192">Lorsque vous appuyez sur F5 pour ouvrir votre application Web dans votre navigateur Web, vous verrez que Microsoft a été défini comme un service d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="91adb-192">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. <span data-ttu-id="91adb-193">Lorsque vous cliquez sur le bouton **Microsoft** , votre navigateur est redirigé vers la page de connexion Microsoft :</span><span class="sxs-lookup"><span data-stu-id="91adb-193">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. <span data-ttu-id="91adb-194">Après avoir entré vos informations d’identification Microsoft et cliqué sur **se connecter**, vous êtes invité à vérifier que votre application Web dispose des autorisations nécessaires pour accéder à votre compte Microsoft :</span><span class="sxs-lookup"><span data-stu-id="91adb-194">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. <span data-ttu-id="91adb-195">Lorsque vous cliquez sur **Oui**, votre navigateur Web est redirigé vers votre application Web, ce qui vous invite à entrer le **nom d’utilisateur** que vous souhaitez associer à votre compte Microsoft :</span><span class="sxs-lookup"><span data-stu-id="91adb-195">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. <span data-ttu-id="91adb-196">Une fois que vous avez entré votre nom d’utilisateur et cliqué sur le bouton s’inscrire, votre application Web affiche la **page** d' **installation** par défaut de votre compte Microsoft :</span><span class="sxs-lookup"><span data-stu-id="91adb-196">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="91adb-197">Activation de l’authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="91adb-197">Enabling Twitter Authentication</span></span>

<span data-ttu-id="91adb-198">L’authentification Twitter vous oblige à créer un compte de développeur et nécessite une clé de consommateur et un secret de consommateur pour fonctionner.</span><span class="sxs-lookup"><span data-stu-id="91adb-198">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="91adb-199">Pour plus d’informations sur la création d’un compte de développeur Twitter et sur l’obtention de votre clé de consommateur et le secret de consommateur, consultez [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="91adb-199">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="91adb-200">Une fois que vous avez obtenu votre clé de consommateur et le secret de consommateur, procédez comme suit pour activer l’authentification Twitter pour votre application Web :</span><span class="sxs-lookup"><span data-stu-id="91adb-200">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="91adb-201">Lorsque votre projet est ouvert dans Visual Studio 2017, ouvrez le fichier *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="91adb-201">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="91adb-202">Recherchez la section de code d’authentification Twitter :</span><span class="sxs-lookup"><span data-stu-id="91adb-202">Locate the Twitter authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="91adb-203">Supprimez le &quot;//&quot; caractères pour supprimer les marques de commentaire des lignes de code en surbrillance, puis ajoutez votre clé de consommateur et le secret de consommateur.</span><span class="sxs-lookup"><span data-stu-id="91adb-203">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="91adb-204">Une fois que vous avez ajouté ces paramètres, vous pouvez recompiler votre projet :</span><span class="sxs-lookup"><span data-stu-id="91adb-204">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="91adb-205">Quand vous appuyez sur F5 pour ouvrir votre application Web dans votre navigateur Web, vous verrez que Twitter a été défini en tant que service d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="91adb-205">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. <span data-ttu-id="91adb-206">Lorsque vous cliquez sur le bouton **Twitter** , votre navigateur est redirigé vers la page de connexion à Twitter :</span><span class="sxs-lookup"><span data-stu-id="91adb-206">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. <span data-ttu-id="91adb-207">Une fois que vous avez entré vos informations d’identification Twitter et cliqué sur **autoriser l’application**, votre navigateur Web est redirigé vers votre application Web, ce qui vous invite à entrer le **nom d’utilisateur** que vous souhaitez associer à votre compte Twitter :</span><span class="sxs-lookup"><span data-stu-id="91adb-207">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. <span data-ttu-id="91adb-208">Une fois que vous avez entré votre nom d’utilisateur et cliqué sur le bouton s’inscrire, votre application Web affiche la **page** d' **installation** par défaut de votre compte Twitter :</span><span class="sxs-lookup"><span data-stu-id="91adb-208">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="91adb-209">Informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="91adb-209">Additional Information</span></span>

<span data-ttu-id="91adb-210">Pour plus d’informations sur la création d’applications qui utilisent OAuth et OpenID, consultez les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="91adb-210">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="91adb-211">Combinaison de services d’authentification externes</span><span class="sxs-lookup"><span data-stu-id="91adb-211">Combining External Authentication Services</span></span>

<span data-ttu-id="91adb-212">Pour une plus grande flexibilité, vous pouvez définir simultanément plusieurs services d’authentification externes. cela permet aux utilisateurs de votre application Web d’utiliser un compte à partir de n’importe quel service d’authentification externe activé :</span><span class="sxs-lookup"><span data-stu-id="91adb-212">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="91adb-213">Configurer IIS Express pour utiliser un nom de domaine complet</span><span class="sxs-lookup"><span data-stu-id="91adb-213">Configure IIS Express to use a fully qualified domain name</span></span>

<span data-ttu-id="91adb-214">Certains fournisseurs d’authentification externes ne prennent pas en charge le test de votre application à l’aide d’une adresse HTTP comme `http://localhost:port/`.</span><span class="sxs-lookup"><span data-stu-id="91adb-214">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="91adb-215">Pour contourner ce problème, vous pouvez ajouter un mappage de nom de domaine complet (FQDN) statique à votre fichier HOSTs et configurer vos options de projet dans Visual Studio 2017 afin d’utiliser le nom de domaine complet (FQDN) à des fins de test ou de débogage.</span><span class="sxs-lookup"><span data-stu-id="91adb-215">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2017 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="91adb-216">Pour ce faire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="91adb-216">To do so, use the following steps:</span></span>

- <span data-ttu-id="91adb-217">Ajoutez un nom de domaine complet statique qui mappe votre fichier HOSTs :</span><span class="sxs-lookup"><span data-stu-id="91adb-217">Add a static FQDN mapping your HOSTS file:</span></span>

  1. <span data-ttu-id="91adb-218">Ouvrez une invite de commandes avec élévation de privilèges dans Windows.</span><span class="sxs-lookup"><span data-stu-id="91adb-218">Open an elevated command prompt in Windows.</span></span>
  2. <span data-ttu-id="91adb-219">Tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="91adb-219">Type the following command:</span></span>

      <span data-ttu-id="91adb-220"><kbd>bloc-notes%WinDir%\system32\drivers\etc\hosts</kbd></span><span class="sxs-lookup"><span data-stu-id="91adb-220"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
  3. <span data-ttu-id="91adb-221">Ajoutez une entrée semblable à la suivante au fichier HOSTs :</span><span class="sxs-lookup"><span data-stu-id="91adb-221">Add an entry like the following to the HOSTS file:</span></span>

      <span data-ttu-id="91adb-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="91adb-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
  4. <span data-ttu-id="91adb-223">Enregistrez et fermez votre fichier HOSTs.</span><span class="sxs-lookup"><span data-stu-id="91adb-223">Save and close your HOSTS file.</span></span>

- <span data-ttu-id="91adb-224">Configurez votre projet Visual Studio pour utiliser le nom de domaine complet :</span><span class="sxs-lookup"><span data-stu-id="91adb-224">Configure your Visual Studio project to use the FQDN:</span></span>

  1. <span data-ttu-id="91adb-225">Lorsque votre projet est ouvert dans Visual Studio 2017, cliquez sur le menu **projet** , puis sélectionnez les propriétés de votre projet.</span><span class="sxs-lookup"><span data-stu-id="91adb-225">When your project is open in Visual Studio 2017, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="91adb-226">Par exemple, vous pouvez sélectionner **Propriétés de WebApplication1**.</span><span class="sxs-lookup"><span data-stu-id="91adb-226">For example, you might select **WebApplication1 Properties**.</span></span>
  2. <span data-ttu-id="91adb-227">Sélectionnez l’onglet **Web** .</span><span class="sxs-lookup"><span data-stu-id="91adb-227">Select the **Web** tab.</span></span>
  3. <span data-ttu-id="91adb-228">Entrez votre nom de domaine complet pour l' <strong>URL du projet</strong>.</span><span class="sxs-lookup"><span data-stu-id="91adb-228">Enter your FQDN for the <strong>Project Url</strong>.</span></span> <span data-ttu-id="91adb-229">Par exemple, vous entrez <kbd><http://www.wingtiptoys.com></kbd> si c’était le mappage de nom de domaine complet que vous avez ajouté à votre fichier hosts.</span><span class="sxs-lookup"><span data-stu-id="91adb-229">For example, you would enter <kbd><http://www.wingtiptoys.com></kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>

- <span data-ttu-id="91adb-230">Configurez IIS Express pour utiliser le nom de domaine complet de votre application :</span><span class="sxs-lookup"><span data-stu-id="91adb-230">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="91adb-231">Ouvrez une invite de commandes avec élévation de privilèges dans Windows.</span><span class="sxs-lookup"><span data-stu-id="91adb-231">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="91adb-232">Tapez la commande suivante pour passer au dossier IIS Express :</span><span class="sxs-lookup"><span data-stu-id="91adb-232">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="91adb-233"><kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="91adb-233"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="91adb-234">Tapez la commande suivante pour ajouter le nom de domaine complet à votre application :</span><span class="sxs-lookup"><span data-stu-id="91adb-234">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="91adb-235"><kbd>appcmd. exe set config-section : System. applicationHost/sites/+&quot;[name = 'Webapplication1 ']. Bindings. [Protocol = 'http', bindingInformation = ' \* : 80 : www. wingtiptoys. com']&quot;/commit : apphost</kbd></span><span class="sxs-lookup"><span data-stu-id="91adb-235"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

  <span data-ttu-id="91adb-236">Où **WebApplication1** est le nom de votre projet et **bindingInformation** contient le numéro de port et le nom de domaine complet que vous souhaitez utiliser pour vos tests.</span><span class="sxs-lookup"><span data-stu-id="91adb-236">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="91adb-237">Comment obtenir les paramètres de votre application pour l’authentification Microsoft</span><span class="sxs-lookup"><span data-stu-id="91adb-237">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="91adb-238">La liaison d’une application à Windows Live pour l’authentification Microsoft est un processus simple.</span><span class="sxs-lookup"><span data-stu-id="91adb-238">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="91adb-239">Si vous n’avez pas encore lié une application à Windows Live, vous pouvez utiliser les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="91adb-239">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="91adb-240">Accédez à [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) et entrez le nom et le mot de passe de votre compte Microsoft lorsque vous y êtes invité, puis cliquez sur **se connecter**:</span><span class="sxs-lookup"><span data-stu-id="91adb-240">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. <span data-ttu-id="91adb-241">Sélectionnez **Ajouter une application** et entrez le nom de votre application lorsque vous y êtes invité, puis cliquez sur **créer**:</span><span class="sxs-lookup"><span data-stu-id="91adb-241">Select **Add an app** and enter the name of your application when prompted, and then click **Create**:</span></span>

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. <span data-ttu-id="91adb-242">Sélectionnez votre application sous **nom** et sa page Propriétés de l’application s’affiche.</span><span class="sxs-lookup"><span data-stu-id="91adb-242">Select your app under **Name** and its application properties page appears.</span></span>

4. <span data-ttu-id="91adb-243">Entrez le domaine de redirection pour votre application.</span><span class="sxs-lookup"><span data-stu-id="91adb-243">Enter the redirect domain for your application.</span></span> <span data-ttu-id="91adb-244">Copiez l’ID de l' **application** et, sous secrets de l' **application**, sélectionnez **générer le mot de passe**.</span><span class="sxs-lookup"><span data-stu-id="91adb-244">Copy the **Application ID** and, under **Application Secrets**, select **Generate Password**.</span></span> <span data-ttu-id="91adb-245">Copiez le mot de passe qui s’affiche.</span><span class="sxs-lookup"><span data-stu-id="91adb-245">Copy the password that appears.</span></span> <span data-ttu-id="91adb-246">L’ID de l’application et le mot de passe sont votre ID client et votre clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="91adb-246">The application ID and password are your client ID and client secret.</span></span> <span data-ttu-id="91adb-247">Sélectionnez **OK** , puis **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="91adb-247">Select **Ok** and then **Save**.</span></span>

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="91adb-248">Facultatif : désactiver l’inscription locale</span><span class="sxs-lookup"><span data-stu-id="91adb-248">Optional: Disable Local Registration</span></span>

<span data-ttu-id="91adb-249">La fonctionnalité d’inscription locale ASP.NET actuelle n’empêche pas les programmes automatisés (robots) de créer des comptes membres. par exemple, en utilisant une technologie de validation et de prévention des robots comme [captcha](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span><span class="sxs-lookup"><span data-stu-id="91adb-249">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="91adb-250">Pour cette raison, vous devez supprimer le formulaire de connexion locale et le lien d’inscription sur la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="91adb-250">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="91adb-251">Pour ce faire, ouvrez la page *\_login. cshtml* dans votre projet, puis commentez les lignes pour le panneau de connexion local et le lien d’inscription.</span><span class="sxs-lookup"><span data-stu-id="91adb-251">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="91adb-252">La page résultante doit ressembler à l’exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="91adb-252">The resulting page should look like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="91adb-253">Une fois que le panneau de connexion local et le lien d’inscription ont été désactivés, votre page de connexion affiche uniquement les fournisseurs d’authentification externes que vous avez activés :</span><span class="sxs-lookup"><span data-stu-id="91adb-253">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
