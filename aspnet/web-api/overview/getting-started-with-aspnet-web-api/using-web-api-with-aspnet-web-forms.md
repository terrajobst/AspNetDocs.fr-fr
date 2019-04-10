---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: À l’aide des API Web avec ASP.NET Web Forms - ASP.NET 4.x
author: MikeWasson
description: Didacticiel avec code étape par étape pour ajouter des API Web à une application de formulaires ASP.NET pour ASP.NET 4.x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59422575"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="c179b-103">Utilisation de l’API web avec ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="c179b-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="c179b-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c179b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c179b-105">Ce didacticiel vous guide à travers les étapes pour ajouter l’API Web à une application ASP.NET Web Forms traditionnelle dans ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="c179b-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="c179b-106">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c179b-106">Overview</span></span>

<span data-ttu-id="c179b-107">Bien que l’API Web ASP.NET est fourni avec ASP.NET MVC, il est facile d’ajouter des API Web à une application ASP.NET Web Forms traditionnelle.</span><span class="sxs-lookup"><span data-stu-id="c179b-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="c179b-108">Pour utiliser les API Web dans une application Web Forms, il existe deux étapes principales :</span><span class="sxs-lookup"><span data-stu-id="c179b-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="c179b-109">Ajouter un contrôleur d’API Web qui dérive de la **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="c179b-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="c179b-110">Ajouter une table de routage pour le **Application\_Démarrer** (méthode).</span><span class="sxs-lookup"><span data-stu-id="c179b-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="c179b-111">Créer un projet Web Forms</span><span class="sxs-lookup"><span data-stu-id="c179b-111">Create a Web Forms Project</span></span>

<span data-ttu-id="c179b-112">Démarrez Visual Studio et sélectionnez **nouveau projet** à partir de la **Démarrer** page.</span><span class="sxs-lookup"><span data-stu-id="c179b-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="c179b-113">Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="c179b-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c179b-114">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud.</span><span class="sxs-lookup"><span data-stu-id="c179b-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c179b-115">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="c179b-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c179b-116">Dans la liste des modèles de projet, sélectionnez **Application ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="c179b-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="c179b-117">Entrez un nom pour le projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="c179b-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="c179b-118">Créer le modèle et le contrôleur</span><span class="sxs-lookup"><span data-stu-id="c179b-118">Create the Model and Controller</span></span>

<span data-ttu-id="c179b-119">Ce didacticiel utilise les mêmes classes de modèle et du contrôleur en tant que le [mise en route](tutorial-your-first-web-api.md) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c179b-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="c179b-120">Tout d’abord, ajoutez une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="c179b-120">First, add a model class.</span></span> <span data-ttu-id="c179b-121">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter une classe**.</span><span class="sxs-lookup"><span data-stu-id="c179b-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="c179b-122">Nommez la classe produit et ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="c179b-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="c179b-123">Ensuite, ajoutez un contrôleur d’API Web au projet., A *contrôleur* est l’objet qui gère les requêtes HTTP pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="c179b-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="c179b-124">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="c179b-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="c179b-125">Sélectionnez **ajouter un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="c179b-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="c179b-126">Sous **modèles installés**, développez **Visual C#** et sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="c179b-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="c179b-127">Ensuite, dans la liste des modèles, sélectionnez **Web API Controller Class**.</span><span class="sxs-lookup"><span data-stu-id="c179b-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="c179b-128">Nommez le contrôleur « ProductsController » et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="c179b-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="c179b-129">Le **ajouter un nouvel élément** Assistant va créer un fichier nommé ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="c179b-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="c179b-130">Supprimez les méthodes de l’Assistant inclus et ajoutez les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="c179b-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="c179b-131">Pour plus d’informations sur le code dans ce contrôleur, consultez le [mise en route](tutorial-your-first-web-api.md) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c179b-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="c179b-132">Ajouter des informations de routage</span><span class="sxs-lookup"><span data-stu-id="c179b-132">Add Routing Information</span></span>

<span data-ttu-id="c179b-133">Ensuite, nous allons donc ajouter un itinéraire URI cet URI sous la forme &quot;/API/produits/&quot; sont acheminés vers le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c179b-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="c179b-134">Dans **l’Explorateur de solutions**, double-cliquez sur Global.asax pour ouvrir le fichier code-behind Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="c179b-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="c179b-135">Ajoutez le code suivant **à l’aide de** instruction.</span><span class="sxs-lookup"><span data-stu-id="c179b-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="c179b-136">Puis ajoutez le code suivant à la **Application\_Démarrer** méthode :</span><span class="sxs-lookup"><span data-stu-id="c179b-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="c179b-137">Pour plus d’informations sur les tables de routage, consultez [routage dans ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c179b-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="c179b-138">Ajouter côté Client AJAX</span><span class="sxs-lookup"><span data-stu-id="c179b-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="c179b-139">Voilà, il que vous suffit de créer une API web que les clients peuvent accéder.</span><span class="sxs-lookup"><span data-stu-id="c179b-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="c179b-140">Maintenant nous allons ajouter une page HTML qui utilise jQuery pour appeler l’API.</span><span class="sxs-lookup"><span data-stu-id="c179b-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="c179b-141">Assurez-vous que votre page maître (par exemple, *Site.Master*) inclut un `ContentPlaceHolder` avec `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="c179b-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="c179b-142">Ouvrez le fichier Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="c179b-142">Open the file Default.aspx.</span></span> <span data-ttu-id="c179b-143">Remplacez le texte réutilisable qui se trouve dans la section de contenu principale, comme indiqué :</span><span class="sxs-lookup"><span data-stu-id="c179b-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="c179b-144">Ensuite, ajoutez une référence au fichier source jQuery dans les `HeaderContent` section :</span><span class="sxs-lookup"><span data-stu-id="c179b-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="c179b-145">Remarque : Vous pouvez facilement ajouter la référence de script en faisant glisser le fichier à partir de **l’Explorateur de solutions** dans la fenêtre d’éditeur de code.</span><span class="sxs-lookup"><span data-stu-id="c179b-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="c179b-146">Sous la balise de script jQuery, ajoutez le bloc de script suivant :</span><span class="sxs-lookup"><span data-stu-id="c179b-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="c179b-147">Lorsque le chargement du document, ce script effectue une requête AJAX à &quot;api/produits&quot;.</span><span class="sxs-lookup"><span data-stu-id="c179b-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="c179b-148">La requête retourne une liste de produits au format JSON.</span><span class="sxs-lookup"><span data-stu-id="c179b-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="c179b-149">Le script ajoute les informations de produit à la table HTML.</span><span class="sxs-lookup"><span data-stu-id="c179b-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="c179b-150">Lorsque vous exécutez l’application, il doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="c179b-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
