---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Partie 2 : contrôleurs | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 2 couvre les contrôleurs.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559877"
---
# <a name="part-2-controllers"></a><span data-ttu-id="117ca-104">Partie 2 : contrôleurs</span><span class="sxs-lookup"><span data-stu-id="117ca-104">Part 2: Controllers</span></span>

<span data-ttu-id="117ca-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="117ca-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="117ca-106">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="117ca-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="117ca-107">Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="117ca-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="117ca-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="117ca-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="117ca-109">La partie 2 couvre les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="117ca-109">Part 2 covers Controllers.</span></span>

<span data-ttu-id="117ca-110">Avec les infrastructures Web traditionnelles, les URL entrantes sont généralement mappées à des fichiers sur disque.</span><span class="sxs-lookup"><span data-stu-id="117ca-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="117ca-111">Par exemple : une requête pour une URL telle que « /Products.aspx » ou « /Products.php » peut être traitée par un fichier « Products. aspx » ou « Products. php ».</span><span class="sxs-lookup"><span data-stu-id="117ca-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="117ca-112">Les frameworks MVC basés sur le Web mappent les URL au code du serveur d’une manière légèrement différente.</span><span class="sxs-lookup"><span data-stu-id="117ca-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="117ca-113">Au lieu de mapper les URL entrantes aux fichiers, elles mappent les URL aux méthodes sur les classes.</span><span class="sxs-lookup"><span data-stu-id="117ca-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="117ca-114">Ces classes sont appelées « contrôleurs » et sont responsables du traitement des demandes HTTP entrantes, de la gestion des entrées d’utilisateur, de la récupération et de l’enregistrement des données et de la détermination de la réponse à renvoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers un autre URL, etc.).</span><span class="sxs-lookup"><span data-stu-id="117ca-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="117ca-115">Ajout d’un HomeController</span><span class="sxs-lookup"><span data-stu-id="117ca-115">Adding a HomeController</span></span>

<span data-ttu-id="117ca-116">Nous allons commencer notre application MVC Music Store en ajoutant une classe de contrôleur qui traitera les URL vers la page d’hébergement de notre site.</span><span class="sxs-lookup"><span data-stu-id="117ca-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="117ca-117">Nous allons suivre les conventions d’affectation de noms par défaut de ASP.NET MVC et l’appeler HomeController.</span><span class="sxs-lookup"><span data-stu-id="117ca-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="117ca-118">Cliquez avec le bouton droit sur le dossier « Controllers » dans le Explorateur de solutions et sélectionnez « Ajouter », puis le « contrôleur... » commande</span><span class="sxs-lookup"><span data-stu-id="117ca-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="117ca-119">La boîte de dialogue « Ajouter un contrôleur » s’affiche.</span><span class="sxs-lookup"><span data-stu-id="117ca-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="117ca-120">Nommez le contrôleur « HomeController » et appuyez sur le bouton Ajouter.</span><span class="sxs-lookup"><span data-stu-id="117ca-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="117ca-121">Cela créera un nouveau fichier, HomeController.cs, avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="117ca-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="117ca-122">Pour démarrer le plus simplement possible, remplacez la méthode d’index par une méthode simple qui retourne simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="117ca-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="117ca-123">Nous allons apporter deux modifications :</span><span class="sxs-lookup"><span data-stu-id="117ca-123">We'll make two changes:</span></span>

- <span data-ttu-id="117ca-124">Modifiez la méthode pour retourner une chaîne au lieu d’un ActionResult</span><span class="sxs-lookup"><span data-stu-id="117ca-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="117ca-125">Modifiez l’instruction return pour retourner « hello from the Accueil »</span><span class="sxs-lookup"><span data-stu-id="117ca-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="117ca-126">La méthode doit maintenant ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="117ca-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="117ca-127">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="117ca-127">Running the Application</span></span>

<span data-ttu-id="117ca-128">À présent, nous allons exécuter le site.</span><span class="sxs-lookup"><span data-stu-id="117ca-128">Now let's run the site.</span></span> <span data-ttu-id="117ca-129">Nous pouvons démarrer notre serveur Web et essayer le site en utilisant l’un des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="117ca-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="117ca-130">Choisissez l’élément de menu Déboguer ⇨ démarrer le débogage</span><span class="sxs-lookup"><span data-stu-id="117ca-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="117ca-131">Cliquez sur le bouton représentant une flèche verte dans la barre d’outils ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="117ca-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="117ca-132">Utilisez le raccourci clavier F5.</span><span class="sxs-lookup"><span data-stu-id="117ca-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="117ca-133">À l’aide de l’une des étapes ci-dessus, nous allons compiler notre projet, puis provoquer le démarrage de la Serveur de développement ASP.NET intégrée à Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="117ca-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="117ca-134">Une notification s’affiche dans le coin inférieur de l’écran pour indiquer que le Serveur de développement ASP.NET a démarré et indique le numéro de port sous lequel il s’exécute.</span><span class="sxs-lookup"><span data-stu-id="117ca-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="117ca-135">Visual Web Developer ouvre automatiquement une fenêtre de navigateur dont l’URL pointe vers notre serveur Web.</span><span class="sxs-lookup"><span data-stu-id="117ca-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="117ca-136">Cela nous permettra de tester rapidement notre application Web :</span><span class="sxs-lookup"><span data-stu-id="117ca-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="117ca-137">Bien, c’était assez rapide : nous avons créé un nouveau site Web, ajouté une fonction à trois lignes, et nous obtenons du texte dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="117ca-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="117ca-138">Pas de fusée science, mais c’est un début.</span><span class="sxs-lookup"><span data-stu-id="117ca-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="117ca-139">*Remarque : Visual Web Developer comprend le Serveur de développement ASP.NET, qui exécutera votre site Web sur un numéro de « port » gratuit. Dans la capture d’écran ci-dessus, le site s’exécute sur `http://localhost:26641/`, donc il utilise le port 26641. Le numéro de votre port sera différent. Lorsque nous parlerons des URL comme/Store/Browse dans ce didacticiel, celles-ci seront placées après le numéro de port. En supposant qu’il s’agit d’un numéro de port de 26641, la navigation vers/Store/Browse signifie `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="117ca-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="117ca-140">Ajout d’un StoreController</span><span class="sxs-lookup"><span data-stu-id="117ca-140">Adding a StoreController</span></span>

<span data-ttu-id="117ca-141">Nous avons ajouté un contrôle HomeController simple qui implémente la page d’hébergement de notre site.</span><span class="sxs-lookup"><span data-stu-id="117ca-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="117ca-142">Nous allons maintenant ajouter un autre contrôleur que nous allons utiliser pour implémenter la fonctionnalité de navigation de notre magasin musical.</span><span class="sxs-lookup"><span data-stu-id="117ca-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="117ca-143">Notre contrôleur de magasin prend en charge trois scénarios :</span><span class="sxs-lookup"><span data-stu-id="117ca-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="117ca-144">Page de liste des genres musicaux dans notre magasin de musique</span><span class="sxs-lookup"><span data-stu-id="117ca-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="117ca-145">Une page de navigation qui répertorie tous les albums musicaux dans un genre particulier</span><span class="sxs-lookup"><span data-stu-id="117ca-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="117ca-146">Page de détails qui affiche des informations sur un album de musique spécifique</span><span class="sxs-lookup"><span data-stu-id="117ca-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="117ca-147">Nous allons commencer par ajouter une nouvelle classe StoreController.</span><span class="sxs-lookup"><span data-stu-id="117ca-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="117ca-148">Si vous ne l’avez pas encore fait, arrêtez l’exécution de l’application en fermant le navigateur ou en sélectionnant l’élément de menu Déboguer ⇨ arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="117ca-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="117ca-149">Ajoutez maintenant un nouveau StoreController.</span><span class="sxs-lookup"><span data-stu-id="117ca-149">Now add a new StoreController.</span></span> <span data-ttu-id="117ca-150">Comme nous l’avons fait avec HomeController, nous allons le faire en cliquant avec le bouton droit sur le dossier « Controllers » dans le Explorateur de solutions et en choisissant l’élément de menu Add-&gt;Controller.</span><span class="sxs-lookup"><span data-stu-id="117ca-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="117ca-151">Notre nouvelle StoreController a déjà une méthode « index ».</span><span class="sxs-lookup"><span data-stu-id="117ca-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="117ca-152">Nous allons utiliser cette méthode « index » pour implémenter notre page de listing qui répertorie tous les genres de notre magasin musical.</span><span class="sxs-lookup"><span data-stu-id="117ca-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="117ca-153">Nous allons également ajouter deux méthodes supplémentaires pour implémenter les deux autres scénarios que nous souhaitons que notre StoreController gère : parcourir et détails.</span><span class="sxs-lookup"><span data-stu-id="117ca-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="117ca-154">Ces méthodes (index, navigation et détails) au sein de notre contrôleur sont appelées « actions de contrôleur », et comme vous l’avez déjà vu avec la méthode d’action HomeController. index (), leur travail est de répondre aux demandes d’URL et (en général) de déterminer le contenu doit être renvoyé au navigateur ou à l’utilisateur qui a appelé l’URL.</span><span class="sxs-lookup"><span data-stu-id="117ca-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="117ca-155">Nous allons démarrer notre implémentation de StoreController en modifiant la méthode theIndex () pour retourner la chaîne « Hello from Store. index () » et nous ajouterons des méthodes similaires pour Browse () et Details () :</span><span class="sxs-lookup"><span data-stu-id="117ca-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="117ca-156">Réexécutez le projet et parcourez les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="117ca-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="117ca-157">/Store</span><span class="sxs-lookup"><span data-stu-id="117ca-157">/Store</span></span>
- <span data-ttu-id="117ca-158">/Store/Browse</span><span class="sxs-lookup"><span data-stu-id="117ca-158">/Store/Browse</span></span>
- <span data-ttu-id="117ca-159">/Store/Details</span><span class="sxs-lookup"><span data-stu-id="117ca-159">/Store/Details</span></span>

<span data-ttu-id="117ca-160">L’accès à ces URL appellera les méthodes d’action dans notre contrôleur et renverra les réponses de chaîne :</span><span class="sxs-lookup"><span data-stu-id="117ca-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="117ca-161">C’est parfait, mais il s’agit simplement de chaînes constantes.</span><span class="sxs-lookup"><span data-stu-id="117ca-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="117ca-162">Nous allons les rendre dynamiques, afin qu’ils prennent les informations de l’URL et les affichent dans la sortie de la page.</span><span class="sxs-lookup"><span data-stu-id="117ca-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="117ca-163">Tout d’abord, nous allons modifier la méthode d’action Browse pour récupérer une valeur QueryString à partir de l’URL.</span><span class="sxs-lookup"><span data-stu-id="117ca-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="117ca-164">Pour ce faire, nous pouvons ajouter un paramètre « genre » à notre méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="117ca-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="117ca-165">Lorsque nous procédons ainsi, ASP.NET MVC passe automatiquement tout paramètre de publication QueryString ou de formulaire nommé « genre » à notre méthode d’action lorsqu’il est appelé.</span><span class="sxs-lookup"><span data-stu-id="117ca-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="117ca-166">*Remarque : nous utilisons la méthode de l’utilitaire HttpUtility. HtmlEncode pour assainir les entrées utilisateur. Cela empêche les utilisateurs d’injecter JavaScript dans notre vue avec un lien tel que/Store/Browse ? Genre =&lt;script&gt;Window. Location = 'http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="117ca-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="117ca-167">Nous allons maintenant accéder à/Store/Browse ? Genre = Disco</span><span class="sxs-lookup"><span data-stu-id="117ca-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="117ca-168">Nous allons ensuite modifier l’action détails pour lire et afficher un paramètre d’entrée nommé ID.</span><span class="sxs-lookup"><span data-stu-id="117ca-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="117ca-169">Contrairement à la méthode précédente, nous n’incorporerons pas la valeur d’ID en tant que paramètre QueryString.</span><span class="sxs-lookup"><span data-stu-id="117ca-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="117ca-170">Au lieu de cela, nous l’incorporons directement dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="117ca-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="117ca-171">Par exemple:/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="117ca-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="117ca-172">ASP.NET MVC nous permet de le faire facilement sans avoir à configurer quoi que ce soit.</span><span class="sxs-lookup"><span data-stu-id="117ca-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="117ca-173">La Convention de routage par défaut de ASP.NET MVC consiste à traiter le segment d’une URL après le nom de la méthode d’action en tant que paramètre nommé « ID ».</span><span class="sxs-lookup"><span data-stu-id="117ca-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="117ca-174">Si votre méthode d’action a un paramètre nommé ID, ASP.NET MVC passe automatiquement le segment d’URL à vous en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="117ca-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="117ca-175">Exécutez l’application et accédez à/Store/Details/5 :</span><span class="sxs-lookup"><span data-stu-id="117ca-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="117ca-176">Récapitulons ce que nous avons fait jusqu’à présent :</span><span class="sxs-lookup"><span data-stu-id="117ca-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="117ca-177">Nous avons créé un nouveau projet MVC ASP.NET dans Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="117ca-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="117ca-178">Nous avons abordé la structure de dossiers de base d’une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="117ca-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="117ca-179">Nous avons appris à exécuter notre site Web à l’aide de l’Serveur de développement ASP.NET</span><span class="sxs-lookup"><span data-stu-id="117ca-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="117ca-180">Nous avons créé deux classes de contrôleur : un HomeController et un StoreController</span><span class="sxs-lookup"><span data-stu-id="117ca-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="117ca-181">Nous avons ajouté des méthodes d’action à nos contrôleurs qui répondent aux demandes d’URL et renvoient du texte au navigateur.</span><span class="sxs-lookup"><span data-stu-id="117ca-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="117ca-182">[Précédent](mvc-music-store-part-1.md)
> [Suivant](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="117ca-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
