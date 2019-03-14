---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Vues fortement typées Fortement typées vues | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: bd00fccc44019b2e401247de1532d2abcb5dd396
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57057016"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="75aa2-103">Vues fortement typées</span><span class="sxs-lookup"><span data-stu-id="75aa2-103">Dynamic v.</span></span> <span data-ttu-id="75aa2-104">et vues dynamiques</span><span class="sxs-lookup"><span data-stu-id="75aa2-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="75aa2-105">par [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="75aa2-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="75aa2-106">Il existe trois façons pour passer des informations à partir d’un contrôleur à une vue dans ASP.NET MVC 3 :</span><span class="sxs-lookup"><span data-stu-id="75aa2-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="75aa2-107">En tant qu’objet modèle fortement typé.</span><span class="sxs-lookup"><span data-stu-id="75aa2-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="75aa2-108">Comme un type dynamique (à l’aide de @model dynamique)</span><span class="sxs-lookup"><span data-stu-id="75aa2-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="75aa2-109">À l’aide de ViewBag</span><span class="sxs-lookup"><span data-stu-id="75aa2-109">Using the ViewBag</span></span>

<span data-ttu-id="75aa2-110">J’ai écrit une application MVC 3 haut Blog simple de comparer ou différencier des vues dynamiques et fortement typées.</span><span class="sxs-lookup"><span data-stu-id="75aa2-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="75aa2-111">Le contrôleur commence par une liste de blogs simple :</span><span class="sxs-lookup"><span data-stu-id="75aa2-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="75aa2-112">Cliquez avec le bouton droit dans la méthode IndexNotStonglyTyped() et ajouter une vue Razor.</span><span class="sxs-lookup"><span data-stu-id="75aa2-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="75aa2-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="75aa2-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="75aa2-114">Assurez-vous que le **créer une vue fortement typée** case n’est pas cochée.</span><span class="sxs-lookup"><span data-stu-id="75aa2-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="75aa2-115">La vue résultante ne contient pas une grande partie :</span><span class="sxs-lookup"><span data-stu-id="75aa2-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="75aa2-116">Étant donné que nous utilisons une dynamique et non une vue fortement typée, intellisense ne nous aider à.</span><span class="sxs-lookup"><span data-stu-id="75aa2-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="75aa2-117">Le code complet est indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="75aa2-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="75aa2-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="75aa2-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="75aa2-119">Nous allons maintenant ajouter une vue fortement typée.</span><span class="sxs-lookup"><span data-stu-id="75aa2-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="75aa2-120">Ajoutez le code suivant pour le contrôleur :</span><span class="sxs-lookup"><span data-stu-id="75aa2-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="75aa2-121">Notez qu’il est exactement le View(topBlogs) retour même ; appeler en tant que la vue non fortement typée.</span><span class="sxs-lookup"><span data-stu-id="75aa2-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="75aa2-122">Cliquez avec le bouton droit à l’intérieur de *StonglyTypedIndex()* et sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="75aa2-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="75aa2-123">Cette fois-ci, sélectionnez le **Blog** classe de modèle et sélectionnez **liste** en tant que le modèle de structure.</span><span class="sxs-lookup"><span data-stu-id="75aa2-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="75aa2-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="75aa2-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="75aa2-125">Dans le nouveau modèle de vue, nous obtenons prise en charge intellisense.</span><span class="sxs-lookup"><span data-stu-id="75aa2-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="75aa2-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="75aa2-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="75aa2-127">Le projet c# peut être téléchargé [ici](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="75aa2-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
