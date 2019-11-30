---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Partie 5 : création d’une interface utilisateur dynamique avec Knockout. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599994"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="cc01d-102">Partie 5 : création d’une interface utilisateur dynamique avec Knockout. js</span><span class="sxs-lookup"><span data-stu-id="cc01d-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="cc01d-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cc01d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cc01d-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="cc01d-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="cc01d-105">Création d’une interface utilisateur dynamique avec Knockout.js</span><span class="sxs-lookup"><span data-stu-id="cc01d-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="cc01d-106">Dans cette section, nous allons utiliser Knockout. js pour ajouter des fonctionnalités à la vue d’administration.</span><span class="sxs-lookup"><span data-stu-id="cc01d-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="cc01d-107">[Knockout. js](http://knockoutjs.com/) est une bibliothèque JavaScript qui facilite la liaison des contrôles HTML aux données.</span><span class="sxs-lookup"><span data-stu-id="cc01d-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="cc01d-108">Knockout. js utilise le modèle MVVM (Model-View-ViewModel).</span><span class="sxs-lookup"><span data-stu-id="cc01d-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="cc01d-109">Le *modèle* est la représentation côté serveur des données dans le domaine d’entreprise (dans notre cas, Products et Orders).</span><span class="sxs-lookup"><span data-stu-id="cc01d-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="cc01d-110">La *vue* est la couche de présentation (html).</span><span class="sxs-lookup"><span data-stu-id="cc01d-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="cc01d-111">Le *modèle d’affichage* est un objet JavaScript qui contient les données du modèle.</span><span class="sxs-lookup"><span data-stu-id="cc01d-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="cc01d-112">Le modèle d’affichage est une abstraction du code de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cc01d-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="cc01d-113">Il n’a aucune connaissance de la représentation HTML.</span><span class="sxs-lookup"><span data-stu-id="cc01d-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="cc01d-114">Au lieu de cela, il représente les fonctionnalités abstraites de la vue, telles que « une liste d’éléments ».</span><span class="sxs-lookup"><span data-stu-id="cc01d-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="cc01d-115">La vue est liée aux données du modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="cc01d-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="cc01d-116">Les mises à jour du modèle d’affichage sont automatiquement reflétées dans la vue.</span><span class="sxs-lookup"><span data-stu-id="cc01d-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="cc01d-117">Le modèle de vue obtient également des événements de la vue, tels que des clics de bouton, et effectue des opérations sur le modèle, telles que la création d’une commande.</span><span class="sxs-lookup"><span data-stu-id="cc01d-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="cc01d-118">Tout d’abord, nous allons définir le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="cc01d-118">First we'll define the view-model.</span></span> <span data-ttu-id="cc01d-119">Après cela, nous allons lier le balisage HTML au modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="cc01d-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="cc01d-120">Ajoutez la section Razor suivante à admin. cshtml :</span><span class="sxs-lookup"><span data-stu-id="cc01d-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="cc01d-121">Vous pouvez ajouter cette section n’importe où dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="cc01d-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="cc01d-122">Lorsque la vue est restituée, la section apparaît au bas de la page HTML, juste avant la balise de fermeture &lt;/Body&gt;.</span><span class="sxs-lookup"><span data-stu-id="cc01d-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="cc01d-123">Tout le script de cette page sera placé dans la balise de script indiquée par le commentaire :</span><span class="sxs-lookup"><span data-stu-id="cc01d-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="cc01d-124">Tout d’abord, définissez une classe de modèle d’affichage :</span><span class="sxs-lookup"><span data-stu-id="cc01d-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="cc01d-125">**Ko. observableArray** est un type spécial d’objet dans Knockout, appelé *observable*.</span><span class="sxs-lookup"><span data-stu-id="cc01d-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="cc01d-126">Dans la [documentation de Knockout. js](http://knockoutjs.com/documentation/observables.html): un observable est un « objet JavaScript qui peut notifier les modifications aux abonnés. »</span><span class="sxs-lookup"><span data-stu-id="cc01d-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="cc01d-127">Lorsque le contenu d’un observable change, la vue est automatiquement mise à jour pour correspondre.</span><span class="sxs-lookup"><span data-stu-id="cc01d-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="cc01d-128">Pour remplir le tableau `products`, effectuez une demande AJAX à l’API Web.</span><span class="sxs-lookup"><span data-stu-id="cc01d-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="cc01d-129">Rappelez-vous que nous avons stocké l’URI de base pour l’API dans le conteneur d’affichage (voir la [partie 4](using-web-api-with-entity-framework-part-4.md) du didacticiel).</span><span class="sxs-lookup"><span data-stu-id="cc01d-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="cc01d-130">Ensuite, ajoutez des fonctions au modèle d’affichage pour créer, mettre à jour et supprimer des produits.</span><span class="sxs-lookup"><span data-stu-id="cc01d-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="cc01d-131">Ces fonctions soumettent les appels AJAX à l’API Web et utilisent les résultats pour mettre à jour le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="cc01d-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="cc01d-132">À présent, la partie la plus importante : lorsque le DOM est chargé, appelez la fonction **Ko. applyBindings** et transmettez une nouvelle instance de l' `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="cc01d-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="cc01d-133">La méthode **Ko. applyBindings** active Knockout et relie le modèle d’affichage à la vue.</span><span class="sxs-lookup"><span data-stu-id="cc01d-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="cc01d-134">Maintenant que nous avons un modèle de vue, nous pouvons créer les liaisons.</span><span class="sxs-lookup"><span data-stu-id="cc01d-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="cc01d-135">Dans Knockout. js, vous pouvez le faire en ajoutant `data-bind` attributs aux éléments HTML.</span><span class="sxs-lookup"><span data-stu-id="cc01d-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="cc01d-136">Par exemple, pour lier une liste HTML à un tableau, utilisez la liaison de `foreach` :</span><span class="sxs-lookup"><span data-stu-id="cc01d-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="cc01d-137">La liaison `foreach` itère au sein du tableau et crée des éléments enfants pour chaque objet dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="cc01d-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="cc01d-138">Les liaisons sur les éléments enfants peuvent faire référence aux propriétés sur les objets du tableau.</span><span class="sxs-lookup"><span data-stu-id="cc01d-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="cc01d-139">Ajoutez les liaisons suivantes à la liste « UPDATE-Products » :</span><span class="sxs-lookup"><span data-stu-id="cc01d-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="cc01d-140">L’élément `<li>` se produit dans la portée de la liaison **foreach** .</span><span class="sxs-lookup"><span data-stu-id="cc01d-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="cc01d-141">Cela signifie que Knockout restituera l’élément une fois pour chaque produit dans le tableau `products`.</span><span class="sxs-lookup"><span data-stu-id="cc01d-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="cc01d-142">Toutes les liaisons au sein de l’élément `<li>` font référence à cette instance du produit.</span><span class="sxs-lookup"><span data-stu-id="cc01d-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="cc01d-143">Par exemple, `$data.Name` fait référence à la propriété `Name` sur le produit.</span><span class="sxs-lookup"><span data-stu-id="cc01d-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="cc01d-144">Pour définir les valeurs des entrées de texte, utilisez la liaison de `value`.</span><span class="sxs-lookup"><span data-stu-id="cc01d-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="cc01d-145">Les boutons sont liés aux fonctions de la vue de modèle, à l’aide de la liaison de `click`.</span><span class="sxs-lookup"><span data-stu-id="cc01d-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="cc01d-146">L’instance Product est transmise en tant que paramètre à chaque fonction.</span><span class="sxs-lookup"><span data-stu-id="cc01d-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="cc01d-147">Pour plus d’informations, la [documentation de Knockout. js](http://knockoutjs.com/documentation/observables.html) contient de bonnes descriptions des différentes liaisons.</span><span class="sxs-lookup"><span data-stu-id="cc01d-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="cc01d-148">Ensuite, ajoutez une liaison pour l’événement **Submit** sur le formulaire ajouter un produit :</span><span class="sxs-lookup"><span data-stu-id="cc01d-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="cc01d-149">Cette liaison appelle la fonction `create` sur le modèle d’affichage pour créer un nouveau produit.</span><span class="sxs-lookup"><span data-stu-id="cc01d-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="cc01d-150">Voici le code complet de la vue d’administration :</span><span class="sxs-lookup"><span data-stu-id="cc01d-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="cc01d-151">Exécutez l’application, connectez-vous avec le compte d’administrateur, puis cliquez sur le lien « admin ».</span><span class="sxs-lookup"><span data-stu-id="cc01d-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="cc01d-152">Vous devez voir la liste des produits et être en mesure de créer, mettre à jour ou supprimer des produits.</span><span class="sxs-lookup"><span data-stu-id="cc01d-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cc01d-153">[Précédent](using-web-api-with-entity-framework-part-4.md)
> [Suivant](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="cc01d-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
