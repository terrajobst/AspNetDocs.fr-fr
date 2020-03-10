---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Utilisation de l’API Web avec ASP.NET Web Forms-ASP.NET 4. x
author: MikeWasson
description: Didacticiel avec code étape par étape pour ajouter une API Web à une application ASP.NET Forms pour ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556776"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="b3314-103">Utilisation de l’API web avec ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="b3314-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="b3314-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b3314-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b3314-105">Ce didacticiel vous guide tout au long des étapes d’ajout de l’API Web à une application ASP.NET Web Forms traditionnelle dans ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="b3314-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="b3314-106">Présentation</span><span class="sxs-lookup"><span data-stu-id="b3314-106">Overview</span></span>

<span data-ttu-id="b3314-107">Bien que API Web ASP.NET soit empaqueté avec ASP.NET MVC, il est facile d’ajouter l’API Web à une application ASP.NET Web Forms traditionnelle.</span><span class="sxs-lookup"><span data-stu-id="b3314-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="b3314-108">Pour utiliser l’API Web dans une application Web Forms, il y a deux étapes principales :</span><span class="sxs-lookup"><span data-stu-id="b3314-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="b3314-109">Ajoutez un contrôleur d’API Web dérivé de la classe **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="b3314-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="b3314-110">Ajoutez une table de routage à l' **Application\_méthode Start** .</span><span class="sxs-lookup"><span data-stu-id="b3314-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="b3314-111">Créer un projet Web Forms</span><span class="sxs-lookup"><span data-stu-id="b3314-111">Create a Web Forms Project</span></span>

<span data-ttu-id="b3314-112">Démarrez Visual Studio et sélectionnez **nouveau projet** dans la page de **démarrage** .</span><span class="sxs-lookup"><span data-stu-id="b3314-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="b3314-113">Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="b3314-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="b3314-114">Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  .</span><span class="sxs-lookup"><span data-stu-id="b3314-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="b3314-115">Sous **visuel C#** , sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="b3314-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="b3314-116">Dans la liste des modèles de projet, sélectionnez **ASP.NET Web Forms application**.</span><span class="sxs-lookup"><span data-stu-id="b3314-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="b3314-117">Entrez un nom pour le projet, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3314-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="b3314-118">Créer le modèle et le contrôleur</span><span class="sxs-lookup"><span data-stu-id="b3314-118">Create the Model and Controller</span></span>

<span data-ttu-id="b3314-119">Ce didacticiel utilise les mêmes classes de modèle et de contrôleur que le didacticiel [prise en main](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="b3314-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="b3314-120">Tout d’abord, ajoutez une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="b3314-120">First, add a model class.</span></span> <span data-ttu-id="b3314-121">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter une classe**.</span><span class="sxs-lookup"><span data-stu-id="b3314-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="b3314-122">Nommez la classe Product, puis ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="b3314-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="b3314-123">Ensuite, ajoutez un contrôleur d’API Web au projet., un *contrôleur* est l’objet qui gère les requêtes http pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="b3314-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="b3314-124">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="b3314-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="b3314-125">Sélectionnez **Ajouter un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="b3314-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="b3314-126">Sous **modèles installés**, développez **visuel C#**  et sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="b3314-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="b3314-127">Ensuite, dans la liste des modèles, sélectionnez **classe de contrôleur d’API Web**.</span><span class="sxs-lookup"><span data-stu-id="b3314-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="b3314-128">Nommez le contrôleur « ProductsController », puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3314-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="b3314-129">L’Assistant **Ajouter un nouvel élément** va créer un fichier nommé ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="b3314-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="b3314-130">Supprimez les méthodes incluses dans l’Assistant et ajoutez les méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b3314-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="b3314-131">Pour plus d’informations sur le code de ce contrôleur, reportez-vous au didacticiel [prise en main](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="b3314-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="b3314-132">Ajouter des informations de routage</span><span class="sxs-lookup"><span data-stu-id="b3314-132">Add Routing Information</span></span>

<span data-ttu-id="b3314-133">Ensuite, nous allons ajouter un itinéraire URI afin que les URI de la forme &quot;/API/Products/&quot; soient acheminés vers le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b3314-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="b3314-134">Dans **Explorateur de solutions**, double-cliquez sur global. asax pour ouvrir le fichier code-behind global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="b3314-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="b3314-135">Ajoutez l’instruction **using** suivante.</span><span class="sxs-lookup"><span data-stu-id="b3314-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="b3314-136">Ajoutez ensuite le code suivant à l' **Application\_méthode Start** :</span><span class="sxs-lookup"><span data-stu-id="b3314-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="b3314-137">Pour plus d’informations sur les tables de routage, consultez [routage dans API Web ASP.net](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b3314-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="b3314-138">Ajouter AJAX côté client</span><span class="sxs-lookup"><span data-stu-id="b3314-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="b3314-139">C’est tout ce dont vous avez besoin pour créer une API Web à laquelle les clients peuvent accéder.</span><span class="sxs-lookup"><span data-stu-id="b3314-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="b3314-140">À présent, nous allons ajouter une page HTML qui utilise jQuery pour appeler l’API.</span><span class="sxs-lookup"><span data-stu-id="b3314-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="b3314-141">Assurez-vous que votre page maître (par exemple, *site. Master*) comprend une `ContentPlaceHolder` avec `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="b3314-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="b3314-142">Ouvrez le fichier default. aspx.</span><span class="sxs-lookup"><span data-stu-id="b3314-142">Open the file Default.aspx.</span></span> <span data-ttu-id="b3314-143">Remplacez le texte réutilisable qui se trouve dans la section de contenu principale, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b3314-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="b3314-144">Ensuite, ajoutez une référence au fichier source jQuery dans la section `HeaderContent` :</span><span class="sxs-lookup"><span data-stu-id="b3314-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="b3314-145">Remarque : vous pouvez facilement ajouter la référence de script en faisant glisser le fichier de **Explorateur de solutions** vers la fenêtre de l’éditeur de code.</span><span class="sxs-lookup"><span data-stu-id="b3314-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="b3314-146">Sous la balise de script jQuery, ajoutez le bloc de script suivant :</span><span class="sxs-lookup"><span data-stu-id="b3314-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="b3314-147">Lors du chargement du document, ce script effectue une demande AJAX à &quot;&quot;des API/produits.</span><span class="sxs-lookup"><span data-stu-id="b3314-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="b3314-148">La requête retourne une liste de produits au format JSON.</span><span class="sxs-lookup"><span data-stu-id="b3314-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="b3314-149">Le script ajoute les informations sur le produit à la table HTML.</span><span class="sxs-lookup"><span data-stu-id="b3314-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="b3314-150">Quand vous exécutez l’application, elle doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="b3314-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
