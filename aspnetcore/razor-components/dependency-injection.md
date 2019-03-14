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
# <a name="razor-components-dependency-injection"></a>Injection de dépendances de composants de Razor

Par [Rainer Stropek](https://www.timecockpit.com)

[L’injection de dépendance (DI)](/aspnet/core/fundamentals/dependency-injection) est intégrée. Applications peuvent utiliser les services intégrés en les injectant dans des composants. Les applications peuvent également définir des services personnalisés et les rendre disponibles via l’injection de dépendances.

## <a name="dependency-injection"></a>Injection de dépendances

L’injection de dépendances est une technique permettant d’accéder aux services configurés dans un emplacement central. Cela peut être utile pour :

* Partager une seule instance d’une classe de service entre plusieurs composants (appelé un *singleton* service).
* Découpler les composants à partir de classes de service concret particulier et référencer uniquement des abstractions. Par exemple, une interface `IDataAccess` est implémentée par une classe concrète `DataAccess`. Quand un composant utilise l’injection de dépendances pour recevoir un `IDataAccess` implémentation, le composant n’est pas couplée avec le type concret. L’implémentation peut être échangée, peut-être pour une implémentation factice dans les tests unitaires.

Le système de l’injection de dépendances est responsable de la fourniture des instances de services de composants. L’injection de dépendances résout également les dépendances de manière récursive afin que les services eux-mêmes peuvent dépendre d’autres services. L’injection de dépendances est configurée lors du démarrage de l’application. Un exemple est présenté plus loin dans cette rubrique.

## <a name="add-services-to-di"></a>Ajouter des services à l’injection de dépendances

Après avoir créé une nouvelle application, examinez le `Startup.ConfigureServices` méthode :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Le `ConfigureServices` est transmis à la méthode un [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), qui est une liste d’objets de descripteurs de service ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)). Services sont ajoutés en fournissant des descripteurs de service à la collection de service. L’exemple de code suivant illustre le concept :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Services peuvent être configurés avec les durées de vie suivantes :

| Méthode      | Description |
| ----------- | ----------- |
| [Singleton](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | L’injection de dépendances crée un *SIS* du service. Tous les composants nécessitant ce service reçoivent une référence à cette instance. |
| [Transient](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | Chaque fois qu’un composant nécessite ce service, il reçoit un *nouvelle instance* du service. |
| [Scoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | Blazor du côté client n’a actuellement le concept d’étendues de l’injection de dépendances. `Scoped` se comporte comme `Singleton`. Toutefois, les composants ASP.NET Core Razor prend en charge la `Scoped` durée de vie. Dans un composant Razor, une inscription de service délimité est limitée à la connexion. Pour cette raison, à l’aide de services délimités est préférable pour les services qui devraient être portées à l’utilisateur actuel (même si l’objectif actuel est de s’exécuter côté client dans le navigateur). |

Le système de l’injection de dépendances est basé sur le système de l’injection de dépendances dans ASP.NET Core. Pour plus d’informations, consultez [l’injection de dépendances dans ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).

## <a name="default-services"></a>Services par défaut

Services par défaut sont automatiquement ajoutés à la collection de service d’une application. Le tableau suivant présente certains des services par défaut utiles fournis.

| Méthode       | Description |
| ------------ | ----------- |
| [HttpClient](/dotnet/api/system.net.http.httpclient) | Fournit des méthodes pour envoyer des requêtes HTTP et recevoir des réponses HTTP à partir d’une ressource identifiée par un URI (singleton). Notez que cette instance de `HttpClient` utilise le navigateur pour gérer le trafic HTTP en arrière-plan. [HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) est automatiquement défini sur le préfixe URI de base de l’application. `HttpClient` est fournie uniquement pour les applications Blazor côté client. |
| `IJSRuntime` | Représente une instance d’un runtime JavaScript à laquelle les appels peuvent être distribués. Pour plus d'informations, consultez <xref:razor-components/javascript-interop>. |
| `IUriHelper` | Programmes d’assistance pour travailler avec l’état de l’URI et de navigation (singleton). `IUriHelper` est fourni pour les deux applications de Blazor et ASP.NET Core Razor composants côté client. |

Notez qu’il est possible d’utiliser un fournisseur de services personnalisés au lieu du fournisseur de service par défaut qui est ajouté par le modèle par défaut. Un fournisseur de services personnalisée ne fournit pas automatiquement les services par défaut répertoriées dans le tableau. Ces services doivent être ajoutés explicitement à nouveau fournisseur de services.

## <a name="request-a-service-in-a-component"></a>Demandez un service dans un composant

Une fois que les services sont ajoutés à la collection de service, ils peuvent être injectées dans les modèles Razor de composants à l’aide de la `@inject` directive Razor. `@inject` a deux paramètres :

* Nom du type : Le type du service à injecter.
* Nom de la propriété : Le nom de la propriété réception injecté app service. Notez que la propriété ne nécessite pas la création manuelle. Le compilateur crée la propriété.

Plusieurs `@inject` instructions peuvent être utilisées pour injecter des différents services.

L'exemple suivant montre comment utiliser `@inject`. Le service qui implémente `Services.IDataAccess` est injecté dans la propriété du composant `DataRepository`. Notez comment le code est uniquement à l’aide de la `IDataAccess` abstraction :

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

En interne, la propriété générée (`DataRepository`) est décorée avec le `InjectAttribute` attribut. En règle générale, cet attribut n’est pas utilisé directement. Si une classe de base est requise pour les composants et propriétés injectées sont également requises pour la classe de base `InjectAttribute` peuvent être ajoutées manuellement :

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

Dans les composants dérivés de la classe de base, le `@inject` directive n’est pas nécessaire. Le `InjectAttribute` de la classe de base est suffisante :

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a>Injection de dépendances dans les services

Services complexes peuvent nécessiter des services supplémentaires. Dans l’exemple précédent, `DataAccess` peut nécessiter la `HttpClient` service par défaut. `@inject` ou le `InjectAttribute` ne peut pas être utilisé dans les services. *L’injection de constructeur* doit être utilisé à la place. Les services requis sont ajoutés en ajoutant des paramètres pour le constructeur du service. Lors de l’injection de dépendances crée le service, il reconnaît les services, il requiert dans le constructeur et les fournit en conséquence.

L’exemple de code suivant illustre le concept :

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

Notez les conditions préalables suivantes pour l’injection de constructeur :

* Il doit y avoir un constructeur dont les arguments peuvent tous être satisfaits par l’injection de dépendances. Notez que les paramètres supplémentaires non couvertes par l’injection de dépendances sont autorisées si les valeurs par défaut sont spécifiées pour eux.
* Le constructeur applicable doit être *public*.
* Il ne doit exister qu’un seul constructeur applicable. Dans le cas d’une ambiguïté, l’injection de dépendances lève une exception.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Injection de dépendances dans ASP.NET Core](/aspnet/core/fundamentals/dependency-injection)
