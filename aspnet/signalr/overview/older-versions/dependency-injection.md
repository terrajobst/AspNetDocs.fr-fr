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
# <a name="dependency-injection-in-signalr-1x"></a>Injection de dépendances dans SignalR 1.x

par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

L’injection de dépendances est un moyen de supprimer les dépendances codées en dur entre les objets, ce qui facilite le remplacement des dépendances d’un objet, soit pour le test (à l’aide d’objets factices), soit pour modifier le comportement au moment de l’exécution. Ce didacticiel montre comment effectuer une injection de dépendances sur les concentrateurs Signalr. Il montre également comment utiliser des conteneurs IoC avec Signalr. Un conteneur IoC est un Framework général pour l’injection de dépendances.

## <a name="what-is-dependency-injection"></a>Qu’est-ce que l’injection de dépendance ?

Ignorez cette section si vous êtes déjà familiarisé avec l’injection de dépendances.

L' *injection de dépendances* (di) est un modèle dans lequel les objets ne sont pas responsables de la création de leurs propres dépendances. Voici un exemple simple pour motiver DI. Supposons que vous ayez un objet qui doit consigner des messages. Vous pouvez définir une interface de journalisation :

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Dans votre objet, vous pouvez créer un `ILogger` pour enregistrer les messages :

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Cela fonctionne, mais ce n’est pas la meilleure conception. Si vous souhaitez remplacer `FileLogger` par une autre implémentation de `ILogger`, vous devez modifier `SomeComponent`. Supposons que de nombreux autres objets utilisent `FileLogger`, vous devrez les modifier tous. Ou si vous décidez de créer `FileLogger` un singleton, vous devrez également apporter des modifications à l’ensemble de l’application.

Une meilleure approche consiste à « injecter » un `ILogger` dans l’objet, par exemple, à l’aide d’un argument de constructeur :

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

À présent, l’objet n’est pas responsable de la sélection des `ILogger` à utiliser. Vous pouvez basculer `ILogger` implémentations sans modifier les objets qui en dépendent.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Ce modèle est appelé [injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Un autre modèle est l’injection d’accesseurs set, où vous définissez la dépendance par le biais d’une méthode ou d’une propriété Setter.

## <a name="simple-dependency-injection-in-signalr"></a>Injection de dépendances simple dans Signalr

Prenons l’exemple de l’application conversation du didacticiel [prise en main avec signalr](../getting-started/tutorial-getting-started-with-signalr.md). Voici la classe de concentrateur de cette application :

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Supposons que vous souhaitez stocker les messages de conversation sur le serveur avant de les envoyer. Vous pouvez définir une interface qui soustrait cette fonctionnalité et utiliser DI pour injecter l’interface dans la classe `ChatHub`.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Le seul problème est qu’une application Signalr ne crée pas directement de concentrateurs ; Signalr les crée pour vous. Par défaut, Signalr attend qu’une classe de concentrateur ait un constructeur sans paramètre. Toutefois, vous pouvez facilement inscrire une fonction pour créer des instances de Hub et utiliser cette fonction pour exécuter DI. Inscrivez la fonction en appelant **GlobalHost. DependencyResolver. Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Désormais, Signalr appellera cette fonction anonyme chaque fois qu’elle a besoin de créer une instance `ChatHub`.

## <a name="ioc-containers"></a>Conteneurs IoC

Le code précédent est parfait pour les cas simples. Toutefois, vous deviez toujours écrire ceci :

[!code-css[Main](dependency-injection/samples/sample8.css)]

Dans une application complexe avec de nombreuses dépendances, vous devrez peut-être écrire un grand nombre de ce code de « câblage ». Ce code peut être difficile à gérer, en particulier si les dépendances sont imbriquées. Il est également difficile à effectuer des tests unitaires.

Une solution consiste à utiliser un conteneur IoC. Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances. Vous inscrivez les types avec le conteneur, puis vous utilisez le conteneur pour créer des objets. Le conteneur détermine automatiquement les relations de dépendance. De nombreux conteneurs IoC vous permettent également de contrôler des éléments tels que la durée de vie des objets et l’étendue.

> [!NOTE]
> « IoC » signifie « inversion de contrôle », qui est un modèle général dans lequel un Framework appelle le code d’application. Un conteneur IoC construit vos objets pour vous, ce qui « inverse » le déroulement habituel du contrôle.

## <a name="using-ioc-containers-in-signalr"></a>Utilisation de conteneurs IoC dans Signalr

L’application de conversation est probablement trop simple pour tirer parti d’un conteneur IoC. Examinons plutôt l’exemple [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .

L’exemple StockTicker définit deux classes principales :

- `StockTickerHub`: la classe de concentrateur, qui gère les connexions clientes.
- `StockTicker`: singleton qui contient des cours boursiers et les met à jour périodiquement.

`StockTickerHub` contient une référence au Singleton `StockTicker`, tandis que `StockTicker` contient une référence à **IHubConnectionContext** pour le `StockTickerHub`. Elle utilise cette interface pour communiquer avec les instances de `StockTickerHub`. (Pour plus d’informations, consultez diffusion sur le [serveur avec ASP.net signalr](index.md).)

Nous pouvons utiliser un conteneur IoC pour démêler ces dépendances un peu plus. Tout d’abord, nous allons simplifier les classes `StockTickerHub` et `StockTicker`. Dans le code suivant, j’ai commenté les parties dont nous n’avons pas besoin.

Supprimez le constructeur sans paramètre de `StockTicker`. Au lieu de cela, nous utiliserons toujours DI pour créer le Hub.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Pour StockTicker, supprimez l’instance singleton. Plus tard, nous allons utiliser le conteneur IoC pour contrôler la durée de vie de StockTicker. En outre, rendez le constructeur public.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Ensuite, nous pouvons Refactoriser le code en créant une interface pour `StockTicker`. Nous allons utiliser cette interface pour découpler le `StockTickerHub` de la classe `StockTicker`.

Visual Studio facilite ce type de refactorisation. Ouvrez le fichier StockTicker.cs, cliquez avec le bouton droit sur la déclaration de classe `StockTicker`, puis sélectionnez **Refactoriser** ... **Extraire l’interface**.

![](dependency-injection/_static/image1.png)

Dans la boîte de dialogue **extraire l’interface** , cliquez sur **Sélectionner tout**. Conservez les autres valeurs par défaut. Cliquez sur **OK**.

![](dependency-injection/_static/image2.png)

Visual Studio crée une nouvelle interface nommée `IStockTicker`et modifie également `StockTicker` pour dériver de `IStockTicker`.

Ouvrez le fichier IStockTicker.cs et remplacez l’interface par **public**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

Dans la classe `StockTickerHub`, modifiez les deux instances de `StockTicker` en `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

La création d’une interface `IStockTicker` n’est pas strictement nécessaire, mais je voulais montrer comment DI peut aider à réduire le couplage entre les composants de votre application.

## <a name="add-the-ninject-library"></a>Ajouter la bibliothèque Ninject

Il existe de nombreux conteneurs IoC Open source pour .NET. Pour ce didacticiel, je vais utiliser [Ninject](http://www.ninject.org/). (D’autres bibliothèques populaires incluent [Castle Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)et [StructureMap](http://docs.structuremap.net).)

Utilisez le gestionnaire de package NuGet pour installer la [bibliothèque Ninject](https://nuget.org/packages/Ninject/3.0.1.10). Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** > **console du gestionnaire de package**. Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Remplacer le programme de résolution de dépendance Signalr

Pour utiliser Ninject dans Signalr, créez une classe qui dérive de **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Cette classe remplace les méthodes **GetService** et **GetServices** de **DefaultDependencyResolver**. Signalr appelle ces méthodes pour créer divers objets au moment de l’exécution, y compris les instances de concentrateur, ainsi que divers services utilisés en interne par Signalr.

- La méthode **GetService** crée une seule instance d’un type. Substituez cette méthode pour appeler la méthode **opération TryGet** du noyau Ninject. Si cette méthode retourne la valeur null, revient au résolveur par défaut.
- La méthode **GetServices** crée une collection d’objets d’un type spécifié. Substituez cette méthode pour concaténer les résultats de Ninject avec les résultats du programme de résolution par défaut.

## <a name="configure-ninject-bindings"></a>Configurer des liaisons Ninject

À présent, nous allons utiliser Ninject pour déclarer des liaisons de type.

Ouvrez le fichier RegisterHubs.cs. Dans la méthode `RegisterHubs.Start`, créez le conteneur Ninject, qui Ninject appelle le *noyau*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Créez une instance de notre résolveur de dépendance personnalisé :

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Créez une liaison pour `IStockTicker` comme suit :

[!code-html[Main](dependency-injection/samples/sample17.html)]

Ce code dit deux choses. Tout d’abord, chaque fois que l’application a besoin d’une `IStockTicker`, le noyau doit créer une instance de `StockTicker`. Deuxièmement, la classe `StockTicker` doit être créée en tant qu’objet singleton. Ninject crée une instance de l’objet et retourne la même instance pour chaque requête.

Créez une liaison pour **IHubConnectionContext** comme suit :

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Ce code crée une fonction anonyme qui retourne un **IHubConnection**. La méthode **WhenInjectedInto** indique à Ninject d’utiliser cette fonction uniquement lors de la création d’instances de `IStockTicker`. Cela est dû au fait que Signalr crée des instances **IHubConnectionContext** en interne, et que nous ne souhaitons pas remplacer la manière dont signalr les crée. Cette fonction s’applique uniquement à notre classe `StockTicker`.

Transmettez le résolveur de dépendance dans la méthode **MapHubs** :

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Désormais, Signalr utilise le programme de résolution spécifié dans **MapHubs**, au lieu du programme de résolution par défaut.

Voici la liste complète du code pour `RegisterHubs.Start`.

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Pour exécuter l’application StockTicker dans Visual Studio, appuyez sur F5. Dans la fenêtre du navigateur, accédez à `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

L’application a exactement la même fonctionnalité qu’auparavant. (Pour obtenir une description, consultez [diffusion sur le serveur avec ASP.net signalr](index.md).) Nous n’avons pas modifié le comportement ; vous venez de rendre le code plus facile à tester, à gérer et à évoluer.
