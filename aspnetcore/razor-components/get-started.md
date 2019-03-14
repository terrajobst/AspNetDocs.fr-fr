---
title: Prise en main des composants de Razor
author: guardrex
description: Découvrez comment bien démarrer avec les composants de Razor en créant et modifiant un projet de composants de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: a9ada603e5ed4e0e75c4aebc5105c331118666e6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036386"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="e7cd1-103">Prise en main des composants de Razor</span><span class="sxs-lookup"><span data-stu-id="e7cd1-103">Get started with Razor Components</span></span>

<span data-ttu-id="e7cd1-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e7cd1-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7cd1-105">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7cd1-105">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e7cd1-106">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="e7cd1-106">Prerequisites:</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

<span data-ttu-id="e7cd1-107">Pour créer votre premier projet de composants de Razor dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="e7cd1-107">To create your first Razor Components project in Visual Studio:</span></span>

1. <span data-ttu-id="e7cd1-108">Sélectionnez **fichier** > **nouveau projet** > **Web** > **Application Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-108">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="e7cd1-109">Assurez-vous que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés en haut.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-109">Make sure **.NET Core** and **ASP.NET Core 3.0** are selected at the top.</span></span>
1. <span data-ttu-id="e7cd1-110">Choisissez le **Razor composants** modèle et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-110">Choose the **Razor Components** template and select **OK**.</span></span>

   ![Boîte de dialogue nouvelle application](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. <span data-ttu-id="e7cd1-112">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-112">Press **F5** to run the app.</span></span>

<span data-ttu-id="e7cd1-113">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="e7cd1-113">Congratulations!</span></span> <span data-ttu-id="e7cd1-114">Vous venez d’exécuter votre première application Razor composants !</span><span class="sxs-lookup"><span data-stu-id="e7cd1-114">You just ran your first Razor Components app!</span></span>

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.


[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e7cd1-115">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e7cd1-115">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="e7cd1-116">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="e7cd1-116">Prerequisites:</span></span>

* [<span data-ttu-id="e7cd1-117">Afficher un aperçu de .NET core SDK 3.0</span><span class="sxs-lookup"><span data-stu-id="e7cd1-117">.NET Core SDK 3.0 Preview</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. <span data-ttu-id="e7cd1-118">Pour créer votre premier projet Razor composants à partir d’une invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="e7cd1-118">To create your first Razor Components project from a command shell:</span></span>

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. <span data-ttu-id="e7cd1-119">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-119">In a browser, navigate to `https://localhost:5001`.</span></span>

<span data-ttu-id="e7cd1-120">Félicitations !</span><span class="sxs-lookup"><span data-stu-id="e7cd1-120">Congratulations!</span></span> <span data-ttu-id="e7cd1-121">Vous venez d’exécuter votre première application Razor composants !</span><span class="sxs-lookup"><span data-stu-id="e7cd1-121">You just ran your first Razor Components app!</span></span>

---

## <a name="razor-components-project"></a><span data-ttu-id="e7cd1-122">Projet de composants de Razor</span><span class="sxs-lookup"><span data-stu-id="e7cd1-122">Razor Components project</span></span>

<span data-ttu-id="e7cd1-123">La solution créée par le modèle de composants de Razor contient deux projets :</span><span class="sxs-lookup"><span data-stu-id="e7cd1-123">The solution created by the Razor Components template contains two projects:</span></span>

* <span data-ttu-id="e7cd1-124">*WebApplication1.Server* &ndash; le projet serveur est un projet ASP.NET Core configuré pour héberger l’application de composants de Razor.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-124">*WebApplication1.Server* &ndash; The server project is an ASP.NET Core project set up to host the Razor Components app.</span></span>
* <span data-ttu-id="e7cd1-125">*WebApplication1.App* &ndash; le projet d’interface utilisateur web côté client qui utilise les composants de Razor.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-125">*WebApplication1.App* &ndash; The client-side web UI project that uses Razor Components.</span></span>

<span data-ttu-id="e7cd1-126">La logique de l’interface utilisateur dans le *WebApplication1.App* projet est séparé du reste de l’application en raison d’une restriction technique dans ASP.NET Core 3.0 Preview 2.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-126">The UI logic in the *WebApplication1.App* project is separated from the rest of the app due to a technical limitation in ASP.NET Core 3.0 Preview 2.</span></span> <span data-ttu-id="e7cd1-127">L’extension de fichier Razor (*.cshtml*) utilisé pour les composants de Razor est également utilisée pour les vues de Pages Razor et MVC.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-127">The Razor file extension (*.cshtml*) used for Razor Components is also used for Razor Pages and MVC views.</span></span> <span data-ttu-id="e7cd1-128">Actuellement, les composants de Razor et Razor Pages/MVC étant modèles différents de compilation, les fichiers Razor composants Razor sont maintenues séparées.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-128">Currently, Razor Components and Razor Pages/MVC have different compilation models, so the Razor Components Razor files are kept separate.</span></span> <span data-ttu-id="e7cd1-129">Dans une prochaine version d’évaluation, nous prévoyons d’introduire une nouvelle extension de fichier pour les composants de Razor (*.razor*).</span><span class="sxs-lookup"><span data-stu-id="e7cd1-129">In a future preview, we plan to introduce a new file extension for Razor Components (*.razor*).</span></span> <span data-ttu-id="e7cd1-130">Composants, les pages et les vues seront hébergées *dans le même projet*.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-130">Components, pages, and views will be hosted *in the same project*.</span></span>

<span data-ttu-id="e7cd1-131">Quand l’application est exécutée, plusieurs pages sont disponibles à partir des onglets dans la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="e7cd1-131">When the app is run, multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="e7cd1-132">Accueil</span><span class="sxs-lookup"><span data-stu-id="e7cd1-132">Home</span></span>
* <span data-ttu-id="e7cd1-133">Counter</span><span class="sxs-lookup"><span data-stu-id="e7cd1-133">Counter</span></span>
* <span data-ttu-id="e7cd1-134">Récupérer des données</span><span class="sxs-lookup"><span data-stu-id="e7cd1-134">Fetch data</span></span>

<span data-ttu-id="e7cd1-135">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-135">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="e7cd1-136">L’incrémentation d’un compteur dans une page web requiert normalement l’écriture JavaScript, mais le projet Composants Razor fournit une meilleure approche à l’aide C#.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-136">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

<span data-ttu-id="e7cd1-137">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7cd1-137">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

<span data-ttu-id="e7cd1-138">Une demande de `/counter` dans le navigateur, comme spécifié par le `@page` directive en haut, entraîne le composant de compteur afficher son contenu.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-138">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the Counter component to render its content.</span></span> <span data-ttu-id="e7cd1-139">Composants restituent dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisé pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-139">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="e7cd1-140">Chaque fois que le **Click me** bouton est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="e7cd1-140">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="e7cd1-141">Le `onclick` événement est déclenché.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-141">The `onclick` event is fired.</span></span>
* <span data-ttu-id="e7cd1-142">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-142">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="e7cd1-143">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-143">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="e7cd1-144">Le composant est une nouvelle fois restitué.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-144">The component is rendered again.</span></span>

<span data-ttu-id="e7cd1-145">Le runtime compare le nouveau contenu au contenu précédent et s’applique uniquement le contenu est modifié pour le modèle DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="e7cd1-145">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="e7cd1-146">Ajouter un composant à un autre composant à l’aide d’une syntaxe de type HTML.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-146">Add a component to another component using an HTML-like syntax.</span></span> <span data-ttu-id="e7cd1-147">Paramètres de composant sont spécifiés à l’aide d’attributs ou contenu enfant.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-147">Component parameters are specified using attributes or child content.</span></span> <span data-ttu-id="e7cd1-148">Par exemple, un composant de compteur peut être ajouté à la page d’accueil de l’application en ajoutant un `<Counter />` élément pour le composant d’Index.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-148">For example, a Counter component can be added to the app's homepage by adding a `<Counter />` element to the Index component.</span></span>

<span data-ttu-id="e7cd1-149">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7cd1-149">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

<span data-ttu-id="e7cd1-150">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-150">Run the app.</span></span> <span data-ttu-id="e7cd1-151">La page d’accueil a son propre compteur.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-151">The homepage has its own counter.</span></span>

<span data-ttu-id="e7cd1-152">Pour ajouter un paramètre au composant de compteur, mettre à jour du composant `@functions` bloc :</span><span class="sxs-lookup"><span data-stu-id="e7cd1-152">To add a parameter to the Counter component, update the component's `@functions` block:</span></span>

* <span data-ttu-id="e7cd1-153">Ajouter une propriété pour `IncrementAmount` décorée avec le `[Parameter]` attribut.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-153">Add a property for `IncrementAmount` decorated with the `[Parameter]` attribute.</span></span>
* <span data-ttu-id="e7cd1-154">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-154">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="e7cd1-155">*WebApplication1.App/Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7cd1-155">*WebApplication1.App/Pages/Counter.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

<span data-ttu-id="e7cd1-156">Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-156">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="e7cd1-157">*WebApplication1.App/Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e7cd1-157">*WebApplication1.App/Pages/Index.cshtml*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

<span data-ttu-id="e7cd1-158">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-158">Run the app.</span></span> <span data-ttu-id="e7cd1-159">La page d’accueil a son propre compteur incrémente par dix chaque fois que le **Click me** bouton est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="e7cd1-159">The homepage has its own counter that increments by ten each time the **Click me** button is selected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7cd1-160">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="e7cd1-160">Next steps</span></span>

<xref:tutorials/first-razor-components-app>
