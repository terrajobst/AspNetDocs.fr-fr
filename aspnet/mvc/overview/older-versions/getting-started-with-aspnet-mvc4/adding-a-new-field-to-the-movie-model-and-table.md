---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Ajout d’un nouveau champ au modèle de film et à la table | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : une version mise à jour de ce didacticiel est disponible ici et utilise ASP.NET MVC 5 et Visual Studio 2013. C’est plus sécurisé, bien plus simple à suivre et à faire une démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457698"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="059e2-104">Ajout d’un nouveau champ à la table et au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="059e2-104">Adding a New Field to the Movie Model and Table</span></span>

<span data-ttu-id="059e2-105">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="059e2-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="059e2-106">Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) et utilise ASP.NET MVC 5 et Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="059e2-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="059e2-107">Il est plus sûr, bien plus simple à suivre et qui illustre davantage de fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="059e2-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="059e2-108">Dans cette section, vous allez utiliser Migrations Entity Framework Code First pour migrer des modifications apportées aux classes de modèle afin que la modification soit appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="059e2-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="059e2-109">Par défaut, lorsque vous utilisez Entity Framework Code First pour créer automatiquement une base de données, comme vous l’avez fait précédemment dans ce didacticiel, Code First ajoute une table à la base de données pour vous aider à déterminer si le schéma de la base de données est synchronisé avec les classes de modèle à partir desquelles il a été généré.</span><span class="sxs-lookup"><span data-stu-id="059e2-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="059e2-110">S’ils ne sont pas synchronisés, le Entity Framework génère une erreur.</span><span class="sxs-lookup"><span data-stu-id="059e2-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="059e2-111">Cela facilite le suivi des problèmes au moment du développement, que vous ne pouvez trouver dans le cas contraire (par des erreurs obscures) au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="059e2-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="059e2-112">Configuration de Migrations Code First pour les modifications de modèle</span><span class="sxs-lookup"><span data-stu-id="059e2-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="059e2-113">Si vous utilisez Visual Studio 2012, double-cliquez sur le fichier *movies. mdf* à partir de Explorateur de solutions pour ouvrir l’outil de base de données.</span><span class="sxs-lookup"><span data-stu-id="059e2-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="059e2-114">Visual Studio Express pour le Web affiche explorateur de base de données, Visual Studio 2012 affiche Explorateur de serveurs.</span><span class="sxs-lookup"><span data-stu-id="059e2-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="059e2-115">Si vous utilisez Visual Studio 2010, utilisez Explorateur d’objets SQL Server.</span><span class="sxs-lookup"><span data-stu-id="059e2-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="059e2-116">Dans l’outil de base de données (explorateur de base de données, Explorateur de serveurs ou Explorateur d’objets SQL Server), cliquez avec le bouton droit sur `MovieDBContext`, puis sélectionnez **supprimer** pour supprimer la base de données de films.</span><span class="sxs-lookup"><span data-stu-id="059e2-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="059e2-117">Revenez à Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="059e2-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="059e2-118">Cliquez avec le bouton droit sur le fichier *movies. mdf* , puis sélectionnez **supprimer** pour supprimer la base de données films.</span><span class="sxs-lookup"><span data-stu-id="059e2-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="059e2-119">Générez l’application pour vous assurer qu’aucune erreur n’est détectée.</span><span class="sxs-lookup"><span data-stu-id="059e2-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="059e2-120">Dans le menu **Outils**, cliquez sur **Gestionnaire de package NuGet**, puis sur **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="059e2-120">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Ajouter un homme à en-tête pack](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="059e2-122">Dans la fenêtre **console du gestionnaire de package** , à l’invite de `PM>`, entrez « Enable-migrations-ContextTypeName MvcMovie. Models. MovieDBContext ».</span><span class="sxs-lookup"><span data-stu-id="059e2-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="059e2-123">La commande **Enable-migrations** (illustrée ci-dessus) crée un fichier *Configuration.cs* dans un nouveau dossier *migrations* .</span><span class="sxs-lookup"><span data-stu-id="059e2-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="059e2-124">Visual Studio ouvre le fichier *Configuration.cs* .</span><span class="sxs-lookup"><span data-stu-id="059e2-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="059e2-125">Remplacez la méthode `Seed` dans le fichier *Configuration.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="059e2-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="059e2-126">Cliquez avec le bouton droit sur la ligne ondulée rouge sous `Movie` puis sélectionnez **résoudre** , puis **Utilisez** **MvcMovie. Models.**</span><span class="sxs-lookup"><span data-stu-id="059e2-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="059e2-127">En procédant ainsi, vous ajoutez l’instruction using suivante :</span><span class="sxs-lookup"><span data-stu-id="059e2-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="059e2-128">Migrations Code First appelle la méthode `Seed` après chaque migration (autrement dit, en appelant **Update-Database** dans la console du gestionnaire de package) et cette méthode met à jour les lignes qui ont déjà été insérées, ou les insère si elles n’existent pas encore.</span><span class="sxs-lookup"><span data-stu-id="059e2-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>

<span data-ttu-id="059e2-129">**Appuyez sur Ctrl-Shift-B pour générer le projet.** (Les étapes suivantes échouent si votre version n’est pas générée à ce stade.)</span><span class="sxs-lookup"><span data-stu-id="059e2-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="059e2-130">L’étape suivante consiste à créer une classe de `DbMigration` pour la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="059e2-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="059e2-131">Cette migration vers crée une nouvelle base de données, c’est pourquoi vous avez supprimé le fichier *Movie. mdf* à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="059e2-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="059e2-132">Dans la fenêtre **console du gestionnaire de package** , entrez la commande « Add-migration initial » pour créer la migration initiale.</span><span class="sxs-lookup"><span data-stu-id="059e2-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="059e2-133">Le nom « initial » est arbitraire et est utilisé pour nommer le fichier de migration créé.</span><span class="sxs-lookup"><span data-stu-id="059e2-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="059e2-134">Migrations Code First crée un autre fichier de classe dans le dossier *migrations* (portant le nom *{date}\_initial.cs* ), et cette classe contient le code qui crée le schéma de la base de données.</span><span class="sxs-lookup"><span data-stu-id="059e2-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="059e2-135">Le nom du fichier de migration est précédé d’un horodateur pour faciliter le classement.</span><span class="sxs-lookup"><span data-stu-id="059e2-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="059e2-136">Examinez le fichier *{Date}\_initial.cs* , qui contient les instructions pour créer le tableau films pour la base de données Movie.</span><span class="sxs-lookup"><span data-stu-id="059e2-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="059e2-137">Lorsque vous mettez à jour la base de données dans les instructions ci-dessous, ce fichier *{Date}\_initial.cs* s’exécute et crée le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="059e2-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="059e2-138">La méthode **Seed** s’exécutera ensuite pour remplir la base de données avec les données de test.</span><span class="sxs-lookup"><span data-stu-id="059e2-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="059e2-139">Dans la **console du gestionnaire de package**, entrez la commande Update-Database pour créer la base de données et exécuter la méthode **Seed** .</span><span class="sxs-lookup"><span data-stu-id="059e2-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="059e2-140">Si vous recevez une erreur indiquant qu’une table existe déjà et qu’elle ne peut pas être créée, cela est probablement dû au fait que vous avez exécuté l’application après avoir supprimé la base de données et avant d’avoir exécuté `update-database`.</span><span class="sxs-lookup"><span data-stu-id="059e2-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="059e2-141">Dans ce cas, supprimez de nouveau le fichier *movies. mdf* et relancez la commande `update-database`.</span><span class="sxs-lookup"><span data-stu-id="059e2-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="059e2-142">Si vous recevez toujours une erreur, supprimez le dossier et le contenu des migrations, puis commencez par suivre les instructions en haut de cette page (effacez le fichier *movies. mdf* , puis passez à activer-migrations).</span><span class="sxs-lookup"><span data-stu-id="059e2-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="059e2-143">Exécutez l’application et accédez à l’URL */movies* .</span><span class="sxs-lookup"><span data-stu-id="059e2-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="059e2-144">Les données de départ sont affichées.</span><span class="sxs-lookup"><span data-stu-id="059e2-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="059e2-145">Ajout d’une propriété Rating au modèle Movie</span><span class="sxs-lookup"><span data-stu-id="059e2-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="059e2-146">Commencez par ajouter une nouvelle propriété `Rating` à la classe `Movie` existante.</span><span class="sxs-lookup"><span data-stu-id="059e2-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="059e2-147">Ouvrez le fichier *Models\Movie.cs* et ajoutez la propriété `Rating` comme celle-ci :</span><span class="sxs-lookup"><span data-stu-id="059e2-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="059e2-148">La classe `Movie` complète ressemble maintenant au code suivant :</span><span class="sxs-lookup"><span data-stu-id="059e2-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="059e2-149">Générez l’application à l’aide de la commande de menu **générer** &gt;**générer un film** ou en appuyant sur Ctrl-Shift-B.</span><span class="sxs-lookup"><span data-stu-id="059e2-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="059e2-150">Maintenant que vous avez mis à jour la classe `Model`, vous devez également mettre à jour les modèles de vue *\Views\Movies\Index.cshtml* et *\Views\Movies\Create.cshtml* pour afficher la nouvelle propriété `Rating` dans la vue du navigateur.</span><span class="sxs-lookup"><span data-stu-id="059e2-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="059e2-151">Ouvrez le fichier<em>\Views\Movies\Index.cshtml</em> et ajoutez un en-tête de colonne `<th>Rating</th>` juste après la colonne <strong>Price</strong> .</span><span class="sxs-lookup"><span data-stu-id="059e2-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="059e2-152">Ajoutez ensuite une `<td>` colonne près de la fin du modèle pour afficher la valeur de `@item.Rating`.</span><span class="sxs-lookup"><span data-stu-id="059e2-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="059e2-153">Voici à quoi ressemble le modèle de vue <em>index. cshtml</em> mis à jour :</span><span class="sxs-lookup"><span data-stu-id="059e2-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="059e2-154">Ensuite, ouvrez le fichier *\Views\Movies\Create.cshtml* et ajoutez le balisage suivant à la fin du formulaire.</span><span class="sxs-lookup"><span data-stu-id="059e2-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="059e2-155">Cela génère le rendu d’une zone de texte afin que vous puissiez spécifier une évaluation lors de la création d’un nouveau film.</span><span class="sxs-lookup"><span data-stu-id="059e2-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="059e2-156">Vous avez maintenant mis à jour le code de l’application pour prendre en charge la nouvelle propriété `Rating`.</span><span class="sxs-lookup"><span data-stu-id="059e2-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="059e2-157">Exécutez maintenant l’application et accédez à l’URL */movies* .</span><span class="sxs-lookup"><span data-stu-id="059e2-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="059e2-158">Lorsque vous procédez ainsi, l’une des erreurs suivantes s’affiche :</span><span class="sxs-lookup"><span data-stu-id="059e2-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="059e2-159">Vous voyez cette erreur, car la classe de modèle `Movie` mise à jour dans l’application est maintenant différente du schéma de la table `Movie` de la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="059e2-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="059e2-160">(Il n’existe pas de colonne `Rating` dans la table de base de données.)</span><span class="sxs-lookup"><span data-stu-id="059e2-160">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="059e2-161">Plusieurs approches sont possibles pour résoudre l’erreur :</span><span class="sxs-lookup"><span data-stu-id="059e2-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="059e2-162">Laissez Entity Framework supprimer et recréer automatiquement la base de données sur la base du nouveau schéma de classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="059e2-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="059e2-163">Cette approche est très pratique lors du développement actif sur une base de données de test. elle vous permet de faire évoluer rapidement le schéma de base de données et le modèle.</span><span class="sxs-lookup"><span data-stu-id="059e2-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="059e2-164">L’inconvénient, cependant, est que vous perdez les données existantes dans la base de données, de sorte que vous *ne souhaitez pas* utiliser cette approche sur une base de données de production.</span><span class="sxs-lookup"><span data-stu-id="059e2-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="059e2-165">L’utilisation d’un initialiseur pour amorcer automatiquement une base de données avec des données de test est souvent un moyen productif de développer une application.</span><span class="sxs-lookup"><span data-stu-id="059e2-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="059e2-166">Pour plus d’informations sur les initialiseurs de base de données Entity Framework, consultez le [didacticiel ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)de Tom Dykstra.</span><span class="sxs-lookup"><span data-stu-id="059e2-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="059e2-167">Modifiez explicitement le schéma de la base de données existante pour le faire correspondre aux classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="059e2-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="059e2-168">L’avantage de cette approche est que vous conservez vos données.</span><span class="sxs-lookup"><span data-stu-id="059e2-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="059e2-169">Vous pouvez apporter cette modification manuellement ou en créant un script de modification de la base de données.</span><span class="sxs-lookup"><span data-stu-id="059e2-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="059e2-170">Utilisez Migrations Code First pour mettre à jour le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="059e2-170">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="059e2-171">Pour ce didacticiel, nous allons utiliser les migrations Code First.</span><span class="sxs-lookup"><span data-stu-id="059e2-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="059e2-172">Mettez à jour la méthode Seed afin qu’elle fournisse une valeur pour la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="059e2-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="059e2-173">Ouvrez le fichier Migrations\Configuration.cs et ajoutez un champ Rating à chaque objet film.</span><span class="sxs-lookup"><span data-stu-id="059e2-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="059e2-174">Générez la solution, puis ouvrez la fenêtre de **console du gestionnaire de package** et entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="059e2-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="059e2-175">La commande `add-migration` indique à l’infrastructure de migration d’examiner le modèle de film actuel avec le schéma de la base de code de la vidéo actuelle et de créer le code nécessaire pour migrer la base de code vers le nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="059e2-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="059e2-176">Le AddRatingMig est arbitraire et est utilisé pour nommer le fichier de migration.</span><span class="sxs-lookup"><span data-stu-id="059e2-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="059e2-177">Il est utile d’utiliser un nom explicite pour l’étape de migration.</span><span class="sxs-lookup"><span data-stu-id="059e2-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="059e2-178">Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe dérivée, et dans la méthode `Up` vous pouvez voir le code qui crée la nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="059e2-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="059e2-179">Générez la solution, puis entrez la commande « Update-Database » dans la fenêtre de **console du gestionnaire de package** .</span><span class="sxs-lookup"><span data-stu-id="059e2-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="059e2-180">L’illustration suivante montre la sortie dans la fenêtre de la **console du gestionnaire de package** (le datage en préAddRatingMig de date est différent.)</span><span class="sxs-lookup"><span data-stu-id="059e2-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="059e2-181">Réexécutez l’application et accédez à l’URL/movies.</span><span class="sxs-lookup"><span data-stu-id="059e2-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="059e2-182">Vous pouvez voir le nouveau champ évaluation.</span><span class="sxs-lookup"><span data-stu-id="059e2-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="059e2-183">Cliquez sur le lien **Create New** pour ajouter un nouveau film.</span><span class="sxs-lookup"><span data-stu-id="059e2-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="059e2-184">Notez que vous pouvez ajouter une évaluation.</span><span class="sxs-lookup"><span data-stu-id="059e2-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="059e2-186">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="059e2-186">Click **Create**.</span></span> <span data-ttu-id="059e2-187">Le nouveau film, y compris l’évaluation, s’affiche désormais dans la liste des films :</span><span class="sxs-lookup"><span data-stu-id="059e2-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="059e2-189">Vous devez également ajouter le champ `Rating` aux modèles de vue Edit, Details et SearchIndex.</span><span class="sxs-lookup"><span data-stu-id="059e2-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="059e2-190">Vous pouvez entrer à nouveau la commande « Update-Database » dans la fenêtre de la **console du gestionnaire de package** et aucune modification n’est apportée, car le schéma correspond au modèle.</span><span class="sxs-lookup"><span data-stu-id="059e2-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="059e2-191">Dans cette section, vous avez vu comment vous pouvez modifier des objets de modèle et maintenir la synchronisation de la base de données avec les modifications.</span><span class="sxs-lookup"><span data-stu-id="059e2-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="059e2-192">Vous avez également appris à remplir une base de données nouvellement créée avec des exemples de données afin de pouvoir essayer des scénarios.</span><span class="sxs-lookup"><span data-stu-id="059e2-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="059e2-193">Voyons ensuite comment vous pouvez ajouter une logique de validation plus riche aux classes de modèle et permettre l’application de certaines règles d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="059e2-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="059e2-194">[Précédent](examining-the-edit-methods-and-edit-view.md)
> [Suivant](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="059e2-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
