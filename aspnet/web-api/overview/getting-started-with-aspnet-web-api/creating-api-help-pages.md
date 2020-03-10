---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Création de pages d’aide pour API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Ce didacticiel contenant du code montre comment créer des pages d’aide pour API Web ASP.NET dans ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556874"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="0a248-103">Création de pages d’aide pour API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0a248-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="0a248-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0a248-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0a248-105">Ce didacticiel contenant du code montre comment créer des pages d’aide pour API Web ASP.NET dans ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="0a248-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="0a248-106">Lorsque vous créez une API Web, il est souvent utile de créer une page d’aide afin que d’autres développeurs sachent comment appeler votre API.</span><span class="sxs-lookup"><span data-stu-id="0a248-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="0a248-107">Vous pouvez créer l’ensemble de la documentation manuellement, mais il est préférable de générer automatiquement le plus possible.</span><span class="sxs-lookup"><span data-stu-id="0a248-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="0a248-108">Pour faciliter cette tâche, API Web ASP.NET fournit une bibliothèque pour la génération automatique des pages d’aide au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0a248-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="0a248-109">Création des pages d’aide de l’API</span><span class="sxs-lookup"><span data-stu-id="0a248-109">Creating API Help Pages</span></span>

<span data-ttu-id="0a248-110">Installez [ASP.net et Web Tools mise à jour 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="0a248-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="0a248-111">Cette mise à jour intègre les pages d’aide dans le modèle de projet d’API Web.</span><span class="sxs-lookup"><span data-stu-id="0a248-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="0a248-112">Ensuite, créez un nouveau projet ASP.NET MVC 4 et sélectionnez le modèle de projet d’API Web.</span><span class="sxs-lookup"><span data-stu-id="0a248-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="0a248-113">Le modèle de projet crée un exemple de contrôleur d’API nommé `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="0a248-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="0a248-114">Le modèle crée également les pages d’aide de l’API.</span><span class="sxs-lookup"><span data-stu-id="0a248-114">The template also creates the API help pages.</span></span> <span data-ttu-id="0a248-115">Tous les fichiers de code de la page d’aide sont placés dans le dossier zones du projet.</span><span class="sxs-lookup"><span data-stu-id="0a248-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="0a248-116">Lorsque vous exécutez l’application, la page d’hébergement contient un lien vers la page d’aide de l’API.</span><span class="sxs-lookup"><span data-stu-id="0a248-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="0a248-117">À partir de la page d’hébergement, le chemin d’accès relatif est/help.</span><span class="sxs-lookup"><span data-stu-id="0a248-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="0a248-118">Ce lien vous amène à une page de résumé des API.</span><span class="sxs-lookup"><span data-stu-id="0a248-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="0a248-119">La vue MVC pour cette page est définie dans Areas/HelpPage/views/Help/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="0a248-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="0a248-120">Vous pouvez modifier cette page pour modifier la disposition, l’introduction, le titre, les styles et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="0a248-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="0a248-121">La partie principale de la page est un tableau d’API, regroupées par contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0a248-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="0a248-122">Les entrées de table sont générées de manière dynamique, à l’aide de l’interface **IApiExplorer** .</span><span class="sxs-lookup"><span data-stu-id="0a248-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="0a248-123">(Je parlerai plus en détail de cette interface ultérieurement.) Si vous ajoutez un nouveau contrôleur d’API, la table est automatiquement mise à jour au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0a248-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="0a248-124">La colonne « API » répertorie la méthode HTTP et l’URI relatif.</span><span class="sxs-lookup"><span data-stu-id="0a248-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="0a248-125">La colonne « Description » contient de la documentation pour chaque API.</span><span class="sxs-lookup"><span data-stu-id="0a248-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="0a248-126">Au départ, la documentation est simplement un texte d’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="0a248-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="0a248-127">Dans la section suivante, je vais vous montrer comment ajouter de la documentation à partir de commentaires XML.</span><span class="sxs-lookup"><span data-stu-id="0a248-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="0a248-128">Chaque API a un lien vers une page contenant des informations plus détaillées, y compris des exemples de corps de demande et de réponse.</span><span class="sxs-lookup"><span data-stu-id="0a248-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="0a248-129">Ajout de pages d’aide à un projet existant</span><span class="sxs-lookup"><span data-stu-id="0a248-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="0a248-130">Vous pouvez ajouter des pages d’aide à un projet d’API Web existant à l’aide du gestionnaire de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="0a248-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="0a248-131">Cette option est utile pour démarrer à partir d’un autre modèle de projet que le modèle d’API Web.</span><span class="sxs-lookup"><span data-stu-id="0a248-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="0a248-132">Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="0a248-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="0a248-133">Dans la fenêtre [console du gestionnaire de package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , tapez l’une des commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="0a248-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="0a248-134">Pour une **C#** application : `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="0a248-134">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="0a248-135">Pour une application **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="0a248-135">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="0a248-136">Il existe deux packages, un pour C# et un pour Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="0a248-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="0a248-137">Veillez à utiliser celle qui correspond à votre projet.</span><span class="sxs-lookup"><span data-stu-id="0a248-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="0a248-138">Cette commande installe les assemblys nécessaires et ajoute les vues MVC pour les pages d’aide (situées dans le dossier Areas/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="0a248-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="0a248-139">Vous devez ajouter manuellement un lien vers la page d’aide.</span><span class="sxs-lookup"><span data-stu-id="0a248-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="0a248-140">L’URI est/help.</span><span class="sxs-lookup"><span data-stu-id="0a248-140">The URI is /Help.</span></span> <span data-ttu-id="0a248-141">Pour créer un lien dans une vue Razor, ajoutez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="0a248-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="0a248-142">Veillez également à inscrire les zones.</span><span class="sxs-lookup"><span data-stu-id="0a248-142">Also, make sure to register areas.</span></span> <span data-ttu-id="0a248-143">Dans le fichier global. asax, ajoutez le code suivant à l' **Application\_méthode Start** , si elle n’existe pas déjà :</span><span class="sxs-lookup"><span data-stu-id="0a248-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="0a248-144">Ajout de la documentation sur les API</span><span class="sxs-lookup"><span data-stu-id="0a248-144">Adding API Documentation</span></span>

<span data-ttu-id="0a248-145">Par défaut, les pages d’aide contiennent des chaînes d’espace réservé pour la documentation.</span><span class="sxs-lookup"><span data-stu-id="0a248-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="0a248-146">Vous pouvez utiliser des [Commentaires de documentation XML](https://msdn.microsoft.com/library/b2s063f7.aspx) pour créer la documentation.</span><span class="sxs-lookup"><span data-stu-id="0a248-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="0a248-147">Pour activer cette fonctionnalité, ouvrez le fichier zones/HelpPage/App\_Start/HelpPageConfig. cs et supprimez les marques de commentaire de la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="0a248-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="0a248-148">Activez à présent la documentation XML.</span><span class="sxs-lookup"><span data-stu-id="0a248-148">Now enable XML documentation.</span></span> <span data-ttu-id="0a248-149">Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="0a248-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="0a248-150">Sélectionnez la page **générer** .</span><span class="sxs-lookup"><span data-stu-id="0a248-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="0a248-151">Sous **sortie**, activez la case à cocher **fichier de documentation XML**.</span><span class="sxs-lookup"><span data-stu-id="0a248-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="0a248-152">Dans la zone d’édition, tapez « App\_Data/XmlDocument. Xml ».</span><span class="sxs-lookup"><span data-stu-id="0a248-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="0a248-153">Ensuite, ouvrez le code du contrôleur d’API `ValuesController`, qui est défini dans/Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="0a248-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="0a248-154">Ajoutez des commentaires de documentation aux méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0a248-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="0a248-155">Exemple :</span><span class="sxs-lookup"><span data-stu-id="0a248-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="0a248-156">Conseil : Si vous placez le signe insertion sur la ligne au-dessus de la méthode et que vous tapez trois barres obliques, Visual Studio insère automatiquement les éléments XML.</span><span class="sxs-lookup"><span data-stu-id="0a248-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="0a248-157">Vous pouvez alors remplir les espaces.</span><span class="sxs-lookup"><span data-stu-id="0a248-157">Then you can fill in the blanks.</span></span>

<span data-ttu-id="0a248-158">Maintenant, générez et exécutez à nouveau l’application, puis accédez aux pages d’aide.</span><span class="sxs-lookup"><span data-stu-id="0a248-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="0a248-159">Les chaînes de documentation doivent apparaître dans la table des API.</span><span class="sxs-lookup"><span data-stu-id="0a248-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="0a248-160">La page d’aide lit les chaînes à partir du fichier XML au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="0a248-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="0a248-161">(Lorsque vous déployez l’application, veillez à déployer le fichier XML.)</span><span class="sxs-lookup"><span data-stu-id="0a248-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="0a248-162">En coulisses</span><span class="sxs-lookup"><span data-stu-id="0a248-162">Under the Hood</span></span>

<span data-ttu-id="0a248-163">Les pages d’aide s’appuient sur la classe **ApiExplorer** , qui fait partie de l’infrastructure de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="0a248-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="0a248-164">La classe **ApiExplorer** fournit la documentation brute pour la création d’une page d’aide.</span><span class="sxs-lookup"><span data-stu-id="0a248-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="0a248-165">Pour chaque API, **ApiExplorer** contient un **ApiDescription** qui décrit l’API.</span><span class="sxs-lookup"><span data-stu-id="0a248-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="0a248-166">À cet effet, une « API » est définie comme la combinaison de la méthode HTTP et de l’URI relatif.</span><span class="sxs-lookup"><span data-stu-id="0a248-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="0a248-167">Par exemple, voici quelques API distinctes :</span><span class="sxs-lookup"><span data-stu-id="0a248-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="0a248-168">Obtient/api/Products</span><span class="sxs-lookup"><span data-stu-id="0a248-168">GET /api/Products</span></span>
- <span data-ttu-id="0a248-169">Obtient/api/Products/{id}</span><span class="sxs-lookup"><span data-stu-id="0a248-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="0a248-170">POSTER/api/Products</span><span class="sxs-lookup"><span data-stu-id="0a248-170">POST /api/Products</span></span>

<span data-ttu-id="0a248-171">Si une action de contrôleur prend en charge plusieurs méthodes HTTP, **ApiExplorer** traite chaque méthode comme une API distincte.</span><span class="sxs-lookup"><span data-stu-id="0a248-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="0a248-172">Pour masquer une API dans **ApiExplorer**, ajoutez l’attribut **ApiExplorerSettings** à l’action et définissez *IgnoreApi* sur true.</span><span class="sxs-lookup"><span data-stu-id="0a248-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="0a248-173">Vous pouvez également ajouter cet attribut au contrôleur pour exclure l’ensemble du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0a248-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="0a248-174">La classe ApiExplorer obtient les chaînes de documentation à partir de l’interface **IDocumentationProvider** .</span><span class="sxs-lookup"><span data-stu-id="0a248-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="0a248-175">Comme vous l’avez vu précédemment, la bibliothèque de pages d’aide fournit un **IDocumentationProvider** qui obtient la documentation des chaînes de documentation XML.</span><span class="sxs-lookup"><span data-stu-id="0a248-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="0a248-176">Le code se trouve dans/Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="0a248-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="0a248-177">Vous pouvez vous procurer la documentation à partir d’une autre source en écrivant votre propre **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="0a248-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="0a248-178">Pour l’associer, appelez la méthode d’extension **SetDocumentationProvider** , définie dans **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="0a248-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="0a248-179">**ApiExplorer** appelle automatiquement l’interface **IDocumentationProvider** pour obtenir des chaînes de documentation pour chaque API.</span><span class="sxs-lookup"><span data-stu-id="0a248-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="0a248-180">Elle les stocke dans la propriété **documentation** des objets **ApiDescription** et **ApiParameterDescription** .</span><span class="sxs-lookup"><span data-stu-id="0a248-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a248-181">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="0a248-181">Next Steps</span></span>

<span data-ttu-id="0a248-182">Vous n’êtes pas limité aux pages d’aide indiquées ici.</span><span class="sxs-lookup"><span data-stu-id="0a248-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="0a248-183">En fait, **ApiExplorer** n’est pas limité à la création de pages d’aide.</span><span class="sxs-lookup"><span data-stu-id="0a248-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="0a248-184">Yao Huang lin a écrit des billets de blog pour vous aider à réfléchir à la boîte :</span><span class="sxs-lookup"><span data-stu-id="0a248-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="0a248-185">Ajout d’un client test simple à API Web ASP.NET page d’aide</span><span class="sxs-lookup"><span data-stu-id="0a248-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="0a248-186">Création d’API Web ASP.NET page d’aide sur les services auto-hébergés</span><span class="sxs-lookup"><span data-stu-id="0a248-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="0a248-187">Génération de la page d’aide (ou du client) au moment du design pour API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0a248-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="0a248-188">Personnalisations de la page d’aide avancée</span><span class="sxs-lookup"><span data-stu-id="0a248-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
