---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Utiliser Migrations Code First pour amorcer la base de données | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557455"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="cfbc5-102">Utiliser Migrations Code First pour amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="cfbc5-102">Use Code First Migrations to Seed the Database</span></span>

<span data-ttu-id="cfbc5-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cfbc5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cfbc5-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="cfbc5-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="cfbc5-105">Dans cette section, vous allez utiliser [migrations code First](https://msdn.microsoft.com/data/jj591621) dans EF pour amorcer la base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="cfbc5-106">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="cfbc5-107">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="cfbc5-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="cfbc5-108">Cette commande ajoute un dossier nommé migrations à votre projet, ainsi qu’un fichier de code nommé Configuration.cs dans le dossier migrations.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="cfbc5-109">Ouvrez le fichier Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="cfbc5-110">Ajoutez l’instruction **using** suivante.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="cfbc5-111">Ajoutez ensuite le code suivant à la méthode **Configuration. Seed** :</span><span class="sxs-lookup"><span data-stu-id="cfbc5-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="cfbc5-112">Dans la fenêtre console du gestionnaire de package, tapez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="cfbc5-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="cfbc5-113">La première commande génère du code qui crée la base de données, et la deuxième commande exécute ce code.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="cfbc5-114">La base de données est créée localement [à l'](https://msdn.microsoft.com/library/hh510202.aspx)aide de la base de données locale.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="cfbc5-115">Explorez l’API (facultatif)</span><span class="sxs-lookup"><span data-stu-id="cfbc5-115">Explore the API (Optional)</span></span>

<span data-ttu-id="cfbc5-116">Appuyez sur F5 pour exécuter l'application en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="cfbc5-117">Visual Studio démarre IIS Express et exécute votre application Web.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="cfbc5-118">Visual Studio lance ensuite un navigateur et ouvre la page d’hébergement de l’application.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="cfbc5-119">Lorsque Visual Studio exécute un projet Web, il attribue un numéro de port.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="cfbc5-120">Dans l’image ci-dessous, le numéro de port est 50524.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="cfbc5-121">Lorsque vous exécutez l’application, vous verrez un numéro de port différent.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="cfbc5-122">La page d’hébergement est implémentée à l’aide de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="cfbc5-123">En haut de la page se trouve un lien indiquant « API ».</span><span class="sxs-lookup"><span data-stu-id="cfbc5-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="cfbc5-124">Ce lien vous amène à une page d’aide générée automatiquement pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="cfbc5-125">(Pour savoir comment cette page d’aide est générée et comment vous pouvez ajouter votre propre documentation à la page, consultez [création de pages d’aide pour API Web ASP.net](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Vous pouvez cliquer sur la page d’aide Liens pour afficher des détails sur l’API, y compris le format de la demande et de la réponse.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="cfbc5-126">L’API permet les opérations CRUD sur la base de données.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="cfbc5-127">Voici un résumé de l’API.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-127">The following summarizes the API.</span></span>

| <span data-ttu-id="cfbc5-128">Auteurs</span><span class="sxs-lookup"><span data-stu-id="cfbc5-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="cfbc5-129">GET api/authors</span><span class="sxs-lookup"><span data-stu-id="cfbc5-129">GET api/authors</span></span> | <span data-ttu-id="cfbc5-130">Récupération de tous les auteurs.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-130">Get all authors.</span></span> |
| <span data-ttu-id="cfbc5-131">GET api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="cfbc5-131">GET api/authors/{id}</span></span> | <span data-ttu-id="cfbc5-132">Obtient un auteur par ID.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-132">Get an author by ID.</span></span> |
| <span data-ttu-id="cfbc5-133">POST /api/authors</span><span class="sxs-lookup"><span data-stu-id="cfbc5-133">POST /api/authors</span></span> | <span data-ttu-id="cfbc5-134">Créez un auteur.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-134">Create a new author.</span></span> |
| <span data-ttu-id="cfbc5-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="cfbc5-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="cfbc5-136">Mettre à jour un auteur existant.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-136">Update an existing author.</span></span> |
| <span data-ttu-id="cfbc5-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="cfbc5-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="cfbc5-138">Supprimer un auteur.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-138">Delete an author.</span></span> |

| <span data-ttu-id="cfbc5-139">Livres</span><span class="sxs-lookup"><span data-stu-id="cfbc5-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="cfbc5-140">GET /api/books</span><span class="sxs-lookup"><span data-stu-id="cfbc5-140">GET /api/books</span></span> | <span data-ttu-id="cfbc5-141">Récupération de tous les livres.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-141">Get all books.</span></span> |
| <span data-ttu-id="cfbc5-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="cfbc5-142">GET /api/books/{id}</span></span> | <span data-ttu-id="cfbc5-143">Obtenir un livre par ID.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-143">Get a book by ID.</span></span> |
| <span data-ttu-id="cfbc5-144">POST /api/books</span><span class="sxs-lookup"><span data-stu-id="cfbc5-144">POST /api/books</span></span> | <span data-ttu-id="cfbc5-145">Créez un livre.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-145">Create a new book.</span></span> |
| <span data-ttu-id="cfbc5-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="cfbc5-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="cfbc5-147">Mettre à jour un livre existant.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-147">Update an existing book.</span></span> |
| <span data-ttu-id="cfbc5-148">DELETE /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="cfbc5-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="cfbc5-149">Supprimer un livre.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="cfbc5-150">Afficher la base de données (facultatif)</span><span class="sxs-lookup"><span data-stu-id="cfbc5-150">View the Database (Optional)</span></span>

<span data-ttu-id="cfbc5-151">Lorsque vous avez exécuté la commande Update-Database, EF a créé la base de données et a appelé la méthode `Seed`.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="cfbc5-152">Lorsque vous exécutez l’application localement, [EF utilise la](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)base de données locale.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="cfbc5-153">Vous pouvez afficher la base de données dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="cfbc5-154">Dans le menu **Vue**, sélectionnez **Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="cfbc5-155">Dans la boîte de dialogue **se connecter au serveur** , dans la zone de texte **nom du serveur** , tapez « (base de données locale) \v11.0 ».</span><span class="sxs-lookup"><span data-stu-id="cfbc5-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="cfbc5-156">Laissez l’option **d’authentification** « authentification Windows ».</span><span class="sxs-lookup"><span data-stu-id="cfbc5-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="cfbc5-157">Cliquez sur **Connexion**.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="cfbc5-158">Visual Studio se connecte à la base de données locale et affiche vos bases de données existantes dans la fenêtre de Explorateur d’objets SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="cfbc5-159">Vous pouvez développer les nœuds pour afficher les tables que EF a créées.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="cfbc5-160">Pour afficher les données, cliquez avec le bouton droit sur une table, puis sélectionnez **afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="cfbc5-161">La capture d’écran suivante montre les résultats de la table Books.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="cfbc5-162">Notez que EF a rempli la base de données avec les données de départ et que la table contient la clé étrangère à la table authors.</span><span class="sxs-lookup"><span data-stu-id="cfbc5-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="cfbc5-163">[Précédent](part-2.md)
> [Suivant](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="cfbc5-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
