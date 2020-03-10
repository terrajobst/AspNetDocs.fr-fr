---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Créer le client JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 74f2cc4e5e401d690042b05b028dfc0c46ae282a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622345"
---
# <a name="create-the-javascript-client"></a><span data-ttu-id="4edc7-102">Créer le client JavaScript</span><span class="sxs-lookup"><span data-stu-id="4edc7-102">Create the JavaScript Client</span></span>

<span data-ttu-id="4edc7-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4edc7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4edc7-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="4edc7-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="4edc7-105">Dans cette section, vous allez créer le client pour l’application, à l’aide de HTML, JavaScript et de la bibliothèque [Knockout. js](http://knockoutjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="4edc7-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="4edc7-106">Nous allons créer l’application cliente par étapes :</span><span class="sxs-lookup"><span data-stu-id="4edc7-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="4edc7-107">Afficher une liste de livres.</span><span class="sxs-lookup"><span data-stu-id="4edc7-107">Showing a list of books.</span></span>
- <span data-ttu-id="4edc7-108">Présentation des détails d’un livre.</span><span class="sxs-lookup"><span data-stu-id="4edc7-108">Showing a book detail.</span></span>
- <span data-ttu-id="4edc7-109">Ajout d’un nouveau livre.</span><span class="sxs-lookup"><span data-stu-id="4edc7-109">Adding a new book.</span></span>

<span data-ttu-id="4edc7-110">La bibliothèque Knockout utilise le modèle MVVM (Model-View-ViewModel) :</span><span class="sxs-lookup"><span data-stu-id="4edc7-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="4edc7-111">Le **modèle** est la représentation côté serveur des données dans le domaine d’entreprise (dans notre cas, livres et auteurs).</span><span class="sxs-lookup"><span data-stu-id="4edc7-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="4edc7-112">La **vue** est la couche de présentation (html).</span><span class="sxs-lookup"><span data-stu-id="4edc7-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="4edc7-113">Le **modèle de vue** est un objet JavaScript qui contient les modèles.</span><span class="sxs-lookup"><span data-stu-id="4edc7-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="4edc7-114">Le modèle de vue est une abstraction du code de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="4edc7-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="4edc7-115">Il n’a aucune connaissance de la représentation HTML.</span><span class="sxs-lookup"><span data-stu-id="4edc7-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="4edc7-116">Au lieu de cela, il représente les fonctionnalités abstraites de la vue, par exemple &quot;une liste de livres&quot;.</span><span class="sxs-lookup"><span data-stu-id="4edc7-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="4edc7-117">La vue est liée aux données du modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="4edc7-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="4edc7-118">Les mises à jour du modèle de vue sont automatiquement reflétées dans la vue.</span><span class="sxs-lookup"><span data-stu-id="4edc7-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="4edc7-119">Le modèle de vue obtient également les événements de la vue, tels que les clics de bouton.</span><span class="sxs-lookup"><span data-stu-id="4edc7-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="4edc7-120">Cette approche facilite la modification de la disposition et de l’interface utilisateur de votre application, car vous pouvez modifier les liaisons sans réécrire de code.</span><span class="sxs-lookup"><span data-stu-id="4edc7-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="4edc7-121">Par exemple, vous pouvez afficher une liste d’éléments sous la forme d’un `<ul>`, puis le modifier ultérieurement en une table.</span><span class="sxs-lookup"><span data-stu-id="4edc7-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="4edc7-122">Ajouter la bibliothèque Knockout</span><span class="sxs-lookup"><span data-stu-id="4edc7-122">Add the Knockout Library</span></span>

<span data-ttu-id="4edc7-123">Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4edc7-123">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="4edc7-124">Sélectionnez ensuite **Console du gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="4edc7-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="4edc7-125">Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4edc7-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="4edc7-126">Cette commande ajoute les fichiers Knockout au dossier scripts.</span><span class="sxs-lookup"><span data-stu-id="4edc7-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="4edc7-127">Créer le modèle de vue</span><span class="sxs-lookup"><span data-stu-id="4edc7-127">Create the View Model</span></span>

<span data-ttu-id="4edc7-128">Ajoutez un fichier JavaScript nommé App. js au dossier scripts.</span><span class="sxs-lookup"><span data-stu-id="4edc7-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="4edc7-129">(Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier scripts, sélectionnez **Ajouter**, puis sélectionnez **fichier JavaScript**.) Collez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="4edc7-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="4edc7-130">Dans Knockout, la classe `observable` active la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="4edc7-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="4edc7-131">Lorsque le contenu d’un observable change, l’observable notifie tous les contrôles liés aux données, afin qu’ils puissent se mettre à jour eux-mêmes.</span><span class="sxs-lookup"><span data-stu-id="4edc7-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="4edc7-132">(La classe `observableArray` est la version de tableau de *observable*.) Pour commencer, notre modèle de vue a deux observables :</span><span class="sxs-lookup"><span data-stu-id="4edc7-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="4edc7-133">`books` contient la liste de livres.</span><span class="sxs-lookup"><span data-stu-id="4edc7-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="4edc7-134">`error` contient un message d’erreur si un appel AJAX échoue.</span><span class="sxs-lookup"><span data-stu-id="4edc7-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="4edc7-135">La méthode `getAllBooks` effectue un appel AJAX pour récupérer la liste de livres.</span><span class="sxs-lookup"><span data-stu-id="4edc7-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="4edc7-136">Il exécute ensuite un push du résultat dans le tableau de `books`.</span><span class="sxs-lookup"><span data-stu-id="4edc7-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="4edc7-137">La méthode `ko.applyBindings` fait partie de la bibliothèque Knockout.</span><span class="sxs-lookup"><span data-stu-id="4edc7-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="4edc7-138">Il prend le modèle de vue en tant que paramètre et définit la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="4edc7-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="4edc7-139">Ajouter un bundle de scripts</span><span class="sxs-lookup"><span data-stu-id="4edc7-139">Add a Script Bundle</span></span>

<span data-ttu-id="4edc7-140">Le regroupement est une fonctionnalité de ASP.NET 4,5 qui facilite la combinaison ou le regroupement de plusieurs fichiers dans un seul fichier.</span><span class="sxs-lookup"><span data-stu-id="4edc7-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="4edc7-141">Le regroupement réduit le nombre de demandes au serveur, ce qui peut améliorer le temps de chargement des pages.</span><span class="sxs-lookup"><span data-stu-id="4edc7-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="4edc7-142">Ouvrez le fichier d’application\_Start/BundleConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="4edc7-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="4edc7-143">Ajoutez le code suivant à la méthode RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="4edc7-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="4edc7-144">[Précédent](part-5.md)
> [Suivant](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="4edc7-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
