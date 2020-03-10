---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Partie 3 : création d’un contrôleur d’administration | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556048"
---
# <a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="c6483-102">Partie 3 : création d’un contrôleur d’administration</span><span class="sxs-lookup"><span data-stu-id="c6483-102">Part 3: Creating an Admin Controller</span></span>

<span data-ttu-id="c6483-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c6483-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c6483-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="c6483-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="c6483-105">Ajouter un contrôleur d’administration</span><span class="sxs-lookup"><span data-stu-id="c6483-105">Add an Admin Controller</span></span>

<span data-ttu-id="c6483-106">Dans cette section, nous allons ajouter un contrôleur d’API Web qui prend en charge les opérations CRUD (création, lecture, mise à jour et suppression) sur les produits.</span><span class="sxs-lookup"><span data-stu-id="c6483-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="c6483-107">Le contrôleur utilise Entity Framework pour communiquer avec la couche de base de données.</span><span class="sxs-lookup"><span data-stu-id="c6483-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="c6483-108">Seuls les administrateurs seront en mesure d’utiliser ce contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c6483-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="c6483-109">Les clients accèderont aux produits par le biais d’un autre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c6483-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="c6483-110">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers.</span><span class="sxs-lookup"><span data-stu-id="c6483-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="c6483-111">Sélectionnez **Ajouter** , puis **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="c6483-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="c6483-112">Dans la boîte de dialogue **Ajouter un contrôleur** , nommez le contrôleur `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="c6483-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="c6483-113">Sous **modèle**, sélectionnez &quot;contrôleur d’API avec des actions de lecture/écriture, à l’aide de Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="c6483-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="c6483-114">Sous **classe de modèle**, sélectionnez « produit (ProductStore. Models) ».</span><span class="sxs-lookup"><span data-stu-id="c6483-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="c6483-115">Sous **contexte de données**, sélectionnez «&lt;nouveau contexte de données&gt;».</span><span class="sxs-lookup"><span data-stu-id="c6483-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="c6483-116">Si la liste déroulante **classe de modèle** n’affiche pas de classes de modèle, assurez-vous que vous avez compilé le projet.</span><span class="sxs-lookup"><span data-stu-id="c6483-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="c6483-117">Entity Framework utilise la réflexion, il a besoin de l’assembly compilé.</span><span class="sxs-lookup"><span data-stu-id="c6483-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>

<span data-ttu-id="c6483-118">Si vous sélectionnez «&lt;nouveau contexte de données&gt;», la boîte de dialogue **nouveau contexte de données** s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="c6483-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="c6483-119">Nommez le contexte de données `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="c6483-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="c6483-120">Cliquez sur **OK** pour fermer la boîte de dialogue **nouveau contexte de données** .</span><span class="sxs-lookup"><span data-stu-id="c6483-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="c6483-121">Dans la boîte de dialogue **Ajouter un contrôleur** , cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="c6483-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="c6483-122">Voici ce qui a été ajouté au projet :</span><span class="sxs-lookup"><span data-stu-id="c6483-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="c6483-123">Classe nommée `OrdersContext` qui dérive de **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="c6483-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="c6483-124">Cette classe fournit le collage entre les modèles POCO et la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6483-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="c6483-125">Un contrôleur d’API Web nommé `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="c6483-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="c6483-126">Ce contrôleur prend en charge les opérations CRUD sur les instances de `Product`.</span><span class="sxs-lookup"><span data-stu-id="c6483-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="c6483-127">Elle utilise la classe `OrdersContext` pour communiquer avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c6483-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="c6483-128">Nouvelle chaîne de connexion de base de données dans le fichier Web. config.</span><span class="sxs-lookup"><span data-stu-id="c6483-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="c6483-129">Ouvrez le fichier OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="c6483-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="c6483-130">Notez que le constructeur spécifie le nom de la chaîne de connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6483-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="c6483-131">Ce nom fait référence à la chaîne de connexion qui a été ajoutée à Web. config.</span><span class="sxs-lookup"><span data-stu-id="c6483-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="c6483-132">Ajoutez les propriétés suivantes à la classe `OrdersContext` :</span><span class="sxs-lookup"><span data-stu-id="c6483-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="c6483-133">Un **DbSet** représente un ensemble d’entités qui peuvent être interrogées.</span><span class="sxs-lookup"><span data-stu-id="c6483-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="c6483-134">Voici la liste complète de la classe `OrdersContext` :</span><span class="sxs-lookup"><span data-stu-id="c6483-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="c6483-135">La classe `AdminController` définit cinq méthodes qui implémentent la fonctionnalité CRUD de base.</span><span class="sxs-lookup"><span data-stu-id="c6483-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="c6483-136">Chaque méthode correspond à un URI que le client peut appeler :</span><span class="sxs-lookup"><span data-stu-id="c6483-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="c6483-137">Méthode du contrôleur</span><span class="sxs-lookup"><span data-stu-id="c6483-137">Controller Method</span></span> | <span data-ttu-id="c6483-138">Description</span><span class="sxs-lookup"><span data-stu-id="c6483-138">Description</span></span> | <span data-ttu-id="c6483-139">URI</span><span class="sxs-lookup"><span data-stu-id="c6483-139">URI</span></span> | <span data-ttu-id="c6483-140">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="c6483-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c6483-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="c6483-141">GetProducts</span></span> | <span data-ttu-id="c6483-142">Obtient tous les produits.</span><span class="sxs-lookup"><span data-stu-id="c6483-142">Gets all products.</span></span> | <span data-ttu-id="c6483-143">API/produits</span><span class="sxs-lookup"><span data-stu-id="c6483-143">api/products</span></span> | <span data-ttu-id="c6483-144">GET</span><span class="sxs-lookup"><span data-stu-id="c6483-144">GET</span></span> |
| <span data-ttu-id="c6483-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="c6483-145">GetProduct</span></span> | <span data-ttu-id="c6483-146">Recherche un produit par ID.</span><span class="sxs-lookup"><span data-stu-id="c6483-146">Finds a product by ID.</span></span> | <span data-ttu-id="c6483-147">API/produits/*ID*</span><span class="sxs-lookup"><span data-stu-id="c6483-147">api/products/*id*</span></span> | <span data-ttu-id="c6483-148">GET</span><span class="sxs-lookup"><span data-stu-id="c6483-148">GET</span></span> |
| <span data-ttu-id="c6483-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="c6483-149">PutProduct</span></span> | <span data-ttu-id="c6483-150">Met à jour un produit.</span><span class="sxs-lookup"><span data-stu-id="c6483-150">Updates a product.</span></span> | <span data-ttu-id="c6483-151">API/produits/*ID*</span><span class="sxs-lookup"><span data-stu-id="c6483-151">api/products/*id*</span></span> | <span data-ttu-id="c6483-152">PUT</span><span class="sxs-lookup"><span data-stu-id="c6483-152">PUT</span></span> |
| <span data-ttu-id="c6483-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="c6483-153">PostProduct</span></span> | <span data-ttu-id="c6483-154">Crée un produit.</span><span class="sxs-lookup"><span data-stu-id="c6483-154">Creates a new product.</span></span> | <span data-ttu-id="c6483-155">API/produits</span><span class="sxs-lookup"><span data-stu-id="c6483-155">api/products</span></span> | <span data-ttu-id="c6483-156">POST</span><span class="sxs-lookup"><span data-stu-id="c6483-156">POST</span></span> |
| <span data-ttu-id="c6483-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="c6483-157">DeleteProduct</span></span> | <span data-ttu-id="c6483-158">Supprime un produit.</span><span class="sxs-lookup"><span data-stu-id="c6483-158">Deletes a product.</span></span> | <span data-ttu-id="c6483-159">API/produits/*ID*</span><span class="sxs-lookup"><span data-stu-id="c6483-159">api/products/*id*</span></span> | <span data-ttu-id="c6483-160">Suppression</span><span class="sxs-lookup"><span data-stu-id="c6483-160">DELETE</span></span> |

<span data-ttu-id="c6483-161">Chaque méthode appelle `OrdersContext` pour interroger la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6483-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="c6483-162">Les méthodes qui modifient la collection (PUT, poster et DELETE) appellent `db.SaveChanges` pour conserver les modifications apportées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6483-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="c6483-163">Les contrôleurs sont créés par requête HTTP, puis supprimés. il est donc nécessaire de conserver les modifications avant le retour d’une méthode.</span><span class="sxs-lookup"><span data-stu-id="c6483-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="c6483-164">Ajouter un initialiseur de base de données</span><span class="sxs-lookup"><span data-stu-id="c6483-164">Add a Database Initializer</span></span>

<span data-ttu-id="c6483-165">Entity Framework dispose d’une fonctionnalité intéressante qui vous permet de remplir la base de données au démarrage et de recréer automatiquement la base de données chaque fois que les modèles changent.</span><span class="sxs-lookup"><span data-stu-id="c6483-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="c6483-166">Cette fonctionnalité est utile lors du développement, car vous avez toujours des données de test, même si vous modifiez les modèles.</span><span class="sxs-lookup"><span data-stu-id="c6483-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="c6483-167">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Models et créez une nouvelle classe nommée `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="c6483-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="c6483-168">Collez l'implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="c6483-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="c6483-169">En héritant de la classe **DropCreateDatabaseIfModelChanges** , nous indiquons Entity Framework de supprimer la base de données chaque fois que nous modifions les classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="c6483-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="c6483-170">Lorsque Entity Framework crée (ou recrée) la base de données, il appelle la méthode **Seed** pour remplir les tables.</span><span class="sxs-lookup"><span data-stu-id="c6483-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="c6483-171">Nous utilisons la méthode **Seed** pour ajouter des exemples de produits, ainsi qu’un exemple d’ordre.</span><span class="sxs-lookup"><span data-stu-id="c6483-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="c6483-172">Cette fonctionnalité est très utile pour les tests, mais n’utilise pas la classe **DropCreateDatabaseIfModelChanges** en production, car vous risquez de perdre vos données si quelqu’un modifie une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="c6483-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="c6483-173">Ensuite, ouvrez global. asax et ajoutez le code suivant à l' **Application\_méthode Start** :</span><span class="sxs-lookup"><span data-stu-id="c6483-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="c6483-174">Envoyer une demande au contrôleur</span><span class="sxs-lookup"><span data-stu-id="c6483-174">Send a Request to the Controller</span></span>

<span data-ttu-id="c6483-175">À ce stade, nous n’avons pas écrit de code client, mais vous pouvez appeler l’API Web à l’aide d’un navigateur Web ou d’un outil de débogage HTTP tel que [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="c6483-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="c6483-176">Dans Visual Studio, appuyez sur F5 pour démarrer le débogage.</span><span class="sxs-lookup"><span data-stu-id="c6483-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="c6483-177">Votre navigateur Web s’ouvre sur `http://localhost:*portnum*/`, où *portnum* est un numéro de port.</span><span class="sxs-lookup"><span data-stu-id="c6483-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="c6483-178">Envoyer une requête HTTP à «`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="c6483-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="c6483-179">La première requête peut être lente à se terminer, car Entity Framework doit créer et amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="c6483-179">The first request may be slow to complete, because Entity Framework needs to create and seed the database.</span></span> <span data-ttu-id="c6483-180">La réponse doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="c6483-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="c6483-181">[Précédent](using-web-api-with-entity-framework-part-2.md)
> [Suivant](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="c6483-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
