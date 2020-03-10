---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Partie 4 : accès aux modèles et aux données | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 4 couvre les modèles et l’accès aux données.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559674"
---
# <a name="part-4-models-and-data-access"></a><span data-ttu-id="7b291-104">Partie 4 : modèles et accès aux données</span><span class="sxs-lookup"><span data-stu-id="7b291-104">Part 4: Models and Data Access</span></span>

<span data-ttu-id="7b291-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="7b291-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="7b291-106">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="7b291-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="7b291-107">Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="7b291-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="7b291-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="7b291-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="7b291-109">La partie 4 couvre les modèles et l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="7b291-109">Part 4 covers Models and Data Access.</span></span>

<span data-ttu-id="7b291-110">Jusqu’à présent, nous venons de transmettre des « données factices » de nos contrôleurs à nos modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="7b291-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="7b291-111">Nous sommes maintenant prêts à raccorder une véritable base de données.</span><span class="sxs-lookup"><span data-stu-id="7b291-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="7b291-112">Dans ce didacticiel, nous allons aborder l’utilisation de SQL Server Compact édition (souvent appelée SQL CE) comme moteur de base de données.</span><span class="sxs-lookup"><span data-stu-id="7b291-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="7b291-113">SQL CE est une base de données gratuite, incorporée et basée sur des fichiers, qui ne nécessite aucune installation ni configuration, ce qui en fait la solution la plus pratique pour le développement local.</span><span class="sxs-lookup"><span data-stu-id="7b291-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="7b291-114">Accès à la base de données avec Entity Framework code-First</span><span class="sxs-lookup"><span data-stu-id="7b291-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="7b291-115">Nous allons utiliser la prise en charge Entity Framework (EF) qui est incluse dans les projets ASP.NET MVC 3 pour interroger et mettre à jour la base de données.</span><span class="sxs-lookup"><span data-stu-id="7b291-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="7b291-116">EF est une API de données de mappage relationnel objet (ORM) flexible qui permet aux développeurs d’interroger et de mettre à jour les données stockées dans une base de données d’une manière orientée objet.</span><span class="sxs-lookup"><span data-stu-id="7b291-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="7b291-117">Entity Framework version 4 prend en charge un paradigme de développement appelé code-First.</span><span class="sxs-lookup"><span data-stu-id="7b291-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="7b291-118">Code-First vous permet de créer un objet de modèle en écrivant des classes simples (également appelées POCO à partir d’objets CLR « Plain-Old ») et peut même créer la base de données à la volée à partir de vos classes.</span><span class="sxs-lookup"><span data-stu-id="7b291-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="7b291-119">Modifications apportées à nos classes de modèle</span><span class="sxs-lookup"><span data-stu-id="7b291-119">Changes to our Model Classes</span></span>

<span data-ttu-id="7b291-120">Nous allons tirer parti de la fonctionnalité de création de base de données dans Entity Framework dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="7b291-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="7b291-121">Avant cela, cependant, nous allons apporter quelques modifications mineures à nos classes de modèle pour ajouter des éléments que nous utiliserons plus tard.</span><span class="sxs-lookup"><span data-stu-id="7b291-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="7b291-122">Ajout des classes du modèle Artist</span><span class="sxs-lookup"><span data-stu-id="7b291-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="7b291-123">Nos albums seront associés à des artistes. nous allons donc ajouter une classe de modèle simple pour décrire un artiste.</span><span class="sxs-lookup"><span data-stu-id="7b291-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="7b291-124">Ajoutez une nouvelle classe au dossier Models nommé Artist.cs à l’aide du code indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7b291-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="7b291-125">Mise à jour de nos classes de modèle</span><span class="sxs-lookup"><span data-stu-id="7b291-125">Updating our Model Classes</span></span>

<span data-ttu-id="7b291-126">Mettez à jour la classe album comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7b291-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="7b291-127">Ensuite, effectuez les mises à jour suivantes de la classe genre.</span><span class="sxs-lookup"><span data-stu-id="7b291-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a><span data-ttu-id="7b291-128">Ajout du dossier de données de l’application\_</span><span class="sxs-lookup"><span data-stu-id="7b291-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="7b291-129">Nous allons ajouter une application\_répertoire de données à notre projet pour stocker les fichiers de base de données SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="7b291-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="7b291-130">Les données d'\_d’application sont un répertoire spécial dans ASP.NET qui possède déjà les autorisations d’accès de sécurité appropriées pour l’accès aux bases de données.</span><span class="sxs-lookup"><span data-stu-id="7b291-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="7b291-131">Dans le menu projet, sélectionnez Ajouter un dossier ASP.NET, puis application\_données.</span><span class="sxs-lookup"><span data-stu-id="7b291-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="7b291-132">Création d’une chaîne de connexion dans le fichier Web. config</span><span class="sxs-lookup"><span data-stu-id="7b291-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="7b291-133">Nous allons ajouter quelques lignes au fichier de configuration du site Web afin que Entity Framework sache comment se connecter à notre base de données.</span><span class="sxs-lookup"><span data-stu-id="7b291-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="7b291-134">Double-cliquez sur le fichier Web. config situé à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="7b291-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="7b291-135">Faites défiler vers le bas de ce fichier et ajoutez une section &lt;connectionStrings&gt; juste au-dessus de la dernière ligne, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7b291-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="7b291-136">Ajout d’une classe de contexte</span><span class="sxs-lookup"><span data-stu-id="7b291-136">Adding a Context Class</span></span>

<span data-ttu-id="7b291-137">Cliquez avec le bouton droit sur le dossier Models et ajoutez une nouvelle classe nommée MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="7b291-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="7b291-138">Cette classe représente le contexte de base de données Entity Framework et gère nos opérations de création, lecture, mise à jour et suppression pour nous.</span><span class="sxs-lookup"><span data-stu-id="7b291-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="7b291-139">Le code de cette classe est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7b291-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="7b291-140">C’est tout ! il n’y a pas d’autres configurations, interfaces spéciales, etc. En étendant la classe de base DbContext, notre classe MusicStoreEntities est en mesure de gérer nos opérations de base de données pour nous.</span><span class="sxs-lookup"><span data-stu-id="7b291-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="7b291-141">Maintenant que nous avons raccroché, nous allons ajouter quelques propriétés à nos classes de modèle pour tirer parti de certaines des informations supplémentaires de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="7b291-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="7b291-142">Ajout de données de catalogue du magasin</span><span class="sxs-lookup"><span data-stu-id="7b291-142">Adding our store catalog data</span></span>

<span data-ttu-id="7b291-143">Nous allons tirer parti d’une fonctionnalité de Entity Framework qui ajoute des données de valeur initiale à une base de données nouvellement créée.</span><span class="sxs-lookup"><span data-stu-id="7b291-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="7b291-144">Cette opération préremplira le catalogue du magasin avec une liste de genres, d’artistes et d’albums.</span><span class="sxs-lookup"><span data-stu-id="7b291-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="7b291-145">Le téléchargement MvcMusicStore-Assets. zip, qui comprenait nos fichiers de conception de site utilisés précédemment dans ce didacticiel, contient un fichier de classe avec ces données de départ, situé dans un dossier nommé code.</span><span class="sxs-lookup"><span data-stu-id="7b291-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="7b291-146">Dans le dossier code/Models, localisez le fichier SampleData.cs et déposez-le dans le dossier Models de notre projet, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7b291-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="7b291-147">À présent, nous devons ajouter une ligne de code pour indiquer à Entity Framework sur cette classe SampleData.</span><span class="sxs-lookup"><span data-stu-id="7b291-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="7b291-148">Double-cliquez sur le fichier global. asax à la racine du projet pour l’ouvrir et ajoutez la ligne suivante au début de la méthode de démarrage de l’application\_.</span><span class="sxs-lookup"><span data-stu-id="7b291-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="7b291-149">À ce stade, nous avons terminé le travail nécessaire pour configurer Entity Framework pour notre projet.</span><span class="sxs-lookup"><span data-stu-id="7b291-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="7b291-150">Interrogation de la base de données</span><span class="sxs-lookup"><span data-stu-id="7b291-150">Querying the Database</span></span>

<span data-ttu-id="7b291-151">Nous allons maintenant mettre à jour notre StoreController afin qu’au lieu d’utiliser des « données factices », elle appelle la base de données pour interroger toutes ses informations.</span><span class="sxs-lookup"><span data-stu-id="7b291-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="7b291-152">Nous allons commencer par déclarer un champ sur le **StoreController** pour contenir une instance de la classe MusicStoreEntities, nommée storeDB :</span><span class="sxs-lookup"><span data-stu-id="7b291-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="7b291-153">Mise à jour de l’index du magasin pour interroger la base de données</span><span class="sxs-lookup"><span data-stu-id="7b291-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="7b291-154">La classe MusicStoreEntities est gérée par le Entity Framework et expose une propriété de collection pour chaque table de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="7b291-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="7b291-155">Nous allons mettre à jour l’action d’index de StoreController pour récupérer tous les genres de notre base de données.</span><span class="sxs-lookup"><span data-stu-id="7b291-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="7b291-156">Auparavant, nous avons effectué ce codage en dur des données de chaîne.</span><span class="sxs-lookup"><span data-stu-id="7b291-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="7b291-157">À présent, nous pouvons simplement utiliser le contexte de Entity Framework de la collection :</span><span class="sxs-lookup"><span data-stu-id="7b291-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="7b291-158">Aucune modification ne doit être apportée à notre modèle de vue, car nous revenons toujours à la même StoreIndexViewModel que celle que nous avons retournée avant-nous nous contentons de renvoyer des données en direct à partir de notre base de données maintenant.</span><span class="sxs-lookup"><span data-stu-id="7b291-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="7b291-159">Lorsque nous exécutons à nouveau le projet et que vous accédez à l’URL « /Store », nous voyons maintenant une liste de tous les genres dans notre base de données :</span><span class="sxs-lookup"><span data-stu-id="7b291-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="7b291-160">Mise à jour de la navigation et des détails du magasin pour utiliser des données actives</span><span class="sxs-lookup"><span data-stu-id="7b291-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="7b291-161">Avec la méthode d’action/Store/Browse ? genre = *[some-genre]* , nous recherchons un genre par nom.</span><span class="sxs-lookup"><span data-stu-id="7b291-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="7b291-162">Nous attendons un seul résultat, puisque nous ne devrions jamais avoir deux entrées pour le même nom de genre, et donc nous pouvons utiliser. Extension unique () dans LINQ pour interroger l’objet genre approprié comme celui-ci (ne le tapez pas encore) :</span><span class="sxs-lookup"><span data-stu-id="7b291-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="7b291-163">La méthode unique prend une expression lambda en tant que paramètre, qui spécifie que nous souhaitons un objet de genre unique, de sorte que son nom corresponde à la valeur que nous avons définie.</span><span class="sxs-lookup"><span data-stu-id="7b291-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="7b291-164">Dans le cas ci-dessus, nous chargeons un objet de genre unique avec une valeur de nom correspondant à Disco.</span><span class="sxs-lookup"><span data-stu-id="7b291-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="7b291-165">Nous allons tirer parti d’une fonctionnalité de Entity Framework qui nous permet d’indiquer d’autres entités associées que nous voulons charger également lorsque l’objet genre est récupéré.</span><span class="sxs-lookup"><span data-stu-id="7b291-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="7b291-166">Cette fonctionnalité est appelée mise en forme des résultats de la requête et nous permet de réduire le nombre de fois où nous avons besoin d’accéder à la base de données pour récupérer toutes les informations dont nous avons besoin.</span><span class="sxs-lookup"><span data-stu-id="7b291-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="7b291-167">Nous souhaitons prérécupérer les albums pour le genre que nous récupérons. nous allons donc mettre à jour notre requête pour inclure from genres. include ("Albums") pour indiquer que nous voulons également les albums associés.</span><span class="sxs-lookup"><span data-stu-id="7b291-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="7b291-168">Cette opération est plus efficace, car elle récupère les données de genre et d’album dans une demande de base de données unique.</span><span class="sxs-lookup"><span data-stu-id="7b291-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="7b291-169">Avec les explications, voici à quoi ressemble notre action de contrôleur de navigation mise à jour :</span><span class="sxs-lookup"><span data-stu-id="7b291-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="7b291-170">Nous pouvons maintenant mettre à jour la vue de navigation dans le magasin pour afficher les albums disponibles dans chaque genre.</span><span class="sxs-lookup"><span data-stu-id="7b291-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="7b291-171">Ouvrez le modèle de vue (situé dans/Views/Store/Browse.cshtml) et ajoutez une liste à puces d’albums comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7b291-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="7b291-172">L’exécution de notre application et l’exploration de/Store/Browse ? genre = jazz montrent que nos résultats sont désormais extraits de la base de données, affichant tous les albums dans le genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="7b291-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="7b291-173">Nous allons apporter la même modification à notre URL/Store/Details/[id] et remplacer nos données fictives par une requête de base de données qui charge un album dont l’ID correspond à la valeur du paramètre.</span><span class="sxs-lookup"><span data-stu-id="7b291-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="7b291-174">L’exécution de notre application et l’exploration de/Store/Details/1 montrent que nos résultats sont désormais extraits de la base de données.</span><span class="sxs-lookup"><span data-stu-id="7b291-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="7b291-175">Maintenant que la page de détails du magasin est configurée pour afficher un album par l’ID de l’album, nous allons mettre à jour la vue **Parcourir** pour établir un lien vers le mode Détails.</span><span class="sxs-lookup"><span data-stu-id="7b291-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="7b291-176">Nous allons utiliser HTML. ActionLink, exactement comme nous l’avons fait pour établir une liaison à partir de l’index de Store à la fin de la section précédente.</span><span class="sxs-lookup"><span data-stu-id="7b291-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="7b291-177">La source complète de la vue Parcourir s’affiche ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7b291-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="7b291-178">Nous sommes maintenant en mesure de naviguer à partir de la page de votre boutique vers une page de genre, qui répertorie les albums disponibles, et en cliquant sur un album, vous pouvez afficher les détails de cet album.</span><span class="sxs-lookup"><span data-stu-id="7b291-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="7b291-179">[Précédent](mvc-music-store-part-3.md)
> [Suivant](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="7b291-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
