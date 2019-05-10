---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Envoi de données de formulaire HTML dans l’API Web ASP.NET : Données Form-UrlEncode - ASP.NET 4.x'
author: MikeWasson
description: Cet article explique comment publier des données de form-UrlEncode sur un contrôleur d’API Web avec ASP.NET 4.x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115468"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="f53e0-103">Envoi de données de formulaire HTML dans l’API Web ASP.NET : données de formulaire encodées dans l’URL</span><span class="sxs-lookup"><span data-stu-id="f53e0-103">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>

<span data-ttu-id="f53e0-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f53e0-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="f53e0-105">Partie 1 : données de formulaire encodées dans l’URL</span><span class="sxs-lookup"><span data-stu-id="f53e0-105">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="f53e0-106">Cet article explique comment publier des données de form-UrlEncode sur un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="f53e0-106">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="f53e0-107">Vue d’ensemble des formulaires HTML</span><span class="sxs-lookup"><span data-stu-id="f53e0-107">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="f53e0-108">Envoi de Types complexes</span><span class="sxs-lookup"><span data-stu-id="f53e0-108">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="f53e0-109">Envoi de données de formulaire via AJAX</span><span class="sxs-lookup"><span data-stu-id="f53e0-109">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="f53e0-110">Envoi de Types simples</span><span class="sxs-lookup"><span data-stu-id="f53e0-110">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="f53e0-111">[Télécharger le projet achevé](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="f53e0-111">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="f53e0-112">Vue d’ensemble des formulaires HTML</span><span class="sxs-lookup"><span data-stu-id="f53e0-112">Overview of HTML Forms</span></span>

<span data-ttu-id="f53e0-113">Utilisation de formulaires HTML soit GET ou POST pour envoyer des données au serveur.</span><span class="sxs-lookup"><span data-stu-id="f53e0-113">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="f53e0-114">Le **méthode** attribut de la **formulaire** élément donne la méthode HTTP :</span><span class="sxs-lookup"><span data-stu-id="f53e0-114">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="f53e0-115">La méthode par défaut est GET.</span><span class="sxs-lookup"><span data-stu-id="f53e0-115">The default method is GET.</span></span> <span data-ttu-id="f53e0-116">Si le formulaire utilise GET, le formulaire de données sont encodées dans l’URI sous la forme d’une chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="f53e0-116">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="f53e0-117">Si le formulaire utilise POST, les données du formulaire sont placées dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="f53e0-117">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="f53e0-118">Pour les données de publication, le **enctype** attribut spécifie le format du corps de la demande :</span><span class="sxs-lookup"><span data-stu-id="f53e0-118">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="f53e0-119">enctype</span><span class="sxs-lookup"><span data-stu-id="f53e0-119">enctype</span></span> | <span data-ttu-id="f53e0-120">Description</span><span class="sxs-lookup"><span data-stu-id="f53e0-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f53e0-121">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="f53e0-121">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="f53e0-122">Données de formulaire sont encodées sous forme de paires nom/valeur similaires à une chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="f53e0-122">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="f53e0-123">Il s’agit du format par défaut d’une requête POST.</span><span class="sxs-lookup"><span data-stu-id="f53e0-123">This is the default format for POST.</span></span> |
| <span data-ttu-id="f53e0-124">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="f53e0-124">multipart/form-data</span></span> | <span data-ttu-id="f53e0-125">Données de formulaire sont encodées comme un message MIME en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="f53e0-125">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="f53e0-126">Utilisez ce format si vous téléchargez un fichier vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="f53e0-126">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="f53e0-127">Partie 1 de cet article examine de format x--www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="f53e0-127">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="f53e0-128">[Partie 2](sending-html-form-data-part-2.md) décrit MIME en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="f53e0-128">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="f53e0-129">Envoi de Types complexes</span><span class="sxs-lookup"><span data-stu-id="f53e0-129">Sending Complex Types</span></span>

<span data-ttu-id="f53e0-130">En règle générale, vous enverrez un type complexe, composé de valeurs provenant de plusieurs contrôles de formulaire.</span><span class="sxs-lookup"><span data-stu-id="f53e0-130">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="f53e0-131">Prenez en compte le modèle suivant qui représente une mise à jour d’état :</span><span class="sxs-lookup"><span data-stu-id="f53e0-131">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="f53e0-132">Voici un contrôleur d’API Web qui accepte un `Update` objet via POST.</span><span class="sxs-lookup"><span data-stu-id="f53e0-132">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="f53e0-133">Ce contrôleur utilise [le routage basé sur l’action](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), de sorte que le modèle d’itinéraire est &quot;api / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="f53e0-133">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="f53e0-134">Le client valide les données à &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="f53e0-134">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>

<span data-ttu-id="f53e0-135">Maintenant nous allons écrire un formulaire HTML pour les utilisateurs à envoyer une mise à jour d’état.</span><span class="sxs-lookup"><span data-stu-id="f53e0-135">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="f53e0-136">Notez que le **action** attribut sur le formulaire est l’URI de notre action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f53e0-136">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="f53e0-137">Voici le formulaire avec des valeurs entrées dans :</span><span class="sxs-lookup"><span data-stu-id="f53e0-137">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="f53e0-138">Lorsque l’utilisateur clique sur Envoyer, le navigateur envoie une requête HTTP similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f53e0-138">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="f53e0-139">Notez que le corps de la requête contient les données du formulaire, sous formatées de paires nom/valeur.</span><span class="sxs-lookup"><span data-stu-id="f53e0-139">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="f53e0-140">API Web convertit automatiquement les paires nom/valeur dans une instance de la `Update` classe.</span><span class="sxs-lookup"><span data-stu-id="f53e0-140">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="f53e0-141">Envoi de données de formulaire via AJAX</span><span class="sxs-lookup"><span data-stu-id="f53e0-141">Sending Form Data via AJAX</span></span>

<span data-ttu-id="f53e0-142">Lorsqu’un utilisateur soumet un formulaire, le navigateur navigue en dehors de la page actuelle et affiche le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="f53e0-142">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="f53e0-143">C’est OK lorsque la réponse est une page HTML.</span><span class="sxs-lookup"><span data-stu-id="f53e0-143">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="f53e0-144">Avec une API web, toutefois, le corps de réponse est généralement soit vide ou contient des données structurées, tel que JSON.</span><span class="sxs-lookup"><span data-stu-id="f53e0-144">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="f53e0-145">Dans ce cas, il est plus judicieux d’envoyer les données du formulaire à l’aide d’AJAX demande, afin que la page peut traiter la réponse.</span><span class="sxs-lookup"><span data-stu-id="f53e0-145">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="f53e0-146">Le code suivant montre comment valider des données de formulaire à l’aide de jQuery.</span><span class="sxs-lookup"><span data-stu-id="f53e0-146">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="f53e0-147">Le jQuery **soumettre** fonction remplace l’action de formulaire par une nouvelle fonction.</span><span class="sxs-lookup"><span data-stu-id="f53e0-147">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="f53e0-148">Cela remplace le comportement par défaut du bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="f53e0-148">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="f53e0-149">Le **sérialiser** fonction sérialise les données de formulaire dans les paires nom/valeur.</span><span class="sxs-lookup"><span data-stu-id="f53e0-149">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="f53e0-150">Pour envoyer les données du formulaire sur le serveur, appelez `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="f53e0-150">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="f53e0-151">Lorsque la demande est terminée, le `.success()` ou `.error()` gestionnaire affiche un message approprié pour l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f53e0-151">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="f53e0-152">Envoi de Types simples</span><span class="sxs-lookup"><span data-stu-id="f53e0-152">Sending Simple Types</span></span>

<span data-ttu-id="f53e0-153">Dans les sections précédentes, nous avons envoyé un type complexe, API Web désérialisée en une instance d’une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="f53e0-153">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="f53e0-154">Vous pouvez également envoyer des types simples, telles qu’une chaîne.</span><span class="sxs-lookup"><span data-stu-id="f53e0-154">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="f53e0-155">Avant d’envoyer un type simple, envisagez d’encapsuler la valeur dans un type complexe à la place.</span><span class="sxs-lookup"><span data-stu-id="f53e0-155">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="f53e0-156">Cela vous offre les avantages de la validation du modèle sur le côté serveur et rend plus facile à étendre votre modèle, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="f53e0-156">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>

<span data-ttu-id="f53e0-157">Les étapes de base pour envoyer un type simple sont les mêmes, mais il existe deux différences subtiles.</span><span class="sxs-lookup"><span data-stu-id="f53e0-157">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="f53e0-158">Tout d’abord, dans le contrôleur, vous devez décorer le nom du paramètre avec le **FromBody** attribut.</span><span class="sxs-lookup"><span data-stu-id="f53e0-158">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="f53e0-159">Par défaut, les API Web tente d’obtenir des types simples à partir de l’URI de demande.</span><span class="sxs-lookup"><span data-stu-id="f53e0-159">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="f53e0-160">Le **FromBody** attribut indique à l’API Web pour lire la valeur à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="f53e0-160">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="f53e0-161">API Web lit le corps de réponse au maximum une fois, uniquement un seul paramètre d’une action peut provenir de corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="f53e0-161">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="f53e0-162">Si vous avez besoin obtenir plusieurs valeurs à partir du corps de demande, définir un type complexe.</span><span class="sxs-lookup"><span data-stu-id="f53e0-162">If you need to get multiple values from the request body, define a complex type.</span></span>

<span data-ttu-id="f53e0-163">En second lieu, le client doit envoyer la valeur au format suivant :</span><span class="sxs-lookup"><span data-stu-id="f53e0-163">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="f53e0-164">Plus précisément, la partie nom de la paire nom/valeur doit être vide pour un type simple.</span><span class="sxs-lookup"><span data-stu-id="f53e0-164">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="f53e0-165">Pas tous les navigateurs prennent en charge pour les formulaires HTML, mais vous créez ce format dans le script comme suit :</span><span class="sxs-lookup"><span data-stu-id="f53e0-165">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="f53e0-166">Voici un exemple de formulaire :</span><span class="sxs-lookup"><span data-stu-id="f53e0-166">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="f53e0-167">Et Voici le script pour envoyer la valeur de formulaire.</span><span class="sxs-lookup"><span data-stu-id="f53e0-167">And here is the script to submit the form value.</span></span> <span data-ttu-id="f53e0-168">La seule différence avec le script précédent est l’argument passé à la **valider** (fonction).</span><span class="sxs-lookup"><span data-stu-id="f53e0-168">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="f53e0-169">Vous pouvez utiliser la même approche pour envoyer un tableau de types simples :</span><span class="sxs-lookup"><span data-stu-id="f53e0-169">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="f53e0-170">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f53e0-170">Additional Resources</span></span>

[<span data-ttu-id="f53e0-171">Partie 2 : Chargement du fichier et MIME à parties multiples</span><span class="sxs-lookup"><span data-stu-id="f53e0-171">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
