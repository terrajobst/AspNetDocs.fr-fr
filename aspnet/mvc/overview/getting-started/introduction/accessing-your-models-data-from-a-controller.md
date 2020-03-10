---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Accès aux données de votre modèle à partir d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5d882d765133d32d3acdba9ffb5d43b69119a273
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615919"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="16599-102">Accès aux données de votre modèle à partir d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="16599-102">Accessing Your Model's Data from a Controller</span></span>

<span data-ttu-id="16599-103">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16599-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="16599-104">Dans cette section, vous allez créer une classe de `MoviesController` et écrire du code qui récupère les données de film et les affiche dans le navigateur à l’aide d’un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="16599-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="16599-105">**Générez l’application** avant de passer à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="16599-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="16599-106">Si vous ne générez pas l’application, vous obtiendrez une erreur lors de l’ajout d’un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="16599-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="16599-107">Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Controllers* , puis cliquez sur **Ajouter**, puis sur **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="16599-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="16599-108">Dans la boîte de dialogue **Ajouter une structure** , cliquez sur **contrôleur MVC 5 avec affichages, à l’aide de Entity Framework**, puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="16599-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="16599-109">Sélectionnez **Movie (MvcMovie. Models)** pour la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="16599-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="16599-110">Sélectionnez **MovieDBContext (MvcMovie. Models)** pour la classe de contexte de données.</span><span class="sxs-lookup"><span data-stu-id="16599-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="16599-111">Pour le nom du contrôleur, entrez **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="16599-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="16599-112">L’image ci-dessous montre la boîte de dialogue terminé.</span><span class="sxs-lookup"><span data-stu-id="16599-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="16599-113">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="16599-113">Click **Add**.</span></span> <span data-ttu-id="16599-114">(Si vous recevez une erreur, vous n’avez probablement pas généré l’application avant de commencer à ajouter le contrôleur.) Visual Studio crée les fichiers et dossiers suivants :</span><span class="sxs-lookup"><span data-stu-id="16599-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="16599-115">*Un fichier MoviesController.cs* dans le dossier *Controllers* .</span><span class="sxs-lookup"><span data-stu-id="16599-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="16599-116">Un dossier *Views\Movies* .</span><span class="sxs-lookup"><span data-stu-id="16599-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="16599-117">*Créez. cshtml, DELETE. cshtml, Details. cshtml, Edit. cshtml*et *index. cshtml* dans le nouveau dossier *Views\Movies* .</span><span class="sxs-lookup"><span data-stu-id="16599-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="16599-118">Visual Studio a créé automatiquement les méthodes d’action et les vues [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (créer, lire, mettre à jour et supprimer) pour vous (la création automatique de méthodes et de vues d’action CRUD porte le nom de génération de modèles automatique).</span><span class="sxs-lookup"><span data-stu-id="16599-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="16599-119">Vous disposez maintenant d’une application Web entièrement fonctionnelle qui vous permet de créer, de répertorier, de modifier et de supprimer des entrées de film.</span><span class="sxs-lookup"><span data-stu-id="16599-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="16599-120">Exécutez l’application et cliquez sur le lien de la **vidéo MVC** (ou accédez au contrôleur `Movies` en ajoutant */movies* à l’URL dans la barre d’adresses de votre navigateur).</span><span class="sxs-lookup"><span data-stu-id="16599-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="16599-121">Étant donné que l’application s’appuie sur le routage par défaut (défini dans le fichier *App\_Start\RouteConfig.cs* ), la demande de navigateur `http://localhost:xxxxx/Movies` est routée vers la méthode d’action par défaut `Index` du contrôleur de `Movies`.</span><span class="sxs-lookup"><span data-stu-id="16599-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="16599-122">En d’autres termes, la demande de navigateur `http://localhost:xxxxx/Movies` est identique à celle de la demande du navigateur `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="16599-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="16599-123">Le résultat est une liste vide de films, car vous n’en avez pas encore ajouté.</span><span class="sxs-lookup"><span data-stu-id="16599-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="16599-124">Création d’un film</span><span class="sxs-lookup"><span data-stu-id="16599-124">Creating a Movie</span></span>

<span data-ttu-id="16599-125">Sélectionnez le lien **Créer nouveau**.</span><span class="sxs-lookup"><span data-stu-id="16599-125">Select the **Create New** link.</span></span> <span data-ttu-id="16599-126">Entrez des détails sur un film, puis cliquez sur le bouton **créer** .</span><span class="sxs-lookup"><span data-stu-id="16599-126">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="16599-127">Vous ne pourrez peut-être pas entrer des virgules ou des virgules dans le champ Price.</span><span class="sxs-lookup"><span data-stu-id="16599-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="16599-128">pour prendre en charge la validation jQuery pour les paramètres régionaux autres que l’anglais qui utilisent une virgule (&quot;,&quot;) pour une virgule décimale et des formats de date autres que l’anglais des États-Unis, vous devez inclure *global. js* et votre fichier *cultures/globaliser. cultures. js* spécifique (à partir de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) et JavaScript pour utiliser `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="16599-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="16599-129">Je vais vous montrer comment procéder dans le didacticiel suivant.</span><span class="sxs-lookup"><span data-stu-id="16599-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="16599-130">Pour le moment, entrez simplement des nombres entiers tels que 10.</span><span class="sxs-lookup"><span data-stu-id="16599-130">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="16599-131">Si vous cliquez sur le bouton **créer** , le formulaire est publié sur le serveur, où les informations sur le film sont enregistrées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="16599-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="16599-132">Vous êtes ensuite redirigé vers l’URL */movies* , où vous pouvez voir le film nouvellement créé dans la liste.</span><span class="sxs-lookup"><span data-stu-id="16599-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="16599-133">Créez deux ou trois autres entrées.</span><span class="sxs-lookup"><span data-stu-id="16599-133">Create a couple more movie entries.</span></span> <span data-ttu-id="16599-134">Essayez les liens **Edit**, **Details** et **Delete**, qui sont tous opérationnels.</span><span class="sxs-lookup"><span data-stu-id="16599-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="16599-135">Examen du code généré</span><span class="sxs-lookup"><span data-stu-id="16599-135">Examining the Generated Code</span></span>

<span data-ttu-id="16599-136">Ouvrez le fichier *Controllers\MoviesController.cs* et examinez la méthode `Index` générée.</span><span class="sxs-lookup"><span data-stu-id="16599-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="16599-137">Une partie du contrôleur Movie avec la méthode `Index` est illustrée ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="16599-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="16599-138">Une demande au contrôleur `Movies` retourne toutes les entrées de la table `Movies` puis passe les résultats à la vue `Index`.</span><span class="sxs-lookup"><span data-stu-id="16599-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="16599-139">La ligne suivante de la classe `MoviesController` instancie un contexte de base de données de films, comme décrit précédemment.</span><span class="sxs-lookup"><span data-stu-id="16599-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="16599-140">Vous pouvez utiliser le contexte de la base de données de films pour interroger, modifier et supprimer des films.</span><span class="sxs-lookup"><span data-stu-id="16599-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="16599-141">Modèles fortement typés et le mot clé @model</span><span class="sxs-lookup"><span data-stu-id="16599-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="16599-142">Plus haut dans ce didacticiel, vous avez vu comment un contrôleur peut passer des données ou des objets à un modèle de vue à l’aide de l’objet `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="16599-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="16599-143">Le `ViewBag` est un objet dynamique qui fournit un moyen pratique à liaison tardive pour passer des informations à une vue.</span><span class="sxs-lookup"><span data-stu-id="16599-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="16599-144">MVC offre également la possibilité de passer des objets *fortement* typés à un modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="16599-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="16599-145">Cette approche fortement typée permet une meilleure vérification au moment de la compilation de votre code et de la [fonctionnalité IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) plus riche dans l’éditeur Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16599-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="16599-146">Le mécanisme de génération de modèles automatique dans Visual Studio a utilisé cette approche (c’est-à-dire passer un modèle *fortement* typé) avec la classe `MoviesController` et les modèles de vue lorsqu’elle a créé les méthodes et les vues.</span><span class="sxs-lookup"><span data-stu-id="16599-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="16599-147">Dans le fichier *Controllers\MoviesController.cs* , examinez la méthode `Details` générée.</span><span class="sxs-lookup"><span data-stu-id="16599-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="16599-148">La méthode `Details` est illustrée ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="16599-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="16599-149">Le paramètre `id` est généralement passé en tant que données d’itinéraire, par exemple `http://localhost:1234/movies/details/1` définit le contrôleur sur le contrôleur Movie, l’action à `details` et le `id` sur 1.</span><span class="sxs-lookup"><span data-stu-id="16599-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="16599-150">Vous pouvez également transmettre l’ID avec une chaîne de requête comme suit :</span><span class="sxs-lookup"><span data-stu-id="16599-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="16599-151">Si un `Movie` est trouvé, une instance du modèle `Movie` est passée à la vue `Details` :</span><span class="sxs-lookup"><span data-stu-id="16599-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="16599-152">Examinez le contenu du fichier *Views\Movies\Details.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="16599-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="16599-153">En incluant une instruction `@model` en haut du fichier de modèle de vue, vous pouvez spécifier le type d’objet attendu par la vue.</span><span class="sxs-lookup"><span data-stu-id="16599-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="16599-154">Quand vous avez créé le contrôleur pour les films, Visual Studio a inclus automatiquement l’instruction `@model` suivante en haut du fichier *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="16599-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="16599-155">Cette directive `@model` vous permet d’accéder au film que le contrôleur a passé à la vue en utilisant un objet `Model` qui est fortement typé.</span><span class="sxs-lookup"><span data-stu-id="16599-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="16599-156">Par exemple, dans le modèle *Details. cshtml* , le code passe chaque champ Movie à la `DisplayNameFor` et [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) les applications auxiliaires HTML avec l’objet `Model` fortement typé.</span><span class="sxs-lookup"><span data-stu-id="16599-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="16599-157">Les méthodes `Create` et `Edit` et les modèles de vue transmettent également un objet de modèle de film.</span><span class="sxs-lookup"><span data-stu-id="16599-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="16599-158">Examinez le modèle de vue *index. cshtml* et la méthode `Index` dans le fichier *MoviesController.cs* .</span><span class="sxs-lookup"><span data-stu-id="16599-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="16599-159">Notez que le code crée un objet [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) lorsqu’il appelle la méthode d’assistance `View` dans la méthode d’action `Index`.</span><span class="sxs-lookup"><span data-stu-id="16599-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="16599-160">Le code transmet ensuite cette liste de `Movies` de la méthode d’action `Index` à la vue :</span><span class="sxs-lookup"><span data-stu-id="16599-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="16599-161">Lorsque vous avez créé le contrôleur de film, Visual Studio inclut automatiquement l’instruction `@model` suivante en haut du fichier *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="16599-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="16599-162">Cette `@model` directive vous permet d’accéder à la liste des films que le contrôleur a transmis à la vue à l’aide d’un objet `Model` fortement typé.</span><span class="sxs-lookup"><span data-stu-id="16599-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="16599-163">Par exemple, dans le modèle *index. cshtml* , le code parcourt les films en exécutant une instruction `foreach` sur l’objet `Model` fortement typé :</span><span class="sxs-lookup"><span data-stu-id="16599-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="16599-164">Étant donné que l’objet `Model` est fortement typé (en tant qu’objet `IEnumerable<Movie>`), chaque objet `item` de la boucle est de type `Movie`.</span><span class="sxs-lookup"><span data-stu-id="16599-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="16599-165">Entre autres avantages, cela signifie que vous bénéficiez d’une vérification au moment de la compilation du code et de la prise en charge complète d’IntelliSense dans l’éditeur de code :</span><span class="sxs-lookup"><span data-stu-id="16599-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="16599-167">Utilisation de SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="16599-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="16599-168">Entity Framework Code First a détecté que la chaîne de connexion de base de données qui a été fournie pointait vers une base de données `Movies` qui n’existait pas encore, donc Code First créé la base de données automatiquement.</span><span class="sxs-lookup"><span data-stu-id="16599-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="16599-169">Vous pouvez vérifier qu’il a été créé en examinant le dossier de données de l' *application\_* .</span><span class="sxs-lookup"><span data-stu-id="16599-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="16599-170">Si vous ne voyez pas le fichier *movies. mdf* , cliquez sur le bouton **Afficher tous les fichiers** dans la barre d’outils **Explorateur de solutions** , cliquez sur le bouton **Actualiser** , puis développez le dossier données de l' *application\_* .</span><span class="sxs-lookup"><span data-stu-id="16599-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="16599-171">Double-cliquez sur *movies. mdf* pour ouvrir l' **Explorateur de serveurs**, puis développez le dossier **tables** pour afficher le tableau films.</span><span class="sxs-lookup"><span data-stu-id="16599-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="16599-172">Notez l’icône de clé en regard de ID.</span><span class="sxs-lookup"><span data-stu-id="16599-172">Note the key icon next to ID.</span></span> <span data-ttu-id="16599-173">Par défaut, EF fait d’une propriété nommée ID la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="16599-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="16599-174">Pour plus d’informations sur EF et MVC, consultez le excellent didacticiel de Tom Dykstra sur [MVC et EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="16599-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="16599-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="16599-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="16599-176">Cliquez avec le bouton droit sur la table `Movies`, puis sélectionnez **afficher les données** de la table pour afficher les données que vous avez créées.</span><span class="sxs-lookup"><span data-stu-id="16599-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="16599-177">Cliquez avec le bouton droit sur la table `Movies` et sélectionnez **ouvrir la définition de table** pour afficher la structure de la table que Entity Framework code First créé pour vous.</span><span class="sxs-lookup"><span data-stu-id="16599-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="16599-178">Notez que le schéma de la table `Movies` est mappé à la classe `Movie` que vous avez créée précédemment.</span><span class="sxs-lookup"><span data-stu-id="16599-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="16599-179">Entity Framework Code First créé automatiquement ce schéma pour vous en fonction de votre classe `Movie`.</span><span class="sxs-lookup"><span data-stu-id="16599-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="16599-180">Lorsque vous avez terminé, fermez la connexion en cliquant avec le bouton droit sur *MovieDBContext* et en sélectionnant **Fermer la connexion**.</span><span class="sxs-lookup"><span data-stu-id="16599-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="16599-181">(Si vous ne fermez pas la connexion, vous risquez de recevoir une erreur la prochaine fois que vous exécuterez le projet).</span><span class="sxs-lookup"><span data-stu-id="16599-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

<span data-ttu-id="16599-182">Vous disposez maintenant d’une base de données et de pages pour afficher, modifier, mettre à jour et supprimer les données.</span><span class="sxs-lookup"><span data-stu-id="16599-182">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="16599-183">Dans le didacticiel suivant, nous allons examiner le reste du code de génération de modèles automatique et ajouter une méthode de `SearchIndex` et une vue de `SearchIndex` qui vous permet de rechercher des films dans cette base de données.</span><span class="sxs-lookup"><span data-stu-id="16599-183">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="16599-184">Pour plus d’informations sur l’utilisation de Entity Framework avec MVC, consultez [création d’un modèle de données Entity Framework pour une Application ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="16599-184">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="16599-185">[Précédent](creating-a-connection-string.md)
> [Suivant](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="16599-185">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
