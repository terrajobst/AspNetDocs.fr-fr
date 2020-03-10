---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Vue d’ensemble des vuesC#MVC ASP.net () | Microsoft Docs
author: StephenWalther
description: Qu’est-ce qu’une vue MVC ASP.NET et comment elle diffère-t-elle d’une page HTML ? Dans ce didacticiel, Stephen Walther vous présente les vues et montre comment vous pouvez...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: b3f44aa9654a2a718381eaf9c856ca3e15ed1e27
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600316"
---
# <a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="31899-104">Vue d’ensemble des vues ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="31899-104">ASP.NET MVC Views Overview (C#)</span></span>

<span data-ttu-id="31899-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="31899-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="31899-106">Qu’est-ce qu’une vue MVC ASP.NET et comment elle diffère-t-elle d’une page HTML ?</span><span class="sxs-lookup"><span data-stu-id="31899-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="31899-107">Dans ce didacticiel, Stephen Walther vous présente les vues et montre comment tirer parti des données d’affichage et des applications auxiliaires HTML dans une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>

<span data-ttu-id="31899-108">L’objectif de ce didacticiel est de vous fournir une brève introduction aux vues ASP.NET MVC, aux données d’affichage et aux applications d’assistance HTML.</span><span class="sxs-lookup"><span data-stu-id="31899-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="31899-109">À la fin de ce didacticiel, vous devez comprendre comment créer des vues, passer des données d’un contrôleur à une vue et utiliser des applications auxiliaires HTML pour générer du contenu dans une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="31899-110">Présentation des vues</span><span class="sxs-lookup"><span data-stu-id="31899-110">Understanding Views</span></span>

<span data-ttu-id="31899-111">Pour les pages ASP.NET ou Active Server, ASP.NET MVC n’inclut rien qui correspond directement à une page.</span><span class="sxs-lookup"><span data-stu-id="31899-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="31899-112">Dans une application MVC ASP.NET, il n’existe pas de page sur le disque qui correspond au chemin d’accès dans l’URL que vous tapez dans la barre d’adresses de votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="31899-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="31899-113">L’élément le plus proche d’une page dans une application ASP.NET MVC est appelé *vue*.</span><span class="sxs-lookup"><span data-stu-id="31899-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="31899-114">Dans une application MVC ASP.NET, les demandes de navigateur entrantes sont mappées à des actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="31899-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="31899-115">Une action de contrôleur peut retourner une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-115">A controller action might return a view.</span></span> <span data-ttu-id="31899-116">Toutefois, une action de contrôleur peut exécuter un autre type d’action, comme la redirection vers une autre action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="31899-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="31899-117">La liste 1 contient un contrôleur simple nommé HomeController.</span><span class="sxs-lookup"><span data-stu-id="31899-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="31899-118">Le HomeController expose deux actions de contrôleur nommées index () et Details ().</span><span class="sxs-lookup"><span data-stu-id="31899-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="31899-119">**Liste 1-HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="31899-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="31899-120">Vous pouvez appeler la première action, l’action index (), en tapant l’URL suivante dans la barre d’adresse de votre navigateur :</span><span class="sxs-lookup"><span data-stu-id="31899-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="31899-121">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="31899-121">/Home/Index</span></span>

<span data-ttu-id="31899-122">Vous pouvez appeler la deuxième action, l’action Details (), en tapant cette adresse dans votre navigateur :</span><span class="sxs-lookup"><span data-stu-id="31899-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="31899-123">/Home/Details</span><span class="sxs-lookup"><span data-stu-id="31899-123">/Home/Details</span></span>

<span data-ttu-id="31899-124">L’action index () renvoie une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-124">The Index() action returns a view.</span></span> <span data-ttu-id="31899-125">La plupart des actions que vous créez retournent des vues.</span><span class="sxs-lookup"><span data-stu-id="31899-125">Most actions that you create will return views.</span></span> <span data-ttu-id="31899-126">Toutefois, une action peut retourner d’autres types de résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="31899-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="31899-127">Par exemple, l’action Details () renvoie un RedirectToActionResult qui redirige la requête entrante vers l’action index ().</span><span class="sxs-lookup"><span data-stu-id="31899-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="31899-128">L’action index () contient la seule ligne de code suivante :</span><span class="sxs-lookup"><span data-stu-id="31899-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="31899-129">View ();</span><span class="sxs-lookup"><span data-stu-id="31899-129">View();</span></span>

<span data-ttu-id="31899-130">Cette ligne de code retourne une vue qui doit se trouver sur le chemin d’accès suivant sur votre serveur Web :</span><span class="sxs-lookup"><span data-stu-id="31899-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="31899-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="31899-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="31899-132">Le chemin d’accès à la vue est déduit à partir du nom du contrôleur et du nom de l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="31899-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="31899-133">Si vous préférez, vous pouvez être explicite sur la vue.</span><span class="sxs-lookup"><span data-stu-id="31899-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="31899-134">La ligne de code suivante retourne une vue nommée Fred :</span><span class="sxs-lookup"><span data-stu-id="31899-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="31899-135">Vue (Fred);</span><span class="sxs-lookup"><span data-stu-id="31899-135">View( Fred );</span></span>

<span data-ttu-id="31899-136">Lorsque cette ligne de code est exécutée, une vue est retournée à partir du chemin d’accès suivant :</span><span class="sxs-lookup"><span data-stu-id="31899-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="31899-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="31899-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="31899-138">Si vous envisagez de créer des tests unitaires pour votre application ASP.NET MVC, il est judicieux d’être explicite en ce qui concerne les noms de vues.</span><span class="sxs-lookup"><span data-stu-id="31899-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="31899-139">De cette façon, vous pouvez créer un test unitaire pour vérifier que la vue attendue a été retournée par une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="31899-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>

## <a name="adding-content-to-a-view"></a><span data-ttu-id="31899-140">Ajout de contenu à une vue</span><span class="sxs-lookup"><span data-stu-id="31899-140">Adding Content to a View</span></span>

<span data-ttu-id="31899-141">Une vue est un document HTML standard (X) qui peut contenir des scripts.</span><span class="sxs-lookup"><span data-stu-id="31899-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="31899-142">Vous utilisez des scripts pour ajouter du contenu dynamique à une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="31899-143">Par exemple, la vue dans la liste 2 affiche la date et l’heure actuelles.</span><span class="sxs-lookup"><span data-stu-id="31899-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="31899-144">**Liste 2-\Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="31899-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="31899-145">Notez que le corps de la page HTML dans le Listing 2 contient le script suivant :</span><span class="sxs-lookup"><span data-stu-id="31899-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="31899-146">&lt;% Response. Write (DateTime. Now);%&gt;</span><span class="sxs-lookup"><span data-stu-id="31899-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="31899-147">Vous utilisez les délimiteurs de script &lt;% et%&gt; pour marquer le début et la fin d’un script.</span><span class="sxs-lookup"><span data-stu-id="31899-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="31899-148">Ce script est écrit dans C#.</span><span class="sxs-lookup"><span data-stu-id="31899-148">This script is written in C#.</span></span> <span data-ttu-id="31899-149">Elle affiche la date et l’heure actuelles en appelant la méthode Response. Write () pour afficher le contenu dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="31899-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="31899-150">Les délimiteurs de script &lt;% et%&gt; peuvent être utilisés pour exécuter une ou plusieurs instructions.</span><span class="sxs-lookup"><span data-stu-id="31899-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="31899-151">Étant donné que vous appelez Response. Write (), Microsoft vous fournit souvent un raccourci pour appeler la méthode Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="31899-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="31899-152">La vue de la liste 3 utilise les délimiteurs &lt;% = et%&gt; comme raccourci pour appeler Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="31899-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="31899-153">**Liste 3-Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="31899-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="31899-154">Vous pouvez utiliser n’importe quel langage .NET pour générer du contenu dynamique dans une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="31899-155">Normalement, vous utiliserez Visual Basic .NET ou C# pour écrire vos contrôleurs et vos vues.</span><span class="sxs-lookup"><span data-stu-id="31899-155">Normally, you'll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="31899-156">Utilisation des applications auxiliaires HTML pour générer le contenu de la vue</span><span class="sxs-lookup"><span data-stu-id="31899-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="31899-157">Pour faciliter l’ajout de contenu à une vue, vous pouvez tirer parti d’un *programme d’assistance HTML*.</span><span class="sxs-lookup"><span data-stu-id="31899-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="31899-158">Une application auxiliaire HTML, en général, est une méthode qui génère une chaîne.</span><span class="sxs-lookup"><span data-stu-id="31899-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="31899-159">Vous pouvez utiliser des applications auxiliaires HTML pour générer des éléments HTML standard, tels que des zones de texte, des liens, des listes déroulantes et des zones de liste.</span><span class="sxs-lookup"><span data-stu-id="31899-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="31899-160">Par exemple, la vue de la liste 4 tire parti de trois applications auxiliaires HTML : BeginForm (), zone de texte () et mot de passe () pour générer un formulaire de connexion (voir la figure 1).</span><span class="sxs-lookup"><span data-stu-id="31899-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="31899-161">**Liste 4--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="31899-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]

<span data-ttu-id="31899-162">[![la boîte de dialogue Nouveau projet](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="31899-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="31899-163">**Figure 01**: formulaire de connexion standard ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-views-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="31899-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>

<span data-ttu-id="31899-164">Toutes les méthodes d’assistance HTML sont appelées sur la propriété HTML de la vue.</span><span class="sxs-lookup"><span data-stu-id="31899-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="31899-165">Par exemple, vous affichez une zone de texte en appelant la méthode html. TextBox ().</span><span class="sxs-lookup"><span data-stu-id="31899-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="31899-166">Notez que vous utilisez les délimiteurs de script &lt;% = et%&gt; lors de l’appel des applications d’assistance HTML. TextBox () et html. Password ().</span><span class="sxs-lookup"><span data-stu-id="31899-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="31899-167">Ces applications auxiliaires retournent simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="31899-167">These helpers simply return a string.</span></span> <span data-ttu-id="31899-168">Vous devez appeler Response. Write () pour afficher la chaîne dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="31899-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="31899-169">L’utilisation des méthodes d’assistance HTML est facultative.</span><span class="sxs-lookup"><span data-stu-id="31899-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="31899-170">Ils facilitent votre vie en réduisant la quantité de code HTML et de script que vous devez écrire.</span><span class="sxs-lookup"><span data-stu-id="31899-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="31899-171">La vue dans la liste 5 affiche exactement la même forme que la vue dans la liste 4 sans utiliser les applications auxiliaires HTML.</span><span class="sxs-lookup"><span data-stu-id="31899-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="31899-172">**Liste 5--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="31899-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="31899-173">Vous avez également la possibilité de créer vos propres applications auxiliaires HTML.</span><span class="sxs-lookup"><span data-stu-id="31899-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="31899-174">Par exemple, vous pouvez créer une méthode d’assistance GridView () qui affiche automatiquement un ensemble d’enregistrements de base de données dans un tableau HTML.</span><span class="sxs-lookup"><span data-stu-id="31899-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="31899-175">Nous explorons cette rubrique dans le didacticiel **création d’applications auxiliaires html personnalisées**.</span><span class="sxs-lookup"><span data-stu-id="31899-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="31899-176">Utilisation de l’affichage des données pour passer des données à une vue</span><span class="sxs-lookup"><span data-stu-id="31899-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="31899-177">Vous utilisez les données d’affichage pour passer des données d’un contrôleur à une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="31899-178">Considérez les données d’affichage comme un package que vous envoyez par courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="31899-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="31899-179">Toutes les données transmises d’un contrôleur à une vue doivent être envoyées à l’aide de ce package.</span><span class="sxs-lookup"><span data-stu-id="31899-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="31899-180">Par exemple, le contrôleur de la liste 6 ajoute un message pour afficher les données.</span><span class="sxs-lookup"><span data-stu-id="31899-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="31899-181">**Liste 6-ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="31899-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="31899-182">La propriété du contrôleur ViewData représente une collection de paires nom/valeur.</span><span class="sxs-lookup"><span data-stu-id="31899-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="31899-183">Dans la liste 6, la méthode index () ajoute un élément à la collection de données d’affichage nommée message avec la valeur Hello World !.</span><span class="sxs-lookup"><span data-stu-id="31899-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="31899-184">Lorsque la vue est retournée par la méthode index (), les données d’affichage sont automatiquement transmises à la vue.</span><span class="sxs-lookup"><span data-stu-id="31899-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="31899-185">La vue dans la liste 7 récupère le message à partir des données d’affichage et restitue le message dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="31899-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="31899-186">**Liste 7--\Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="31899-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="31899-187">Notez que la vue tire parti de la méthode d’assistance HTML HTML. Encode () lors du rendu du message.</span><span class="sxs-lookup"><span data-stu-id="31899-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="31899-188">Le programme d’assistance HTML HTML. Encode () code les caractères spéciaux tels que les &lt; et les &gt; en caractères qui peuvent être affichés en toute sécurité dans une page Web.</span><span class="sxs-lookup"><span data-stu-id="31899-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="31899-189">Chaque fois que vous affichez le contenu qu’un utilisateur envoie à un site Web, vous devez encoder le contenu pour empêcher les attaques par injection de code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="31899-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="31899-190">(Étant donné que nous avons créé le message nous-mêmes dans le ProductController, nous n’avons pas vraiment besoin d’encoder le message.</span><span class="sxs-lookup"><span data-stu-id="31899-190">(Because we created the message ourselves in the ProductController, we don't really need to encode the message.</span></span> <span data-ttu-id="31899-191">Toutefois, il est judicieux de toujours appeler la méthode html. Encode () lors de l’affichage de contenu récupéré à partir de données d’affichage dans une vue.)</span><span class="sxs-lookup"><span data-stu-id="31899-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="31899-192">Dans la liste 7, nous avons tiré parti des données d’affichage pour passer un message de chaîne simple d’un contrôleur à une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="31899-193">Vous pouvez également utiliser afficher les données pour transmettre d’autres types de données, tels qu’une collection d’enregistrements de base de données, d’un contrôleur à une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="31899-194">Par exemple, si vous souhaitez afficher le contenu de la table de base de données Products dans une vue, vous devez passer la collection d’enregistrements de base de données dans les données d’affichage.</span><span class="sxs-lookup"><span data-stu-id="31899-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="31899-195">Vous avez également la possibilité de passer des données d’affichage fortement typées d’un contrôleur à une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="31899-196">Nous explorons cette rubrique dans le didacticiel **Présentation des vues et des données d’affichage fortement typées**.</span><span class="sxs-lookup"><span data-stu-id="31899-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="31899-197">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="31899-197">Summary</span></span>

<span data-ttu-id="31899-198">Ce didacticiel vous a présenté une brève introduction aux vues ASP.NET MVC, à l’affichage des données et aux applications auxiliaires HTML.</span><span class="sxs-lookup"><span data-stu-id="31899-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="31899-199">Dans la première section, vous avez appris à ajouter de nouvelles vues à votre projet.</span><span class="sxs-lookup"><span data-stu-id="31899-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="31899-200">Vous avez appris que vous devez ajouter une vue au dossier approprié afin de l’appeler à partir d’un contrôleur particulier.</span><span class="sxs-lookup"><span data-stu-id="31899-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="31899-201">Nous avons ensuite abordé le sujet des applications auxiliaires HTML.</span><span class="sxs-lookup"><span data-stu-id="31899-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="31899-202">Vous avez appris comment les applications auxiliaires HTML vous permettent de générer facilement du contenu HTML standard.</span><span class="sxs-lookup"><span data-stu-id="31899-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="31899-203">Enfin, vous avez appris à tirer parti des données d’affichage pour passer des données d’un contrôleur à une vue.</span><span class="sxs-lookup"><span data-stu-id="31899-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="31899-204">Next</span><span class="sxs-lookup"><span data-stu-id="31899-204">Next</span></span>](creating-custom-html-helpers-cs.md)
