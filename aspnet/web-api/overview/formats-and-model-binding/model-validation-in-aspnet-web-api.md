---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validation de modèle dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Vue d’ensemble de la validation de modèle dans API Web ASP.NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557238"
---
# <a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="32de2-103">Validation de modèle dans API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="32de2-103">Model Validation in ASP.NET Web API</span></span>

<span data-ttu-id="32de2-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="32de2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="32de2-105">Cet article explique comment annoter vos modèles, utiliser les annotations pour la validation des données et gérer les erreurs de validation dans votre API Web.</span><span class="sxs-lookup"><span data-stu-id="32de2-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span> <span data-ttu-id="32de2-106">Lorsqu’un client envoie des données à votre API Web, vous devez souvent valider les données avant d’effectuer un traitement.</span><span class="sxs-lookup"><span data-stu-id="32de2-106">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> 

## <a name="data-annotations"></a><span data-ttu-id="32de2-107">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="32de2-107">Data Annotations</span></span>

<span data-ttu-id="32de2-108">Dans API Web ASP.NET, vous pouvez utiliser les attributs de l’espace de noms [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) pour définir des règles de validation pour les propriétés de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="32de2-108">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="32de2-109">Considérez le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="32de2-109">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="32de2-110">Si vous avez utilisé la validation de modèle dans ASP.NET MVC, cela devrait vous paraître familier.</span><span class="sxs-lookup"><span data-stu-id="32de2-110">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="32de2-111">L’attribut **Required** indique que la propriété `Name` ne doit pas être null.</span><span class="sxs-lookup"><span data-stu-id="32de2-111">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="32de2-112">L’attribut de **plage** indique que `Weight` doit être compris entre zéro et 999.</span><span class="sxs-lookup"><span data-stu-id="32de2-112">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="32de2-113">Supposons qu’un client envoie une demande de publication avec la représentation JSON suivante :</span><span class="sxs-lookup"><span data-stu-id="32de2-113">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="32de2-114">Vous pouvez voir que le client n’a pas inclus la propriété `Name`, qui est marquée comme étant obligatoire.</span><span class="sxs-lookup"><span data-stu-id="32de2-114">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="32de2-115">Lorsque l’API Web convertit le JSON en instance `Product`, il valide le `Product` par rapport aux attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="32de2-115">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="32de2-116">Dans votre action de contrôleur, vous pouvez vérifier si le modèle est valide :</span><span class="sxs-lookup"><span data-stu-id="32de2-116">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="32de2-117">La validation de modèle ne garantit pas la sécurité des données du client.</span><span class="sxs-lookup"><span data-stu-id="32de2-117">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="32de2-118">Une validation supplémentaire peut être nécessaire dans d’autres couches de l’application.</span><span class="sxs-lookup"><span data-stu-id="32de2-118">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="32de2-119">(Par exemple, la couche de données peut appliquer des contraintes de clé étrangère.) Le didacticiel [sur l’utilisation de l’API Web avec Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explore certains de ces problèmes.</span><span class="sxs-lookup"><span data-stu-id="32de2-119">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="32de2-120">**« Sous-publication »** : sous-publication se produit lorsque le client quitte certaines propriétés.</span><span class="sxs-lookup"><span data-stu-id="32de2-120">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="32de2-121">Par exemple, supposons que le client envoie les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="32de2-121">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="32de2-122">Ici, le client n’a pas spécifié de valeurs pour `Price` ou `Weight`.</span><span class="sxs-lookup"><span data-stu-id="32de2-122">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="32de2-123">Le module de formatage JSON assigne une valeur par défaut de zéro aux propriétés manquantes.</span><span class="sxs-lookup"><span data-stu-id="32de2-123">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="32de2-124">L’état du modèle est valide, car zéro est une valeur valide pour ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="32de2-124">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="32de2-125">Le fait qu’il s’agisse d’un problème dépend de votre scénario.</span><span class="sxs-lookup"><span data-stu-id="32de2-125">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="32de2-126">Par exemple, dans une opération de mise à jour, vous souhaiterez peut-être faire la distinction entre « zéro » et « non défini ».</span><span class="sxs-lookup"><span data-stu-id="32de2-126">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="32de2-127">Pour forcer les clients à définir une valeur, rendez la propriété Nullable et définissez l’attribut **requis** :</span><span class="sxs-lookup"><span data-stu-id="32de2-127">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="32de2-128">**« Dépassement de la publication »** : un client peut également envoyer *plus* de données que prévu.</span><span class="sxs-lookup"><span data-stu-id="32de2-128">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="32de2-129">Exemple :</span><span class="sxs-lookup"><span data-stu-id="32de2-129">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="32de2-130">Ici, le JSON comprend une propriété (« Color ») qui n’existe pas dans le modèle `Product`.</span><span class="sxs-lookup"><span data-stu-id="32de2-130">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="32de2-131">Dans ce cas, le module de formatage JSON ignore simplement cette valeur.</span><span class="sxs-lookup"><span data-stu-id="32de2-131">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="32de2-132">(Le formateur XML fait de même.) La survalidation entraîne des problèmes si votre modèle a des propriétés que vous souhaitez être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="32de2-132">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="32de2-133">Exemple :</span><span class="sxs-lookup"><span data-stu-id="32de2-133">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="32de2-134">Vous ne voulez pas que les utilisateurs mettent à jour la propriété `IsAdmin` et s’élèvent aux administrateurs !</span><span class="sxs-lookup"><span data-stu-id="32de2-134">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="32de2-135">La stratégie la plus sûre consiste à utiliser une classe de modèle qui correspond exactement à ce que le client est autorisé à envoyer :</span><span class="sxs-lookup"><span data-stu-id="32de2-135">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="32de2-136">Le billet de blog de Brad Wilson «[validation des entrées et validation de modèle dans ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)» présente une bonne présentation de la sous-publication et de la survalidation.</span><span class="sxs-lookup"><span data-stu-id="32de2-136">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="32de2-137">Bien que la publication concerne ASP.NET MVC 2, les problèmes sont toujours pertinents pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="32de2-137">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>

## <a name="handling-validation-errors"></a><span data-ttu-id="32de2-138">Gestion des erreurs de validation</span><span class="sxs-lookup"><span data-stu-id="32de2-138">Handling Validation Errors</span></span>

<span data-ttu-id="32de2-139">L’API Web ne renvoie pas automatiquement une erreur au client lorsque la validation échoue.</span><span class="sxs-lookup"><span data-stu-id="32de2-139">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="32de2-140">Il revient à l’action du contrôleur de vérifier l’état du modèle et de répondre de manière appropriée.</span><span class="sxs-lookup"><span data-stu-id="32de2-140">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="32de2-141">Vous pouvez également créer un filtre d’action pour vérifier l’état du modèle avant l’appel de l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="32de2-141">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="32de2-142">Le code suivant montre un exemple :</span><span class="sxs-lookup"><span data-stu-id="32de2-142">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="32de2-143">Si la validation du modèle échoue, ce filtre retourne une réponse HTTP qui contient les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="32de2-143">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="32de2-144">Dans ce cas, l’action du contrôleur n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="32de2-144">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="32de2-145">Pour appliquer ce filtre à tous les contrôleurs d’API Web, ajoutez une instance du filtre à la collection **HttpConfiguration. filters** au cours de la configuration :</span><span class="sxs-lookup"><span data-stu-id="32de2-145">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="32de2-146">Une autre option consiste à définir le filtre en tant qu’attribut sur des contrôleurs ou des actions de contrôleur individuels :</span><span class="sxs-lookup"><span data-stu-id="32de2-146">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
