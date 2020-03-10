---
uid: signalr/overview/advanced/dependency-injection
title: Injection de dépendances dans Signalr | Microsoft Docs
author: bradygaster
description: Versions logicielles utilisées dans cette rubrique Visual Studio 2013 .NET 4,5 Signalr version 2 versions précédentes de cette rubrique pour plus d’informations sur les versions antérieures de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537330"
---
# <a name="dependency-injection-in-signalr"></a><span data-ttu-id="017b3-103">Injection de dépendances dans SignalR</span><span class="sxs-lookup"><span data-stu-id="017b3-103">Dependency Injection in SignalR</span></span>

<span data-ttu-id="017b3-104">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="017b3-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="017b3-105">Versions logicielles utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="017b3-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="017b3-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="017b3-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="017b3-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="017b3-107">.NET 4.5</span></span>
> - <span data-ttu-id="017b3-108">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="017b3-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="017b3-109">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="017b3-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="017b3-110">Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="017b3-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="017b3-111">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="017b3-111">Questions and comments</span></span>
>
> <span data-ttu-id="017b3-112">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="017b3-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="017b3-113">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="017b3-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="017b3-114">L’injection de dépendances est un moyen de supprimer les dépendances codées en dur entre les objets, ce qui facilite le remplacement des dépendances d’un objet, soit pour le test (à l’aide d’objets factices), soit pour modifier le comportement au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="017b3-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="017b3-115">Ce didacticiel montre comment effectuer une injection de dépendances sur les concentrateurs Signalr.</span><span class="sxs-lookup"><span data-stu-id="017b3-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="017b3-116">Il montre également comment utiliser des conteneurs IoC avec Signalr.</span><span class="sxs-lookup"><span data-stu-id="017b3-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="017b3-117">Un conteneur IoC est un Framework général pour l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="017b3-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="017b3-118">Qu’est-ce que l’injection de dépendance ?</span><span class="sxs-lookup"><span data-stu-id="017b3-118">What is Dependency Injection?</span></span>

<span data-ttu-id="017b3-119">Ignorez cette section si vous êtes déjà familiarisé avec l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="017b3-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="017b3-120">L' *injection de dépendances* (di) est un modèle dans lequel les objets ne sont pas responsables de la création de leurs propres dépendances.</span><span class="sxs-lookup"><span data-stu-id="017b3-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="017b3-121">Voici un exemple simple pour motiver DI.</span><span class="sxs-lookup"><span data-stu-id="017b3-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="017b3-122">Supposons que vous ayez un objet qui doit consigner des messages.</span><span class="sxs-lookup"><span data-stu-id="017b3-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="017b3-123">Vous pouvez définir une interface de journalisation :</span><span class="sxs-lookup"><span data-stu-id="017b3-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="017b3-124">Dans votre objet, vous pouvez créer un `ILogger` pour enregistrer les messages :</span><span class="sxs-lookup"><span data-stu-id="017b3-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="017b3-125">Cela fonctionne, mais ce n’est pas la meilleure conception.</span><span class="sxs-lookup"><span data-stu-id="017b3-125">This works, but it's not the best design.</span></span> <span data-ttu-id="017b3-126">Si vous souhaitez remplacer `FileLogger` par une autre implémentation de `ILogger`, vous devez modifier `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="017b3-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="017b3-127">Supposons que de nombreux autres objets utilisent `FileLogger`, vous devrez les modifier tous.</span><span class="sxs-lookup"><span data-stu-id="017b3-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="017b3-128">Ou si vous décidez de créer `FileLogger` un singleton, vous devrez également apporter des modifications à l’ensemble de l’application.</span><span class="sxs-lookup"><span data-stu-id="017b3-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="017b3-129">Une meilleure approche consiste à « injecter » un `ILogger` dans l’objet, par exemple, à l’aide d’un argument de constructeur :</span><span class="sxs-lookup"><span data-stu-id="017b3-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="017b3-130">À présent, l’objet n’est pas responsable de la sélection des `ILogger` à utiliser.</span><span class="sxs-lookup"><span data-stu-id="017b3-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="017b3-131">Vous pouvez basculer `ILogger` implémentations sans modifier les objets qui en dépendent.</span><span class="sxs-lookup"><span data-stu-id="017b3-131">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="017b3-132">Ce modèle est appelé [injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="017b3-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="017b3-133">Un autre modèle est l’injection d’accesseurs set, où vous définissez la dépendance par le biais d’une méthode ou d’une propriété Setter.</span><span class="sxs-lookup"><span data-stu-id="017b3-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="017b3-134">Injection de dépendances simple dans Signalr</span><span class="sxs-lookup"><span data-stu-id="017b3-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="017b3-135">Prenons l’exemple de l’application conversation du didacticiel [prise en main avec signalr](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="017b3-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="017b3-136">Voici la classe de concentrateur de cette application :</span><span class="sxs-lookup"><span data-stu-id="017b3-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="017b3-137">Supposons que vous souhaitez stocker les messages de conversation sur le serveur avant de les envoyer.</span><span class="sxs-lookup"><span data-stu-id="017b3-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="017b3-138">Vous pouvez définir une interface qui soustrait cette fonctionnalité et utiliser DI pour injecter l’interface dans la classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="017b3-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="017b3-139">Le seul problème est qu’une application Signalr ne crée pas directement de concentrateurs ; Signalr les crée pour vous.</span><span class="sxs-lookup"><span data-stu-id="017b3-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="017b3-140">Par défaut, Signalr attend qu’une classe de concentrateur ait un constructeur sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="017b3-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="017b3-141">Toutefois, vous pouvez facilement inscrire une fonction pour créer des instances de Hub et utiliser cette fonction pour exécuter DI.</span><span class="sxs-lookup"><span data-stu-id="017b3-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="017b3-142">Inscrivez la fonction en appelant **GlobalHost. DependencyResolver. Register**.</span><span class="sxs-lookup"><span data-stu-id="017b3-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="017b3-143">Désormais, Signalr appellera cette fonction anonyme chaque fois qu’elle a besoin de créer une instance `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="017b3-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="017b3-144">Conteneurs IoC</span><span class="sxs-lookup"><span data-stu-id="017b3-144">IoC Containers</span></span>

<span data-ttu-id="017b3-145">Le code précédent est parfait pour les cas simples.</span><span class="sxs-lookup"><span data-stu-id="017b3-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="017b3-146">Toutefois, vous deviez toujours écrire ceci :</span><span class="sxs-lookup"><span data-stu-id="017b3-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="017b3-147">Dans une application complexe avec de nombreuses dépendances, vous devrez peut-être écrire un grand nombre de ce code de « câblage ».</span><span class="sxs-lookup"><span data-stu-id="017b3-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="017b3-148">Ce code peut être difficile à gérer, en particulier si les dépendances sont imbriquées.</span><span class="sxs-lookup"><span data-stu-id="017b3-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="017b3-149">Il est également difficile à effectuer des tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="017b3-149">It is also hard to unit test.</span></span>

<span data-ttu-id="017b3-150">Une solution consiste à utiliser un conteneur IoC.</span><span class="sxs-lookup"><span data-stu-id="017b3-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="017b3-151">Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances. Vous inscrivez les types avec le conteneur, puis vous utilisez le conteneur pour créer des objets.</span><span class="sxs-lookup"><span data-stu-id="017b3-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="017b3-152">Le conteneur détermine automatiquement les relations de dépendance.</span><span class="sxs-lookup"><span data-stu-id="017b3-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="017b3-153">De nombreux conteneurs IoC vous permettent également de contrôler des éléments tels que la durée de vie des objets et l’étendue.</span><span class="sxs-lookup"><span data-stu-id="017b3-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="017b3-154">« IoC » signifie « inversion de contrôle », qui est un modèle général dans lequel un Framework appelle le code d’application.</span><span class="sxs-lookup"><span data-stu-id="017b3-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="017b3-155">Un conteneur IoC construit vos objets pour vous, ce qui « inverse » le déroulement habituel du contrôle.</span><span class="sxs-lookup"><span data-stu-id="017b3-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="017b3-156">Utilisation de conteneurs IoC dans Signalr</span><span class="sxs-lookup"><span data-stu-id="017b3-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="017b3-157">L’application de conversation est probablement trop simple pour tirer parti d’un conteneur IoC.</span><span class="sxs-lookup"><span data-stu-id="017b3-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="017b3-158">Examinons plutôt l’exemple [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .</span><span class="sxs-lookup"><span data-stu-id="017b3-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="017b3-159">L’exemple StockTicker définit deux classes principales :</span><span class="sxs-lookup"><span data-stu-id="017b3-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="017b3-160">`StockTickerHub`: la classe de concentrateur, qui gère les connexions clientes.</span><span class="sxs-lookup"><span data-stu-id="017b3-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="017b3-161">`StockTicker`: singleton qui contient des cours boursiers et les met à jour périodiquement.</span><span class="sxs-lookup"><span data-stu-id="017b3-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="017b3-162">`StockTickerHub` contient une référence au Singleton `StockTicker`, tandis que `StockTicker` contient une référence à **IHubConnectionContext** pour le `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="017b3-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="017b3-163">Elle utilise cette interface pour communiquer avec les instances de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="017b3-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="017b3-164">(Pour plus d’informations, consultez diffusion sur le [serveur avec ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="017b3-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="017b3-165">Nous pouvons utiliser un conteneur IoC pour démêler ces dépendances un peu plus.</span><span class="sxs-lookup"><span data-stu-id="017b3-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="017b3-166">Tout d’abord, nous allons simplifier les classes `StockTickerHub` et `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="017b3-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="017b3-167">Dans le code suivant, j’ai commenté les parties dont nous n’avons pas besoin.</span><span class="sxs-lookup"><span data-stu-id="017b3-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="017b3-168">Supprimez le constructeur sans paramètre de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="017b3-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="017b3-169">Au lieu de cela, nous utiliserons toujours DI pour créer le Hub.</span><span class="sxs-lookup"><span data-stu-id="017b3-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="017b3-170">Pour StockTicker, supprimez l’instance singleton.</span><span class="sxs-lookup"><span data-stu-id="017b3-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="017b3-171">Plus tard, nous allons utiliser le conteneur IoC pour contrôler la durée de vie de StockTicker.</span><span class="sxs-lookup"><span data-stu-id="017b3-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="017b3-172">En outre, rendez le constructeur public.</span><span class="sxs-lookup"><span data-stu-id="017b3-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="017b3-173">Ensuite, nous pouvons Refactoriser le code en créant une interface pour `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="017b3-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="017b3-174">Nous allons utiliser cette interface pour découpler le `StockTickerHub` de la classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="017b3-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="017b3-175">Visual Studio facilite ce type de refactorisation.</span><span class="sxs-lookup"><span data-stu-id="017b3-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="017b3-176">Ouvrez le fichier StockTicker.cs, cliquez avec le bouton droit sur la déclaration de classe `StockTicker`, puis sélectionnez **Refactoriser** ... **Extraire l’interface**.</span><span class="sxs-lookup"><span data-stu-id="017b3-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="017b3-177">Dans la boîte de dialogue **extraire l’interface** , cliquez sur **Sélectionner tout**.</span><span class="sxs-lookup"><span data-stu-id="017b3-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="017b3-178">Conservez les autres valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="017b3-178">Leave the other defaults.</span></span> <span data-ttu-id="017b3-179">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="017b3-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="017b3-180">Visual Studio crée une nouvelle interface nommée `IStockTicker`et modifie également `StockTicker` pour dériver de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="017b3-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="017b3-181">Ouvrez le fichier IStockTicker.cs et remplacez l’interface par **public**.</span><span class="sxs-lookup"><span data-stu-id="017b3-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="017b3-182">Dans la classe `StockTickerHub`, modifiez les deux instances de `StockTicker` en `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="017b3-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="017b3-183">La création d’une interface `IStockTicker` n’est pas strictement nécessaire, mais je voulais montrer comment DI peut aider à réduire le couplage entre les composants de votre application.</span><span class="sxs-lookup"><span data-stu-id="017b3-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="017b3-184">Ajouter la bibliothèque Ninject</span><span class="sxs-lookup"><span data-stu-id="017b3-184">Add the Ninject Library</span></span>

<span data-ttu-id="017b3-185">Il existe de nombreux conteneurs IoC Open source pour .NET.</span><span class="sxs-lookup"><span data-stu-id="017b3-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="017b3-186">Pour ce didacticiel, je vais utiliser [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="017b3-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="017b3-187">(D’autres bibliothèques populaires incluent [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)et [StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="017b3-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="017b3-188">Utilisez le gestionnaire de package NuGet pour installer la [bibliothèque Ninject](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="017b3-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="017b3-189">Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** > **console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="017b3-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="017b3-190">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="017b3-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="017b3-191">Remplacer le programme de résolution de dépendance Signalr</span><span class="sxs-lookup"><span data-stu-id="017b3-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="017b3-192">Pour utiliser Ninject dans Signalr, créez une classe qui dérive de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="017b3-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="017b3-193">Cette classe remplace les méthodes **GetService** et **GetServices** de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="017b3-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="017b3-194">Signalr appelle ces méthodes pour créer divers objets au moment de l’exécution, y compris les instances de concentrateur, ainsi que divers services utilisés en interne par Signalr.</span><span class="sxs-lookup"><span data-stu-id="017b3-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="017b3-195">La méthode **GetService** crée une seule instance d’un type.</span><span class="sxs-lookup"><span data-stu-id="017b3-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="017b3-196">Substituez cette méthode pour appeler la méthode **opération TryGet** du noyau Ninject.</span><span class="sxs-lookup"><span data-stu-id="017b3-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="017b3-197">Si cette méthode retourne la valeur null, revient au résolveur par défaut.</span><span class="sxs-lookup"><span data-stu-id="017b3-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="017b3-198">La méthode **GetServices** crée une collection d’objets d’un type spécifié.</span><span class="sxs-lookup"><span data-stu-id="017b3-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="017b3-199">Substituez cette méthode pour concaténer les résultats de Ninject avec les résultats du programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="017b3-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="017b3-200">Configurer des liaisons Ninject</span><span class="sxs-lookup"><span data-stu-id="017b3-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="017b3-201">À présent, nous allons utiliser Ninject pour déclarer des liaisons de type.</span><span class="sxs-lookup"><span data-stu-id="017b3-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="017b3-202">Ouvrez la classe Startup.cs de votre application (que vous avez créée manuellement en fonction des instructions du package dans `readme.txt`ou qui a été créée en ajoutant l’authentification à votre projet).</span><span class="sxs-lookup"><span data-stu-id="017b3-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="017b3-203">Dans la méthode `Startup.Configuration`, créez le conteneur Ninject, qui Ninject appelle le *noyau*.</span><span class="sxs-lookup"><span data-stu-id="017b3-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="017b3-204">Créez une instance de notre résolveur de dépendance personnalisé :</span><span class="sxs-lookup"><span data-stu-id="017b3-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="017b3-205">Créez une liaison pour `IStockTicker` comme suit :</span><span class="sxs-lookup"><span data-stu-id="017b3-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="017b3-206">Ce code dit deux choses.</span><span class="sxs-lookup"><span data-stu-id="017b3-206">This code is saying two things.</span></span> <span data-ttu-id="017b3-207">Tout d’abord, chaque fois que l’application a besoin d’une `IStockTicker`, le noyau doit créer une instance de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="017b3-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="017b3-208">Deuxièmement, la classe `StockTicker` doit être créée en tant qu’objet singleton.</span><span class="sxs-lookup"><span data-stu-id="017b3-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="017b3-209">Ninject crée une instance de l’objet et retourne la même instance pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="017b3-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="017b3-210">Créez une liaison pour **IHubConnectionContext** comme suit :</span><span class="sxs-lookup"><span data-stu-id="017b3-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="017b3-211">Ce code crée une fonction anonyme qui retourne un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="017b3-211">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="017b3-212">La méthode **WhenInjectedInto** indique à Ninject d’utiliser cette fonction uniquement lors de la création d’instances de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="017b3-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="017b3-213">Cela est dû au fait que Signalr crée des instances **IHubConnectionContext** en interne, et que nous ne souhaitons pas remplacer la manière dont signalr les crée.</span><span class="sxs-lookup"><span data-stu-id="017b3-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="017b3-214">Cette fonction s’applique uniquement à notre classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="017b3-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="017b3-215">Transmettez le résolveur de dépendance dans la méthode **MapSignalR** en ajoutant une configuration de concentrateur :</span><span class="sxs-lookup"><span data-stu-id="017b3-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="017b3-216">Mettez à jour la méthode Startup. ConfigureSignalR dans la classe Startup de l’exemple avec le nouveau paramètre :</span><span class="sxs-lookup"><span data-stu-id="017b3-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="017b3-217">Désormais, Signalr utilise le programme de résolution spécifié dans **MapSignalR**, au lieu du programme de résolution par défaut.</span><span class="sxs-lookup"><span data-stu-id="017b3-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="017b3-218">Voici la liste complète du code pour `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="017b3-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="017b3-219">Pour exécuter l’application StockTicker dans Visual Studio, appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="017b3-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="017b3-220">Dans la fenêtre du navigateur, accédez à `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="017b3-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="017b3-221">L’application a exactement la même fonctionnalité qu’auparavant.</span><span class="sxs-lookup"><span data-stu-id="017b3-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="017b3-222">(Pour obtenir une description, consultez [diffusion sur le serveur avec ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md).) Nous n’avons pas modifié le comportement ; vous venez de rendre le code plus facile à tester, à gérer et à évoluer.</span><span class="sxs-lookup"><span data-stu-id="017b3-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
