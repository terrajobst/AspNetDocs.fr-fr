---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Partie 9 : inscription et extraction | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 9 couvre l’inscription et l’extraction.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559534"
---
# <a name="part-9-registration-and-checkout"></a><span data-ttu-id="211b0-104">Partie 9 : inscription et extraction</span><span class="sxs-lookup"><span data-stu-id="211b0-104">Part 9: Registration and Checkout</span></span>

<span data-ttu-id="211b0-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="211b0-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="211b0-106">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="211b0-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="211b0-107">Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="211b0-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="211b0-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="211b0-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="211b0-109">La partie 9 couvre l’inscription et l’extraction.</span><span class="sxs-lookup"><span data-stu-id="211b0-109">Part 9 covers Registration and Checkout.</span></span>

<span data-ttu-id="211b0-110">Dans cette section, nous allons créer un CheckoutController qui collecte les informations relatives à l’adresse et au paiement du client.</span><span class="sxs-lookup"><span data-stu-id="211b0-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="211b0-111">Nous aurons besoin que les utilisateurs s’inscrivent auprès de notre site avant de procéder à l’extraction, de sorte que ce contrôleur nécessite une autorisation.</span><span class="sxs-lookup"><span data-stu-id="211b0-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="211b0-112">Les utilisateurs accèdent au processus de validation à partir de leur panier en cliquant sur le bouton « Checkout ».</span><span class="sxs-lookup"><span data-stu-id="211b0-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="211b0-113">Si l’utilisateur n’est pas connecté, il est invité à le faire.</span><span class="sxs-lookup"><span data-stu-id="211b0-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="211b0-114">Une fois la connexion établie, l’utilisateur affiche l’affichage adresse et paiement.</span><span class="sxs-lookup"><span data-stu-id="211b0-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="211b0-115">Une fois qu’ils ont rempli le formulaire et envoyé la commande, l’écran de confirmation de commande s’affiche.</span><span class="sxs-lookup"><span data-stu-id="211b0-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="211b0-116">Si vous tentez d’afficher un ordre inexistant ou une commande qui n’appartient pas à vous, l’affichage des erreurs s’affiche.</span><span class="sxs-lookup"><span data-stu-id="211b0-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="211b0-117">Migration du panier d’achat</span><span class="sxs-lookup"><span data-stu-id="211b0-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="211b0-118">Lorsque le processus d’achat est anonyme, lorsque l’utilisateur clique sur le bouton Checkout, il doit s’inscrire et se connecter.</span><span class="sxs-lookup"><span data-stu-id="211b0-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="211b0-119">Les utilisateurs s’attendent à conserver leurs informations sur le panier d’achat entre les visites. nous devons donc associer les informations du panier d’achat à un utilisateur lors de l’inscription ou de la connexion.</span><span class="sxs-lookup"><span data-stu-id="211b0-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="211b0-120">C’est en fait très simple à faire, car notre classe ShoppingCart a déjà une méthode qui associe tous les éléments du panier en cours à un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="211b0-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="211b0-121">Il vous suffit d’appeler cette méthode lorsqu’un utilisateur termine l’inscription ou la connexion.</span><span class="sxs-lookup"><span data-stu-id="211b0-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="211b0-122">Ouvrez la classe **AccountController** que nous avons ajoutée lors de la configuration de l’appartenance et de l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="211b0-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="211b0-123">Ajoutez une instruction using référençant MvcMusicStore. Models, puis ajoutez la méthode MigrateShoppingCart suivante :</span><span class="sxs-lookup"><span data-stu-id="211b0-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="211b0-124">Modifiez ensuite l’action de publication d’ouverture de session pour appeler MigrateShoppingCart une fois que l’utilisateur a été validé, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="211b0-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="211b0-125">Apportez la même modification à l’action de publication du Registre, juste après la création du compte d’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="211b0-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="211b0-126">Voilà, un panier d’achat anonyme sera automatiquement transféré vers un compte d’utilisateur lors de l’inscription ou de la connexion.</span><span class="sxs-lookup"><span data-stu-id="211b0-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="211b0-127">Création du CheckoutController</span><span class="sxs-lookup"><span data-stu-id="211b0-127">Creating the CheckoutController</span></span>

<span data-ttu-id="211b0-128">Cliquez avec le bouton droit sur le dossier Controllers et ajoutez un nouveau contrôleur au projet nommé CheckoutController à l’aide du modèle de contrôleur vide.</span><span class="sxs-lookup"><span data-stu-id="211b0-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="211b0-129">Tout d’abord, ajoutez l’attribut Authorize au-dessus de la déclaration de classe du contrôleur pour demander aux utilisateurs de s’inscrire avant d’effectuer l’extraction :</span><span class="sxs-lookup"><span data-stu-id="211b0-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="211b0-130">*Remarque : cela est similaire à la modification que nous avons apportée précédemment à StoreManagerController, mais dans ce cas, l’attribut Authorize nécessite que l’utilisateur soit dans un rôle d’administrateur. Dans le contrôleur d’extraction, nous exigeons que l’utilisateur soit connecté, mais qu’il ne soit pas obligé d’être administrateur.*</span><span class="sxs-lookup"><span data-stu-id="211b0-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="211b0-131">Par souci de simplicité, nous ne traiterons pas les informations de paiement dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="211b0-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="211b0-132">Au lieu de cela, nous autorisons les utilisateurs à extraire à l’aide d’un code promotionnel.</span><span class="sxs-lookup"><span data-stu-id="211b0-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="211b0-133">Nous allons stocker ce code promotionnel à l’aide d’une constante nommée code promo.</span><span class="sxs-lookup"><span data-stu-id="211b0-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="211b0-134">Comme dans le StoreController, nous allons déclarer un champ qui contiendra une instance de la classe MusicStoreEntities, nommée storeDB.</span><span class="sxs-lookup"><span data-stu-id="211b0-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="211b0-135">Pour utiliser la classe MusicStoreEntities, vous devez ajouter une instruction using pour l’espace de noms MvcMusicStore. Models.</span><span class="sxs-lookup"><span data-stu-id="211b0-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="211b0-136">La partie supérieure de notre contrôleur de validation apparaît ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="211b0-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="211b0-137">Le CheckoutController aura les actions de contrôleur suivantes :</span><span class="sxs-lookup"><span data-stu-id="211b0-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="211b0-138">**AddressAndPayment (méthode d’extraction)** affiche un formulaire pour permettre à l’utilisateur d’entrer ses informations.</span><span class="sxs-lookup"><span data-stu-id="211b0-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="211b0-139">**AddressAndPayment (méthode de publication)** valide l’entrée et traite la commande.</span><span class="sxs-lookup"><span data-stu-id="211b0-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="211b0-140">**Terminer** s’affiche une fois que l’utilisateur a terminé le processus d’extraction.</span><span class="sxs-lookup"><span data-stu-id="211b0-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="211b0-141">Cette vue inclut le numéro de commande de l’utilisateur comme confirmation.</span><span class="sxs-lookup"><span data-stu-id="211b0-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="211b0-142">Tout d’abord, nous allons renommer l’action du contrôleur d’index (générée lors de la création du contrôleur) sur AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="211b0-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="211b0-143">Cette action de contrôleur affiche simplement le formulaire d’extraction, de sorte qu’elle ne nécessite aucune information de modèle.</span><span class="sxs-lookup"><span data-stu-id="211b0-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="211b0-144">Notre méthode de publication AddressAndPayment suit le même modèle que celui que nous avons utilisé dans le StoreManagerController : il essaiera d’accepter l’envoi du formulaire et de compléter la commande, puis d’afficher de nouveau le formulaire en cas d’échec.</span><span class="sxs-lookup"><span data-stu-id="211b0-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="211b0-145">Une fois que la validation de l’entrée de formulaire est conforme à nos exigences de validation pour une commande, nous vérifions directement la valeur du formulaire code promo.</span><span class="sxs-lookup"><span data-stu-id="211b0-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="211b0-146">En supposant que tout est correct, nous enregistrons les informations mises à jour dans la commande, indiquons à l’objet ShoppingCart de terminer le processus de commande et redirigent vers l’action terminer.</span><span class="sxs-lookup"><span data-stu-id="211b0-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="211b0-147">Une fois le processus d’extraction terminé, les utilisateurs sont redirigés vers l’action de contrôleur complète.</span><span class="sxs-lookup"><span data-stu-id="211b0-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="211b0-148">Cette action effectue une vérification simple pour confirmer que la commande appartient effectivement à l’utilisateur connecté avant d’afficher le numéro de commande en tant que confirmation.</span><span class="sxs-lookup"><span data-stu-id="211b0-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="211b0-149">*Remarque : l’affichage des erreurs a été créé automatiquement pour nous dans le dossier/Views/Shared lorsque nous avons commencé le projet.*</span><span class="sxs-lookup"><span data-stu-id="211b0-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="211b0-150">Le code CheckoutController complet est le suivant :</span><span class="sxs-lookup"><span data-stu-id="211b0-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="211b0-151">Ajout de la vue AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="211b0-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="211b0-152">Créons maintenant la vue AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="211b0-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="211b0-153">Cliquez avec le bouton droit sur l’une des actions du contrôleur AddressAndPayment et ajoutez une vue nommée AddressAndPayment, qui est fortement typée comme un ordre et utilise le modèle de modification, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="211b0-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="211b0-154">Cette vue utilise deux des techniques que nous avons examinées lors de la création de la vue StoreManagerEdit :</span><span class="sxs-lookup"><span data-stu-id="211b0-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="211b0-155">Nous utiliserons html. EditorForModel () pour afficher les champs de formulaire pour le modèle de commande</span><span class="sxs-lookup"><span data-stu-id="211b0-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="211b0-156">Nous allons utiliser des règles de validation à l’aide d’une classe Order avec des attributs de validation</span><span class="sxs-lookup"><span data-stu-id="211b0-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="211b0-157">Nous allons commencer par mettre à jour le code du formulaire pour utiliser HTML. EditorForModel (), suivi d’une zone de texte supplémentaire pour le code de promotion.</span><span class="sxs-lookup"><span data-stu-id="211b0-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="211b0-158">Le code complet de la vue AddressAndPayment est illustré ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="211b0-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="211b0-159">Définition des règles de validation pour la commande</span><span class="sxs-lookup"><span data-stu-id="211b0-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="211b0-160">Maintenant que notre vue est configurée, nous allons configurer les règles de validation pour notre modèle de commande, comme nous l’avons fait précédemment pour le modèle d’album.</span><span class="sxs-lookup"><span data-stu-id="211b0-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="211b0-161">Cliquez avec le bouton droit sur le dossier Models et ajoutez une classe nommée Order.</span><span class="sxs-lookup"><span data-stu-id="211b0-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="211b0-162">En plus des attributs de validation que nous avons utilisés précédemment pour l’album, nous utiliserons également une expression régulière pour valider l’adresse de messagerie de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="211b0-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="211b0-163">Si vous tentez d’envoyer le formulaire avec des informations manquantes ou non valides, un message d’erreur s’affiche à l’aide de la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="211b0-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="211b0-164">Bien, nous avons effectué la plupart des tâches difficiles pour le processus de validation. Nous avons juste quelques chances et se terminent.</span><span class="sxs-lookup"><span data-stu-id="211b0-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="211b0-165">Nous devons ajouter deux vues simples et nous devons prendre en charge le transfert des informations du panier pendant le processus de connexion.</span><span class="sxs-lookup"><span data-stu-id="211b0-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="211b0-166">Ajout de la vue d’extraction complète</span><span class="sxs-lookup"><span data-stu-id="211b0-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="211b0-167">La vue extraction terminée est assez simple, car elle doit simplement afficher l’ID de commande.</span><span class="sxs-lookup"><span data-stu-id="211b0-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="211b0-168">Cliquez avec le bouton droit sur l’action terminer le contrôleur et ajoutez une vue nommée Complete, qui est fortement typée comme int.</span><span class="sxs-lookup"><span data-stu-id="211b0-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="211b0-169">À présent, nous allons mettre à jour le code de vue pour afficher l’ID de commande, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="211b0-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="211b0-170">Mise à jour de l’affichage des erreurs</span><span class="sxs-lookup"><span data-stu-id="211b0-170">Updating The Error view</span></span>

<span data-ttu-id="211b0-171">Le modèle par défaut comprend un affichage des erreurs dans le dossier vues partagées afin qu’il puisse être réutilisé ailleurs dans le site.</span><span class="sxs-lookup"><span data-stu-id="211b0-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="211b0-172">Cet affichage des erreurs contient une erreur très simple et n’utilise pas notre disposition de site. nous allons donc le mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="211b0-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="211b0-173">Étant donné qu’il s’agit d’une page d’erreur générique, le contenu est très simple.</span><span class="sxs-lookup"><span data-stu-id="211b0-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="211b0-174">Nous inclurons un message et un lien pour accéder à la page précédente dans l’historique si l’utilisateur souhaite réessayer son action.</span><span class="sxs-lookup"><span data-stu-id="211b0-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> <span data-ttu-id="211b0-175">[Précédent](mvc-music-store-part-8.md)
> [Suivant](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="211b0-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
