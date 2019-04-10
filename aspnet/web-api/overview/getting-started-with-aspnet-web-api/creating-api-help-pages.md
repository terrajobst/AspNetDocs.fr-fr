---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Création de Pages d’aide pour l’API Web ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Ce didacticiel avec le code montre comment créer des pages d’aide pour l’API Web ASP.NET dans ASP.NET 4.x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: e3f6a9b8a6835b034a075d580cd9a33136969990
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59395012"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="ffb70-103">Création de Pages d’aide pour l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ffb70-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="ffb70-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ffb70-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ffb70-105">Ce didacticiel avec le code montre comment créer des pages d’aide pour l’API Web ASP.NET dans ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="ffb70-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="ffb70-106">Lorsque vous créez une API web, il est souvent utile de créer une page d’aide, afin que les autres développeurs sache comment appeler votre API.</span><span class="sxs-lookup"><span data-stu-id="ffb70-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="ffb70-107">Vous pouvez créer manuellement toute la documentation, mais il est préférable de créer automatiquement autant que possible.</span><span class="sxs-lookup"><span data-stu-id="ffb70-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="ffb70-108">Pour faciliter cette tâche, l’API Web ASP.NET fournit une bibliothèque pour générer automatiquement des pages d’aide en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="ffb70-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="ffb70-109">Création de Pages d’aide de l’API</span><span class="sxs-lookup"><span data-stu-id="ffb70-109">Creating API Help Pages</span></span>

<span data-ttu-id="ffb70-110">Installer [ASP.NET et Web Tools 2012.2 mise à jour](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="ffb70-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="ffb70-111">Cette mise à jour intègre des pages d’aide dans le modèle de projet API Web.</span><span class="sxs-lookup"><span data-stu-id="ffb70-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="ffb70-112">Ensuite, créez un projet ASP.NET MVC 4 et sélectionnez le modèle de projet API Web.</span><span class="sxs-lookup"><span data-stu-id="ffb70-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="ffb70-113">Le modèle de projet crée un contrôleur de l’exemple d’API nommé `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="ffb70-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="ffb70-114">Le modèle crée également les pages d’aide API.</span><span class="sxs-lookup"><span data-stu-id="ffb70-114">The template also creates the API help pages.</span></span> <span data-ttu-id="ffb70-115">Tous les fichiers de code pour la page d’aide sont placés dans le dossier des zones du projet.</span><span class="sxs-lookup"><span data-stu-id="ffb70-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="ffb70-116">Lorsque vous exécutez l’application, la page d’accueil contient un lien vers la page d’aide API.</span><span class="sxs-lookup"><span data-stu-id="ffb70-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="ffb70-117">À partir de la page d’accueil, le chemin d’accès relatif est /Help.</span><span class="sxs-lookup"><span data-stu-id="ffb70-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="ffb70-118">Ce lien vous dirige vers une page de résumé des API.</span><span class="sxs-lookup"><span data-stu-id="ffb70-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="ffb70-119">La vue MVC pour cette page est définie dans Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="ffb70-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="ffb70-120">Vous pouvez modifier cette page pour modifier la mise en page introduction, titre, styles et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="ffb70-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="ffb70-121">La partie principale de la page est une table d’API, regroupées par contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ffb70-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="ffb70-122">Les entrées de table sont générées dynamiquement, à l’aide de la **IApiExplorer** interface.</span><span class="sxs-lookup"><span data-stu-id="ffb70-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="ffb70-123">(Je reviendrai sur cette interface ultérieurement.) Si vous ajoutez un nouveau contrôleur d’API, la table est automatiquement mis à jour en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="ffb70-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="ffb70-124">La colonne « API » répertorie la méthode HTTP et un URI relatif.</span><span class="sxs-lookup"><span data-stu-id="ffb70-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="ffb70-125">La colonne « Description » contient la documentation pour chaque API.</span><span class="sxs-lookup"><span data-stu-id="ffb70-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="ffb70-126">Initialement, la documentation est simplement texte d’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="ffb70-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="ffb70-127">Dans la section suivante, je vous montrerai comment ajouter la documentation à partir de commentaires XML.</span><span class="sxs-lookup"><span data-stu-id="ffb70-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="ffb70-128">Chaque API comporte un lien vers une page avec des informations plus détaillées, y compris le corps de demande et de réponse d’exemple.</span><span class="sxs-lookup"><span data-stu-id="ffb70-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="ffb70-129">Ajout de Pages d’aide à un projet existant</span><span class="sxs-lookup"><span data-stu-id="ffb70-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="ffb70-130">Vous pouvez ajouter des pages d’aide à un projet d’API Web existant à l’aide du Gestionnaire de Package NuGet.</span><span class="sxs-lookup"><span data-stu-id="ffb70-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="ffb70-131">Cette option est utile de que commencer à partir d’un autre modèle de projet que le modèle « API Web ».</span><span class="sxs-lookup"><span data-stu-id="ffb70-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="ffb70-132">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="ffb70-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="ffb70-133">Dans le [Console du Gestionnaire de Package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) fenêtre, tapez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="ffb70-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="ffb70-134">Pour un **c#** application :</span><span class="sxs-lookup"><span data-stu-id="ffb70-134">For a **C#** application:</span></span> `Install-Package Microsoft.AspNet.WebApi.HelpPage`

<span data-ttu-id="ffb70-135">Pour un **Visual Basic** application :</span><span class="sxs-lookup"><span data-stu-id="ffb70-135">For a **Visual Basic** application:</span></span> `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

<span data-ttu-id="ffb70-136">Il existe deux packages, pour c# et l’autre pour Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ffb70-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="ffb70-137">Veillez à utiliser celui qui correspond à votre projet.</span><span class="sxs-lookup"><span data-stu-id="ffb70-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="ffb70-138">Cette commande installe les assemblys nécessaires et ajoute les vues MVC pour les pages d’aide (situés dans le dossier zones/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="ffb70-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="ffb70-139">Vous devez manuellement ajouter un lien vers la page d’aide.</span><span class="sxs-lookup"><span data-stu-id="ffb70-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="ffb70-140">L’URI est /Help.</span><span class="sxs-lookup"><span data-stu-id="ffb70-140">The URI is /Help.</span></span> <span data-ttu-id="ffb70-141">Pour créer un lien dans une vue razor, ajoutez ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="ffb70-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="ffb70-142">En outre, assurez-vous qu’inscrire les zones.</span><span class="sxs-lookup"><span data-stu-id="ffb70-142">Also, make sure to register areas.</span></span> <span data-ttu-id="ffb70-143">Dans le fichier Global.asax, ajoutez le code suivant à la **Application\_Démarrer** méthode, s’il y ne figure pas déjà :</span><span class="sxs-lookup"><span data-stu-id="ffb70-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="ffb70-144">Ajout de Documentation de l’API</span><span class="sxs-lookup"><span data-stu-id="ffb70-144">Adding API Documentation</span></span>

<span data-ttu-id="ffb70-145">Par défaut, l’aide de pages contiennent des chaînes d’espace réservé pour la documentation.</span><span class="sxs-lookup"><span data-stu-id="ffb70-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="ffb70-146">Vous pouvez utiliser [commentaires de documentation XML](https://msdn.microsoft.com/library/b2s063f7.aspx) pour créer la documentation.</span><span class="sxs-lookup"><span data-stu-id="ffb70-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="ffb70-147">Pour activer cette fonctionnalité, ouvrez le fichier de zones/HelpPage/application\_Start/HelpPageConfig.cs et supprimez les commentaires de la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="ffb70-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="ffb70-148">Activez maintenant la documentation XML.</span><span class="sxs-lookup"><span data-stu-id="ffb70-148">Now enable XML documentation.</span></span> <span data-ttu-id="ffb70-149">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="ffb70-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="ffb70-150">Sélectionnez le **Build** page.</span><span class="sxs-lookup"><span data-stu-id="ffb70-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="ffb70-151">Sous **sortie**, vérifiez **fichier de documentation XML**.</span><span class="sxs-lookup"><span data-stu-id="ffb70-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="ffb70-152">Dans la zone d’édition, tapez « application\_Data/XmlDocument.xml ».</span><span class="sxs-lookup"><span data-stu-id="ffb70-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="ffb70-153">Ensuite, ouvrez le code pour le `ValuesController` contrôleur d’API, qui est défini dans /Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="ffb70-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="ffb70-154">Ajouter des commentaires de documentation pour les méthodes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ffb70-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="ffb70-155">Exemple :</span><span class="sxs-lookup"><span data-stu-id="ffb70-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="ffb70-156">Conseil : Si vous positionnez le signe insertion sur la ligne au-dessus de la méthode et que vous tapez les trois barres obliques, Visual Studio insère automatiquement les éléments XML.</span><span class="sxs-lookup"><span data-stu-id="ffb70-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="ffb70-157">Vous pouvez ensuite renseigner les champs vides.</span><span class="sxs-lookup"><span data-stu-id="ffb70-157">Then you can fill in the blanks.</span></span>


<span data-ttu-id="ffb70-158">Maintenant générer et exécuter à nouveau l’application et naviguer vers les pages d’aide.</span><span class="sxs-lookup"><span data-stu-id="ffb70-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="ffb70-159">Les chaînes de documentation doivent apparaître dans la table API.</span><span class="sxs-lookup"><span data-stu-id="ffb70-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="ffb70-160">La page d’aide lit les chaînes à partir du fichier XML en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="ffb70-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="ffb70-161">(Lorsque vous déployez l’application, veillez à déployer le fichier XML.)</span><span class="sxs-lookup"><span data-stu-id="ffb70-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="ffb70-162">Sous le capot</span><span class="sxs-lookup"><span data-stu-id="ffb70-162">Under the Hood</span></span>

<span data-ttu-id="ffb70-163">Les pages d’aide sont appuient sur le **ApiExplorer** (classe), qui fait partie de l’infrastructure API Web.</span><span class="sxs-lookup"><span data-stu-id="ffb70-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="ffb70-164">Le **ApiExplorer** classe fournit la matière première pour la création d’une page d’aide.</span><span class="sxs-lookup"><span data-stu-id="ffb70-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="ffb70-165">Pour chaque API, **ApiExplorer** contient un **ApiDescription** qui décrit l’API.</span><span class="sxs-lookup"><span data-stu-id="ffb70-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="ffb70-166">Pour cela, une « API » est définie comme la combinaison de méthodes HTTP et l’URI relatif.</span><span class="sxs-lookup"><span data-stu-id="ffb70-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="ffb70-167">Par exemple, Voici certaines API distinctes :</span><span class="sxs-lookup"><span data-stu-id="ffb70-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="ffb70-168">OBTENIR /api/Products</span><span class="sxs-lookup"><span data-stu-id="ffb70-168">GET /api/Products</span></span>
- <span data-ttu-id="ffb70-169">OBTENIR/API/produits / {id}</span><span class="sxs-lookup"><span data-stu-id="ffb70-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="ffb70-170">VALIDER les produits/api /</span><span class="sxs-lookup"><span data-stu-id="ffb70-170">POST /api/Products</span></span>

<span data-ttu-id="ffb70-171">Si une action de contrôleur prend en charge plusieurs méthodes HTTP, le **ApiExplorer** traite chaque méthode comme une API distincte.</span><span class="sxs-lookup"><span data-stu-id="ffb70-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="ffb70-172">Pour masquer une API à partir de la **ApiExplorer**, ajouter le **ApiExplorerSettings** à l’action et un ensemble d’attributs *IgnoreApi* sur true.</span><span class="sxs-lookup"><span data-stu-id="ffb70-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="ffb70-173">Vous pouvez également ajouter cet attribut au contrôleur, pour exclure l’ensemble du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ffb70-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="ffb70-174">La classe ApiExplorer Obtient les chaînes de documentation à partir de la **IDocumentationProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="ffb70-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="ffb70-175">Comme vous l’avez vu précédemment, la bibliothèque de Pages d’aide fournit un **IDocumentationProvider** qui obtient la documentation à partir de chaînes de documentation XML.</span><span class="sxs-lookup"><span data-stu-id="ffb70-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="ffb70-176">Le code se trouve dans /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="ffb70-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="ffb70-177">Vous pouvez obtenir documentation à partir d’une autre source en écrivant votre propre **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="ffb70-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="ffb70-178">Pour rattacher, appelez le **SetDocumentationProvider** méthode d’extension, définie dans **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="ffb70-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="ffb70-179">**ApiExplorer** appelle automatiquement la **IDocumentationProvider** interface permettant d’obtenir des chaînes de documentation pour chaque API.</span><span class="sxs-lookup"><span data-stu-id="ffb70-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="ffb70-180">Elle les stocke dans le **Documentation** propriété de la **ApiDescription** et **ApiParameterDescription** objets.</span><span class="sxs-lookup"><span data-stu-id="ffb70-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ffb70-181">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ffb70-181">Next Steps</span></span>

<span data-ttu-id="ffb70-182">Vous n’êtes pas limité aux pages d’aide indiqués ici.</span><span class="sxs-lookup"><span data-stu-id="ffb70-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="ffb70-183">En fait, **ApiExplorer** n’est pas limité à la création de pages d’aide.</span><span class="sxs-lookup"><span data-stu-id="ffb70-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="ffb70-184">Yao Huang Lin a écrit qu'un excellent blog publie pour susciter votre réflexion prêts à l’emploi :</span><span class="sxs-lookup"><span data-stu-id="ffb70-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="ffb70-185">Ajout d’un Client de Test simple à la Page d’aide ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="ffb70-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="ffb70-186">Faire les API vous aider à la Page Web ASP.NET à fonctionner sur les services auto-hébergés</span><span class="sxs-lookup"><span data-stu-id="ffb70-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="ffb70-187">Génération au moment du design de help page (ou client) pour l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ffb70-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="ffb70-188">Personnalisations de Page d’aide avancées</span><span class="sxs-lookup"><span data-stu-id="ffb70-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
