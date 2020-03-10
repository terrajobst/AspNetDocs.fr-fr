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
# <a name="dependency-injection-in-aspnet-web-api-2"></a>Injection de dépendances dans API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Ce didacticiel montre comment injecter des dépendances dans votre contrôleur de API Web ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - API Web 2
> - [Bloc d’application Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (la version 5 fonctionne également)

## <a name="what-is-dependency-injection"></a>Qu’est-ce que l’injection de dépendance ?

Une *dépendance* est un objet qui nécessite un autre objet. Par exemple, il est courant de définir un [référentiel](http://martinfowler.com/eaaCatalog/repository.html) qui gère l’accès aux données. Prenons un exemple. Tout d’abord, nous allons définir un modèle de domaine :

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Voici une classe de référentiel simple qui stocke des éléments dans une base de données, à l’aide de Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Nous allons maintenant définir un contrôleur d’API Web qui prend en charge les demandes d’extraction pour les entités `Product`. (Je laisse la publication et d’autres méthodes pour plus de simplicité.) Voici une première tentative :

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Notez que la classe Controller dépend de `ProductRepository`, et que nous autorisons le contrôleur à créer l’instance `ProductRepository`. Toutefois, il est déconseillé de coder en dur la dépendance de cette manière, pour plusieurs raisons.

- Si vous souhaitez remplacer `ProductRepository` par une autre implémentation, vous devez également modifier la classe de contrôleur.
- Si le `ProductRepository` a des dépendances, vous devez les configurer à l’intérieur du contrôleur. Dans le cas d’un grand projet avec plusieurs contrôleurs, votre code de configuration est éparpillé dans votre projet.
- Il est difficile d’effectuer des tests unitaires, car le contrôleur est codé en dur pour interroger la base de données. Pour un test unitaire, vous devez utiliser un référentiel fictif ou stub, ce qui n’est pas possible avec la conception actuelle.

Nous pouvons résoudre ces problèmes en *injectant* le référentiel dans le contrôleur. Tout d’abord, refactorisez la classe `ProductRepository` dans une interface :

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Fournissez ensuite le `IProductRepository` en tant que paramètre de constructeur :

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Cet exemple utilise l' [injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Vous pouvez également utiliser l' *injection d’accesseurs set*, où vous définissez la dépendance par le biais d’une méthode ou d’une propriété Setter.

Mais maintenant, il y a un problème, car votre application ne crée pas directement le contrôleur. L’API Web crée le contrôleur lors de l’acheminement de la demande, et l’API Web ne sait rien sur `IProductRepository`. C’est là qu’intervient le programme de résolution des dépendances de l’API Web.

## <a name="the-web-api-dependency-resolver"></a>Le programme de résolution des dépendances de l’API Web

L’API Web définit l’interface **IDependencyResolver** pour la résolution des dépendances. Voici la définition de l’interface :

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

L’interface **IDependencyScope** a deux méthodes :

- **GetService** crée une instance d’un type.
- **GetServices** crée une collection d’objets d’un type spécifié.

La méthode **IDependencyResolver** hérite de **IDependencyScope** et ajoute la méthode **BeginScope** . Nous parlerons des étendues plus loin dans ce didacticiel.

Lorsque l’API Web crée une instance de contrôleur, elle appelle d’abord **IDependencyResolver. GetService**, en passant le type de contrôleur. Vous pouvez utiliser ce hook d’extensibilité pour créer le contrôleur, en résolvant toutes les dépendances. Si **GetService** retourne null, l’API Web recherche un constructeur sans paramètre sur la classe Controller.

## <a name="dependency-resolution-with-the-unity-container"></a>Résolution des dépendances avec le conteneur Unity

Bien que vous puissiez écrire une implémentation complète de **IDependencyResolver** à partir de zéro, l’interface est vraiment conçue pour agir en tant que pont entre l’API Web et les conteneurs IOC existants.

Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances. Vous inscrivez les types avec le conteneur, puis vous utilisez le conteneur pour créer des objets. Le conteneur détermine automatiquement les relations de dépendance. De nombreux conteneurs IoC vous permettent également de contrôler des éléments tels que la durée de vie des objets et l’étendue.

> [!NOTE]
> « IoC » signifie « inversion de contrôle », qui est un modèle général dans lequel un Framework appelle le code d’application. Un conteneur IoC construit vos objets pour vous, ce qui « inverse » le déroulement habituel du contrôle.

Pour ce didacticiel, nous allons utiliser [Unity](https://msdn.microsoft.com/library/ff647202.aspx) de Microsoft patterns &amp; Practices. (D’autres bibliothèques populaires incluent [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)et [StructureMap](http://structuremap.github.io/documentation/).) Vous pouvez utiliser le gestionnaire de package NuGet pour installer Unity. Dans le menu **Outils** de Visual Studio, sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre de la console du gestionnaire de package, tapez la commande suivante :

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Voici une implémentation de **IDependencyResolver** qui encapsule un conteneur Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Si la méthode **GetService** ne peut pas résoudre un type, elle doit retourner la **valeur null**. Si la méthode **GetServices** ne peut pas résoudre un type, elle doit retourner un objet de collection vide. Ne levez pas d’exceptions pour les types inconnus.

## <a name="configuring-the-dependency-resolver"></a>Configuration du programme de résolution des dépendances

Définissez le résolveur de dépendances sur la propriété **DependencyResolver** de l’objet global **HttpConfiguration** .

Le code suivant inscrit l’interface `IProductRepository` avec Unity, puis crée une `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Durée de vie du contrôleur et de la portée de dépendance

Les contrôleurs sont créés par demande. Pour gérer les durées de vie des objets, **IDependencyResolver** utilise le concept d' *étendue*.

Le programme de résolution des dépendances attaché à l’objet **HttpConfiguration** a une portée globale. Lorsque l’API Web crée un contrôleur, elle appelle **BeginScope**. Cette méthode retourne un **IDependencyScope** qui représente une portée enfant.

L’API Web appelle ensuite **GetService** sur l’étendue enfant pour créer le contrôleur. Lorsque la demande est terminée, l’API Web appelle **dispose** sur l’étendue enfant. Utilisez la méthode **dispose** pour supprimer les dépendances du contrôleur.

La façon dont vous implémentez **BeginScope** dépend du conteneur IoC. Pour Unity, Scope correspond à un conteneur enfant :

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

La plupart des conteneurs IoC ont des équivalents similaires.
