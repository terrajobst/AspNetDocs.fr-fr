---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Création d’une actionC#() | Microsoft Docs
author: microsoft
description: Découvrez comment ajouter une nouvelle action à un contrôleur MVC ASP.NET. En savoir plus sur la configuration requise pour qu’une méthode soit une action.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: ebba935383819935ad85c95245666f4eaf6a0dca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582053"
---
# <a name="creating-an-action-c"></a><span data-ttu-id="99b89-104">Création d’une action (C#)</span><span class="sxs-lookup"><span data-stu-id="99b89-104">Creating an Action (C#)</span></span>

<span data-ttu-id="99b89-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="99b89-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="99b89-106">Découvrez comment ajouter une nouvelle action à un contrôleur MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="99b89-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="99b89-107">En savoir plus sur la configuration requise pour qu’une méthode soit une action.</span><span class="sxs-lookup"><span data-stu-id="99b89-107">Learn about the requirements for a method to be an action.</span></span>

<span data-ttu-id="99b89-108">L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer une nouvelle action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="99b89-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="99b89-109">Vous en saurez plus sur les exigences d’une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="99b89-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="99b89-110">Vous apprendrez également comment empêcher une méthode d’être exposée en tant qu’action.</span><span class="sxs-lookup"><span data-stu-id="99b89-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="99b89-111">Ajout d’une action à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="99b89-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="99b89-112">Pour ajouter une nouvelle action à un contrôleur, ajoutez une nouvelle méthode au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="99b89-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="99b89-113">Par exemple, le contrôleur de la liste 1 contient une action nommée index () et une action nommée SayHello ().</span><span class="sxs-lookup"><span data-stu-id="99b89-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="99b89-114">Les deux méthodes sont exposées en tant qu’actions.</span><span class="sxs-lookup"><span data-stu-id="99b89-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="99b89-115">**Liste 1-Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="99b89-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="99b89-116">Pour être exposé à l’univers en tant qu’action, une méthode doit répondre à certaines exigences :</span><span class="sxs-lookup"><span data-stu-id="99b89-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="99b89-117">La méthode doit être publique.</span><span class="sxs-lookup"><span data-stu-id="99b89-117">The method must be public.</span></span>
- <span data-ttu-id="99b89-118">La méthode ne peut pas être une méthode statique.</span><span class="sxs-lookup"><span data-stu-id="99b89-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="99b89-119">La méthode ne peut pas être une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="99b89-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="99b89-120">La méthode ne peut pas être un constructeur, un accesseur Get ou un accesseur Set.</span><span class="sxs-lookup"><span data-stu-id="99b89-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="99b89-121">La méthode ne peut pas avoir de types génériques ouverts.</span><span class="sxs-lookup"><span data-stu-id="99b89-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="99b89-122">La méthode n’est pas une méthode de la classe de base du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="99b89-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="99b89-123">La méthode ne peut pas contenir de paramètres **ref** ou **out** .</span><span class="sxs-lookup"><span data-stu-id="99b89-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="99b89-124">Notez qu’il n’existe aucune restriction sur le type de retour d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="99b89-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="99b89-125">Une action de contrôleur peut retourner une chaîne, une valeur DateTime, une instance de la classe Random ou void.</span><span class="sxs-lookup"><span data-stu-id="99b89-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="99b89-126">L’infrastructure MVC ASP.NET convertit tout type de retour qui n’est pas un résultat d’action en une chaîne et affiche la chaîne dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="99b89-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="99b89-127">Lorsque vous ajoutez une méthode qui ne viole pas ces exigences à un contrôleur, la méthode est exposée en tant qu’action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="99b89-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="99b89-128">Soyez prudent ici.</span><span class="sxs-lookup"><span data-stu-id="99b89-128">Be careful here.</span></span> <span data-ttu-id="99b89-129">Une action de contrôleur peut être appelée par toute personne connectée à Internet.</span><span class="sxs-lookup"><span data-stu-id="99b89-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="99b89-130">Ne créez pas, par exemple, une action de contrôleur DeleteMyWebsite ().</span><span class="sxs-lookup"><span data-stu-id="99b89-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="99b89-131">Empêcher l’appel d’une méthode publique</span><span class="sxs-lookup"><span data-stu-id="99b89-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="99b89-132">Si vous avez besoin de créer une méthode publique dans une classe de contrôleur et que vous ne souhaitez pas exposer la méthode en tant qu’action de contrôleur, vous pouvez empêcher la méthode d’être appelée à l’aide de l’attribut [non-action].</span><span class="sxs-lookup"><span data-stu-id="99b89-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="99b89-133">Par exemple, le contrôleur de la liste 2 contient une méthode publique nommée CompanySecrets () qui est décorée avec l’attribut [non-action].</span><span class="sxs-lookup"><span data-stu-id="99b89-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="99b89-134">**Liste 2-Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="99b89-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="99b89-135">Si vous tentez d’appeler l’action du contrôleur CompanySecrets () en tapant/Work/CompanySecrets dans la barre d’adresses de votre navigateur, vous obtiendrez le message d’erreur dans la figure 1.</span><span class="sxs-lookup"><span data-stu-id="99b89-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>

<span data-ttu-id="99b89-136">[![de l’appel d’une méthode qui n’est pas une action](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="99b89-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="99b89-137">**Figure 01**: appel d’une méthode non-action ([cliquez pour afficher l’image en taille réelle](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="99b89-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="99b89-138">[Précédent](creating-a-controller-cs.md)
> [Suivant](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="99b89-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
