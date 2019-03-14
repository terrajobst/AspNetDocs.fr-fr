---
title: Injection de dépendances de composants de Razor
author: guardrex
description: Découvrez comment les applications Blazor et composants de Razor peuvent utiliser des services intégrés en les injectant dans des composants.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 6ce8fa74f20145f48797d267c20ef2593368b941
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042426"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="3820c-103">Injection de dépendances de composants de Razor</span><span class="sxs-lookup"><span data-stu-id="3820c-103">Razor Components dependency injection</span></span>

<span data-ttu-id="3820c-104">Par [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="3820c-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="3820c-105">[L’injection de dépendance (DI)](/aspnet/core/fundamentals/dependency-injection) est intégrée.</span><span class="sxs-lookup"><span data-stu-id="3820c-105">[Dependency injection (DI)](/aspnet/core/fundamentals/dependency-injection) is built-in.</span></span> <span data-ttu-id="3820c-106">Applications peuvent utiliser les services intégrés en les injectant dans des composants.</span><span class="sxs-lookup"><span data-stu-id="3820c-106">Apps can use built-in services by having them injected into components.</span></span> <span data-ttu-id="3820c-107">Les applications peuvent également définir des services personnalisés et les rendre disponibles via l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="3820c-107">Apps can also define custom services and make them available via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="3820c-108">Injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="3820c-108">Dependency injection</span></span>

<span data-ttu-id="3820c-109">L’injection de dépendances est une technique permettant d’accéder aux services configurés dans un emplacement central.</span><span class="sxs-lookup"><span data-stu-id="3820c-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="3820c-110">Cela peut être utile pour :</span><span class="sxs-lookup"><span data-stu-id="3820c-110">This can be useful to:</span></span>

* <span data-ttu-id="3820c-111">Partager une seule instance d’une classe de service entre plusieurs composants (appelé un *singleton* service).</span><span class="sxs-lookup"><span data-stu-id="3820c-111">Share a single instance of a service class across many components (known as a *singleton* service).</span></span>
* <span data-ttu-id="3820c-112">Découpler les composants à partir de classes de service concret particulier et référencer uniquement des abstractions.</span><span class="sxs-lookup"><span data-stu-id="3820c-112">Decouple components from particular concrete service classes and only reference abstractions.</span></span> <span data-ttu-id="3820c-113">Par exemple, une interface `IDataAccess` est implémentée par une classe concrète `DataAccess`.</span><span class="sxs-lookup"><span data-stu-id="3820c-113">For example, an interface `IDataAccess` is implemented by a concrete class `DataAccess`.</span></span> <span data-ttu-id="3820c-114">Quand un composant utilise l’injection de dépendances pour recevoir un `IDataAccess` implémentation, le composant n’est pas couplée avec le type concret.</span><span class="sxs-lookup"><span data-stu-id="3820c-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="3820c-115">L’implémentation peut être échangée, peut-être pour une implémentation factice dans les tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="3820c-115">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="3820c-116">Le système de l’injection de dépendances est responsable de la fourniture des instances de services de composants.</span><span class="sxs-lookup"><span data-stu-id="3820c-116">The DI system is responsible for supplying instances of services to components.</span></span> <span data-ttu-id="3820c-117">L’injection de dépendances résout également les dépendances de manière récursive afin que les services eux-mêmes peuvent dépendre d’autres services.</span><span class="sxs-lookup"><span data-stu-id="3820c-117">DI also resolves dependencies recursively so that services themselves can depend on further services.</span></span> <span data-ttu-id="3820c-118">L’injection de dépendances est configurée lors du démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="3820c-118">DI is configured during startup of the app.</span></span> <span data-ttu-id="3820c-119">Un exemple est présenté plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="3820c-119">An example is shown later in this topic.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="3820c-120">Ajouter des services à l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="3820c-120">Add services to DI</span></span>

<span data-ttu-id="3820c-121">Après avoir créé une nouvelle application, examinez le `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="3820c-121">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="3820c-122">Le `ConfigureServices` est transmis à la méthode un [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), qui est une liste d’objets de descripteurs de service ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span><span class="sxs-lookup"><span data-stu-id="3820c-122">The `ConfigureServices` method is passed an [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), which is a list of service descriptor objects ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span></span> <span data-ttu-id="3820c-123">Services sont ajoutés en fournissant des descripteurs de service à la collection de service.</span><span class="sxs-lookup"><span data-stu-id="3820c-123">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="3820c-124">L’exemple de code suivant illustre le concept :</span><span class="sxs-lookup"><span data-stu-id="3820c-124">The following code sample demonstrates the concept:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="3820c-125">Services peuvent être configurés avec les durées de vie suivantes :</span><span class="sxs-lookup"><span data-stu-id="3820c-125">Services can be configured with the following lifetimes:</span></span>

| <span data-ttu-id="3820c-126">Méthode</span><span class="sxs-lookup"><span data-stu-id="3820c-126">Method</span></span>      | <span data-ttu-id="3820c-127">Description</span><span class="sxs-lookup"><span data-stu-id="3820c-127">Description</span></span> |
| ----------- | ----------- |
| [<span data-ttu-id="3820c-128">Singleton</span><span class="sxs-lookup"><span data-stu-id="3820c-128">Singleton</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | <span data-ttu-id="3820c-129">L’injection de dépendances crée un *SIS* du service.</span><span class="sxs-lookup"><span data-stu-id="3820c-129">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="3820c-130">Tous les composants nécessitant ce service reçoivent une référence à cette instance.</span><span class="sxs-lookup"><span data-stu-id="3820c-130">All components requiring this service receive a reference to this instance.</span></span> |
| [<span data-ttu-id="3820c-131">Transient</span><span class="sxs-lookup"><span data-stu-id="3820c-131">Transient</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | <span data-ttu-id="3820c-132">Chaque fois qu’un composant nécessite ce service, il reçoit un *nouvelle instance* du service.</span><span class="sxs-lookup"><span data-stu-id="3820c-132">Whenever a component requires this service, it receives a *new instance* of the service.</span></span> |
| [<span data-ttu-id="3820c-133">Scoped</span><span class="sxs-lookup"><span data-stu-id="3820c-133">Scoped</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | <span data-ttu-id="3820c-134">Blazor du côté client n’a actuellement le concept d’étendues de l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="3820c-134">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="3820c-135">`Scoped` se comporte comme `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="3820c-135">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="3820c-136">Toutefois, les composants ASP.NET Core Razor prend en charge la `Scoped` durée de vie.</span><span class="sxs-lookup"><span data-stu-id="3820c-136">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="3820c-137">Dans un composant Razor, une inscription de service délimité est limitée à la connexion.</span><span class="sxs-lookup"><span data-stu-id="3820c-137">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="3820c-138">Pour cette raison, à l’aide de services délimités est préférable pour les services qui devraient être portées à l’utilisateur actuel (même si l’objectif actuel est de s’exécuter côté client dans le navigateur).</span><span class="sxs-lookup"><span data-stu-id="3820c-138">For this reason, using scoped services is preferred for services that should be scoped to the current user (even if the current intent is to run client-side in the browser).</span></span> |

<span data-ttu-id="3820c-139">Le système de l’injection de dépendances est basé sur le système de l’injection de dépendances dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3820c-139">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="3820c-140">Pour plus d’informations, consultez [l’injection de dépendances dans ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3820c-140">For more information, see [Dependency injection in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span></span>

## <a name="default-services"></a><span data-ttu-id="3820c-141">Services par défaut</span><span class="sxs-lookup"><span data-stu-id="3820c-141">Default services</span></span>

<span data-ttu-id="3820c-142">Services par défaut sont automatiquement ajoutés à la collection de service d’une application.</span><span class="sxs-lookup"><span data-stu-id="3820c-142">Default services are automatically added to the service collection of an app.</span></span> <span data-ttu-id="3820c-143">Le tableau suivant présente certains des services par défaut utiles fournis.</span><span class="sxs-lookup"><span data-stu-id="3820c-143">The following table shows some of the useful default services provided.</span></span>

| <span data-ttu-id="3820c-144">Méthode</span><span class="sxs-lookup"><span data-stu-id="3820c-144">Method</span></span>       | <span data-ttu-id="3820c-145">Description</span><span class="sxs-lookup"><span data-stu-id="3820c-145">Description</span></span> |
| ------------ | ----------- |
| [<span data-ttu-id="3820c-146">HttpClient</span><span class="sxs-lookup"><span data-stu-id="3820c-146">HttpClient</span></span>](/dotnet/api/system.net.http.httpclient) | <span data-ttu-id="3820c-147">Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP à partir d’une ressource identifiée par un URI (singleton).</span><span class="sxs-lookup"><span data-stu-id="3820c-147">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="3820c-148">Notez que cette instance de `HttpClient` utilise le navigateur pour gérer le trafic HTTP en arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="3820c-148">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="3820c-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) est automatiquement défini sur le préfixe URI de base de l’application.</span><span class="sxs-lookup"><span data-stu-id="3820c-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="3820c-150">`HttpClient` est fournie uniquement pour les applications Blazor côté client.</span><span class="sxs-lookup"><span data-stu-id="3820c-150">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="3820c-151">Représente une instance d’un runtime JavaScript à laquelle les appels peuvent être distribués.</span><span class="sxs-lookup"><span data-stu-id="3820c-151">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="3820c-152">Pour plus d'informations, consultez <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="3820c-152">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="3820c-153">Programmes d’assistance pour travailler avec l’état de l’URI et de navigation (singleton).</span><span class="sxs-lookup"><span data-stu-id="3820c-153">Helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="3820c-154">`IUriHelper` est fourni pour les deux applications de Blazor et ASP.NET Core Razor composants côté client.</span><span class="sxs-lookup"><span data-stu-id="3820c-154">`IUriHelper` is provided to both client-side Blazor and ASP.NET Core Razor Components apps.</span></span> |

<span data-ttu-id="3820c-155">Notez qu’il est possible d’utiliser un fournisseur de services personnalisés au lieu du fournisseur de service par défaut qui est ajouté par le modèle par défaut.</span><span class="sxs-lookup"><span data-stu-id="3820c-155">Note that it is possible to use a custom services provider instead of the default service provider that's added by the default template.</span></span> <span data-ttu-id="3820c-156">Un fournisseur de services personnalisée ne fournit pas automatiquement les services par défaut répertoriées dans le tableau.</span><span class="sxs-lookup"><span data-stu-id="3820c-156">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="3820c-157">Ces services doivent être ajoutés explicitement à nouveau fournisseur de services.</span><span class="sxs-lookup"><span data-stu-id="3820c-157">Those services must be added to the new service provider explicitly.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="3820c-158">Demandez un service dans un composant</span><span class="sxs-lookup"><span data-stu-id="3820c-158">Request a service in a component</span></span>

<span data-ttu-id="3820c-159">Une fois que les services sont ajoutés à la collection de service, ils peuvent être injectées dans les modèles Razor de composants à l’aide de la `@inject` directive Razor.</span><span class="sxs-lookup"><span data-stu-id="3820c-159">Once services are added to the service collection, they can be injected into the components' Razor templates using the `@inject` Razor directive.</span></span> <span data-ttu-id="3820c-160">`@inject` a deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="3820c-160">`@inject` has two parameters:</span></span>

* <span data-ttu-id="3820c-161">Nom du type : Le type du service à injecter.</span><span class="sxs-lookup"><span data-stu-id="3820c-161">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="3820c-162">Nom de la propriété : Le nom de la propriété réception injecté app service.</span><span class="sxs-lookup"><span data-stu-id="3820c-162">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="3820c-163">Notez que la propriété ne nécessite pas la création manuelle.</span><span class="sxs-lookup"><span data-stu-id="3820c-163">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="3820c-164">Le compilateur crée la propriété.</span><span class="sxs-lookup"><span data-stu-id="3820c-164">The compiler creates the property.</span></span>

<span data-ttu-id="3820c-165">Plusieurs `@inject` instructions peuvent être utilisées pour injecter des différents services.</span><span class="sxs-lookup"><span data-stu-id="3820c-165">Multiple `@inject` statements can be used to inject different services.</span></span>

<span data-ttu-id="3820c-166">L'exemple suivant montre comment utiliser `@inject`.</span><span class="sxs-lookup"><span data-stu-id="3820c-166">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="3820c-167">Le service qui implémente `Services.IDataAccess` est injecté dans la propriété du composant `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="3820c-167">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="3820c-168">Notez comment le code est uniquement à l’aide de la `IDataAccess` abstraction :</span><span class="sxs-lookup"><span data-stu-id="3820c-168">Note how the code is only using the `IDataAccess` abstraction:</span></span>

```csharp
@page "/customer-list"
@using Services
@inject IDataAccess DataRepository

<ul>
    @if (Customers != null)
    {
        @foreach (var customer in Customers)
        {
            <li>@customer.FirstName @customer.LastName</li>
        }
    }
</ul>

@functions {
    private IReadOnlyList<Customer> Customers;

    protected override async Task OnInitAsync()
    {
        // The property DataRepository received an implementation
        // of IDataAccess through dependency injection. Use 
        // DataRepository to obtain data from the server.
        Customers = await DataRepository.GetAllCustomersAsync();
    }
}
```

<span data-ttu-id="3820c-169">En interne, la propriété générée (`DataRepository`) est décorée avec le `InjectAttribute` attribut.</span><span class="sxs-lookup"><span data-stu-id="3820c-169">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="3820c-170">En règle générale, cet attribut n’est pas utilisé directement.</span><span class="sxs-lookup"><span data-stu-id="3820c-170">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="3820c-171">Si une classe de base est requise pour les composants et propriétés injectées sont également requises pour la classe de base `InjectAttribute` peuvent être ajoutées manuellement :</span><span class="sxs-lookup"><span data-stu-id="3820c-171">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : BlazorComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="3820c-172">Dans les composants dérivés de la classe de base, le `@inject` directive n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="3820c-172">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="3820c-173">Le `InjectAttribute` de la classe de base est suffisante :</span><span class="sxs-lookup"><span data-stu-id="3820c-173">The `InjectAttribute` of the base class is sufficient:</span></span>

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="3820c-174">Injection de dépendances dans les services</span><span class="sxs-lookup"><span data-stu-id="3820c-174">Dependency injection in services</span></span>

<span data-ttu-id="3820c-175">Services complexes peuvent nécessiter des services supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="3820c-175">Complex services might require additional services.</span></span> <span data-ttu-id="3820c-176">Dans l’exemple précédent, `DataAccess` peut nécessiter la `HttpClient` service par défaut.</span><span class="sxs-lookup"><span data-stu-id="3820c-176">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="3820c-177">`@inject` ou le `InjectAttribute` ne peut pas être utilisé dans les services.</span><span class="sxs-lookup"><span data-stu-id="3820c-177">`@inject` or the `InjectAttribute` can't be used in services.</span></span> <span data-ttu-id="3820c-178">*L’injection de constructeur* doit être utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="3820c-178">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="3820c-179">Les services requis sont ajoutés en ajoutant des paramètres pour le constructeur du service.</span><span class="sxs-lookup"><span data-stu-id="3820c-179">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="3820c-180">Lors de l’injection de dépendances crée le service, il reconnaît les services, il requiert dans le constructeur et les fournit en conséquence.</span><span class="sxs-lookup"><span data-stu-id="3820c-180">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

<span data-ttu-id="3820c-181">L’exemple de code suivant illustre le concept :</span><span class="sxs-lookup"><span data-stu-id="3820c-181">The following code sample demonstrates the concept:</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
    ...
}
```

<span data-ttu-id="3820c-182">Notez les conditions préalables suivantes pour l’injection de constructeur :</span><span class="sxs-lookup"><span data-stu-id="3820c-182">Note the following prerequisites for constructor injection:</span></span>

* <span data-ttu-id="3820c-183">Il doit y avoir un constructeur dont les arguments peuvent tous être satisfaits par l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="3820c-183">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="3820c-184">Notez que les paramètres supplémentaires non couvertes par l’injection de dépendances sont autorisées si les valeurs par défaut sont spécifiées pour eux.</span><span class="sxs-lookup"><span data-stu-id="3820c-184">Note that additional parameters not covered by DI are allowed if default values are specified for them.</span></span>
* <span data-ttu-id="3820c-185">Le constructeur applicable doit être *public*.</span><span class="sxs-lookup"><span data-stu-id="3820c-185">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="3820c-186">Il ne doit exister qu’un seul constructeur applicable.</span><span class="sxs-lookup"><span data-stu-id="3820c-186">There must only be one applicable constructor.</span></span> <span data-ttu-id="3820c-187">Dans le cas d’une ambiguïté, l’injection de dépendances lève une exception.</span><span class="sxs-lookup"><span data-stu-id="3820c-187">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3820c-188">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3820c-188">Additional resources</span></span>

* [<span data-ttu-id="3820c-189">Injection de dépendances dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3820c-189">Dependency injection in ASP.NET Core</span></span>](/aspnet/core/fundamentals/dependency-injection)
