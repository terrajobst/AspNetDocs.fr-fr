---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Création d’itinéraires personnalisésC#() | Microsoft Docs
author: microsoft
description: Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à modifier la table de routage par défaut dans le fichier global. asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 58f72e390f0053d136ef00ddbda0b071ba225d98
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601331"
---
# <a name="creating-custom-routes-c"></a><span data-ttu-id="eb54d-104">Création de routes personnalisées (C#)</span><span class="sxs-lookup"><span data-stu-id="eb54d-104">Creating Custom Routes (C#)</span></span>

<span data-ttu-id="eb54d-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="eb54d-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="eb54d-106">Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eb54d-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="eb54d-107">Dans ce didacticiel, vous allez apprendre à modifier la table de routage par défaut dans le fichier global. asax.</span><span class="sxs-lookup"><span data-stu-id="eb54d-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>

<span data-ttu-id="eb54d-108">Dans ce didacticiel, vous allez apprendre à ajouter un itinéraire personnalisé à une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eb54d-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="eb54d-109">Vous allez apprendre à modifier la table de routage par défaut dans le fichier global. asax avec un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="eb54d-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="eb54d-110">Pour de nombreuses applications ASP.NET MVC simples, la table de routage par défaut fonctionne parfaitement.</span><span class="sxs-lookup"><span data-stu-id="eb54d-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="eb54d-111">Toutefois, vous pouvez découvrir que vous avez des besoins de routage spécialisés.</span><span class="sxs-lookup"><span data-stu-id="eb54d-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="eb54d-112">Dans ce cas, vous pouvez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="eb54d-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="eb54d-113">Imaginez, par exemple, que vous créez une application de blog.</span><span class="sxs-lookup"><span data-stu-id="eb54d-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="eb54d-114">Vous souhaiterez peut-être gérer les demandes entrantes qui ressemblent à ceci :</span><span class="sxs-lookup"><span data-stu-id="eb54d-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="eb54d-115">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="eb54d-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="eb54d-116">Quand un utilisateur entre cette demande, vous souhaitez retourner l’entrée de blog qui correspond à la date 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="eb54d-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="eb54d-117">Pour gérer ce type de demande, vous devez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="eb54d-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="eb54d-118">Le fichier global. asax de la liste 1 contient un nouvel itinéraire personnalisé, nommé blog, qui gère les demandes qui ressemblent*à la date d’entrée*de/Archive/.</span><span class="sxs-lookup"><span data-stu-id="eb54d-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="eb54d-119">**Liste 1-global. asax (avec itinéraire personnalisé)**</span><span class="sxs-lookup"><span data-stu-id="eb54d-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="eb54d-120">L’ordre des itinéraires que vous ajoutez à la table de routage est important.</span><span class="sxs-lookup"><span data-stu-id="eb54d-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="eb54d-121">Notre nouvel itinéraire de blog personnalisé est ajouté avant l’itinéraire par défaut existant.</span><span class="sxs-lookup"><span data-stu-id="eb54d-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="eb54d-122">Si vous avez inversé l’ordre, l’itinéraire par défaut est toujours appelé à la place de l’itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="eb54d-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="eb54d-123">L’itinéraire de blog personnalisé correspond à toutes les demandes qui commencent par/Archive/.</span><span class="sxs-lookup"><span data-stu-id="eb54d-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="eb54d-124">Par conséquent, elle correspond à toutes les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="eb54d-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="eb54d-125">/Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="eb54d-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="eb54d-126">/Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="eb54d-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="eb54d-127">/Archive/apple</span><span class="sxs-lookup"><span data-stu-id="eb54d-127">/Archive/apple</span></span>

<span data-ttu-id="eb54d-128">L’itinéraire personnalisé mappe la requête entrante à un contrôleur nommé Archive et appelle l’action d’entrée ().</span><span class="sxs-lookup"><span data-stu-id="eb54d-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="eb54d-129">Lorsque la méthode Entry () est appelée, la date d’entrée est passée en tant que paramètre nommé entryDate.</span><span class="sxs-lookup"><span data-stu-id="eb54d-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="eb54d-130">Vous pouvez utiliser l’itinéraire personnalisé du blog avec le contrôleur dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="eb54d-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="eb54d-131">**Liste 2-ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="eb54d-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="eb54d-132">Notez que la méthode Entry () de la liste 2 accepte un paramètre de type DateTime.</span><span class="sxs-lookup"><span data-stu-id="eb54d-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="eb54d-133">L’infrastructure MVC est suffisamment intelligente pour convertir automatiquement la date d’entrée de l’URL en valeur DateTime.</span><span class="sxs-lookup"><span data-stu-id="eb54d-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="eb54d-134">Si le paramètre de date d’entrée de l’URL ne peut pas être converti en DateTime, une erreur est générée (voir la figure 1).</span><span class="sxs-lookup"><span data-stu-id="eb54d-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="eb54d-135">**Figure 1-erreur lors de la conversion du paramètre**</span><span class="sxs-lookup"><span data-stu-id="eb54d-135">**Figure 1 - Error from converting parameter**</span></span>

<span data-ttu-id="eb54d-136">[![la boîte de dialogue Nouveau projet](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eb54d-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="eb54d-137">**Figure 01**: erreur de conversion du paramètre ([cliquez pour afficher l’image en taille réelle](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="eb54d-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>

## <a name="summary"></a><span data-ttu-id="eb54d-138">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="eb54d-138">Summary</span></span>

<span data-ttu-id="eb54d-139">L’objectif de ce didacticiel était de montrer comment vous pouvez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="eb54d-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="eb54d-140">Vous avez appris à ajouter un itinéraire personnalisé à la table de routage dans le fichier global. asax qui représente les entrées de blog.</span><span class="sxs-lookup"><span data-stu-id="eb54d-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="eb54d-141">Nous avons vu comment mapper des demandes d’entrées de blog à un contrôleur nommé ArchiveController et une action de contrôleur nommée Entry ().</span><span class="sxs-lookup"><span data-stu-id="eb54d-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eb54d-142">[Précédent](aspnet-mvc-controllers-overview-cs.md)
> [Suivant](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="eb54d-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
