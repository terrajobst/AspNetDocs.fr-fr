---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Partie 3 : vues et ViewModels | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store. La partie 3 couvre les vues et les ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559807"
---
# <a name="part-3-views-and-viewmodels"></a><span data-ttu-id="2baa6-104">Partie 3 : vues et ViewModels</span><span class="sxs-lookup"><span data-stu-id="2baa6-104">Part 3: Views and ViewModels</span></span>

<span data-ttu-id="2baa6-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="2baa6-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="2baa6-106">Le magasin de musique MVC est une application de didacticiel qui présente et explique pas à pas comment utiliser ASP.NET MVC et Visual Studio pour le développement Web.</span><span class="sxs-lookup"><span data-stu-id="2baa6-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="2baa6-107">Le magasin de musique MVC est une implémentation de magasin légère qui vend des albums musicaux en ligne et implémente l’administration de site de base, la connexion utilisateur et la fonctionnalité de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="2baa6-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="2baa6-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="2baa6-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="2baa6-109">La partie 3 couvre les vues et les ViewModels.</span><span class="sxs-lookup"><span data-stu-id="2baa6-109">Part 3 covers Views and ViewModels.</span></span>

<span data-ttu-id="2baa6-110">Jusqu’à présent, nous venons de retourner des chaînes à partir d’actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="2baa6-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="2baa6-111">C’est un excellent moyen d’avoir une idée du fonctionnement des contrôleurs, mais il ne s’agit pas de la façon dont vous souhaiteriez créer une application Web réelle.</span><span class="sxs-lookup"><span data-stu-id="2baa6-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="2baa6-112">Nous allons avoir besoin d’une meilleure façon de générer du code HTML dans les navigateurs qui visitent notre site, où nous pouvons utiliser des fichiers de modèle pour personnaliser plus facilement le contenu HTML renvoyé.</span><span class="sxs-lookup"><span data-stu-id="2baa6-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="2baa6-113">C’est exactement ce que font les vues.</span><span class="sxs-lookup"><span data-stu-id="2baa6-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="2baa6-114">Ajout d’un modèle de vue</span><span class="sxs-lookup"><span data-stu-id="2baa6-114">Adding a View template</span></span>

<span data-ttu-id="2baa6-115">Pour utiliser un modèle de vue, nous allons modifier la méthode de l’index HomeController pour retourner un ActionResult et le faire retourner View (), comme ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="2baa6-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="2baa6-116">La modification ci-dessus indique que, au lieu de retourner une chaîne, nous souhaitons plutôt utiliser une « vue » pour générer un résultat en retour.</span><span class="sxs-lookup"><span data-stu-id="2baa6-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="2baa6-117">Nous allons maintenant ajouter un modèle de vue approprié à notre projet.</span><span class="sxs-lookup"><span data-stu-id="2baa6-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="2baa6-118">Pour ce faire, nous allons positionner le curseur de texte dans la méthode d’action index, puis cliquer avec le bouton droit et sélectionner « Add View » (ajouter une vue).</span><span class="sxs-lookup"><span data-stu-id="2baa6-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="2baa6-119">La boîte de dialogue Ajouter une vue s’affiche :</span><span class="sxs-lookup"><span data-stu-id="2baa6-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="2baa6-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2baa6-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="2baa6-121">La boîte de dialogue « Ajouter une vue » nous permet de générer rapidement et facilement des fichiers de modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="2baa6-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="2baa6-122">Par défaut, la boîte de dialogue « Ajouter une vue » prérenseigne le nom du modèle de vue à créer afin qu’il corresponde à la méthode d’action qui va l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="2baa6-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="2baa6-123">Étant donné que nous avons utilisé le menu contextuel « ajouter une vue » dans la méthode d’action index () de notre HomeController, la boîte de dialogue « Ajouter une vue » ci-dessus contient « index » comme nom de vue prérempli par défaut.</span><span class="sxs-lookup"><span data-stu-id="2baa6-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="2baa6-124">Nous n’avons pas besoin de modifier les options de cette boîte de dialogue. par conséquent, cliquez sur le bouton Ajouter.</span><span class="sxs-lookup"><span data-stu-id="2baa6-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="2baa6-125">Lorsque nous cliquons sur le bouton Ajouter, Visual Web Developer crée un modèle d’affichage index. cshtml pour nous dans le répertoire \Views\Home, en créant le dossier s’il n’existe pas déjà.</span><span class="sxs-lookup"><span data-stu-id="2baa6-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="2baa6-126">Le nom et l’emplacement du dossier du fichier « index. cshtml » sont importants et suivent les conventions d’attribution de noms ASP.NET MVC par défaut.</span><span class="sxs-lookup"><span data-stu-id="2baa6-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="2baa6-127">Le nom du répertoire, \Views\Home, correspond au contrôleur, appelé HomeController.</span><span class="sxs-lookup"><span data-stu-id="2baa6-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="2baa6-128">Le nom du modèle de vue, index, correspond à la méthode d’action du contrôleur qui affiche la vue.</span><span class="sxs-lookup"><span data-stu-id="2baa6-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="2baa6-129">ASP.NET MVC nous permet d’éviter d’avoir à spécifier explicitement le nom ou l’emplacement d’un modèle de vue lorsque nous utilisons cette Convention d’affectation de noms pour retourner une vue.</span><span class="sxs-lookup"><span data-stu-id="2baa6-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="2baa6-130">Par défaut, il affiche le modèle de vue \Views\Home\Index.cshtml quand nous écrivons du code comme ci-dessous dans le HomeController :</span><span class="sxs-lookup"><span data-stu-id="2baa6-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="2baa6-131">Visual Web Developer a créé et ouvert le modèle de vue « index. cshtml » après avoir cliqué sur le bouton « Ajouter » dans la boîte de dialogue « Ajouter une vue ».</span><span class="sxs-lookup"><span data-stu-id="2baa6-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="2baa6-132">Le contenu de index. cshtml est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2baa6-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="2baa6-133">Cette vue utilise le syntaxe Razor, qui est plus concis que le moteur d’affichage des Web Forms utilisé dans ASP.NET Web Forms et les versions précédentes de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2baa6-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="2baa6-134">Le moteur d’affichage des Web Forms est toujours disponible dans ASP.NET MVC 3, mais de nombreux développeurs trouvent que le moteur d’affichage Razor est très bien adapté au développement ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2baa6-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="2baa6-135">Les trois premières lignes définissent le titre de la page à l’aide de ViewBag. title.</span><span class="sxs-lookup"><span data-stu-id="2baa6-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="2baa6-136">Nous allons voir comment cela fonctionne plus rapidement, mais nous allons tout d’abord mettre à jour le texte de l’en-tête de texte et afficher la page.</span><span class="sxs-lookup"><span data-stu-id="2baa6-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="2baa6-137">Mettez à jour la balise &lt;H2&gt; pour indiquer « il s’agit de la page d’hébergement », comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2baa6-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="2baa6-138">L’exécution de l’application montre que notre nouveau texte est visible sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="2baa6-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="2baa6-139">Utilisation d’une disposition pour les éléments de site communs</span><span class="sxs-lookup"><span data-stu-id="2baa6-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="2baa6-140">La plupart des sites Web disposent de contenu partagé entre de nombreuses pages : navigation, pieds de page, images de logos, références de feuille de style, etc. Le moteur d’affichage Razor facilite la gestion à l’aide d’une page appelée \_Layout. cshtml, qui a été créée automatiquement pour nous dans le dossier/Views/Shared</span><span class="sxs-lookup"><span data-stu-id="2baa6-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="2baa6-141">Double-cliquez sur ce dossier pour afficher son contenu, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2baa6-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="2baa6-142">Le contenu de nos vues individuelles est affiché par la commande @RenderBody() et tout contenu courant que vous souhaitez voir apparaître en dehors de peut être ajouté au balisage \_Layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="2baa6-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="2baa6-143">Nous souhaitons que notre magasin de musique MVC ait un en-tête commun avec des liens vers notre page d’hébergement et une zone de stockage sur toutes les pages du site. nous allons donc l’ajouter au modèle juste au-dessus de l’instruction @RenderBody().</span><span class="sxs-lookup"><span data-stu-id="2baa6-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="2baa6-144">Mise à jour de la feuille de style</span><span class="sxs-lookup"><span data-stu-id="2baa6-144">Updating the StyleSheet</span></span>

<span data-ttu-id="2baa6-145">Le modèle de projet vide comprend un fichier CSS très rationalisé qui comprend simplement les styles utilisés pour afficher les messages de validation.</span><span class="sxs-lookup"><span data-stu-id="2baa6-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="2baa6-146">Notre concepteur a fourni des CSS et des images supplémentaires pour définir l’apparence de notre site. nous allons donc les ajouter maintenant.</span><span class="sxs-lookup"><span data-stu-id="2baa6-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="2baa6-147">Le fichier CSS et les images mis à jour sont inclus dans le répertoire de contenu de MvcMusicStore-Assets. zip, qui est disponible dans [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="2baa6-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="2baa6-148">Nous allons les sélectionner dans l’Explorateur Windows et les déposer dans le dossier content de la solution dans Visual Web Developer, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="2baa6-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="2baa6-149">Vous êtes invité à confirmer si vous souhaitez remplacer le fichier site. CSS existant.</span><span class="sxs-lookup"><span data-stu-id="2baa6-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="2baa6-150">Cliquez sur Oui.</span><span class="sxs-lookup"><span data-stu-id="2baa6-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="2baa6-151">Le dossier de contenu de votre application s’affiche à présent comme suit :</span><span class="sxs-lookup"><span data-stu-id="2baa6-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="2baa6-152">À présent, nous allons exécuter l’application et voir comment nos modifications apparaissent sur la page d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="2baa6-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="2baa6-153">Passons en revue ce qui a changé : la méthode d’action d’index du HomeController a trouvé et affiché le modèle \Views\Home\Index.cshtmlView, bien que notre code appelé « Return View () », parce que notre modèle de vue suivait la Convention d’affectation de noms standard.</span><span class="sxs-lookup"><span data-stu-id="2baa6-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="2baa6-154">La page d’accueil affiche un message d’accueil simple défini dans le modèle de vue \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="2baa6-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="2baa6-155">La page d’accueil utilise notre modèle \_Layout. cshtml. le message d’accueil est donc contenu dans la disposition HTML du site standard.</span><span class="sxs-lookup"><span data-stu-id="2baa6-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="2baa6-156">Utilisation d’un modèle pour transmettre des informations à notre vue</span><span class="sxs-lookup"><span data-stu-id="2baa6-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="2baa6-157">Un modèle de vue qui affiche simplement du code HTML codé en dur n’est pas destiné à créer un site Web très intéressant.</span><span class="sxs-lookup"><span data-stu-id="2baa6-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="2baa6-158">Pour créer un site Web dynamique, nous voulons plutôt transmettre des informations de nos actions de contrôleur à nos modèles de vue.</span><span class="sxs-lookup"><span data-stu-id="2baa6-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="2baa6-159">Dans le modèle Model-View-Controller, le terme modèle fait référence aux objets qui représentent les données de l’application.</span><span class="sxs-lookup"><span data-stu-id="2baa6-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="2baa6-160">Souvent, les objets de modèle correspondent aux tables de votre base de données, mais ils n’en ont pas besoin.</span><span class="sxs-lookup"><span data-stu-id="2baa6-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="2baa6-161">Les méthodes d’action du contrôleur qui retournent un ActionResult peuvent passer un objet de modèle à la vue.</span><span class="sxs-lookup"><span data-stu-id="2baa6-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="2baa6-162">Cela permet à un contrôleur de regrouper correctement toutes les informations nécessaires à la génération d’une réponse, puis de transmettre ces informations à un modèle de vue à utiliser pour générer la réponse HTML appropriée.</span><span class="sxs-lookup"><span data-stu-id="2baa6-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="2baa6-163">Cela est plus facile à comprendre en l’regardant en action, donc commençons.</span><span class="sxs-lookup"><span data-stu-id="2baa6-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="2baa6-164">Tout d’abord, nous allons créer des classes de modèle pour représenter des genres et des albums au sein de notre magasin.</span><span class="sxs-lookup"><span data-stu-id="2baa6-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="2baa6-165">Commençons par créer une classe genre.</span><span class="sxs-lookup"><span data-stu-id="2baa6-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="2baa6-166">Cliquez avec le bouton droit sur le dossier « Models » dans votre projet, choisissez l’option « Ajouter une classe » et nommez le fichier « Genre.cs ».</span><span class="sxs-lookup"><span data-stu-id="2baa6-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="2baa6-167">Ajoutez ensuite une propriété de nom de chaîne publique à la classe qui a été créée :</span><span class="sxs-lookup"><span data-stu-id="2baa6-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="2baa6-168">*Remarque : au cas où vous vous poseriez la question, la notation {obtient ; Set ;} C#utilise la fonctionnalité de propriétés implémentées automatiquement. Cela nous offre les avantages d’une propriété sans que nous puissions déclarer un champ de stockage.*</span><span class="sxs-lookup"><span data-stu-id="2baa6-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="2baa6-169">Ensuite, suivez les mêmes étapes pour créer une classe album (nommée Album.cs) qui a un titre et une propriété genre :</span><span class="sxs-lookup"><span data-stu-id="2baa6-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="2baa6-170">Nous pouvons maintenant modifier le StoreController pour utiliser des vues qui affichent des informations dynamiques à partir de notre modèle.</span><span class="sxs-lookup"><span data-stu-id="2baa6-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="2baa6-171">Si, à des fins de démonstration, nous avons nommé nos albums en fonction de l’ID de la demande, nous pourrions afficher ces informations comme dans l’affichage ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2baa6-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="2baa6-172">Nous allons commencer par modifier l’action Détails du magasin pour afficher les informations relatives à un seul album.</span><span class="sxs-lookup"><span data-stu-id="2baa6-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="2baa6-173">Ajoutez une instruction « using » en haut de la classe **StoreControllers** pour inclure l’espace de noms MvcMusicStore. Models. par conséquent, nous n’avons pas besoin de taper MvcMusicStore. Models. album chaque fois que nous souhaitons utiliser la classe album.</span><span class="sxs-lookup"><span data-stu-id="2baa6-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="2baa6-174">La section « usings » de cette classe doit maintenant apparaître comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2baa6-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="2baa6-175">Ensuite, nous allons mettre à jour l’action du contrôleur des détails afin qu’elle renvoie un ActionResult plutôt qu’une chaîne, comme nous l’avons fait avec la méthode d’index du HomeController.</span><span class="sxs-lookup"><span data-stu-id="2baa6-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="2baa6-176">À présent, nous pouvons modifier la logique pour retourner un objet album à la vue.</span><span class="sxs-lookup"><span data-stu-id="2baa6-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="2baa6-177">Plus loin dans ce didacticiel, nous allons extraire les données d’une base de données. pour l’instant, nous allons utiliser des « données factices » pour commencer.</span><span class="sxs-lookup"><span data-stu-id="2baa6-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="2baa6-178">*Remarque : Si vous n’êtes pas familiarisé C#avec, vous pouvez supposer que l’utilisation de var signifie que notre variable album est à liaison tardive. Ce n’est pas correct : C# le compilateur utilise l’inférence de type en fonction de ce que nous attribuons à la variable pour déterminer que cet album est de type album et la compilation de la variable album locale en tant que type d’album. nous obtenons donc la vérification au moment de la compilation et la prise en charge de l’éditeur de code Visual Studio.*</span><span class="sxs-lookup"><span data-stu-id="2baa6-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="2baa6-179">Créons maintenant un modèle de vue qui utilise notre album pour générer une réponse HTML.</span><span class="sxs-lookup"><span data-stu-id="2baa6-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="2baa6-180">Avant cela, nous devons générer le projet afin que la boîte de dialogue Ajouter une vue sache la classe d’album nouvellement créée.</span><span class="sxs-lookup"><span data-stu-id="2baa6-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="2baa6-181">Vous pouvez générer le projet en sélectionnant l’élément de menu Debug ⇨ Build MvcMusicStore (pour un crédit supplémentaire, vous pouvez utiliser le raccourci Ctrl-Shift-B pour générer le projet).</span><span class="sxs-lookup"><span data-stu-id="2baa6-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="2baa6-182">Maintenant que nous avons configuré nos classes de prise en charge, nous sommes prêts à créer notre modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="2baa6-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="2baa6-183">Cliquez avec le bouton droit dans la méthode Details et sélectionnez « Ajouter une vue... » dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="2baa6-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="2baa6-184">Nous allons créer un modèle de vue comme nous l’avons fait précédemment avec le HomeController.</span><span class="sxs-lookup"><span data-stu-id="2baa6-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="2baa6-185">Comme nous l’avons créé à partir du StoreController, il sera généré par défaut dans un fichier \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="2baa6-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="2baa6-186">Contrairement à ce qui précède, nous allons cocher la case « créer une vue fortement typée ».</span><span class="sxs-lookup"><span data-stu-id="2baa6-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="2baa6-187">Nous allons ensuite sélectionner notre classe « album » dans la liste déroulante « Afficher la classe de données ».</span><span class="sxs-lookup"><span data-stu-id="2baa6-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="2baa6-188">Dans ce cas, la boîte de dialogue « Ajouter une vue » crée un modèle de vue qui s’attend à ce qu’un objet album lui soit transmis pour être utilisé.</span><span class="sxs-lookup"><span data-stu-id="2baa6-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="2baa6-189">Lorsque nous cliquons sur le bouton « Ajouter », notre modèle de vue \Views\Store\Details.cshtml sera créé, contenant le code suivant.</span><span class="sxs-lookup"><span data-stu-id="2baa6-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="2baa6-190">Notez la première ligne, qui indique que cette vue est fortement typée dans notre classe album.</span><span class="sxs-lookup"><span data-stu-id="2baa6-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="2baa6-191">Le moteur de vue Razor comprend qu’il a été passé à un objet album. nous pouvons donc facilement accéder aux propriétés du modèle et même bénéficier de l’avantage d’IntelliSense dans l’éditeur de Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="2baa6-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="2baa6-192">Mettez à jour la balise &lt;H2&gt; pour qu’elle affiche la propriété Title de l’album en modifiant cette ligne de manière à ce qu’elle apparaisse comme suit.</span><span class="sxs-lookup"><span data-stu-id="2baa6-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="2baa6-193">Notez qu’IntelliSense est déclenché lorsque vous entrez le point après le mot clé @Model, en indiquant les propriétés et les méthodes prises en charge par la classe album.</span><span class="sxs-lookup"><span data-stu-id="2baa6-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="2baa6-194">Réexécutez maintenant notre projet et visitez l’URL/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="2baa6-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="2baa6-195">Nous allons voir les détails d’un album comme ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2baa6-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="2baa6-196">Nous allons maintenant effectuer une mise à jour similaire pour la méthode d’action Store Browse.</span><span class="sxs-lookup"><span data-stu-id="2baa6-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="2baa6-197">Mettez à jour la méthode afin qu’elle retourne un ActionResult et modifiez la logique de la méthode afin qu’elle crée un nouvel objet genre et la retourne à la vue.</span><span class="sxs-lookup"><span data-stu-id="2baa6-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="2baa6-198">Cliquez avec le bouton droit dans la méthode browse et sélectionnez « Ajouter une vue... » dans le menu contextuel, ajoutez une vue fortement typée ajouter un fortement typé à la classe genre.</span><span class="sxs-lookup"><span data-stu-id="2baa6-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="2baa6-199">Mettez à jour l’élément &lt;H2&gt; dans le code de vue (dans/Views/Store/Browse.cshtml) pour afficher les informations de genre.</span><span class="sxs-lookup"><span data-stu-id="2baa6-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="2baa6-200">Nous allons maintenant réexécuter notre projet et accéder au/Store/Browse ? Genre = URL Disco.</span><span class="sxs-lookup"><span data-stu-id="2baa6-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="2baa6-201">La page parcourir s’affiche comme ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2baa6-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="2baa6-202">Enfin, nous allons créer une mise à jour légèrement plus complexe de la méthode et de la vue d’action de l' **index Store** pour afficher une liste de tous les genres de notre magasin.</span><span class="sxs-lookup"><span data-stu-id="2baa6-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="2baa6-203">Nous allons le faire en utilisant une liste de genres comme objet de modèle, plutôt qu’un simple genre.</span><span class="sxs-lookup"><span data-stu-id="2baa6-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="2baa6-204">Cliquez avec le bouton droit dans la méthode d’action stocker l’index et sélectionnez Ajouter une vue comme auparavant, sélectionnez genre comme classe de modèle, puis appuyez sur le bouton Ajouter.</span><span class="sxs-lookup"><span data-stu-id="2baa6-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="2baa6-205">Tout d’abord, nous allons modifier la déclaration de @model pour indiquer que la vue attendra plusieurs objets de genre plutôt qu’un seul.</span><span class="sxs-lookup"><span data-stu-id="2baa6-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="2baa6-206">Modifiez la première ligne de/Store/Index.cshtml comme suit :</span><span class="sxs-lookup"><span data-stu-id="2baa6-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="2baa6-207">Cela indique au moteur de vue Razor qu’il utilisera un objet de modèle pouvant contenir plusieurs objets de genre.</span><span class="sxs-lookup"><span data-stu-id="2baa6-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="2baa6-208">Nous utilisons une&gt; de genre IEnumerable&lt;plutôt qu’une liste&lt;&gt; de genre dans la mesure où elle est plus générique, ce qui nous permet de changer le type de modèle plus tard en tout type d’objet qui prend en charge l’interface IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="2baa6-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="2baa6-209">Ensuite, nous allons parcourir les objets de genre dans le modèle, comme indiqué dans le code de vue terminé ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="2baa6-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="2baa6-210">Notez que nous disposons d’une prise en charge complète d’IntelliSense, car nous entrons ce code, de sorte que lorsque vous tapez «@Model».</span><span class="sxs-lookup"><span data-stu-id="2baa6-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="2baa6-211">Nous voyons toutes les méthodes et les propriétés prises en charge par un IEnumerable de type genre.</span><span class="sxs-lookup"><span data-stu-id="2baa6-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="2baa6-212">Dans notre boucle « foreach », Visual Web Developer sait que chaque élément est de type genre. nous voyons donc IntelliSense pour chaque type de genre.</span><span class="sxs-lookup"><span data-stu-id="2baa6-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="2baa6-213">Ensuite, la fonctionnalité de génération de modèles automatique a examiné l’objet de genre et déterminé que chaque aura une propriété Name, de sorte qu’il effectue une boucle et l’écrit. Elle génère également des liens de modification, de détails et de suppression pour chaque élément individuel.</span><span class="sxs-lookup"><span data-stu-id="2baa6-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="2baa6-214">Nous allons tirer parti de cela plus tard dans notre responsable du magasin, mais pour l’instant, nous aimerions avoir une liste simple à la place.</span><span class="sxs-lookup"><span data-stu-id="2baa6-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="2baa6-215">Lorsque nous exécutons l’application et que vous accédez à/Store, nous voyons que le nombre et la liste de genres s’affichent.</span><span class="sxs-lookup"><span data-stu-id="2baa6-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="2baa6-216">Ajout de liens entre les pages</span><span class="sxs-lookup"><span data-stu-id="2baa6-216">Adding Links between pages</span></span>

<span data-ttu-id="2baa6-217">Notre URL/Store qui répertorie les genres répertorie actuellement les noms de genres simplement en texte brut.</span><span class="sxs-lookup"><span data-stu-id="2baa6-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="2baa6-218">Nous allons modifier cela de sorte qu’au lieu de texte brut, nous ayons les noms de genres liés à l’URL/Store/Browse appropriée, afin que le fait de cliquer sur un genre musical comme « Disco » accède à l’URL/Store/Browse ? genre = Disco.</span><span class="sxs-lookup"><span data-stu-id="2baa6-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="2baa6-219">Nous pourrions mettre à jour notre modèle de vue \Views\Store\Index.cshtml pour générer ces liens à l’aide d’un code comme ci-dessous **(ne tapez pas ceci dans, nous allons l’améliorer)** :</span><span class="sxs-lookup"><span data-stu-id="2baa6-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="2baa6-220">Cela fonctionne, mais cela peut entraîner des problèmes ultérieurement, car il s’appuie sur une chaîne codée en dur.</span><span class="sxs-lookup"><span data-stu-id="2baa6-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="2baa6-221">Par exemple, si nous voulions renommer le contrôleur, nous aurions besoin de parcourir notre code pour rechercher les liens qui doivent être mis à jour.</span><span class="sxs-lookup"><span data-stu-id="2baa6-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="2baa6-222">Une autre approche que nous pouvons utiliser est de tirer parti d’une méthode d’assistance HTML.</span><span class="sxs-lookup"><span data-stu-id="2baa6-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="2baa6-223">ASP.NET MVC comprend des méthodes d’assistance HTML qui sont disponibles à partir de notre modèle de vue pour effectuer diverses tâches courantes, comme ceci.</span><span class="sxs-lookup"><span data-stu-id="2baa6-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="2baa6-224">La méthode d’assistance HTML. ActionLink () est particulièrement utile et facilite la création de code HTML &lt;un&gt; des liens et prend en charge des détails ennuyeux, par exemple s’assurer que les chemins d’URL sont correctement encodés dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="2baa6-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="2baa6-225">Html. ActionLink () a plusieurs surcharges différentes pour permettre de spécifier autant d’informations que nécessaire pour vos liens.</span><span class="sxs-lookup"><span data-stu-id="2baa6-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="2baa6-226">Dans le cas le plus simple, vous fournissez uniquement le texte du lien et la méthode d’action à atteindre lorsque l’utilisateur clique sur le lien hypertexte sur le client.</span><span class="sxs-lookup"><span data-stu-id="2baa6-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="2baa6-227">Par exemple, nous pouvons créer un lien vers la méthode « /store/ » index () sur la page Store Details avec le texte du lien « Go to the store index » à l’aide de l’appel suivant :</span><span class="sxs-lookup"><span data-stu-id="2baa6-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="2baa6-228">*Remarque : dans ce cas, nous n’avons pas besoin de spécifier le nom du contrôleur, car nous créons simplement une liaison à une autre action dans le même contrôleur que celui qui rend l’affichage actuel.*</span><span class="sxs-lookup"><span data-stu-id="2baa6-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="2baa6-229">Les liens vers la page de navigation doivent cependant passer un paramètre. nous allons donc utiliser une autre surcharge de la méthode html. ActionLink qui accepte trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="2baa6-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="2baa6-230">Texte du lien qui affichera le nom du genre</span><span class="sxs-lookup"><span data-stu-id="2baa6-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="2baa6-231">Nom de l’action du contrôleur (Parcourir)</span><span class="sxs-lookup"><span data-stu-id="2baa6-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="2baa6-232">Router les valeurs des paramètres, en spécifiant à la fois le nom (genre) et la valeur (genre Name)</span><span class="sxs-lookup"><span data-stu-id="2baa6-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="2baa6-233">Pour ce faire, voici comment nous allons écrire ces liens dans la vue store index :</span><span class="sxs-lookup"><span data-stu-id="2baa6-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="2baa6-234">Maintenant, lorsque nous exécutons à nouveau notre projet et que vous accédez à l’URL/Store/, une liste de genres s’affiche.</span><span class="sxs-lookup"><span data-stu-id="2baa6-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="2baa6-235">Chaque genre est un lien hypertexte. quand vous cliquez dessus, nous nous remercions de notre URL/Store/Browse ? genre = *[genre]* .</span><span class="sxs-lookup"><span data-stu-id="2baa6-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="2baa6-236">Le code HTML de la liste de genres ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="2baa6-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> <span data-ttu-id="2baa6-237">[Précédent](mvc-music-store-part-2.md)
> [Suivant](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="2baa6-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
