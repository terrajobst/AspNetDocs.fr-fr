---
uid: signalr/overview/older-versions/dependency-injection
title: Injection de dépendances dans Signalr 1. x | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: de838ab6b3a299eb1e5ebeb9fa3c583478ce3e56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536952"
---
# <a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="824f1-102">Injection de dépendances dans SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="824f1-102">Dependency Injection in SignalR 1.x</span></span>

<span data-ttu-id="824f1-103">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="824f1-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="824f1-104">L’injection de dépendances est un moyen de supprimer les dépendances codées en dur entre les objets, ce qui facilite le remplacement des dépendances d’un objet, soit pour le test (à l’aide d’objets factices), soit pour modifier le comportement au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="824f1-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="824f1-105">Ce didacticiel montre comment effectuer une injection de dépendances sur les concentrateurs Signalr.</span><span class="sxs-lookup"><span data-stu-id="824f1-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="824f1-106">Il montre également comment utiliser des conteneurs IoC avec Signalr.</span><span class="sxs-lookup"><span data-stu-id="824f1-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="824f1-107">Un conteneur IoC est un Framework général pour l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="824f1-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="824f1-108">Qu’est-ce que l’injection de dépendance ?</span><span class="sxs-lookup"><span data-stu-id="824f1-108">What is Dependency Injection?</span></span>

<span data-ttu-id="824f1-109">Ignorez cette section si vous êtes déjà familiarisé avec l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="824f1-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="824f1-110">L' *injection de dépendances* (di) est un modèle dans lequel les objets ne sont pas responsables de la création de leurs propres dépendances.</span><span class="sxs-lookup"><span data-stu-id="824f1-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="824f1-111">Voici un exemple simple pour motiver DI.</span><span class="sxs-lookup"><span data-stu-id="824f1-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="824f1-112">Supposons que vous ayez un objet qui doit consigner des messages.</span><span class="sxs-lookup"><span data-stu-id="824f1-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="824f1-113">Vous pouvez définir une interface de journalisation :</span><span class="sxs-lookup"><span data-stu-id="824f1-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="824f1-114">Dans votre objet, vous pouvez créer un `ILogger` pour enregistrer les messages :</span><span class="sxs-lookup"><span data-stu-id="824f1-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="824f1-115">Cela fonctionne, mais ce n’est pas la meilleure conception.</span><span class="sxs-lookup"><span data-stu-id="824f1-115">This works, but it's not the best design.</span></span> <span data-ttu-id="824f1-116">Si vous souhaitez remplacer `FileLogger` par une autre implémentation de `ILogger`, vous devez modifier `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="824f1-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="824f1-117">Supposons que de nombreux autres objets utilisent `FileLogger`, vous devrez les modifier tous.</span><span class="sxs-lookup"><span data-stu-id="824f1-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="824f1-118">Ou si vous décidez de créer `FileLogger` un singleton, vous devrez également apporter des modifications à l’ensemble de l’application.</span><span class="sxs-lookup"><span data-stu-id="824f1-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="824f1-119">Une meilleure approche consiste à « injecter » un `ILogger` dans l’objet, par exemple, à l’aide d’un argument de constructeur :</span><span class="sxs-lookup"><span data-stu-id="824f1-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="824f1-120">À présent, l’objet n’est pas responsable de la sélection des `ILogger` à utiliser.</span><span class="sxs-lookup"><span data-stu-id="824f1-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="824f1-121">Vous pouvez basculer `ILogger` implémentations sans modifier les objets qui en dépendent.</span><span class="sxs-lookup"><span data-stu-id="824f1-121">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="824f1-122">Ce modèle est appelé [injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="824f1-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="824f1-123">Un autre modèle est l’injection d’accesseurs set, où vous définissez la dépendance par le biais d’une méthode ou d’une propriété Setter.</span><span class="sxs-lookup"><span data-stu-id="824f1-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="824f1-124">Injection de dépendances simple dans Signalr</span><span class="sxs-lookup"><span data-stu-id="824f1-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="824f1-125">Prenons l’exemple de l’application conversation du didacticiel [prise en main avec signalr](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="824f1-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="824f1-126">Voici la classe de concentrateur de cette application :</span><span class="sxs-lookup"><span data-stu-id="824f1-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="824f1-127">Supposons que vous souhaitez stocker les messages de conversation sur le serveur avant de les envoyer.</span><span class="sxs-lookup"><span data-stu-id="824f1-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="824f1-128">Vous pouvez définir une interface qui soustrait cette fonctionnalité et utiliser DI pour injecter l’interface dans la classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="824f1-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="824f1-129">Le seul problème est qu’une application Signalr ne crée pas directement de concentrateurs ; Signalr les crée pour vous.</span><span class="sxs-lookup"><span data-stu-id="824f1-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="824f1-130">Par défaut, Signalr attend qu’une classe de concentrateur ait un constructeur sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="824f1-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="824f1-131">Toutefois, vous pouvez facilement inscrire une fonction pour créer des instances de Hub et utiliser cette fonction pour exécuter DI.</span><span class="sxs-lookup"><span data-stu-id="824f1-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="824f1-132">Inscrivez la fonction en appelant **GlobalHost. DependencyResolver. Register**.</span><span class="sxs-lookup"><span data-stu-id="824f1-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="824f1-133">Désormais, Signalr appellera cette fonction anonyme chaque fois qu’elle a besoin de créer une instance `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="824f1-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="824f1-134">Conteneurs IoC</span><span class="sxs-lookup"><span data-stu-id="824f1-134">IoC Containers</span></span>

<span data-ttu-id="824f1-135">Le code précédent est parfait pour les cas simples.</span><span class="sxs-lookup"><span data-stu-id="824f1-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="824f1-136">Toutefois, vous deviez toujours écrire ceci :</span><span class="sxs-lookup"><span data-stu-id="824f1-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="824f1-137">Dans une application complexe avec de nombreuses dépendances, vous devrez peut-être écrire un grand nombre de ce code de « câblage ».</span><span class="sxs-lookup"><span data-stu-id="824f1-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="824f1-138">Ce code peut être difficile à gérer, en particulier si les dépendances sont imbriquées.</span><span class="sxs-lookup"><span data-stu-id="824f1-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="824f1-139">Il est également difficile à effectuer des tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="824f1-139">It is also hard to unit test.</span></span>

<span data-ttu-id="824f1-140">Une solution consiste à utiliser un conteneur IoC.</span><span class="sxs-lookup"><span data-stu-id="824f1-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="824f1-141">Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances. Vous inscrivez les types avec le conteneur, puis vous utilisez le conteneur pour créer des objets.</span><span class="sxs-lookup"><span data-stu-id="824f1-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="824f1-142">Le conteneur détermine automatiquement les relations de dépendance.</span><span class="sxs-lookup"><span data-stu-id="824f1-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="824f1-143">De nombreux conteneurs IoC vous permettent également de contrôler des éléments tels que la durée de vie des objets et l’étendue.</span><span class="sxs-lookup"><span data-stu-id="824f1-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="824f1-144">« IoC » signifie « inversion de contrôle », qui est un modèle général dans lequel un Framework appelle le code d’application.</span><span class="sxs-lookup"><span data-stu-id="824f1-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="824f1-145">Un conteneur IoC construit vos objets pour vous, ce qui « inverse » le déroulement habituel du contrôle.</span><span class="sxs-lookup"><span data-stu-id="824f1-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="824f1-146">Utilisation de conteneurs IoC dans Signalr</span><span class="sxs-lookup"><span data-stu-id="824f1-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="824f1-147">L’application de conversation est probablement trop simple pour tirer parti d’un conteneur IoC.</span><span class="sxs-lookup"><span data-stu-id="824f1-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="824f1-148">Examinons plutôt l’exemple [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .</span><span class="sxs-lookup"><span data-stu-id="824f1-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="824f1-149">L’exemple StockTicker définit deux classes principales :</span><span class="sxs-lookup"><span data-stu-id="824f1-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="824f1-150">`StockTickerHub`: la classe de concentrateur, qui gère les connexions clientes.</span><span class="sxs-lookup"><span data-stu-id="824f1-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="824f1-151">`StockTicker`: singleton qui contient des cours boursiers et les met à jour périodiquement.</span><span class="sxs-lookup"><span data-stu-id="824f1-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="824f1-152">`StockTickerHub` contient une référence au Singleton `StockTicker`, tandis que `StockTicker` contient une référence à **IHubConnectionContext** pour le `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="824f1-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="824f1-153">Elle utilise cette interface pour communiquer avec les instances de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="824f1-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="824f1-154">(Pour plus d’informations, consultez diffusion sur le [serveur avec ASP.net signalr](index.md).)</span><span class="sxs-lookup"><span data-stu-id="824f1-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="824f1-155">Nous pouvons utiliser un conteneur IoC pour démêler ces dépendances un peu plus.</span><span class="sxs-lookup"><span data-stu-id="824f1-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="824f1-156">Tout d’abord, nous allons simplifier les classes `StockTickerHub` et `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="824f1-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="824f1-157">Dans le code suivant, j’ai commenté les parties dont nous n’avons pas besoin.</span><span class="sxs-lookup"><span data-stu-id="824f1-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="824f1-158">Supprimez le constructeur sans paramètre de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="824f1-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="824f1-159">Au lieu de cela, nous utiliserons toujours DI pour créer le Hub.</span><span class="sxs-lookup"><span data-stu-id="824f1-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="824f1-160">Pour StockTicker, supprimez l’instance singleton.</span><span class="sxs-lookup"><span data-stu-id="824f1-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="824f1-161">Plus tard, nous allons utiliser le conteneur IoC pour contrôler la durée de vie de StockTicker.</span><span class="sxs-lookup"><span data-stu-id="824f1-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="824f1-162">En outre, rendez le constructeur public.</span><span class="sxs-lookup"><span data-stu-id="824f1-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="824f1-163">Ensuite, nous pouvons Refactoriser le code en créant une interface pour `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="824f1-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="824f1-164">Nous allons utiliser cette interface pour découpler le `StockTickerHub` de la classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="824f1-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="824f1-165">Visual Studio facilite ce type de refactorisation.</span><span class="sxs-lookup"><span data-stu-id="824f1-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="824f1-166">Ouvrez le fichier StockTicker.cs, cliquez avec le bouton droit sur la déclaration de classe `StockTicker`, puis sélectionnez **Refactoriser** ... **Extraire l’interface**.</span><span class="sxs-lookup"><span data-stu-id="824f1-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="824f1-167">Dans la boîte de dialogue **extraire l’interface** , cliquez sur **Sélectionner tout**.</span><span class="sxs-lookup"><span data-stu-id="824f1-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="824f1-168">Conservez les autres valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="824f1-168">Leave the other defaults.</span></span> <span data-ttu-id="824f1-169">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="824f1-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="824f1-170">Visual Studio crée une nouvelle interface nommée `IStockTicker`et modifie également `StockTicker` pour dériver de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="824f1-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="824f1-171">Ouvrez le fichier IStockTicker.cs et remplacez l’interface par **public**.</span><span class="sxs-lookup"><span data-stu-id="824f1-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="824f1-172">Dans la classe `StockTickerHub`, modifiez les deux instances de `StockTicker` en `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="824f1-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="824f1-173">La création d’une interface `IStockTicker` n’est pas strictement nécessaire, mais je voulais montrer comment DI peut aider à réduire le couplage entre les composants de votre application.</span><span class="sxs-lookup"><span data-stu-id="824f1-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="824f1-174">Ajouter la bibliothèque Ninject</span><span class="sxs-lookup"><span data-stu-id="824f1-174">Add the Ninject Library</span></span>

<span data-ttu-id="824f1-175">Il existe de nombreux conteneurs IoC Open source pour .NET.</span><span class="sxs-lookup"><span data-stu-id="824f1-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="824f1-176">Pour ce didacticiel, je vais utiliser [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="824f1-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="824f1-177">(D’autres bibliothèques populaires incluent [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)et [StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="824f1-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="824f1-178">Utilisez le gestionnaire de package NuGet pour installer la [bibliothèque Ninject](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="824f1-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="824f1-179">Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** > **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="824f1-179">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="824f1-180">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="824f1-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="824f1-181">Remplacer le programme de résolution de dépendance Signalr</span><span class="sxs-lookup"><span data-stu-id="824f1-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="824f1-182">Pour utiliser Ninject dans Signalr, créez une classe qui dérive de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="824f1-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="824f1-183">Cette classe remplace les méthodes **GetService** et **GetServices** de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="824f1-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="824f1-184">Signalr appelle ces méthodes pour créer divers objets au moment de l’exécution, y compris les instances de concentrateur, ainsi que divers services utilisés en interne par Signalr.</span><span class="sxs-lookup"><span data-stu-id="824f1-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="824f1-185">La méthode **GetService** crée une seule instance d’un type.</span><span class="sxs-lookup"><span data-stu-id="824f1-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="824f1-186">Substituez cette méthode pour appeler la méthode **opération TryGet** du noyau Ninject.</span><span class="sxs-lookup"><span data-stu-id="824f1-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="824f1-187">Si cette méthode retourne la valeur null, revient au résolveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="824f1-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="824f1-188">La méthode **GetServices** crée une collection d’objets d’un type spécifié.</span><span class="sxs-lookup"><span data-stu-id="824f1-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="824f1-189">Substituez cette méthode pour concaténer les résultats de Ninject avec les résultats du programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="824f1-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="824f1-190">Configurer des liaisons Ninject</span><span class="sxs-lookup"><span data-stu-id="824f1-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="824f1-191">À présent, nous allons utiliser Ninject pour déclarer des liaisons de type.</span><span class="sxs-lookup"><span data-stu-id="824f1-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="824f1-192">Ouvrez le fichier RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="824f1-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="824f1-193">Dans la méthode `RegisterHubs.Start`, créez le conteneur Ninject, qui Ninject appelle le *noyau*.</span><span class="sxs-lookup"><span data-stu-id="824f1-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="824f1-194">Créez une instance de notre résolveur de dépendance personnalisé :</span><span class="sxs-lookup"><span data-stu-id="824f1-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="824f1-195">Créez une liaison pour `IStockTicker` comme suit :</span><span class="sxs-lookup"><span data-stu-id="824f1-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="824f1-196">Ce code dit deux choses.</span><span class="sxs-lookup"><span data-stu-id="824f1-196">This code is saying two things.</span></span> <span data-ttu-id="824f1-197">Tout d’abord, chaque fois que l’application a besoin d’une `IStockTicker`, le noyau doit créer une instance de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="824f1-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="824f1-198">Deuxièmement, la classe `StockTicker` doit être créée en tant qu’objet singleton.</span><span class="sxs-lookup"><span data-stu-id="824f1-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="824f1-199">Ninject crée une instance de l’objet et retourne la même instance pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="824f1-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="824f1-200">Créez une liaison pour **IHubConnectionContext** comme suit :</span><span class="sxs-lookup"><span data-stu-id="824f1-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="824f1-201">Ce code crée une fonction anonyme qui retourne un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="824f1-201">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="824f1-202">La méthode **WhenInjectedInto** indique à Ninject d’utiliser cette fonction uniquement lors de la création d’instances de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="824f1-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="824f1-203">Cela est dû au fait que Signalr crée des instances **IHubConnectionContext** en interne, et que nous ne souhaitons pas remplacer la manière dont signalr les crée.</span><span class="sxs-lookup"><span data-stu-id="824f1-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="824f1-204">Cette fonction s’applique uniquement à notre classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="824f1-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="824f1-205">Transmettez le résolveur de dépendance dans la méthode **MapHubs** :</span><span class="sxs-lookup"><span data-stu-id="824f1-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="824f1-206">Désormais, Signalr utilise le programme de résolution spécifié dans **MapHubs**, au lieu du programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="824f1-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="824f1-207">Voici la liste complète du code pour `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="824f1-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="824f1-208">Pour exécuter l’application StockTicker dans Visual Studio, appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="824f1-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="824f1-209">Dans la fenêtre du navigateur, accédez à `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="824f1-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="824f1-210">L’application a exactement la même fonctionnalité qu’auparavant.</span><span class="sxs-lookup"><span data-stu-id="824f1-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="824f1-211">(Pour obtenir une description, consultez [diffusion sur le serveur avec ASP.net signalr](index.md).) Nous n’avons pas modifié le comportement ; vous venez de rendre le code plus facile à tester, à gérer et à évoluer.</span><span class="sxs-lookup"><span data-stu-id="824f1-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
