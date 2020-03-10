---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Partie 7 : appartenance et autorisation | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 7 couvre l’appartenance et l’autorisation.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539199"
---
# <a name="part-7-membership-and-authorization"></a><span data-ttu-id="2a370-104">Partie 7 : appartenance et autorisation</span><span class="sxs-lookup"><span data-stu-id="2a370-104">Part 7: Membership and Authorization</span></span>

<span data-ttu-id="2a370-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="2a370-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="2a370-106">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="2a370-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="2a370-107">Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="2a370-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="2a370-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="2a370-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="2a370-109">La partie 7 couvre l’appartenance et l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="2a370-109">Part 7 covers Membership and Authorization.</span></span>

<span data-ttu-id="2a370-110">Notre contrôleur Store Manager est actuellement accessible à toute personne visitant notre site.</span><span class="sxs-lookup"><span data-stu-id="2a370-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="2a370-111">Modifions ceci pour restreindre l’autorisation aux administrateurs de site.</span><span class="sxs-lookup"><span data-stu-id="2a370-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="2a370-112">Ajout de AccountController et de vues</span><span class="sxs-lookup"><span data-stu-id="2a370-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="2a370-113">L’une des différences entre le modèle d’application Web ASP.NET MVC 3 complet et le modèle d’application Web vide ASP.NET MVC 3 est que le modèle vide n’inclut pas de contrôleur de compte.</span><span class="sxs-lookup"><span data-stu-id="2a370-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="2a370-114">Nous allons ajouter un contrôleur de compte en copiant quelques fichiers à partir d’une nouvelle application ASP.NET MVC créée à partir du modèle d’application Web ASP.NET MVC 3 complet.</span><span class="sxs-lookup"><span data-stu-id="2a370-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="2a370-115">Créez une nouvelle application ASP.NET MVC à l’aide du modèle d’application Web ASP.NET MVC 3 complet et copiez les fichiers suivants dans les mêmes répertoires de notre projet :</span><span class="sxs-lookup"><span data-stu-id="2a370-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="2a370-116">Copier AccountController.cs dans le répertoire Controllers</span><span class="sxs-lookup"><span data-stu-id="2a370-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="2a370-117">Copier AccountModels dans le répertoire des modèles</span><span class="sxs-lookup"><span data-stu-id="2a370-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="2a370-118">Créer un répertoire de compte dans le répertoire views et copier les quatre affichages dans</span><span class="sxs-lookup"><span data-stu-id="2a370-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="2a370-119">Modifiez l’espace de noms pour les classes de contrôleur et de modèle afin qu’elles commencent par MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="2a370-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="2a370-120">La classe AccountController doit utiliser l’espace de noms MvcMusicStore. Controllers, et la classe AccountModels doit utiliser l’espace de noms MvcMusicStore. Models.</span><span class="sxs-lookup"><span data-stu-id="2a370-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="2a370-121">*Remarque : ces fichiers sont également disponibles dans le téléchargement MvcMusicStore-Assets. zip à partir duquel nous avons copié nos fichiers de conception de site au début du didacticiel. Les fichiers d’appartenance se trouvent dans le répertoire de code.*</span><span class="sxs-lookup"><span data-stu-id="2a370-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="2a370-122">La solution mise à jour doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="2a370-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="2a370-123">Ajout d’un utilisateur administratif à l’aide du site de configuration ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2a370-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="2a370-124">Avant de demander une autorisation sur notre site Web, nous devrons créer un utilisateur avec accès.</span><span class="sxs-lookup"><span data-stu-id="2a370-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="2a370-125">Le moyen le plus simple de créer un utilisateur consiste à utiliser le site Web de configuration ASP.NET intégré.</span><span class="sxs-lookup"><span data-stu-id="2a370-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="2a370-126">Lancez le site Web de configuration ASP.NET en cliquant sur le suivi de l’icône dans la Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="2a370-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="2a370-127">Un site Web de configuration est lancé.</span><span class="sxs-lookup"><span data-stu-id="2a370-127">This launches a configuration website.</span></span> <span data-ttu-id="2a370-128">Cliquez sur l’onglet sécurité sur l’écran d’accueil, puis cliquez sur le lien activer les rôles au centre de l’écran.</span><span class="sxs-lookup"><span data-stu-id="2a370-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="2a370-129">Cliquez sur le lien « créer ou gérer des rôles ».</span><span class="sxs-lookup"><span data-stu-id="2a370-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="2a370-130">Entrez « administrateur » comme nom de rôle, puis cliquez sur le bouton Ajouter un rôle.</span><span class="sxs-lookup"><span data-stu-id="2a370-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="2a370-131">Cliquez sur le bouton précédent, puis sur le lien créer un utilisateur sur le côté gauche.</span><span class="sxs-lookup"><span data-stu-id="2a370-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="2a370-132">Renseignez les champs d’informations utilisateur sur la gauche à l’aide des informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="2a370-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="2a370-133">**Champ**</span><span class="sxs-lookup"><span data-stu-id="2a370-133">**Field**</span></span> | <span data-ttu-id="2a370-134">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="2a370-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="2a370-135">**Nom d'utilisateur**</span><span class="sxs-lookup"><span data-stu-id="2a370-135">**User Name**</span></span> | <span data-ttu-id="2a370-136">Administrateur</span><span class="sxs-lookup"><span data-stu-id="2a370-136">Administrator</span></span> |
| <span data-ttu-id="2a370-137">**Mot de passe**</span><span class="sxs-lookup"><span data-stu-id="2a370-137">**Password**</span></span> | <span data-ttu-id="2a370-138">password123!</span><span class="sxs-lookup"><span data-stu-id="2a370-138">password123!</span></span> |
| <span data-ttu-id="2a370-139">**Confirmer le mot de passe**</span><span class="sxs-lookup"><span data-stu-id="2a370-139">**Confirm Password**</span></span> | <span data-ttu-id="2a370-140">password123!</span><span class="sxs-lookup"><span data-stu-id="2a370-140">password123!</span></span> |
| <span data-ttu-id="2a370-141">**Message électronique**</span><span class="sxs-lookup"><span data-stu-id="2a370-141">**E-mail**</span></span> | <span data-ttu-id="2a370-142">(toute adresse de messagerie fonctionnera)</span><span class="sxs-lookup"><span data-stu-id="2a370-142">(any email address will work)</span></span> |
| <span data-ttu-id="2a370-143">**Question de sécurité**</span><span class="sxs-lookup"><span data-stu-id="2a370-143">**Security Question**</span></span> | <span data-ttu-id="2a370-144">(tout ce que vous aimez)</span><span class="sxs-lookup"><span data-stu-id="2a370-144">(whatever you like)</span></span> |
| <span data-ttu-id="2a370-145">**Réponse de sécurité**</span><span class="sxs-lookup"><span data-stu-id="2a370-145">**Security Answer**</span></span> | <span data-ttu-id="2a370-146">(tout ce que vous aimez)</span><span class="sxs-lookup"><span data-stu-id="2a370-146">(whatever you like)</span></span> |

<span data-ttu-id="2a370-147">*Remarque : vous pouvez bien sûr utiliser le mot de passe de votre choix. Les paramètres de sécurité de mot de passe par défaut requièrent un mot de passe de 7 caractères et contiennent un caractère non alphanumérique.*</span><span class="sxs-lookup"><span data-stu-id="2a370-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="2a370-148">Sélectionnez le rôle administrateur pour cet utilisateur, puis cliquez sur le bouton créer un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2a370-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="2a370-149">À ce stade, vous devriez voir un message indiquant que l’utilisateur a été créé avec succès.</span><span class="sxs-lookup"><span data-stu-id="2a370-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="2a370-150">Vous pouvez maintenant fermer la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="2a370-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="2a370-151">Autorisation basée sur les rôles</span><span class="sxs-lookup"><span data-stu-id="2a370-151">Role-based Authorization</span></span>

<span data-ttu-id="2a370-152">À présent, nous pouvons limiter l’accès à StoreManagerController à l’aide de l’attribut [Authorize], en spécifiant que l’utilisateur doit avoir le rôle d’administrateur pour accéder à n’importe quelle action de contrôleur dans la classe.</span><span class="sxs-lookup"><span data-stu-id="2a370-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="2a370-153">*Remarque : l’attribut [Authorize] peut être placé sur des méthodes d’action spécifiques, ainsi que au niveau de la classe du contrôleur.*</span><span class="sxs-lookup"><span data-stu-id="2a370-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="2a370-154">À présent, la navigation vers/StoreManager affiche une boîte de dialogue de connexion :</span><span class="sxs-lookup"><span data-stu-id="2a370-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="2a370-155">Une fois connecté avec notre nouveau compte d’administrateur, nous pouvons accéder à l’écran de modification de l’album comme auparavant.</span><span class="sxs-lookup"><span data-stu-id="2a370-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2a370-156">[Précédent](mvc-music-store-part-6.md)
> [Suivant](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="2a370-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
