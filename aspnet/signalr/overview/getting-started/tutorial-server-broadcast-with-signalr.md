---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Didacticiel : diffusion sur le serveur avec Signalr 2 | Microsoft Docs'
author: tdykstra
description: Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de diffusion serveur.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536602"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="ec9ec-103">Didacticiel : diffusion sur le serveur avec Signalr 2</span><span class="sxs-lookup"><span data-stu-id="ec9ec-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="ec9ec-104">Ce didacticiel montre comment créer une application Web qui utilise ASP.NET Signalr 2 pour fournir une fonctionnalité de diffusion serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="ec9ec-105">La diffusion serveur signifie que le serveur démarre les communications envoyées aux clients.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="ec9ec-106">L’application que vous allez créer dans ce didacticiel simule un ticker boursier, un scénario classique de fonctionnalités de diffusion serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="ec9ec-107">Régulièrement, le serveur met à jour de façon aléatoire les cotations boursières et diffuse les mises à jour à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="ec9ec-108">Dans le navigateur, les nombres et les symboles de la **modification** et **%** colonnes changent de manière dynamique en réponse aux notifications du serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="ec9ec-109">Si vous ouvrez des navigateurs supplémentaires sur la même URL, ils affichent tous les mêmes données et les mêmes modifications simultanément aux données.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Créer un site Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="ec9ec-111">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ec9ec-112">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="ec9ec-112">Create the project</span></span>
> * <span data-ttu-id="ec9ec-113">Configurer le code du serveur</span><span class="sxs-lookup"><span data-stu-id="ec9ec-113">Set up the server code</span></span>
> * <span data-ttu-id="ec9ec-114">Examiner le code du serveur</span><span class="sxs-lookup"><span data-stu-id="ec9ec-114">Examine the server code</span></span>
> * <span data-ttu-id="ec9ec-115">Configurer le code client</span><span class="sxs-lookup"><span data-stu-id="ec9ec-115">Set up the client code</span></span>
> * <span data-ttu-id="ec9ec-116">Examiner le code client</span><span class="sxs-lookup"><span data-stu-id="ec9ec-116">Examine the client code</span></span>
> * <span data-ttu-id="ec9ec-117">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="ec9ec-117">Test the application</span></span>
> * <span data-ttu-id="ec9ec-118">Activer la journalisation</span><span class="sxs-lookup"><span data-stu-id="ec9ec-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec9ec-119">Si vous ne souhaitez pas suivre les étapes de création de l’application, vous pouvez installer le package Signalr. Sample dans un nouveau projet d’application Web ASP.NET vide.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="ec9ec-120">Si vous installez le package NuGet sans effectuer les étapes de ce didacticiel, vous devez suivre les instructions du fichier *Readme. txt* .</span><span class="sxs-lookup"><span data-stu-id="ec9ec-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="ec9ec-121">Pour exécuter le package, vous devez ajouter une classe de démarrage OWIN qui appelle la méthode `ConfigureSignalR` dans le package installé.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="ec9ec-122">Vous recevrez une erreur si vous n’ajoutez pas la classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="ec9ec-123">Consultez la section [installer l’exemple StockTicker](#install-the-stockticker-sample) de cet article.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec9ec-124">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="ec9ec-124">Prerequisites</span></span>

* <span data-ttu-id="ec9ec-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la charge de travail **Développement ASP.NET et web**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="ec9ec-126">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="ec9ec-126">Create the project</span></span>

<span data-ttu-id="ec9ec-127">Cette section montre comment utiliser Visual Studio 2017 pour créer une application Web ASP.NET vide.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="ec9ec-128">Dans Visual Studio, créez une application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Créer un site Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="ec9ec-130">Dans la fenêtre **nouvelle application Web ASP.net-signalr. StockTicker** , laissez **vide** sélectionné et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="ec9ec-131">Configurer le code du serveur</span><span class="sxs-lookup"><span data-stu-id="ec9ec-131">Set up the server code</span></span>

<span data-ttu-id="ec9ec-132">Dans cette section, vous allez configurer le code qui s’exécute sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="ec9ec-133">Créer la classe stock</span><span class="sxs-lookup"><span data-stu-id="ec9ec-133">Create the Stock class</span></span>

<span data-ttu-id="ec9ec-134">Commencez par créer la classe de modèle *stock* que vous utiliserez pour stocker et transmettre des informations sur une action.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="ec9ec-135">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="ec9ec-136">Nommez la classe *stock* et ajoutez-la au projet.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="ec9ec-137">Remplacez le code du fichier *stock.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="ec9ec-138">Les deux propriétés que vous allez définir lorsque vous créez des actions sont `Symbol` (par exemple, MSFT pour Microsoft) et `Price`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="ec9ec-139">Les autres propriétés dépendent de la façon dont et quand vous définissez `Price`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="ec9ec-140">La première fois que vous définissez `Price`, la valeur est propagée à `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="ec9ec-141">Après cela, lorsque vous définissez `Price`, l’application calcule les valeurs de propriété `Change` et `PercentChange` en fonction de la différence entre `Price` et `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="ec9ec-142">Créer les classes StockTickerHub et StockTicker</span><span class="sxs-lookup"><span data-stu-id="ec9ec-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="ec9ec-143">Vous allez utiliser l’API Hub Signalr pour gérer l’interaction de serveur à client.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="ec9ec-144">Une classe `StockTickerHub` qui dérive de la classe Signalr `Hub` va gérer la réception des connexions et des appels de méthode à partir des clients.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-144">A `StockTickerHub` class that derives from the SignalR `Hub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="ec9ec-145">Vous devez également gérer les données boursières et exécuter un objet `Timer`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="ec9ec-146">L’objet `Timer` déclenche périodiquement des mises à jour de prix indépendamment des connexions clientes.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="ec9ec-147">Vous ne pouvez pas placer ces fonctions dans une classe `Hub`, car les hubs sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="ec9ec-148">L’application crée une instance de classe `Hub` pour chaque tâche sur le concentrateur, comme les connexions et les appels du client vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="ec9ec-149">Le mécanisme qui conserve les données boursières, met à jour les prix et diffuse les mises à jour de prix doit s’exécuter dans une classe distincte.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="ec9ec-150">Vous nommez la classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-150">You'll name the class `StockTicker`.</span></span>

![Diffusion à partir de StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="ec9ec-152">Vous souhaitez qu’une seule instance de la classe `StockTicker` s’exécute sur le serveur. vous devez donc configurer une référence de chaque `StockTickerHub` instance sur l’instance de `StockTicker` Singleton.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="ec9ec-153">La classe `StockTicker` doit être diffusée aux clients, car elle contient les données boursières et déclenche les mises à jour, mais `StockTicker` n’est pas une classe `Hub`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="ec9ec-154">La classe `StockTicker` doit obtenir une référence à l’objet de contexte de connexion du concentrateur Signalr.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="ec9ec-155">Il peut ensuite utiliser l’objet de contexte de connexion Signalr pour la diffusion vers les clients.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="ec9ec-156">Créer StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="ec9ec-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="ec9ec-157">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="ec9ec-158">Dans **Ajouter un nouvel élément-signalr. StockTicker**, **Sélectionnez installé** > **Visual C#**  > **Web** > **signalr** , puis sélectionnez **classe de concentrateur signalr (v2)** .</span><span class="sxs-lookup"><span data-stu-id="ec9ec-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="ec9ec-159">Nommez la classe *StockTickerHub* et ajoutez-la au projet.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="ec9ec-160">Cette étape crée le fichier de classe *StockTickerHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="ec9ec-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="ec9ec-161">Simultanément, elle ajoute un ensemble de fichiers de script et de références d’assembly qui prend en charge Signalr au projet.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="ec9ec-162">Remplacez le code du fichier *StockTickerHub.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="ec9ec-163">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-163">Save the file.</span></span>

<span data-ttu-id="ec9ec-164">L’application utilise la classe de [concentrateur](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) pour définir les méthodes que les clients peuvent appeler sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="ec9ec-165">Vous définissez une méthode : `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="ec9ec-166">Lorsqu’un client se connecte initialement au serveur, il appelle cette méthode pour obtenir la liste de tous les stocks avec leurs prix actuels.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="ec9ec-167">La méthode peut s’exécuter de façon synchrone et retourner `IEnumerable<Stock>`, car elle retourne des données à partir de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="ec9ec-168">Si la méthode devait obtenir les données en réalisant une opération impliquant l’attente, comme une recherche de base de données ou un appel de service Web, vous spécifiez `Task<IEnumerable<Stock>>` comme valeur de retour pour activer le traitement asynchrone.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="ec9ec-169">Pour plus d’informations, consultez Guide de l' [API hubs signalr ASP.net-serveur-quand exécuter de manière asynchrone](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="ec9ec-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="ec9ec-170">L’attribut `HubName` spécifie la manière dont l’application fait référence au hub dans le code JavaScript sur le client.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="ec9ec-171">Le nom par défaut sur le client si vous n’utilisez pas cet attribut, est une version la casse mixte du nom de la classe, qui dans ce cas est `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="ec9ec-172">Comme vous le verrez plus tard lorsque vous créerez la classe `StockTicker`, l’application crée une instance singleton de cette classe dans sa propriété statique `Instance`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="ec9ec-173">Cette instance singleton de `StockTicker` est en mémoire, quel que soit le nombre de clients qui se connectent ou se déconnectent.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="ec9ec-174">Cette instance est celle utilisée par la méthode `GetAllStocks()` pour retourner les informations boursières actuelles.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="ec9ec-175">Créer StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="ec9ec-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="ec9ec-176">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="ec9ec-177">Nommez la classe *StockTicker* et ajoutez-la au projet.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="ec9ec-178">Remplacez le code du fichier *stockticker.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="ec9ec-179">Étant donné que tous les threads exécuteront la même instance du code StockTicker, la classe StockTicker doit être thread-safe.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="ec9ec-180">Examiner le code du serveur</span><span class="sxs-lookup"><span data-stu-id="ec9ec-180">Examine the server code</span></span>

<span data-ttu-id="ec9ec-181">Si vous examinez le code du serveur, il vous aidera à comprendre le fonctionnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="ec9ec-182">Stockage de l’instance singleton dans un champ statique</span><span class="sxs-lookup"><span data-stu-id="ec9ec-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="ec9ec-183">Le code initialise le champ `_instance` statique qui stocke la propriété `Instance` avec une instance de la classe.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="ec9ec-184">Étant donné que le constructeur est privé, il s’agit de la seule instance de la classe que l’application peut créer.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="ec9ec-185">L’application utilise l' [initialisation tardive](/dotnet/framework/performance/lazy-initialization) pour le champ `_instance`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="ec9ec-186">Ce n’est pas pour des raisons de performances.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-186">It's not for performance reasons.</span></span> <span data-ttu-id="ec9ec-187">Cela permet de s’assurer que la création de l’instance est thread-safe.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="ec9ec-188">Chaque fois qu’un client se connecte au serveur, une nouvelle instance de la classe StockTickerHub qui s’exécute dans un thread distinct obtient l’instance singleton de StockTicker à partir de la propriété statique `StockTicker.Instance`, comme vous l’avez vu précédemment dans la classe `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="ec9ec-189">Stockage des données boursières dans un ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="ec9ec-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="ec9ec-190">Le constructeur initialise la collection `_stocks` avec des exemples de données stock et `GetAllStocks` retourne les stocks.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="ec9ec-191">Comme vous l’avez vu précédemment, cette collection de stocks est retournée par `StockTickerHub.GetAllStocks`, qui est une méthode serveur dans la classe `Hub` que les clients peuvent appeler.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="ec9ec-192">La collection d’actions est définie en tant que type [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) pour la sécurité des threads.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="ec9ec-193">Vous pouvez également utiliser un objet [dictionnaire](https://msdn.microsoft.com/library/xfhwa508.aspx) et verrouiller explicitement le dictionnaire lorsque vous y apportez des modifications.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="ec9ec-194">Pour cet exemple d’application, il est OK de stocker les données d’application en mémoire et de perdre les données quand l’application supprime l’instance de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="ec9ec-195">Dans une application réelle, vous travaillez avec un magasin de données principal comme une base de données.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="ec9ec-196">Mise à jour périodique des cours boursiers</span><span class="sxs-lookup"><span data-stu-id="ec9ec-196">Periodically updating stock prices</span></span>

<span data-ttu-id="ec9ec-197">Le constructeur démarre un objet `Timer` qui appelle périodiquement des méthodes qui mettent à jour les cotations boursières de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="ec9ec-198">`Timer` appelle `UpdateStockPrices`, qui passe la valeur null dans le paramètre d’État.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="ec9ec-199">Avant de mettre à jour les tarifs, l’application prend un verrou sur l’objet `_updateStockPricesLock`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="ec9ec-200">Le code vérifie si un autre thread est déjà en cours de mise à jour des prix, puis appelle `TryUpdateStockPrice` sur chaque stock de la liste.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="ec9ec-201">La méthode `TryUpdateStockPrice` décide s’il faut modifier le cours de l’action et la quantité de modification.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="ec9ec-202">Si le cours de l’action change, l’application appelle `BroadcastStockPrice` pour diffuser la modification du cours de l’action à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="ec9ec-203">L’indicateur de `_updatingStockPrices` désigné comme [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) pour s’assurer qu’il est thread-safe.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="ec9ec-204">Dans une application réelle, la méthode `TryUpdateStockPrice` appelle un service Web pour rechercher le prix.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="ec9ec-205">Dans ce code, l’application utilise un générateur de nombres aléatoires pour apporter des modifications de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="ec9ec-206">Obtention du contexte Signalr afin que la classe StockTicker puisse être diffusée aux clients</span><span class="sxs-lookup"><span data-stu-id="ec9ec-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="ec9ec-207">Étant donné que les changements de prix proviennent ici dans l’objet `StockTicker`, il s’agit de l’objet qui doit appeler une méthode `updateStockPrice` sur tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="ec9ec-208">Dans une classe `Hub`, vous avez une API pour appeler des méthodes clientes, mais `StockTicker` ne dérive pas de la classe `Hub` et n’a pas de référence à un objet `Hub`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="ec9ec-209">Pour la diffusion vers des clients connectés, la classe `StockTicker` doit obtenir l’instance de contexte Signalr pour la classe `StockTickerHub` et l’utiliser pour appeler des méthodes sur les clients.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="ec9ec-210">Le code obtient une référence au contexte de Signalr lorsqu’il crée l’instance de classe singleton, passe cette référence au constructeur, et le constructeur le place dans la propriété `Clients`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="ec9ec-211">Il existe deux raisons pour lesquelles vous ne souhaitez obtenir le contexte qu’une seule fois : l’obtention du contexte est une tâche coûteuse et l’obtention d’une fois garantit que l’application conserve l’ordre prévu des messages envoyés aux clients.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="ec9ec-212">L’obtention de la propriété `Clients` du contexte et son ajout dans la propriété `StockTickerClient` vous permet d’écrire du code pour appeler des méthodes clientes qui se présente comme dans une classe `Hub`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="ec9ec-213">Par exemple, pour diffuser sur tous les clients, vous pouvez écrire des `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="ec9ec-214">La méthode `updateStockPrice` que vous appelez dans `BroadcastStockPrice` n’existe pas encore.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="ec9ec-215">Vous l’ajouterez ultérieurement lorsque vous écrirez du code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="ec9ec-216">Vous pouvez vous référer à `updateStockPrice` ici, car `Clients.All` est dynamique, ce qui signifie que l’application évalue l’expression au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="ec9ec-217">Lorsque cet appel de méthode s’exécute, Signalr envoie le nom de la méthode et la valeur du paramètre au client, et si le client a une méthode nommée `updateStockPrice`, l’application appelle cette méthode et lui transmet la valeur de paramètre.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="ec9ec-218">`Clients.All` signifie envoyer à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="ec9ec-219">Signalr vous offre d’autres options pour spécifier les clients ou groupes de clients à envoyer à.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="ec9ec-220">Pour plus d’informations, consultez [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="ec9ec-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="ec9ec-221">Inscrire l’itinéraire Signalr</span><span class="sxs-lookup"><span data-stu-id="ec9ec-221">Register the SignalR route</span></span>

<span data-ttu-id="ec9ec-222">Le serveur doit connaître l’URL à intercepter et diriger vers Signalr.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="ec9ec-223">Pour ce faire, ajoutez une classe de démarrage OWIN :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="ec9ec-224">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="ec9ec-225">Dans **Ajouter un nouvel élément-signalr. StockTicker** , sélectionnez **installé** > **Visual C#**  > **Web** , puis sélectionnez **classe de démarrage OWIN**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="ec9ec-226">Nommez le *démarrage* de la classe, puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="ec9ec-227">Remplacez le code par défaut dans le fichier *Startup.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="ec9ec-228">Vous avez maintenant terminé la configuration du code serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="ec9ec-229">Dans la section suivante, vous allez configurer le client.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="ec9ec-230">Configurer le code client</span><span class="sxs-lookup"><span data-stu-id="ec9ec-230">Set up the client code</span></span>

<span data-ttu-id="ec9ec-231">Dans cette section, vous allez configurer le code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="ec9ec-232">Créer la page HTML et le fichier JavaScript</span><span class="sxs-lookup"><span data-stu-id="ec9ec-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="ec9ec-233">La page HTML affichera les données et le fichier JavaScript organisera les données.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="ec9ec-234">Créer StockTicker. html</span><span class="sxs-lookup"><span data-stu-id="ec9ec-234">Create StockTicker.html</span></span>

<span data-ttu-id="ec9ec-235">Tout d’abord, vous allez ajouter le client HTML.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="ec9ec-236">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter** > **page HTML**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="ec9ec-237">Nommez le fichier *StockTicker* , puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="ec9ec-238">Remplacez le code par défaut dans le fichier *stockticker. html* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="ec9ec-239">Le code HTML crée une table avec cinq colonnes, une ligne d’en-tête et une ligne de données avec une seule cellule qui s’étend sur les cinq colonnes.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="ec9ec-240">La ligne de données affiche « chargement en cours... » momentanément au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="ec9ec-241">Le code JavaScript supprimera cette ligne et ajoutera à ses lignes de place les données de stock extraites du serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="ec9ec-242">Les balises de script spécifient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-242">The script tags specify:</span></span>

    * <span data-ttu-id="ec9ec-243">Fichier de script jQuery.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-243">The jQuery script file.</span></span>

    * <span data-ttu-id="ec9ec-244">Fichier de script du noyau Signalr.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="ec9ec-245">Fichier de script des proxies Signalr.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="ec9ec-246">Un fichier de script StockTicker que vous allez créer ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="ec9ec-247">L’application génère dynamiquement le fichier de script des proxies Signalr.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="ec9ec-248">Il spécifie l’URL « /signalr/hubs » et définit les méthodes proxy pour les méthodes sur la classe de concentrateur, dans ce cas, pour `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="ec9ec-249">Si vous préférez, vous pouvez générer ce fichier JavaScript manuellement à l’aide des [utilitaires signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="ec9ec-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="ec9ec-250">N’oubliez pas de désactiver la création de fichiers dynamiques dans l’appel de méthode `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="ec9ec-251">Dans **Explorateur de solutions**, développez **scripts**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="ec9ec-252">Les bibliothèques de scripts pour jQuery et Signalr sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ec9ec-253">Le gestionnaire de package installe une version plus récente des scripts Signalr.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="ec9ec-254">Mettez à jour les références de script dans le bloc de code afin qu’elles correspondent aux versions des fichiers de script dans le projet.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="ec9ec-255">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *stockticker. html*, puis sélectionnez **définir comme page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="ec9ec-256">Créer StockTicker. js</span><span class="sxs-lookup"><span data-stu-id="ec9ec-256">Create StockTicker.js</span></span>

<span data-ttu-id="ec9ec-257">Créez maintenant le fichier JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="ec9ec-258">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **fichier JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="ec9ec-259">Nommez le fichier *StockTicker* , puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="ec9ec-260">Ajoutez ce code au fichier *stockticker. js* :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="ec9ec-261">Examiner le code client</span><span class="sxs-lookup"><span data-stu-id="ec9ec-261">Examine the client code</span></span>

<span data-ttu-id="ec9ec-262">Si vous examinez le code client, cela vous permettra d’apprendre comment le code client interagit avec le code serveur pour que l’application fonctionne.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="ec9ec-263">Démarrage de la connexion</span><span class="sxs-lookup"><span data-stu-id="ec9ec-263">Starting the connection</span></span>

<span data-ttu-id="ec9ec-264">`$.connection` fait référence aux proxies Signalr.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="ec9ec-265">Le code obtient une référence au proxy pour la classe `StockTickerHub` et le place dans la variable `ticker`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="ec9ec-266">Le nom du proxy est le nom qui a été défini par l’attribut `HubName` :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="ec9ec-267">Une fois que vous avez défini toutes les variables et fonctions, la dernière ligne de code du fichier initialise la connexion Signalr en appelant la fonction Signalr `start`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="ec9ec-268">La fonction `start` s’exécute de façon asynchrone et retourne un [objet différé jQuery](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="ec9ec-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="ec9ec-269">Vous pouvez appeler la fonction Done pour spécifier la fonction à appeler lorsque l’application termine l’action asynchrone.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="ec9ec-270">Obtention de tous les stocks</span><span class="sxs-lookup"><span data-stu-id="ec9ec-270">Getting all the stocks</span></span>

<span data-ttu-id="ec9ec-271">La fonction `init` appelle la fonction `getAllStocks` sur le serveur et utilise les informations renvoyées par le serveur pour mettre à jour la table stock.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="ec9ec-272">Notez que, par défaut, vous devez utiliser camelCasing sur le client, même si le nom de la méthode est en Pascal sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="ec9ec-273">La règle camelCasing s’applique uniquement aux méthodes, pas aux objets.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="ec9ec-274">Par exemple, vous faites référence à `stock.Symbol` et `stock.Price`, et non à `stock.symbol` ou `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="ec9ec-275">Dans la méthode `init`, l’application crée du code HTML pour une ligne de table pour chaque objet stock reçu du serveur en appelant `formatStock` pour mettre en forme les propriétés de l’objet `stock`, puis en appelant `supplant` pour remplacer les espaces réservés dans la variable `rowTemplate` par les valeurs de propriété de l’objet `stock`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="ec9ec-276">Le code HTML qui en résulte est ensuite ajouté à la table stock.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="ec9ec-277">Vous appelez `init` en le transmettant en tant que fonction `callback` qui s’exécute une fois la fonction de `start` asynchrone terminée.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="ec9ec-278">Si vous avez appelé `init` en tant qu’instruction JavaScript distincte après l’appel de `start`, la fonction échoue parce qu’elle s’exécute immédiatement sans attendre la fin de l’établissement de la connexion par la fonction Start.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="ec9ec-279">Dans ce cas, la fonction `init` essaiera d’appeler la fonction `getAllStocks` avant que l’application établisse une connexion au serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="ec9ec-280">Obtention des cours mis à jour</span><span class="sxs-lookup"><span data-stu-id="ec9ec-280">Getting updated stock prices</span></span>

<span data-ttu-id="ec9ec-281">Lorsque le serveur modifie le prix d’une action, il appelle la `updateStockPrice` sur les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="ec9ec-282">L’application ajoute la fonction à la propriété cliente du proxy `stockTicker` pour la rendre disponible aux appels à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="ec9ec-283">La fonction `updateStockPrice` met en forme un objet stock reçu à partir du serveur dans une ligne de table de la même façon que dans la fonction `init`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="ec9ec-284">Au lieu d’ajouter la ligne à la table, elle trouve la ligne active du stock dans la table et remplace cette ligne par la nouvelle.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="ec9ec-285">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="ec9ec-285">Test the application</span></span>

<span data-ttu-id="ec9ec-286">Vous pouvez tester l’application pour vous assurer qu’elle fonctionne correctement.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="ec9ec-287">Toutes les fenêtres de navigateur affichent la table des stocks en direct avec des fluctuations des cours.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="ec9ec-288">Dans la barre d’outils, activez le **débogage de script** , puis sélectionnez le bouton de lecture pour exécuter l’application en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Capture d’écran de l’activation du mode de débogage par l’utilisateur et de la sélection de lecture.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="ec9ec-290">Une fenêtre de navigateur s’ouvre et affiche la **table des stocks en direct**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="ec9ec-291">La table stock affiche initialement le chargement... puis, après une brève période, l’application affiche les données boursières initiales, puis les cours des actions commencent à changer.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="ec9ec-292">Copiez l’URL à partir du navigateur, ouvrez deux autres navigateurs, puis collez les URL dans les barres d’adresses.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="ec9ec-293">L’affichage du stock initial est le même que celui du premier navigateur et les modifications se produisent simultanément.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="ec9ec-294">Fermez tous les navigateurs, ouvrez un nouveau navigateur, puis accédez à la même URL.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="ec9ec-295">L’objet singleton de StockTicker a continué à s’exécuter sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="ec9ec-296">La **table** des stocks en direct indique que les stocks ont continué à changer.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="ec9ec-297">Vous ne voyez pas la table initiale avec des chiffres de modification zéro.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="ec9ec-298">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="ec9ec-299">Activer la journalisation</span><span class="sxs-lookup"><span data-stu-id="ec9ec-299">Enable logging</span></span>

<span data-ttu-id="ec9ec-300">Signalr possède une fonction de journalisation intégrée que vous pouvez activer sur le client pour faciliter la résolution des problèmes.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="ec9ec-301">Dans cette section, vous allez activer la journalisation et voir des exemples montrant comment les journaux vous indiquent les méthodes de transport signalant les suivantes :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="ec9ec-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), pris en charge par IIS 8 et les navigateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="ec9ec-303">[Événements envoyés par le serveur](http://en.wikipedia.org/wiki/Server-sent_events), pris en charge par les navigateurs autres qu’Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="ec9ec-304">[Cadre illimité](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), pris en charge par Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="ec9ec-305">[Interrogation longue Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), prise en charge par tous les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="ec9ec-306">Pour une connexion donnée, Signalr choisit la meilleure méthode de transport que le serveur et le client prennent en charge.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="ec9ec-307">Ouvrez *stockticker. js*.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="ec9ec-308">Ajoutez cette ligne de code en surbrillance pour activer la journalisation juste avant le code qui initialise la connexion à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="ec9ec-309">Appuyez sur **F5** pour exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="ec9ec-310">Ouvrez la fenêtre outils de développement de votre navigateur, puis sélectionnez la console pour afficher les journaux.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="ec9ec-311">Vous devrez peut-être actualiser la page pour afficher les journaux de Signalr qui négocient la méthode de transport pour une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="ec9ec-312">Si vous exécutez Internet Explorer 10 sur Windows 8 (IIS 8), la méthode de transport est **WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="ec9ec-313">Si vous exécutez Internet Explorer 10 sur Windows 7 (IIS 7,5), la méthode de transport est **IFRAME**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="ec9ec-314">Si vous exécutez Firefox 19 sur Windows 8 (IIS 8), la méthode de transport est **WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="ec9ec-315">Dans Firefox, installez le complément Firebug pour obtenir une fenêtre de console.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="ec9ec-316">Si vous exécutez Firefox 19 sur Windows 7 (IIS 7,5), la méthode de transport est **un événement envoyé par le serveur** .</span><span class="sxs-lookup"><span data-stu-id="ec9ec-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="ec9ec-317">Installer l’exemple StockTicker</span><span class="sxs-lookup"><span data-stu-id="ec9ec-317">Install the StockTicker sample</span></span>

<span data-ttu-id="ec9ec-318">[Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installe l’application stockticker.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="ec9ec-319">Le package NuGet comprend plus de fonctionnalités que la version simplifiée que vous avez créée à partir de zéro.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="ec9ec-320">Dans cette section du didacticiel, vous installez le package NuGet et passez en revue les nouvelles fonctionnalités et le code qui les implémente.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec9ec-321">Si vous installez le package sans effectuer les étapes précédentes de ce didacticiel, vous devez ajouter une classe de démarrage OWIN à votre projet.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="ec9ec-322">Ce fichier Readme. txt pour le package NuGet explique cette étape.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="ec9ec-323">Installez Signalr. exemple de package NuGet</span><span class="sxs-lookup"><span data-stu-id="ec9ec-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="ec9ec-324">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="ec9ec-325">Dans le **Gestionnaire de package NuGet : signalr. StockTicker**, sélectionnez **Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="ec9ec-326">À partir de la **source du package**, sélectionnez **NuGet.org**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="ec9ec-327">Entrez *signalr. Sample* dans la zone de recherche et sélectionnez **Microsoft. Aspnet. signalr. Sample** > **installer**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="ec9ec-328">Dans **Explorateur de solutions**, développez le dossier *signalr. Sample* .</span><span class="sxs-lookup"><span data-stu-id="ec9ec-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="ec9ec-329">Installation du package Signalr. l’exemple a créé le dossier et son contenu.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="ec9ec-330">Dans le dossier *signalr. Sample* , cliquez avec le bouton droit sur *stockticker. html*, puis sélectionnez **définir comme page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ec9ec-331">Installation de Signalr. l’exemple de package NuGet peut modifier la version de jQuery que vous avez dans votre dossier *scripts* .</span><span class="sxs-lookup"><span data-stu-id="ec9ec-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="ec9ec-332">Le nouveau fichier *stockticker. html* que le package installe dans le dossier *signalr. Sample* sera synchronisé avec la version jQuery installée par le package, mais si vous souhaitez réexécuter votre fichier *stockticker. html* d’origine, vous devrez peut-être mettre à jour la référence jQuery dans la balise de script en premier.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="ec9ec-333">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="ec9ec-333">Run the application</span></span>

 <span data-ttu-id="ec9ec-334">La table que vous avez vues dans la première application avait des fonctionnalités utiles.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="ec9ec-335">L’application Full stock ticker affiche de nouvelles fonctionnalités : une fenêtre de défilement horizontal qui affiche les données boursières et les stocks qui changent de couleur à mesure qu’ils s’élèvent et chutent.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="ec9ec-336">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="ec9ec-337">Lorsque vous exécutez l’application pour la première fois, le « marché » est « fermé » et vous voyez une table statique et une fenêtre de téléscripteur qui ne défile pas.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="ec9ec-338">Sélectionnez **Open Market**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-338">Select **Open Market**.</span></span>

    ![Capture d’écran du téléscripteur en direct.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="ec9ec-340">La zone de cotations **boursières dynamiques** commence à faire défiler horizontalement et le serveur commence à diffuser régulièrement les modifications du cours de l’action sur une base aléatoire.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="ec9ec-341">Chaque fois qu’un cours boursier change, l’application met à jour la **table des stocks en direct** et le **téléscripteur en direct**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="ec9ec-342">Lorsque le changement de prix d’une action est positif, l’application affiche l’action avec un arrière-plan vert.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="ec9ec-343">Lorsque la modification est négative, l’application affiche le brut avec un arrière-plan rouge.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="ec9ec-344">Sélectionnez **Fermer le marché**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="ec9ec-345">La table met à jour stop.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-345">The table updates stop.</span></span>

    * <span data-ttu-id="ec9ec-346">Le téléscripteur cesse de faire défiler.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="ec9ec-347">Sélectionnez **Réinitialiser**.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-347">Select **Reset**.</span></span>

    * <span data-ttu-id="ec9ec-348">Toutes les données boursières sont réinitialisées.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-348">All stock data is reset.</span></span>

    * <span data-ttu-id="ec9ec-349">L’application restaure l’état initial avant le début des changements de prix.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="ec9ec-350">Copiez l’URL à partir du navigateur, ouvrez deux autres navigateurs, puis collez les URL dans les barres d’adresses.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="ec9ec-351">Les mêmes données sont mises à jour de manière dynamique en même temps dans chaque navigateur.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="ec9ec-352">Lorsque vous sélectionnez l’un des contrôles, tous les navigateurs répondent de la même manière en même temps.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="ec9ec-353">Affichage de cotations boursières en direct</span><span class="sxs-lookup"><span data-stu-id="ec9ec-353">Live Stock Ticker display</span></span>

<span data-ttu-id="ec9ec-354">L’affichage de **cotations boursières en direct** est une liste non triée dans un élément `<div>` mis en forme en une seule ligne par les styles CSS.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="ec9ec-355">L’application initialise et met à jour le téléscripteur de la même façon que la table : en remplaçant les espaces réservés dans une chaîne de modèle `<li>` et en ajoutant dynamiquement les éléments `<li>` à l’élément `<ul>`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="ec9ec-356">L’application comprend le défilement à l’aide de la fonction jQuery `animate` pour faire varier la marge à gauche de la liste non triée dans le `<div>`.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="ec9ec-357">Signalr. exemple de StockTicker. html</span><span class="sxs-lookup"><span data-stu-id="ec9ec-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="ec9ec-358">Code HTML du téléscripteur de stock :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="ec9ec-359">Signalr. exemple de StockTicker. CSS</span><span class="sxs-lookup"><span data-stu-id="ec9ec-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="ec9ec-360">Code CSS du ticker de stock :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="ec9ec-361">Signalr. exemple Signalr. StockTicker. js</span><span class="sxs-lookup"><span data-stu-id="ec9ec-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="ec9ec-362">Code jQuery qui le fait défiler :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="ec9ec-363">Méthodes supplémentaires sur le serveur que le client peut appeler</span><span class="sxs-lookup"><span data-stu-id="ec9ec-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="ec9ec-364">Pour ajouter de la flexibilité à l’application, il existe des méthodes supplémentaires que l’application peut appeler.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="ec9ec-365">Signalr. exemple StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="ec9ec-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="ec9ec-366">La classe `StockTickerHub` définit quatre méthodes supplémentaires que le client peut appeler :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="ec9ec-367">L’application appelle `OpenMarket`, `CloseMarket`et `Reset` en réponse aux boutons situés en haut de la page.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="ec9ec-368">Elles illustrent le modèle d’un client déclenchant une modification de l’État immédiatement propagé à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="ec9ec-369">Chacune de ces méthodes appelle une méthode dans la classe `StockTicker` qui provoque la modification de l’état du marché, puis diffuse le nouvel État.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="ec9ec-370">Signalr. exemple StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="ec9ec-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="ec9ec-371">Dans la classe `StockTicker`, l’application maintient l’état du marché avec une propriété `MarketState` qui retourne une valeur d’énumération `MarketState` :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="ec9ec-372">Chacune des méthodes qui modifient l’état du marché le fait à l’intérieur d’un bloc de verrouillage, car la classe `StockTicker` doit être thread-safe :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="ec9ec-373">Pour vous assurer que ce code est thread-safe, le champ `_marketState` qui stocke la propriété `MarketState` désignée `volatile`:</span><span class="sxs-lookup"><span data-stu-id="ec9ec-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="ec9ec-374">Les méthodes `BroadcastMarketStateChange` et `BroadcastMarketReset` sont similaires à la méthode BroadcastStockPrice que vous avez déjà vue, à ceci près qu’elles appellent différentes méthodes définies sur le client :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="ec9ec-375">Fonctions supplémentaires sur le client que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="ec9ec-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="ec9ec-376">La fonction `updateStockPrice` gère désormais à la fois la table et l’affichage du téléscripteur, et elle utilise `jQuery.Color` pour flasher les couleurs rouges et vertes.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="ec9ec-377">Nouvelles fonctions dans *signalr. stockticker. js* activent et désactivent les boutons en fonction de l’état du marché.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="ec9ec-378">Ils arrêtent ou démarrent également le défilement horizontal des cotations **boursières dynamiques** .</span><span class="sxs-lookup"><span data-stu-id="ec9ec-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="ec9ec-379">Étant donné que de nombreuses fonctions sont ajoutées à `ticker.client`, l’application utilise la [fonction d’extension jQuery](http://api.jquery.com/jQuery.extend/) pour les ajouter.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="ec9ec-380">Configuration supplémentaire du client après l’établissement de la connexion</span><span class="sxs-lookup"><span data-stu-id="ec9ec-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="ec9ec-381">Une fois que le client a établi la connexion, il a d’autres tâches à effectuer :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="ec9ec-382">Déterminez si le marché est ouvert ou fermé pour appeler la fonction de `marketOpened` ou de `marketClosed` appropriée.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="ec9ec-383">Attachez les appels de méthode serveur aux boutons.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="ec9ec-384">Les méthodes de serveur ne sont pas raccordées aux boutons tant que l’application n’a pas établi la connexion.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="ec9ec-385">C’est pourquoi le code ne peut pas appeler les méthodes du serveur avant qu’elles ne soient disponibles.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ec9ec-386">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ec9ec-386">Additional resources</span></span>

<span data-ttu-id="ec9ec-387">Dans ce didacticiel, vous avez appris à programmer une application Signalr qui diffuse les messages du serveur à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="ec9ec-388">Vous pouvez désormais diffuser des messages régulièrement et en réponse aux notifications de n’importe quel client.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="ec9ec-389">Vous pouvez utiliser le concept d’instance de Singleton multithread pour maintenir l’état du serveur dans les scénarios de jeux en ligne multi-joueurs.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="ec9ec-390">Pour obtenir un exemple, consultez [le jeu de pousseurs basé sur signalr](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="ec9ec-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="ec9ec-391">Pour obtenir des didacticiels montrant les scénarios de communication d’égal à égal, consultez [prise en main avec signalr](introduction-to-signalr.md) et [mise à jour en temps réel avec signalr](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="ec9ec-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="ec9ec-392">Pour plus d’informations sur Signalr, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="ec9ec-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="ec9ec-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="ec9ec-394">Projet signalr</span><span class="sxs-lookup"><span data-stu-id="ec9ec-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="ec9ec-395">Signalr GitHub et exemples</span><span class="sxs-lookup"><span data-stu-id="ec9ec-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="ec9ec-396">Wiki signalr</span><span class="sxs-lookup"><span data-stu-id="ec9ec-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="ec9ec-397">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ec9ec-397">Next steps</span></span>

<span data-ttu-id="ec9ec-398">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ec9ec-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ec9ec-399">Création du projet</span><span class="sxs-lookup"><span data-stu-id="ec9ec-399">Created the project</span></span>
> * <span data-ttu-id="ec9ec-400">Configurer le code du serveur</span><span class="sxs-lookup"><span data-stu-id="ec9ec-400">Set up the server code</span></span>
> * <span data-ttu-id="ec9ec-401">Examen du code du serveur</span><span class="sxs-lookup"><span data-stu-id="ec9ec-401">Examined the server code</span></span>
> * <span data-ttu-id="ec9ec-402">Configurer le code client</span><span class="sxs-lookup"><span data-stu-id="ec9ec-402">Set up the client code</span></span>
> * <span data-ttu-id="ec9ec-403">Examen du code client</span><span class="sxs-lookup"><span data-stu-id="ec9ec-403">Examined the client code</span></span>
> * <span data-ttu-id="ec9ec-404">Test de l’application</span><span class="sxs-lookup"><span data-stu-id="ec9ec-404">Tested the application</span></span>
> * <span data-ttu-id="ec9ec-405">Journalisation activée</span><span class="sxs-lookup"><span data-stu-id="ec9ec-405">Enabled logging</span></span>

<span data-ttu-id="ec9ec-406">Passez à l’article suivant pour apprendre à créer une application Web en temps réel qui utilise ASP.NET Signalr 2.</span><span class="sxs-lookup"><span data-stu-id="ec9ec-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ec9ec-407">Créer une application Web en temps réel avec Signalr</span><span class="sxs-lookup"><span data-stu-id="ec9ec-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)
