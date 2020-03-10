---
uid: single-page-application/overview/templates/emberjs-template
title: Modèle EmberJS | Microsoft Docs
author: xqiu
description: Modèle EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578504"
---
# <a name="emberjs-template"></a><span data-ttu-id="bde30-103">Modèle EmberJS</span><span class="sxs-lookup"><span data-stu-id="bde30-103">EmberJS template</span></span>

<span data-ttu-id="bde30-104">par [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="bde30-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="bde30-105">Le modèle MVC EmberJS est écrit par Nathan Totten, Thiago Santos et Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="bde30-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="bde30-106">Télécharger le modèle MVC EmberJS</span><span class="sxs-lookup"><span data-stu-id="bde30-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)

<span data-ttu-id="bde30-107">Le modèle SPA EmberJS est conçu pour vous aider à créer rapidement des applications Web interactives côté client à l’aide de EmberJS.</span><span class="sxs-lookup"><span data-stu-id="bde30-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="bde30-108">« Une application à page unique » (SPA) est le terme général pour une application Web qui charge une seule page HTML, puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages.</span><span class="sxs-lookup"><span data-stu-id="bde30-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="bde30-109">Après le chargement initial de la page, le SPA discute avec le serveur via des demandes AJAX.</span><span class="sxs-lookup"><span data-stu-id="bde30-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="bde30-110">AJAX n’est pas nouveau, mais aujourd’hui, il existe des infrastructures JavaScript qui facilitent la création et la maintenance d’une application SPA sophistiquée de grande taille.</span><span class="sxs-lookup"><span data-stu-id="bde30-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="bde30-111">En outre, HTML 5 et CSS3 facilitent la création d’interfaces utilisateur riches.</span><span class="sxs-lookup"><span data-stu-id="bde30-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="bde30-112">Le modèle SPA EmberJS utilise la bibliothèque JavaScript [Ember](http://emberjs.com/) pour gérer les mises à jour de page à partir des demandes Ajax.</span><span class="sxs-lookup"><span data-stu-id="bde30-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="bde30-113">Ember. js utilise la liaison de données pour synchroniser la page avec les données les plus récentes.</span><span class="sxs-lookup"><span data-stu-id="bde30-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="bde30-114">De cette façon, vous n’êtes pas obligé d’écrire le code qui parcourt les données JSON et met à jour le DOM.</span><span class="sxs-lookup"><span data-stu-id="bde30-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="bde30-115">Au lieu de cela, vous placez des attributs déclaratifs dans le code HTML qui indiquent à Ember. js comment présenter les données.</span><span class="sxs-lookup"><span data-stu-id="bde30-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="bde30-116">Côté serveur, le modèle EmberJS est presque identique au [modèle KNOCKOUTJS Spa](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="bde30-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="bde30-117">Il utilise ASP.NET MVC pour servir des documents HTML et API Web ASP.NET pour gérer les requêtes AJAX à partir du client.</span><span class="sxs-lookup"><span data-stu-id="bde30-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="bde30-118">Pour plus d’informations sur ces aspects du modèle, reportez-vous à la documentation du [modèle KnockoutJS](../introduction/knockoutjs-template.md) .</span><span class="sxs-lookup"><span data-stu-id="bde30-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="bde30-119">Cette rubrique se concentre sur les différences entre le modèle Knockout et le modèle EmberJS.</span><span class="sxs-lookup"><span data-stu-id="bde30-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="bde30-120">Créer un projet de modèle SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="bde30-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="bde30-121">Téléchargez et installez le modèle en cliquant sur le bouton de téléchargement ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="bde30-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="bde30-122">Vous devrez peut-être redémarrer Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bde30-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="bde30-123">Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  .</span><span class="sxs-lookup"><span data-stu-id="bde30-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="bde30-124">Sous **visuel C#** , sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="bde30-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="bde30-125">Dans la liste des modèles de projet, sélectionnez **application Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="bde30-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="bde30-126">Nommez le projet, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="bde30-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="bde30-127">Dans l’Assistant **nouveau projet** , sélectionnez le **projet Ember. js Spa**.</span><span class="sxs-lookup"><span data-stu-id="bde30-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="bde30-128">Vue d’ensemble du modèle EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="bde30-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="bde30-129">Le modèle EmberJS utilise une combinaison de jQuery, Ember. js, guidon. js pour créer une interface utilisateur lisse et interactive.</span><span class="sxs-lookup"><span data-stu-id="bde30-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="bde30-130">Ember. js est une bibliothèque JavaScript qui utilise un modèle MVC côté client.</span><span class="sxs-lookup"><span data-stu-id="bde30-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="bde30-131">Un *modèle*, écrit dans le langage de création de guidon, décrit l’interface utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="bde30-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="bde30-132">En mode mise en sortie, le [compilateur guidon](https://github.com/Myslik/csharp-ember-handlebars) est utilisé pour regrouper et compiler le modèle de guidon.</span><span class="sxs-lookup"><span data-stu-id="bde30-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="bde30-133">Un *modèle* stocke les données d’application qu’il obtient à partir du serveur (listes TODO et éléments TODO).</span><span class="sxs-lookup"><span data-stu-id="bde30-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="bde30-134">Un *contrôleur* stocke l’état de l’application.</span><span class="sxs-lookup"><span data-stu-id="bde30-134">A *controller* stores application state.</span></span> <span data-ttu-id="bde30-135">Les contrôleurs présentent souvent des données de modèle aux modèles correspondants.</span><span class="sxs-lookup"><span data-stu-id="bde30-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="bde30-136">Une *vue* traduit les événements primitifs de l’application et les transmet au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="bde30-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="bde30-137">Un *routeur* gère l’état de l’application, en conservant la synchronisation des URL et des modèles.</span><span class="sxs-lookup"><span data-stu-id="bde30-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="bde30-138">En outre, la bibliothèque de données Ember peut être utilisée pour synchroniser des objets JSON (obtenus à partir du serveur via une API RESTful) et les modèles clients.</span><span class="sxs-lookup"><span data-stu-id="bde30-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="bde30-139">Le modèle SPA EmberJS organise les scripts en huit couches :</span><span class="sxs-lookup"><span data-stu-id="bde30-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="bde30-140">WebAPI\_adapter. js, WebAPI\_serializer. js : étend la bibliothèque de données Ember pour qu’elle fonctionne avec API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bde30-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="bde30-141">Scripts/Helper. js : définit les nouvelles applications auxiliaires de GUID de Ember.</span><span class="sxs-lookup"><span data-stu-id="bde30-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="bde30-142">Scripts/App. js : crée l’application et configure l’adaptateur et le sérialiseur.</span><span class="sxs-lookup"><span data-stu-id="bde30-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="bde30-143">Scripts/App/Models/\*. js : définit les modèles.</span><span class="sxs-lookup"><span data-stu-id="bde30-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="bde30-144">Scripts/app/views/\*. js : définit les vues.</span><span class="sxs-lookup"><span data-stu-id="bde30-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="bde30-145">Scripts/App/Controllers/\*. js : définit les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="bde30-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="bde30-146">Scripts/application/itinéraires, scripts/App/Router. js : définit les itinéraires.</span><span class="sxs-lookup"><span data-stu-id="bde30-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="bde30-147">Templates/\*. BH : définit les modèles de guidon.</span><span class="sxs-lookup"><span data-stu-id="bde30-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="bde30-148">Examinons quelques-uns de ces scripts plus en détail.</span><span class="sxs-lookup"><span data-stu-id="bde30-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="bde30-149">Modèles</span><span class="sxs-lookup"><span data-stu-id="bde30-149">Models</span></span>

<span data-ttu-id="bde30-150">Les modèles sont définis dans le dossier scripts/App/Models.</span><span class="sxs-lookup"><span data-stu-id="bde30-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="bde30-151">Il existe deux fichiers de modèle : todoItem. js et todoList. js.</span><span class="sxs-lookup"><span data-stu-id="bde30-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="bde30-152">**TODO. Model. js** définit les modèles côté client (navigateur) pour les listes de tâches.</span><span class="sxs-lookup"><span data-stu-id="bde30-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="bde30-153">Il existe deux classes de modèle : todoItem et todoList.</span><span class="sxs-lookup"><span data-stu-id="bde30-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="bde30-154">Dans Ember, les modèles sont des sous-classes de DS. Modélisation.</span><span class="sxs-lookup"><span data-stu-id="bde30-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="bde30-155">Un modèle peut avoir des propriétés avec des attributs :</span><span class="sxs-lookup"><span data-stu-id="bde30-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="bde30-156">Les modèles peuvent définir des relations avec d’autres modèles :</span><span class="sxs-lookup"><span data-stu-id="bde30-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="bde30-157">Les modèles peuvent avoir des propriétés calculées qui sont liées à d’autres propriétés :</span><span class="sxs-lookup"><span data-stu-id="bde30-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="bde30-158">Les modèles peuvent avoir des fonctions observateur, qui sont appelées lorsqu’une propriété observée change :</span><span class="sxs-lookup"><span data-stu-id="bde30-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="bde30-159">Affichages</span><span class="sxs-lookup"><span data-stu-id="bde30-159">Views</span></span>

<span data-ttu-id="bde30-160">Les affichages sont définis dans le dossier scripts/app/views.</span><span class="sxs-lookup"><span data-stu-id="bde30-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="bde30-161">Une vue traduit les événements à partir de l’interface utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="bde30-161">A view translates events from the application UI.</span></span> <span data-ttu-id="bde30-162">Un gestionnaire d’événements peut rappeler des fonctions de contrôleur ou simplement appeler directement le contexte de données.</span><span class="sxs-lookup"><span data-stu-id="bde30-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="bde30-163">Par exemple, le code suivant provient de views/TodoItemEditView. js.</span><span class="sxs-lookup"><span data-stu-id="bde30-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="bde30-164">Il définit la gestion des événements pour un champ de texte d’entrée.</span><span class="sxs-lookup"><span data-stu-id="bde30-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="bde30-165">Contrôleur</span><span class="sxs-lookup"><span data-stu-id="bde30-165">Controller</span></span>

<span data-ttu-id="bde30-166">Les contrôleurs sont définis dans le dossier scripts/App/Controllers.</span><span class="sxs-lookup"><span data-stu-id="bde30-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="bde30-167">Pour représenter un modèle unique, étendez `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="bde30-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="bde30-168">Un contrôleur peut également représenter une collection de modèles en étendant `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="bde30-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="bde30-169">Par exemple, TodoListController représente un tableau d’objets `todoList`.</span><span class="sxs-lookup"><span data-stu-id="bde30-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="bde30-170">Le contrôleur trie par ID todoList, dans l’ordre décroissant :</span><span class="sxs-lookup"><span data-stu-id="bde30-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="bde30-171">Le contrôleur définit une fonction nommée `addTodoList`, qui crée un nouveau todoList et l’ajoute au tableau.</span><span class="sxs-lookup"><span data-stu-id="bde30-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="bde30-172">Pour voir comment cette fonction est appelée, ouvrez le fichier de modèle nommé todoListTemplate. html dans le dossier modèles.</span><span class="sxs-lookup"><span data-stu-id="bde30-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="bde30-173">Le code de modèle suivant lie un bouton à la fonction `addTodoList` :</span><span class="sxs-lookup"><span data-stu-id="bde30-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="bde30-174">Le contrôleur contient également une propriété `error`, qui contient un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="bde30-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="bde30-175">Voici le code du modèle permettant d’afficher le message d’erreur (également dans todoListTemplate. html) :</span><span class="sxs-lookup"><span data-stu-id="bde30-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="bde30-176">Routes</span><span class="sxs-lookup"><span data-stu-id="bde30-176">Routes</span></span>

<span data-ttu-id="bde30-177">Router. js définit les itinéraires et le modèle par défaut à afficher, définit l’état de l’application et met en correspondance les URL avec les itinéraires :</span><span class="sxs-lookup"><span data-stu-id="bde30-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="bde30-178">TodoListRoute. js charge des données pour TodoListRoute en remplaçant la fonction setupController :</span><span class="sxs-lookup"><span data-stu-id="bde30-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="bde30-179">Ember utilise des conventions d’affectation de noms pour faire correspondre les URL, les noms d’itinéraires, les contrôleurs et les modèles.</span><span class="sxs-lookup"><span data-stu-id="bde30-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="bde30-180">Pour plus d’informations, consultez [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) dans la documentation EmberJS.</span><span class="sxs-lookup"><span data-stu-id="bde30-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="bde30-181">Modèles</span><span class="sxs-lookup"><span data-stu-id="bde30-181">Templates</span></span>

<span data-ttu-id="bde30-182">Le dossier modèles contient quatre modèles :</span><span class="sxs-lookup"><span data-stu-id="bde30-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="bde30-183">application. BH : modèle par défaut qui est rendu lorsque l’application est démarrée.</span><span class="sxs-lookup"><span data-stu-id="bde30-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="bde30-184">about. BH : modèle pour l’itinéraire « /about ».</span><span class="sxs-lookup"><span data-stu-id="bde30-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="bde30-185">index. BH : modèle pour l’itinéraire racine « / ».</span><span class="sxs-lookup"><span data-stu-id="bde30-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="bde30-186">todoList. BH : modèle pour l’itinéraire « /todo ».</span><span class="sxs-lookup"><span data-stu-id="bde30-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="bde30-187">\_barre de navigation. BH : le modèle définit le menu de navigation.</span><span class="sxs-lookup"><span data-stu-id="bde30-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="bde30-188">Le modèle d’application agit comme une page maître.</span><span class="sxs-lookup"><span data-stu-id="bde30-188">The application template acts like a master page.</span></span> <span data-ttu-id="bde30-189">Elle contient un en-tête, un pied de page et un « {{Outlet}} » pour insérer d’autres modèles dans en fonction de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bde30-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="bde30-190">Pour plus d’informations sur les modèles d’application dans Ember, consultez [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="bde30-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="bde30-191">Le modèle « /todoList » contient deux expressions de boucle.</span><span class="sxs-lookup"><span data-stu-id="bde30-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="bde30-192">La boucle extérieure est `{{#each controller}}`, et la boucle interne est `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="bde30-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="bde30-193">Le code suivant illustre une vue intégrée `Ember.Checkbox`, une `App.TodoItemEditView`personnalisée et un lien avec une action `deleteTodo`.</span><span class="sxs-lookup"><span data-stu-id="bde30-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="bde30-194">La classe `HtmlHelperExtensions`, définie dans Controllers/HtmlHelperExtensions. cs, définit une fonction d’assistance pour mettre en cache et insérer des fichiers modèles lorsque **Debug** a la valeur **true** dans le fichier Web. config.</span><span class="sxs-lookup"><span data-stu-id="bde30-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExtensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="bde30-195">Cette fonction est appelée à partir du fichier de vue MVC ASP.NET défini dans views/domotique/App. cshtml :</span><span class="sxs-lookup"><span data-stu-id="bde30-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="bde30-196">Appelée sans argument, la fonction affiche tous les fichiers de modèle dans le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="bde30-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="bde30-197">Vous pouvez également spécifier un sous-dossier ou un fichier de modèle spécifique.</span><span class="sxs-lookup"><span data-stu-id="bde30-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="bde30-198">Lorsque **Debug** a la **valeur false** dans Web. config, l’application comprend l’élément Bundle « ~/bundles/Templates ».</span><span class="sxs-lookup"><span data-stu-id="bde30-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="bde30-199">Cet élément de regroupement est ajouté dans BundleConfig.cs, à l’aide de la bibliothèque du compilateur guidon :</span><span class="sxs-lookup"><span data-stu-id="bde30-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
