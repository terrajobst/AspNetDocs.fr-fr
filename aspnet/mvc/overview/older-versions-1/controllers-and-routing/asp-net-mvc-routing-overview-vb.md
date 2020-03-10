---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Vue d’ensemble du routage ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes de navigateur aux actions de contrôleur.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ed043d76b89ce31945cf3423b0c5afca9383cc21
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601534"
---
# <a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="e685b-103">Vue d’ensemble du routage ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="e685b-103">ASP.NET MVC Routing Overview (VB)</span></span>

<span data-ttu-id="e685b-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e685b-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e685b-105">Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes de navigateur aux actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e685b-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>

<span data-ttu-id="e685b-106">Dans ce didacticiel, vous allez découvrir une fonctionnalité importante de chaque application ASP.NET MVC appelée *routage ASP.net*.</span><span class="sxs-lookup"><span data-stu-id="e685b-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="e685b-107">Le module de routage ASP.NET est chargé de mapper les demandes de navigateur entrantes à des actions de contrôleur MVC particulières.</span><span class="sxs-lookup"><span data-stu-id="e685b-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="e685b-108">À la fin de ce didacticiel, vous comprendrez comment la table de routage standard mappe les demandes aux actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e685b-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="e685b-109">Utilisation de la table de routage par défaut</span><span class="sxs-lookup"><span data-stu-id="e685b-109">Using the Default Route Table</span></span>

<span data-ttu-id="e685b-110">Lorsque vous créez une application MVC ASP.NET, l’application est déjà configurée pour utiliser le routage ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e685b-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="e685b-111">Le routage ASP.NET est configuré à deux emplacements.</span><span class="sxs-lookup"><span data-stu-id="e685b-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="e685b-112">Tout d’abord, le routage ASP.NET est activé dans le fichier de configuration Web de votre application (fichier Web. config).</span><span class="sxs-lookup"><span data-stu-id="e685b-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="e685b-113">Quatre sections du fichier de configuration sont pertinentes pour le routage : la section System. Web. httpModules, la section System. Web. httpHandlers, la section System. webserver. modules et la section System. webserver. Handlers.</span><span class="sxs-lookup"><span data-stu-id="e685b-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="e685b-114">Veillez à ne pas supprimer ces sections, car sans ces sections le routage ne fonctionnera plus.</span><span class="sxs-lookup"><span data-stu-id="e685b-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="e685b-115">Deuxièmement, et plus important encore, une table de routage est créée dans le fichier global. asax de l’application.</span><span class="sxs-lookup"><span data-stu-id="e685b-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="e685b-116">Le fichier global. asax est un fichier spécial qui contient des gestionnaires d’événements pour les événements de cycle de vie d’application ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e685b-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="e685b-117">La table de routage est créée lors de l’événement de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="e685b-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="e685b-118">Le fichier dans la liste 1 contient le fichier global. asax par défaut pour une application MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e685b-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="e685b-119">**Liste 1-global. asax. vb**</span><span class="sxs-lookup"><span data-stu-id="e685b-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="e685b-120">Lors du premier démarrage d’une application MVC, l’application\_méthode Start () est appelée.</span><span class="sxs-lookup"><span data-stu-id="e685b-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="e685b-121">Cette méthode appelle à son tour la méthode RegisterRoutes ().</span><span class="sxs-lookup"><span data-stu-id="e685b-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="e685b-122">La méthode RegisterRoutes () crée la table de routage.</span><span class="sxs-lookup"><span data-stu-id="e685b-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="e685b-123">La table de routage par défaut contient un seul itinéraire (nommé Default).</span><span class="sxs-lookup"><span data-stu-id="e685b-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="e685b-124">L’itinéraire par défaut mappe le premier segment d’une URL à un nom de contrôleur, le deuxième segment d’une URL à une action de contrôleur et le troisième segment à un paramètre nommé **ID**.</span><span class="sxs-lookup"><span data-stu-id="e685b-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="e685b-125">Imaginez que vous entrez l’URL suivante dans la barre d’adresse de votre navigateur Web :</span><span class="sxs-lookup"><span data-stu-id="e685b-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="e685b-126">/Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="e685b-126">/Home/Index/3</span></span>

<span data-ttu-id="e685b-127">L’itinéraire par défaut mappe cette URL aux paramètres suivants :</span><span class="sxs-lookup"><span data-stu-id="e685b-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="e685b-128">contrôleur = page d’hébergement</span><span class="sxs-lookup"><span data-stu-id="e685b-128">controller = Home</span></span>

- <span data-ttu-id="e685b-129">action = index</span><span class="sxs-lookup"><span data-stu-id="e685b-129">action = Index</span></span>

- <span data-ttu-id="e685b-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="e685b-130">id = 3</span></span>

<span data-ttu-id="e685b-131">Lorsque vous demandez l’URL/Home/Index/3, le code suivant est exécuté :</span><span class="sxs-lookup"><span data-stu-id="e685b-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="e685b-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="e685b-132">HomeController.Index(3)</span></span>

<span data-ttu-id="e685b-133">L’itinéraire par défaut comprend les valeurs par défaut pour les trois paramètres.</span><span class="sxs-lookup"><span data-stu-id="e685b-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="e685b-134">Si vous ne fournissez pas de contrôleur, le paramètre de contrôleur a par défaut la valeur **orig**.</span><span class="sxs-lookup"><span data-stu-id="e685b-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="e685b-135">Si vous ne fournissez pas d’action, le paramètre d’action est défini par défaut sur l' **index**de valeur.</span><span class="sxs-lookup"><span data-stu-id="e685b-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="e685b-136">Enfin, si vous ne fournissez pas d’ID, le paramètre ID est une chaîne vide par défaut.</span><span class="sxs-lookup"><span data-stu-id="e685b-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="e685b-137">Examinons quelques exemples de la façon dont l’itinéraire par défaut mappe les URL aux actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e685b-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="e685b-138">Imaginez que vous entrez l’URL suivante dans la barre d’adresse de votre navigateur :</span><span class="sxs-lookup"><span data-stu-id="e685b-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="e685b-139">/Home</span><span class="sxs-lookup"><span data-stu-id="e685b-139">/Home</span></span>

<span data-ttu-id="e685b-140">En raison des valeurs par défaut des paramètres d’itinéraire par défaut, l’entrée de cette URL entraîne l’appel de la méthode index () de la classe HomeController dans Listing 2.</span><span class="sxs-lookup"><span data-stu-id="e685b-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="e685b-141">**Liste 2-HomeController. vb**</span><span class="sxs-lookup"><span data-stu-id="e685b-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="e685b-142">Dans la liste 2, la classe HomeController contient une méthode nommée index () qui accepte un paramètre unique nommé ID. L’URL/Home provoque l’appel de la méthode index () avec la valeur Nothing comme valeur du paramètre ID.</span><span class="sxs-lookup"><span data-stu-id="e685b-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="e685b-143">En raison de la façon dont l’infrastructure MVC appelle les actions de contrôleur, l’URL/Home correspond également à la méthode index () de la classe HomeController dans le Listing 3.</span><span class="sxs-lookup"><span data-stu-id="e685b-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="e685b-144">**Liste 3-HomeController. vb (action d’index sans paramètre)**</span><span class="sxs-lookup"><span data-stu-id="e685b-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="e685b-145">La méthode index () de la liste 3 n’accepte aucun paramètre.</span><span class="sxs-lookup"><span data-stu-id="e685b-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="e685b-146">L’URL/Home entraînera l’appel de cette méthode index ().</span><span class="sxs-lookup"><span data-stu-id="e685b-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="e685b-147">L’URL/Home/Index/3 appelle également cette méthode (l’ID est ignoré).</span><span class="sxs-lookup"><span data-stu-id="e685b-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="e685b-148">L’URL/Home correspond également à la méthode index () de la classe HomeController de la liste 4.</span><span class="sxs-lookup"><span data-stu-id="e685b-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="e685b-149">**Liste 4-HomeController. vb (action d’index avec paramètre Nullable)**</span><span class="sxs-lookup"><span data-stu-id="e685b-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="e685b-150">Dans la liste 4, la méthode index () a un paramètre entier.</span><span class="sxs-lookup"><span data-stu-id="e685b-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="e685b-151">Étant donné que le paramètre est un paramètre Nullable (peut avoir la valeur Nothing), l’index () peut être appelé sans déclencher d’erreur.</span><span class="sxs-lookup"><span data-stu-id="e685b-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="e685b-152">Enfin, l’appel de la méthode index () dans la liste 5 avec l’URL/Home provoque une exception, car le paramètre ID *n’est pas* un paramètre Nullable.</span><span class="sxs-lookup"><span data-stu-id="e685b-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="e685b-153">Si vous tentez d’appeler la méthode index (), vous recevez l’erreur affichée à la figure 1.</span><span class="sxs-lookup"><span data-stu-id="e685b-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="e685b-154">**Liste 5-HomeController. vb (action d’index avec paramètre ID)**</span><span class="sxs-lookup"><span data-stu-id="e685b-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]

<span data-ttu-id="e685b-155">[![de l’appel d’une action de contrôleur qui attend une valeur de paramètre](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e685b-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="e685b-156">**Figure 01**: appel d’une action de contrôleur qui attend une valeur de paramètre ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e685b-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>

<span data-ttu-id="e685b-157">L’URL/Home/Index/3, en revanche, fonctionne parfaitement avec l’action du contrôleur d’index dans la liste 5.</span><span class="sxs-lookup"><span data-stu-id="e685b-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="e685b-158">La requête/Home/Index/3 provoque l’appel de la méthode index () avec un paramètre ID qui a la valeur 3.</span><span class="sxs-lookup"><span data-stu-id="e685b-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="e685b-159">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="e685b-159">Summary</span></span>

<span data-ttu-id="e685b-160">L’objectif de ce didacticiel était de vous fournir une brève présentation du routage ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e685b-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="e685b-161">Nous avons examiné la table de routage par défaut que vous avez obtenue avec une nouvelle application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e685b-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="e685b-162">Vous avez appris comment l’itinéraire par défaut mappe les URL aux actions du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e685b-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e685b-163">[Précédent](creating-an-action-cs.md)
> [Suivant](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e685b-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
