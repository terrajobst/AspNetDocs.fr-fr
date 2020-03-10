---
uid: web-api/overview/advanced/dependency-injection
title: Injection de dépendances dans API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: Ce didacticiel montre comment injecter des dépendances dans votre contrôleur de API Web ASP.NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622604"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="d0686-103">Injection de dépendances dans API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="d0686-103">Dependency Injection in ASP.NET Web API 2</span></span>

<span data-ttu-id="d0686-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d0686-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d0686-105">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="d0686-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="d0686-106">Ce didacticiel montre comment injecter des dépendances dans votre contrôleur de API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d0686-106">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d0686-107">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="d0686-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d0686-108">API Web 2</span><span class="sxs-lookup"><span data-stu-id="d0686-108">Web API 2</span></span>
> - [<span data-ttu-id="d0686-109">Bloc d’application Unity</span><span class="sxs-lookup"><span data-stu-id="d0686-109">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="d0686-110">Entity Framework 6 (la version 5 fonctionne également)</span><span class="sxs-lookup"><span data-stu-id="d0686-110">Entity Framework 6 (version 5 also works)</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="d0686-111">Qu’est-ce que l’injection de dépendance ?</span><span class="sxs-lookup"><span data-stu-id="d0686-111">What is Dependency Injection?</span></span>

<span data-ttu-id="d0686-112">Une *dépendance* est un objet qui nécessite un autre objet.</span><span class="sxs-lookup"><span data-stu-id="d0686-112">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="d0686-113">Par exemple, il est courant de définir un [référentiel](http://martinfowler.com/eaaCatalog/repository.html) qui gère l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="d0686-113">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="d0686-114">Prenons un exemple.</span><span class="sxs-lookup"><span data-stu-id="d0686-114">Let's illustrate with an example.</span></span> <span data-ttu-id="d0686-115">Tout d’abord, nous allons définir un modèle de domaine :</span><span class="sxs-lookup"><span data-stu-id="d0686-115">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="d0686-116">Voici une classe de référentiel simple qui stocke des éléments dans une base de données, à l’aide de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d0686-116">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="d0686-117">Nous allons maintenant définir un contrôleur d’API Web qui prend en charge les demandes d’extraction pour les entités `Product`.</span><span class="sxs-lookup"><span data-stu-id="d0686-117">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="d0686-118">(Je laisse la publication et d’autres méthodes pour plus de simplicité.) Voici une première tentative :</span><span class="sxs-lookup"><span data-stu-id="d0686-118">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="d0686-119">Notez que la classe Controller dépend de `ProductRepository`, et que nous autorisons le contrôleur à créer l’instance `ProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="d0686-119">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="d0686-120">Toutefois, il est déconseillé de coder en dur la dépendance de cette manière, pour plusieurs raisons.</span><span class="sxs-lookup"><span data-stu-id="d0686-120">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="d0686-121">Si vous souhaitez remplacer `ProductRepository` par une autre implémentation, vous devez également modifier la classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d0686-121">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="d0686-122">Si le `ProductRepository` a des dépendances, vous devez les configurer à l’intérieur du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d0686-122">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="d0686-123">Dans le cas d’un grand projet avec plusieurs contrôleurs, votre code de configuration est éparpillé dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="d0686-123">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="d0686-124">Il est difficile d’effectuer des tests unitaires, car le contrôleur est codé en dur pour interroger la base de données.</span><span class="sxs-lookup"><span data-stu-id="d0686-124">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="d0686-125">Pour un test unitaire, vous devez utiliser un référentiel fictif ou stub, ce qui n’est pas possible avec la conception actuelle.</span><span class="sxs-lookup"><span data-stu-id="d0686-125">For a unit test, you should use a mock or stub repository, which is not possible with the current design.</span></span>

<span data-ttu-id="d0686-126">Nous pouvons résoudre ces problèmes en *injectant* le référentiel dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d0686-126">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="d0686-127">Tout d’abord, refactorisez la classe `ProductRepository` dans une interface :</span><span class="sxs-lookup"><span data-stu-id="d0686-127">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="d0686-128">Fournissez ensuite le `IProductRepository` en tant que paramètre de constructeur :</span><span class="sxs-lookup"><span data-stu-id="d0686-128">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="d0686-129">Cet exemple utilise l' [injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="d0686-129">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="d0686-130">Vous pouvez également utiliser l' *injection d’accesseurs set*, où vous définissez la dépendance par le biais d’une méthode ou d’une propriété Setter.</span><span class="sxs-lookup"><span data-stu-id="d0686-130">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="d0686-131">Mais maintenant, il y a un problème, car votre application ne crée pas directement le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d0686-131">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="d0686-132">L’API Web crée le contrôleur lors de l’acheminement de la demande, et l’API Web ne sait rien sur `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="d0686-132">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="d0686-133">C’est là qu’intervient le programme de résolution des dépendances de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="d0686-133">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="d0686-134">Le programme de résolution des dépendances de l’API Web</span><span class="sxs-lookup"><span data-stu-id="d0686-134">The Web API Dependency Resolver</span></span>

<span data-ttu-id="d0686-135">L’API Web définit l’interface **IDependencyResolver** pour la résolution des dépendances.</span><span class="sxs-lookup"><span data-stu-id="d0686-135">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="d0686-136">Voici la définition de l’interface :</span><span class="sxs-lookup"><span data-stu-id="d0686-136">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="d0686-137">L’interface **IDependencyScope** a deux méthodes :</span><span class="sxs-lookup"><span data-stu-id="d0686-137">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="d0686-138">**GetService** crée une instance d’un type.</span><span class="sxs-lookup"><span data-stu-id="d0686-138">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="d0686-139">**GetServices** crée une collection d’objets d’un type spécifié.</span><span class="sxs-lookup"><span data-stu-id="d0686-139">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="d0686-140">La méthode **IDependencyResolver** hérite de **IDependencyScope** et ajoute la méthode **BeginScope** .</span><span class="sxs-lookup"><span data-stu-id="d0686-140">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="d0686-141">Nous parlerons des étendues plus loin dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="d0686-141">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="d0686-142">Lorsque l’API Web crée une instance de contrôleur, elle appelle d’abord **IDependencyResolver. GetService**, en passant le type de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d0686-142">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="d0686-143">Vous pouvez utiliser ce hook d’extensibilité pour créer le contrôleur, en résolvant toutes les dépendances.</span><span class="sxs-lookup"><span data-stu-id="d0686-143">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="d0686-144">Si **GetService** retourne null, l’API Web recherche un constructeur sans paramètre sur la classe Controller.</span><span class="sxs-lookup"><span data-stu-id="d0686-144">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="d0686-145">Résolution des dépendances avec le conteneur Unity</span><span class="sxs-lookup"><span data-stu-id="d0686-145">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="d0686-146">Bien que vous puissiez écrire une implémentation complète de **IDependencyResolver** à partir de zéro, l’interface est vraiment conçue pour agir en tant que pont entre l’API Web et les conteneurs IOC existants.</span><span class="sxs-lookup"><span data-stu-id="d0686-146">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="d0686-147">Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances.</span><span class="sxs-lookup"><span data-stu-id="d0686-147">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="d0686-148">Vous inscrivez les types avec le conteneur, puis vous utilisez le conteneur pour créer des objets.</span><span class="sxs-lookup"><span data-stu-id="d0686-148">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="d0686-149">Le conteneur détermine automatiquement les relations de dépendance.</span><span class="sxs-lookup"><span data-stu-id="d0686-149">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="d0686-150">De nombreux conteneurs IoC vous permettent également de contrôler des éléments tels que la durée de vie des objets et l’étendue.</span><span class="sxs-lookup"><span data-stu-id="d0686-150">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="d0686-151">« IoC » signifie « inversion de contrôle », qui est un modèle général dans lequel un Framework appelle le code d’application.</span><span class="sxs-lookup"><span data-stu-id="d0686-151">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="d0686-152">Un conteneur IoC construit vos objets pour vous, ce qui « inverse » le déroulement habituel du contrôle.</span><span class="sxs-lookup"><span data-stu-id="d0686-152">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

<span data-ttu-id="d0686-153">Pour ce didacticiel, nous allons utiliser [Unity](https://msdn.microsoft.com/library/ff647202.aspx) de Microsoft patterns &amp; Practices.</span><span class="sxs-lookup"><span data-stu-id="d0686-153">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="d0686-154">(D’autres bibliothèques populaires incluent [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)et [StructureMap](http://structuremap.github.io/documentation/).) Vous pouvez utiliser le gestionnaire de package NuGet pour installer Unity.</span><span class="sxs-lookup"><span data-stu-id="d0686-154">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="d0686-155">Dans le menu **Outils** de Visual Studio, sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="d0686-155">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d0686-156">Dans la fenêtre de la console du gestionnaire de package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d0686-156">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="d0686-157">Voici une implémentation de **IDependencyResolver** qui encapsule un conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="d0686-157">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="d0686-158">Si la méthode **GetService** ne peut pas résoudre un type, elle doit retourner la **valeur null**.</span><span class="sxs-lookup"><span data-stu-id="d0686-158">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="d0686-159">Si la méthode **GetServices** ne peut pas résoudre un type, elle doit retourner un objet de collection vide.</span><span class="sxs-lookup"><span data-stu-id="d0686-159">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="d0686-160">Ne levez pas d’exceptions pour les types inconnus.</span><span class="sxs-lookup"><span data-stu-id="d0686-160">Don't throw exceptions for unknown types.</span></span>

## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="d0686-161">Configuration du programme de résolution des dépendances</span><span class="sxs-lookup"><span data-stu-id="d0686-161">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="d0686-162">Définissez le résolveur de dépendances sur la propriété **DependencyResolver** de l’objet global **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="d0686-162">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="d0686-163">Le code suivant inscrit l’interface `IProductRepository` avec Unity, puis crée une `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="d0686-163">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="d0686-164">Durée de vie du contrôleur et de la portée de dépendance</span><span class="sxs-lookup"><span data-stu-id="d0686-164">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="d0686-165">Les contrôleurs sont créés par demande.</span><span class="sxs-lookup"><span data-stu-id="d0686-165">Controllers are created per request.</span></span> <span data-ttu-id="d0686-166">Pour gérer les durées de vie des objets, **IDependencyResolver** utilise le concept d' *étendue*.</span><span class="sxs-lookup"><span data-stu-id="d0686-166">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="d0686-167">Le programme de résolution des dépendances attaché à l’objet **HttpConfiguration** a une portée globale.</span><span class="sxs-lookup"><span data-stu-id="d0686-167">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="d0686-168">Lorsque l’API Web crée un contrôleur, elle appelle **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="d0686-168">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="d0686-169">Cette méthode retourne un **IDependencyScope** qui représente une portée enfant.</span><span class="sxs-lookup"><span data-stu-id="d0686-169">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="d0686-170">L’API Web appelle ensuite **GetService** sur l’étendue enfant pour créer le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d0686-170">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="d0686-171">Lorsque la demande est terminée, l’API Web appelle **dispose** sur l’étendue enfant.</span><span class="sxs-lookup"><span data-stu-id="d0686-171">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="d0686-172">Utilisez la méthode **dispose** pour supprimer les dépendances du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d0686-172">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="d0686-173">La façon dont vous implémentez **BeginScope** dépend du conteneur IoC.</span><span class="sxs-lookup"><span data-stu-id="d0686-173">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="d0686-174">Pour Unity, Scope correspond à un conteneur enfant :</span><span class="sxs-lookup"><span data-stu-id="d0686-174">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="d0686-175">La plupart des conteneurs IoC ont des équivalents similaires.</span><span class="sxs-lookup"><span data-stu-id="d0686-175">Most IoC containers have similar equivalents.</span></span>
