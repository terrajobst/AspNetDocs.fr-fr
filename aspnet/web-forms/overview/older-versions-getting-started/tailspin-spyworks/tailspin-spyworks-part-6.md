---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Partie 6 : ASP.NET Membership | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 6 ajoute l’appartenance ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564182"
---
# <a name="part-6-aspnet-membership"></a><span data-ttu-id="4dc77-104">Partie 6 : appartenance à ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4dc77-104">Part 6: ASP.NET Membership</span></span>

<span data-ttu-id="4dc77-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4dc77-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4dc77-106">Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="4dc77-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4dc77-107">Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="4dc77-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4dc77-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="4dc77-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4dc77-109">La partie 6 ajoute l’appartenance ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4dc77-109">Part 6 adds ASP.NET Membership.</span></span>

## <a id="_Toc260221672"></a><span data-ttu-id="4dc77-110">Utilisation de l’appartenance ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4dc77-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="4dc77-111">Cliquer sur sécurité</span><span class="sxs-lookup"><span data-stu-id="4dc77-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="4dc77-112">Assurez-vous que nous utilisons l’authentification par formulaire.</span><span class="sxs-lookup"><span data-stu-id="4dc77-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="4dc77-113">Utilisez le lien « créer un utilisateur » pour créer deux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="4dc77-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="4dc77-114">Lorsque vous avez terminé, reportez-vous à la fenêtre de Explorateur de solutions et actualisez la vue.</span><span class="sxs-lookup"><span data-stu-id="4dc77-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="4dc77-115">Notez que ASPNETDB. MDF fine a été créé.</span><span class="sxs-lookup"><span data-stu-id="4dc77-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="4dc77-116">Ce fichier contient les tables pour prendre en charge les services ASP.NET principaux comme l’appartenance.</span><span class="sxs-lookup"><span data-stu-id="4dc77-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="4dc77-117">Nous pouvons maintenant commencer à implémenter le processus d’extraction.</span><span class="sxs-lookup"><span data-stu-id="4dc77-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="4dc77-118">Commencez par créer une page CheckOut. aspx.</span><span class="sxs-lookup"><span data-stu-id="4dc77-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="4dc77-119">La page CheckOut. aspx ne doit être accessible qu’aux utilisateurs qui se sont connectés. nous allons donc restreindre l’accès aux utilisateurs connectés et rediriger les utilisateurs qui ne sont pas connectés à la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="4dc77-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="4dc77-120">Pour ce faire, nous allons ajouter le code suivant à la section de configuration de notre fichier Web. config.</span><span class="sxs-lookup"><span data-stu-id="4dc77-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="4dc77-121">Le modèle pour ASP.NET Web Forms applications a automatiquement ajouté une section d’authentification à notre fichier Web. config et a établi la page de connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="4dc77-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="4dc77-122">Nous devons modifier le fichier code-behind login. aspx pour migrer un panier d’achat anonyme lorsque l’utilisateur se connecte.</span><span class="sxs-lookup"><span data-stu-id="4dc77-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="4dc77-123">Modifiez la page\_événement de chargement comme suit.</span><span class="sxs-lookup"><span data-stu-id="4dc77-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="4dc77-124">Ajoutez ensuite un gestionnaire d’événements « journalisé » comme celui-ci pour définir le nom de session sur l’utilisateur qui vient d’être connecté, puis remplacez l’ID de session temporaire du panier d’achat par celui de l’utilisateur en appelant la méthode MigrateCart dans notre classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="4dc77-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="4dc77-125">(Implémenté dans le fichier. cs)</span><span class="sxs-lookup"><span data-stu-id="4dc77-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="4dc77-126">Implémentez la méthode MigrateCart () comme suit.</span><span class="sxs-lookup"><span data-stu-id="4dc77-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="4dc77-127">Dans Checkout. aspx, nous allons utiliser un contrôle EntityDataSource et un GridView dans notre page d’extraction de la même façon que nous l’avons fait dans la page du panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="4dc77-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="4dc77-128">Notez que notre contrôle GridView spécifie un gestionnaire d’événements « OnDataBound » nommé MyList\_RowDataBound donc implémenter ce gestionnaire d’événements comme celui-ci.</span><span class="sxs-lookup"><span data-stu-id="4dc77-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="4dc77-129">Cette méthode conserve un total cumulé du panier d’achat à mesure que chaque ligne est liée et met à jour la ligne inférieure du contrôle GridView.</span><span class="sxs-lookup"><span data-stu-id="4dc77-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="4dc77-130">À ce niveau, nous avons mis en œuvre une présentation « Review » (révision) de la commande à placer.</span><span class="sxs-lookup"><span data-stu-id="4dc77-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="4dc77-131">Nous allons gérer un scénario de panier vide en ajoutant quelques lignes de code à notre page\_événement de chargement :</span><span class="sxs-lookup"><span data-stu-id="4dc77-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="4dc77-132">Quand l’utilisateur clique sur le bouton « envoyer », nous allons exécuter le code suivant dans le gestionnaire d’événements de clic sur le bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="4dc77-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="4dc77-133">La « viande » du processus de soumission de commande doit être implémentée dans la méthode SubmitOrder () de notre classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="4dc77-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="4dc77-134">SubmitOrder :</span><span class="sxs-lookup"><span data-stu-id="4dc77-134">SubmitOrder will:</span></span>

- <span data-ttu-id="4dc77-135">Prenez toutes les lignes du panier d’achat et utilisez-les pour créer un nouvel enregistrement de commande et les enregistrements OrderDetails associés.</span><span class="sxs-lookup"><span data-stu-id="4dc77-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="4dc77-136">Calculez la date d’expédition.</span><span class="sxs-lookup"><span data-stu-id="4dc77-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="4dc77-137">Effacez le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="4dc77-137">Clear the shopping cart.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="4dc77-138">Dans le cadre de cet exemple d’application, nous calculerons une date d’expédition en ajoutant simplement deux jours à la date actuelle.</span><span class="sxs-lookup"><span data-stu-id="4dc77-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="4dc77-139">L’exécution de l’application nous permettra de tester le processus d’achat du début à la fin.</span><span class="sxs-lookup"><span data-stu-id="4dc77-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4dc77-140">[Précédent](tailspin-spyworks-part-5.md)
> [Suivant](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="4dc77-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
