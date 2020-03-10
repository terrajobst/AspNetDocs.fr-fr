---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Envoi de données de formulaire HTML dans API Web ASP.NET : form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: Cet article explique comment poster des données form-urlencoded sur un contrôleur d’API Web avec ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557602"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="2c7ff-103">Envoi de données de formulaire HTML dans API Web ASP.NET : données form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="2c7ff-103">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>

<span data-ttu-id="2c7ff-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2c7ff-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="2c7ff-105">Partie 1 : données form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="2c7ff-105">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="2c7ff-106">Cet article explique comment poster des données form-urlencoded vers un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-106">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="2c7ff-107">Vue d’ensemble des formulaires HTML</span><span class="sxs-lookup"><span data-stu-id="2c7ff-107">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="2c7ff-108">Envoi de types complexes</span><span class="sxs-lookup"><span data-stu-id="2c7ff-108">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="2c7ff-109">Envoi de données de formulaire via AJAX</span><span class="sxs-lookup"><span data-stu-id="2c7ff-109">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="2c7ff-110">Envoi de types simples</span><span class="sxs-lookup"><span data-stu-id="2c7ff-110">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="2c7ff-111">[Téléchargez le projet terminé](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="2c7ff-111">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="2c7ff-112">Vue d’ensemble des formulaires HTML</span><span class="sxs-lookup"><span data-stu-id="2c7ff-112">Overview of HTML Forms</span></span>

<span data-ttu-id="2c7ff-113">Les formulaires HTML utilisent l’extraction ou la publication pour envoyer des données au serveur.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-113">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="2c7ff-114">L’attribut **Method** de l’élément **Form** fournit la méthode http :</span><span class="sxs-lookup"><span data-stu-id="2c7ff-114">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="2c7ff-115">La méthode par défaut est obtient.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-115">The default method is GET.</span></span> <span data-ttu-id="2c7ff-116">Si le formulaire utilise la fonction obtenir, les données de formulaire sont encodées dans l’URI en tant que chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-116">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="2c7ff-117">Si le formulaire utilise la publication, les données du formulaire sont placées dans le corps de la requête.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-117">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="2c7ff-118">Pour les données publiées, l’attribut **enctype** spécifie le format du corps de la demande :</span><span class="sxs-lookup"><span data-stu-id="2c7ff-118">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="2c7ff-119">enctype</span><span class="sxs-lookup"><span data-stu-id="2c7ff-119">enctype</span></span> | <span data-ttu-id="2c7ff-120">Description</span><span class="sxs-lookup"><span data-stu-id="2c7ff-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2c7ff-121">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="2c7ff-121">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="2c7ff-122">Les données de formulaire sont encodées sous forme de paires nom/valeur, comme dans le cas d’une chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-122">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="2c7ff-123">Il s’agit du format par défaut pour la publication.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-123">This is the default format for POST.</span></span> |
| <span data-ttu-id="2c7ff-124">multipart/formulaire-données</span><span class="sxs-lookup"><span data-stu-id="2c7ff-124">multipart/form-data</span></span> | <span data-ttu-id="2c7ff-125">Les données de formulaire sont encodées sous la forme d’un message MIME à parties multiples.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-125">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="2c7ff-126">Utilisez ce format si vous téléchargez un fichier sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-126">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="2c7ff-127">La première partie de cet article examine le format x-www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-127">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="2c7ff-128">La [partie 2](sending-html-form-data-part-2.md) décrit le MIME à parties multiples.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-128">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="2c7ff-129">Envoi de types complexes</span><span class="sxs-lookup"><span data-stu-id="2c7ff-129">Sending Complex Types</span></span>

<span data-ttu-id="2c7ff-130">En général, vous envoyez un type complexe, composé de valeurs extraites de plusieurs contrôles de formulaire.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-130">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="2c7ff-131">Prenons le modèle suivant qui représente une mise à jour d’État :</span><span class="sxs-lookup"><span data-stu-id="2c7ff-131">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="2c7ff-132">Voici un contrôleur d’API Web qui accepte un objet `Update` par le biais de la publication.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-132">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="2c7ff-133">Ce contrôleur utilise le [routage basé sur les actions](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), de sorte que le modèle de routage est &quot;API/{Controller}/{action}/{id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-133">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="2c7ff-134">Le client va poster les données vers &quot;&quot;/API/updates/Complex.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-134">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>

<span data-ttu-id="2c7ff-135">Nous allons maintenant écrire un formulaire HTML pour que les utilisateurs envoient une mise à jour d’État.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-135">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="2c7ff-136">Notez que l’attribut **action** du formulaire est l’URI de notre action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-136">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="2c7ff-137">Voici le formulaire avec des valeurs entrées dans :</span><span class="sxs-lookup"><span data-stu-id="2c7ff-137">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="2c7ff-138">Quand l’utilisateur clique sur envoyer, le navigateur envoie une requête HTTP semblable à la suivante :</span><span class="sxs-lookup"><span data-stu-id="2c7ff-138">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="2c7ff-139">Notez que le corps de la demande contient les données de formulaire, mises en forme en tant que paires nom/valeur.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-139">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="2c7ff-140">L’API Web convertit automatiquement les paires nom/valeur en une instance de la classe `Update`.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-140">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="2c7ff-141">Envoi de données de formulaire via AJAX</span><span class="sxs-lookup"><span data-stu-id="2c7ff-141">Sending Form Data via AJAX</span></span>

<span data-ttu-id="2c7ff-142">Lorsqu’un utilisateur envoie un formulaire, le navigateur quitte la page actuelle et restitue le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-142">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="2c7ff-143">C’est OK lorsque la réponse est une page HTML.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-143">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="2c7ff-144">Toutefois, avec une API Web, le corps de la réponse est généralement vide ou contient des données structurées, telles que JSON.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-144">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="2c7ff-145">Dans ce cas, il est plus logique d’envoyer les données de formulaire à l’aide d’une requête AJAX, afin que la page puisse traiter la réponse.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-145">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="2c7ff-146">Le code suivant montre comment poster des données de formulaire à l’aide de jQuery.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-146">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="2c7ff-147">La fonction jQuery **Submit** remplace l’action de formulaire par une nouvelle fonction.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-147">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="2c7ff-148">Cela remplace le comportement par défaut du bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-148">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="2c7ff-149">La fonction **Serialize** sérialise les données de formulaire dans des paires nom/valeur.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-149">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="2c7ff-150">Pour envoyer les données de formulaire au serveur, appelez `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-150">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="2c7ff-151">Une fois la demande terminée, le gestionnaire de `.success()` ou de `.error()` affiche un message approprié à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-151">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="2c7ff-152">Envoi de types simples</span><span class="sxs-lookup"><span data-stu-id="2c7ff-152">Sending Simple Types</span></span>

<span data-ttu-id="2c7ff-153">Dans les sections précédentes, nous avons envoyé un type complexe, que l’API Web désérialisée en une instance d’une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-153">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="2c7ff-154">Vous pouvez également envoyer des types simples, tels qu’une chaîne.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-154">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="2c7ff-155">Avant d’envoyer un type simple, encapsulez la valeur dans un type complexe à la place.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-155">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="2c7ff-156">Cela vous donne les avantages de la validation de modèle côté serveur, et facilite l’extension de votre modèle si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-156">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>

<span data-ttu-id="2c7ff-157">Les étapes de base pour envoyer un type simple sont identiques, mais il existe deux légères différences.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-157">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="2c7ff-158">Tout d’abord, dans le contrôleur, vous devez décorer le nom du paramètre avec l’attribut **FromBody** .</span><span class="sxs-lookup"><span data-stu-id="2c7ff-158">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="2c7ff-159">Par défaut, l’API Web tente d’obtenir des types simples à partir de l’URI de la demande.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-159">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="2c7ff-160">L’attribut **FromBody** indique à l’API Web de lire la valeur du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-160">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="2c7ff-161">L’API Web lit le corps de la réponse au plus une fois, de sorte qu’un seul paramètre d’une action peut provenir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-161">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="2c7ff-162">Si vous devez obtenir plusieurs valeurs à partir du corps de la demande, définissez un type complexe.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-162">If you need to get multiple values from the request body, define a complex type.</span></span>

<span data-ttu-id="2c7ff-163">Deuxièmement, le client doit envoyer la valeur au format suivant :</span><span class="sxs-lookup"><span data-stu-id="2c7ff-163">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="2c7ff-164">Plus précisément, la partie nom de la paire nom/valeur doit être vide pour un type simple.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-164">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="2c7ff-165">Tous les navigateurs ne prennent pas en charge cette fonction pour les formulaires HTML, mais vous créez ce format dans le script comme suit :</span><span class="sxs-lookup"><span data-stu-id="2c7ff-165">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="2c7ff-166">Voici un exemple de formulaire :</span><span class="sxs-lookup"><span data-stu-id="2c7ff-166">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="2c7ff-167">Et voici le script pour envoyer la valeur du formulaire.</span><span class="sxs-lookup"><span data-stu-id="2c7ff-167">And here is the script to submit the form value.</span></span> <span data-ttu-id="2c7ff-168">La seule différence par rapport au script précédent est l’argument passé dans la fonction de **publication** .</span><span class="sxs-lookup"><span data-stu-id="2c7ff-168">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="2c7ff-169">Vous pouvez utiliser la même approche pour envoyer un tableau de types simples :</span><span class="sxs-lookup"><span data-stu-id="2c7ff-169">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="2c7ff-170">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2c7ff-170">Additional Resources</span></span>

[<span data-ttu-id="2c7ff-171">Partie 2 : Téléchargement de fichiers et MIME à parties multiples</span><span class="sxs-lookup"><span data-stu-id="2c7ff-171">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
