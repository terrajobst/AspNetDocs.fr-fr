---
uid: web-api/overview/advanced/dependency-injection
title: L’Injection de dépendances dans ASP.NET Web API 2 - ASP.NET 4.x
author: MikeWasson
description: Ce didacticiel montre comment injecter des dépendances dans votre contrôleur d’API Web ASP.NET pour ASP.NET 4.x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 138ccb5800e801d382c11e3989ec3e3c074a79fe
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115702"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="ef098-103">Injection de dépendances dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="ef098-103">Dependency Injection in ASP.NET Web API 2</span></span>

<span data-ttu-id="ef098-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ef098-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ef098-105">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="ef098-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="ef098-106">Ce didacticiel montre comment injecter des dépendances dans votre contrôleur d’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ef098-106">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ef098-107">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="ef098-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ef098-108">Web API 2</span><span class="sxs-lookup"><span data-stu-id="ef098-108">Web API 2</span></span>
> - [<span data-ttu-id="ef098-109">Unity Application Block</span><span class="sxs-lookup"><span data-stu-id="ef098-109">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="ef098-110">Entity Framework 6 (version 5 fonctionne également)</span><span class="sxs-lookup"><span data-stu-id="ef098-110">Entity Framework 6 (version 5 also works)</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="ef098-111">Quelle est l’Injection de dépendances ?</span><span class="sxs-lookup"><span data-stu-id="ef098-111">What is Dependency Injection?</span></span>

<span data-ttu-id="ef098-112">Une *dépendance* est un objet qui nécessite un autre objet.</span><span class="sxs-lookup"><span data-stu-id="ef098-112">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="ef098-113">Par exemple, il est courant de définir un [référentiel](http://martinfowler.com/eaaCatalog/repository.html) qui gère l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="ef098-113">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="ef098-114">Nous allons illustrer avec un exemple.</span><span class="sxs-lookup"><span data-stu-id="ef098-114">Let's illustrate with an example.</span></span> <span data-ttu-id="ef098-115">Tout d’abord, nous allons définir un modèle de domaine :</span><span class="sxs-lookup"><span data-stu-id="ef098-115">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="ef098-116">Voici une classe de référentiel simple qui stocke des éléments dans une base de données, à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ef098-116">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="ef098-117">Maintenant, nous allons définir un contrôleur d’API Web qui prend en charge les requêtes GET `Product` entités.</span><span class="sxs-lookup"><span data-stu-id="ef098-117">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="ef098-118">(Je laisse les POST et les autres méthodes par souci de simplicité.) Voici une première tentative de :</span><span class="sxs-lookup"><span data-stu-id="ef098-118">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="ef098-119">Notez que la classe de contrôleur dépend `ProductRepository`, et nous allons laisser le contrôleur de créer le `ProductRepository` instance.</span><span class="sxs-lookup"><span data-stu-id="ef098-119">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="ef098-120">Toutefois, il est déconseillé de coder en dur la dépendance de cette façon, pour plusieurs raisons.</span><span class="sxs-lookup"><span data-stu-id="ef098-120">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="ef098-121">Si vous souhaitez remplacer `ProductRepository` avec une implémentation différente, vous devez également modifier la classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ef098-121">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="ef098-122">Si le `ProductRepository` possède des dépendances, vous devez configurer dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ef098-122">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="ef098-123">Pour un grand projet avec plusieurs contrôleurs, votre code de configuration devienne répartis sur votre projet.</span><span class="sxs-lookup"><span data-stu-id="ef098-123">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="ef098-124">Il est difficile de test unitaire, étant donné que le contrôleur est codé en dur pour interroger la base de données.</span><span class="sxs-lookup"><span data-stu-id="ef098-124">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="ef098-125">Pour un test unitaire, vous devez utiliser un référentiel simulacre ou stub, ce qui n’est pas possible avec la conception actuelle.</span><span class="sxs-lookup"><span data-stu-id="ef098-125">For a unit test, you should use a mock or stub repository, which is not possible with the current design.</span></span>

<span data-ttu-id="ef098-126">Nous pouvons résoudre ces problèmes en *injection* le référentiel dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ef098-126">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="ef098-127">Tout d’abord, refactoriser la `ProductRepository` classe dans une interface :</span><span class="sxs-lookup"><span data-stu-id="ef098-127">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="ef098-128">Puis fournissez le `IProductRepository` comme paramètre de constructeur :</span><span class="sxs-lookup"><span data-stu-id="ef098-128">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="ef098-129">Cet exemple utilise [l’injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="ef098-129">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="ef098-130">Vous pouvez également utiliser *l’injection de setter*vous permet de définir la dépendance par une méthode setter ou une propriété.</span><span class="sxs-lookup"><span data-stu-id="ef098-130">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="ef098-131">Mais maintenant il existe un problème, étant donné que votre application ne crée pas directement le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ef098-131">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="ef098-132">API Web crée le contrôleur lorsqu’il achemine la demande et API Web ne sait pas quoi que ce soit sur `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ef098-132">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="ef098-133">Il s’agit là qu’intervient le résolveur de dépendance d’API Web.</span><span class="sxs-lookup"><span data-stu-id="ef098-133">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="ef098-134">Le résolveur de dépendance d’API Web</span><span class="sxs-lookup"><span data-stu-id="ef098-134">The Web API Dependency Resolver</span></span>

<span data-ttu-id="ef098-135">API Web définit les **IDependencyResolver** interface pour la résolution des dépendances.</span><span class="sxs-lookup"><span data-stu-id="ef098-135">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="ef098-136">Voici la définition de l’interface :</span><span class="sxs-lookup"><span data-stu-id="ef098-136">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="ef098-137">Le **IDependencyScope** interface possède deux méthodes :</span><span class="sxs-lookup"><span data-stu-id="ef098-137">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="ef098-138">**GetService** crée une instance d’un type.</span><span class="sxs-lookup"><span data-stu-id="ef098-138">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="ef098-139">**GetServices** crée une collection d’objets d’un type spécifié.</span><span class="sxs-lookup"><span data-stu-id="ef098-139">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="ef098-140">Le **IDependencyResolver** hérite de la méthode **IDependencyScope** et ajoute le **BeginScope** (méthode).</span><span class="sxs-lookup"><span data-stu-id="ef098-140">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="ef098-141">Je parlerai étendues plus loin dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="ef098-141">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="ef098-142">Lors de l’API Web crée une instance de contrôleur, il appelle d’abord **IDependencyResolver.GetService**, en passant le type de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ef098-142">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="ef098-143">Vous pouvez utiliser ce hook d’extensibilité pour créer le contrôleur, résolution des dépendances.</span><span class="sxs-lookup"><span data-stu-id="ef098-143">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="ef098-144">Si **GetService** retourne null, recherche un constructeur sans paramètre sur la classe de contrôleur API Web.</span><span class="sxs-lookup"><span data-stu-id="ef098-144">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="ef098-145">Résolution des dépendances avec le conteneur Unity</span><span class="sxs-lookup"><span data-stu-id="ef098-145">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="ef098-146">Bien que vous pouvez écrire un complète **IDependencyResolver** implémentation à partir de zéro, l’interface est vraiment conçue pour agir en tant que pont entre les API Web et les conteneurs d’inversion de contrôle existants.</span><span class="sxs-lookup"><span data-stu-id="ef098-146">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="ef098-147">Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances.</span><span class="sxs-lookup"><span data-stu-id="ef098-147">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="ef098-148">Vous inscrivez des types auprès du conteneur et ensuite utilisez le conteneur pour créer des objets.</span><span class="sxs-lookup"><span data-stu-id="ef098-148">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="ef098-149">Le conteneur détermine automatiquement les relations de dépendance.</span><span class="sxs-lookup"><span data-stu-id="ef098-149">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="ef098-150">De nombreux conteneurs d’inversion de contrôle vous permettent également de vous permettent de contrôler des éléments tels que la durée de vie et la portée.</span><span class="sxs-lookup"><span data-stu-id="ef098-150">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="ef098-151">« IoC » est l’acronyme « d’inversion de contrôle », qui est un modèle général où un framework appelle du code d’application.</span><span class="sxs-lookup"><span data-stu-id="ef098-151">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="ef098-152">Un conteneur IoC construit vos objets, ce qui le flux habituel de contrôle « inverse ».</span><span class="sxs-lookup"><span data-stu-id="ef098-152">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

<span data-ttu-id="ef098-153">Pour ce didacticiel, nous allons utiliser [Unity](https://msdn.microsoft.com/library/ff647202.aspx) à partir de Microsoft Patterns &amp; pratiques.</span><span class="sxs-lookup"><span data-stu-id="ef098-153">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="ef098-154">(Incluent d’autres bibliothèques populaires [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), et [StructureMap ](http://structuremap.github.io/documentation/).) Vous pouvez utiliser le Gestionnaire de Package NuGet pour installer Unity.</span><span class="sxs-lookup"><span data-stu-id="ef098-154">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="ef098-155">À partir de la **outils** menu dans Visual Studio, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="ef098-155">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ef098-156">Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="ef098-156">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="ef098-157">Voici une implémentation de **IDependencyResolver** qui encapsule un conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="ef098-157">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="ef098-158">Si le **GetService** méthode ne peut pas résoudre un type, elle doit retourner **null**.</span><span class="sxs-lookup"><span data-stu-id="ef098-158">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="ef098-159">Si le **GetServices** méthode ne peut pas résoudre un type, elle doit retourner un objet de collection vide.</span><span class="sxs-lookup"><span data-stu-id="ef098-159">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="ef098-160">Ne pas lever d’exceptions pour les types inconnus.</span><span class="sxs-lookup"><span data-stu-id="ef098-160">Don't throw exceptions for unknown types.</span></span>

## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="ef098-161">Configurer le résolveur de dépendance</span><span class="sxs-lookup"><span data-stu-id="ef098-161">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="ef098-162">Définir le résolveur de dépendance sur le **DependencyResolver** propriété de global **HttpConfiguration** objet.</span><span class="sxs-lookup"><span data-stu-id="ef098-162">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="ef098-163">Le code suivant enregistre le `IProductRepository` interface avec Unity et crée ensuite un `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="ef098-163">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="ef098-164">Portée des dépendances et durée de vie de contrôleur</span><span class="sxs-lookup"><span data-stu-id="ef098-164">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="ef098-165">Les contrôleurs sont créés par demande.</span><span class="sxs-lookup"><span data-stu-id="ef098-165">Controllers are created per request.</span></span> <span data-ttu-id="ef098-166">Pour gérer les durées de vie des objets, **IDependencyResolver** utilise le concept d’un *étendue*.</span><span class="sxs-lookup"><span data-stu-id="ef098-166">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="ef098-167">Le résolveur de dépendance attachée à la **HttpConfiguration** objet a une portée globale.</span><span class="sxs-lookup"><span data-stu-id="ef098-167">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="ef098-168">Lors de l’API Web crée un contrôleur, il appelle **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="ef098-168">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="ef098-169">Cette méthode retourne un **IDependencyScope** qui représente une étendue enfant.</span><span class="sxs-lookup"><span data-stu-id="ef098-169">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="ef098-170">API Web appelle ensuite **GetService** sur l’étendue enfant pour créer le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ef098-170">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="ef098-171">Lors de la demande est terminée, les API Web appelle **Dispose** sur l’étendue enfant.</span><span class="sxs-lookup"><span data-stu-id="ef098-171">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="ef098-172">Utilisez le **Dispose** méthode pour supprimer les dépendances du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ef098-172">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="ef098-173">Comment implémenter **BeginScope** varie selon le conteneur IoC.</span><span class="sxs-lookup"><span data-stu-id="ef098-173">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="ef098-174">Pour Unity, étendue correspond à un conteneur enfant :</span><span class="sxs-lookup"><span data-stu-id="ef098-174">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="ef098-175">La plupart des conteneurs IoC ont des équivalents similaire.</span><span class="sxs-lookup"><span data-stu-id="ef098-175">Most IoC containers have similar equivalents.</span></span>
