---
title: Interopérabilité des composants JavaScript Razor
author: guardrex
description: Découvrez comment appeler des fonctions JavaScript à partir de .NET et .NET méthodes à partir de JavaScript dans les applications Blazor et composants de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: 9f822fee8990b03ff15ffa9857a2ddd95328a6ce
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050366"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="6a401-103">Interopérabilité des composants JavaScript Razor</span><span class="sxs-lookup"><span data-stu-id="6a401-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="6a401-104">Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6a401-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6a401-105">Une application Razor composants peut appeler des fonctions JavaScript à partir de .NET et .NET méthodes depuis le code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6a401-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="6a401-106">Appeler des fonctions JavaScript à partir de méthodes .NET</span><span class="sxs-lookup"><span data-stu-id="6a401-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="6a401-107">Il est parfois lorsque le code .NET est requis pour appeler une fonction JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6a401-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="6a401-108">Par exemple, un appel JavaScript peut exposer des fonctionnalités du navigateur ou fonctionnalités à partir d’une bibliothèque JavaScript à l’application.</span><span class="sxs-lookup"><span data-stu-id="6a401-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="6a401-109">Pour appeler en JavaScript à partir de .NET, utilisez la `IJSRuntime` abstraction.</span><span class="sxs-lookup"><span data-stu-id="6a401-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="6a401-110">Le `InvokeAsync<T>` méthode sur `IJSRuntime` prend un identificateur pour la fonction JavaScript que vous souhaitez appeler avec n’importe quel nombre d’arguments JSON sérialisable.</span><span class="sxs-lookup"><span data-stu-id="6a401-110">The `InvokeAsync<T>` method on `IJSRuntime` takes an identifier for the JavaScript function you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="6a401-111">L’identificateur de fonction est relatif à la portée globale (`window`).</span><span class="sxs-lookup"><span data-stu-id="6a401-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="6a401-112">Si vous souhaitez appeler `window.someScope.someFunction`, l’identificateur est `someScope.someFunction`.</span><span class="sxs-lookup"><span data-stu-id="6a401-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="6a401-113">Il est inutile pour inscrire la fonction avant qu’elle est appelée.</span><span class="sxs-lookup"><span data-stu-id="6a401-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="6a401-114">Le type de retour `T` JSON doit également être sérialisable.</span><span class="sxs-lookup"><span data-stu-id="6a401-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="6a401-115">Pour les applications ASP.NET Core Razor composants côté serveur :</span><span class="sxs-lookup"><span data-stu-id="6a401-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="6a401-116">Plusieurs demandes d’utilisateur sont traitées par l’application côté serveur.</span><span class="sxs-lookup"><span data-stu-id="6a401-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="6a401-117">N’appelez pas `JSRuntime.Current` dans un composant à appeler des fonctions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6a401-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="6a401-118">Injecter le `IJSRuntime` abstraction et l’utilisation de l’objet injecté pour émettre des appels interop JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6a401-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="6a401-119">L’exemple suivant est basé sur [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un décodeur JavaScript expérimental.</span><span class="sxs-lookup"><span data-stu-id="6a401-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="6a401-120">L’exemple montre comment appeler une fonction JavaScript à partir d’un C# (méthode).</span><span class="sxs-lookup"><span data-stu-id="6a401-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="6a401-121">La fonction JavaScript accepte un tableau d’octets à partir d’un C# (méthode), décode le tableau et retourne le texte au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="6a401-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="6a401-122">À l’intérieur de la `<head>` élément de *wwwroot/index.HTML*, fournissez une fonction qui utilise `TextDecoder` pour décoder un tableau passé :</span><span class="sxs-lookup"><span data-stu-id="6a401-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

<span data-ttu-id="6a401-123">Code JavaScript, telles que le code indiqué dans l’exemple précédent, permettre également être chargé par un fichier JavaScript avec une référence au fichier de script dans le *wwwroot/index.HTML* fichier.</span><span class="sxs-lookup"><span data-stu-id="6a401-123">JavaScript code, such as the code shown in the preceding example, can also be loaded by a JavaScript file with a reference to the script file in the *wwwroot/index.html* file.</span></span>

<span data-ttu-id="6a401-124">Le composant suivant :</span><span class="sxs-lookup"><span data-stu-id="6a401-124">The following component:</span></span>

* <span data-ttu-id="6a401-125">Appelle le `ConvertArray` à l’aide de fonction JavaScript `JsRuntime` quand un bouton de composant (**convertir un tableau**) est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="6a401-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="6a401-126">Une fois que la fonction JavaScript est appelée, le tableau passé est converti en une chaîne.</span><span class="sxs-lookup"><span data-stu-id="6a401-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="6a401-127">La chaîne est retournée au composant pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="6a401-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
@using Microsoft.JSInterop;
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

<span data-ttu-id="6a401-128">Pour les applications de Blazor côté client, le `IJSRuntime` abstraction est accessible à partir de `JSRuntime.Current`, qui fait référence à la demande de l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="6a401-128">For client-side Blazor apps, the `IJSRuntime` abstraction is accessible from `JSRuntime.Current`, which refers to the current user's request.</span></span> <span data-ttu-id="6a401-129">Étant donné qu’un seul utilisateur d’une application de Blazor côté client, à l’aide `JSRuntime.Current` pour appeler un code JavaScript fonctionne normalement.</span><span class="sxs-lookup"><span data-stu-id="6a401-129">Because there's only a single user of a client-side Blazor app, using `JSRuntime.Current` to invoke a JavaScript function works normally.</span></span> <span data-ttu-id="6a401-130">Utilisez uniquement `JSRuntime.Current` dans les applications Blazor côté client.</span><span class="sxs-lookup"><span data-stu-id="6a401-130">Only use `JSRuntime.Current` in client-side Blazor apps.</span></span>

<span data-ttu-id="6a401-131">Dans l’application exemple côté client qui accompagne cette rubrique, les deux fonctions JavaScript sont disponibles pour l’application côté client qui interagissent avec le DOM à l’entrée d’utilisateur et afficher un message d’accueil :</span><span class="sxs-lookup"><span data-stu-id="6a401-131">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="6a401-132">`showPrompt` &ndash; Génère une invite d’accepter l’entrée d’utilisateur (le nom d’utilisateur) et retourne le nom à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="6a401-132">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="6a401-133">`displayWelcome` &ndash; Assigne un message d’accueil de l’appelant à un objet DOM avec un `id` de `welcome`.</span><span class="sxs-lookup"><span data-stu-id="6a401-133">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="6a401-134">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="6a401-134">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="6a401-135">Place le `<script>` balise qui référence le fichier JavaScript dans le *wwwroot/index.HTML* fichier :</span><span class="sxs-lookup"><span data-stu-id="6a401-135">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

<span data-ttu-id="6a401-136">Ne pas de placer une balise de script dans un fichier de composant, car la balise de script ne peut pas être mis à jour dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="6a401-136">Don't place a script tag in a component file because the script tag can't be updated dynamically.</span></span>

<span data-ttu-id="6a401-137">Interopérabilité des méthodes .NET avec les fonctions JavaScript en appelant `InvokeAsync<T>` méthode sur `IJSRuntime`.</span><span class="sxs-lookup"><span data-stu-id="6a401-137">.NET methods interop with the JavaScript functions by calling `InvokeAsync<T>` method on `IJSRuntime`.</span></span>

<span data-ttu-id="6a401-138">L’exemple d’application utilise une paire de C# méthodes, `Prompt` et `Display`, pour appeler le `showPrompt` et `displayWelcome` fonctions JavaScript :</span><span class="sxs-lookup"><span data-stu-id="6a401-138">The sample app uses a pair of C# methods, `Prompt` and `Display`, to invoke the `showPrompt` and `displayWelcome` JavaScript functions:</span></span>

<span data-ttu-id="6a401-139">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a401-139">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

<span data-ttu-id="6a401-140">Le `IJSRuntime` abstraction est asynchrone pour permettre des scénarios côté serveur.</span><span class="sxs-lookup"><span data-stu-id="6a401-140">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="6a401-141">Si l’application s’exécute côté client et que vous souhaitez appeler une fonction JavaScript de manière synchrone, effectuer un downcast à `IJSInProcessRuntime` et appelez `Invoke<T>` à la place.</span><span class="sxs-lookup"><span data-stu-id="6a401-141">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="6a401-142">Nous recommandons que la plupart des utilisation d’interop bibliothèques JavaScript les API pour garantir les bibliothèques asynchrones sont disponibles dans tous les scénarios, côté client ou côté serveur.</span><span class="sxs-lookup"><span data-stu-id="6a401-142">We recommend that most JavaScript interop libraries use the async APIs to ensure the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="6a401-143">L’exemple d’application inclut un composant pour illustrer l’interopérabilité JS.</span><span class="sxs-lookup"><span data-stu-id="6a401-143">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="6a401-144">Le composant :</span><span class="sxs-lookup"><span data-stu-id="6a401-144">The component:</span></span>

* <span data-ttu-id="6a401-145">Reçoit l’entrée utilisateur via une invite de JS.</span><span class="sxs-lookup"><span data-stu-id="6a401-145">Receives user input via a JS prompt.</span></span>
* <span data-ttu-id="6a401-146">Retourne le texte au composant pour traitement.</span><span class="sxs-lookup"><span data-stu-id="6a401-146">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="6a401-147">Appelle une deuxième fonction JS qui interagit avec le DOM pour afficher un message d’accueil.</span><span class="sxs-lookup"><span data-stu-id="6a401-147">Calls a second JS function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="6a401-148">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6a401-148">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. <span data-ttu-id="6a401-149">Lorsque `TriggerJsPrompt` est exécutée en sélectionnant le composant **déclencheur JavaScript invite** bouton, le `ExampleJsInterop.Prompt` méthode dans C# code est appelé.</span><span class="sxs-lookup"><span data-stu-id="6a401-149">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the `ExampleJsInterop.Prompt` method in C# code is called.</span></span>
1. <span data-ttu-id="6a401-150">Le `Prompt` méthode s’exécute le code JavaScript `showPrompt` fournie dans le *wwwroot/exampleJsInterop.js* fichier.</span><span class="sxs-lookup"><span data-stu-id="6a401-150">The `Prompt` method executes the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file.</span></span>
1. <span data-ttu-id="6a401-151">Le `showPrompt` fonction accepte l’entrée d’utilisateur (nom de l’utilisateur), qui est codée en HTML et renvoyée à le `Prompt` (méthode), puis finalement vers le composant.</span><span class="sxs-lookup"><span data-stu-id="6a401-151">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the `Prompt` method and ultimately back to the component.</span></span> <span data-ttu-id="6a401-152">Le composant stocke le nom d’utilisateur dans une variable locale, `name`.</span><span class="sxs-lookup"><span data-stu-id="6a401-152">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="6a401-153">La chaîne stockée dans `name` est incorporé dans un message d’accueil, qui est transmis à un deuxième C# (méthode), `ExampleJsInterop.Display`.</span><span class="sxs-lookup"><span data-stu-id="6a401-153">The string stored in `name` is incorporated into a welcome message, which is passed to a second C# method, `ExampleJsInterop.Display`.</span></span>
1. <span data-ttu-id="6a401-154">`Display` appelle une fonction JavaScript, `displayWelcome`, qui restitue le message d’accueil dans une balise d’en-tête.</span><span class="sxs-lookup"><span data-stu-id="6a401-154">`Display` calls a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="6a401-155">Capturer les références aux éléments</span><span class="sxs-lookup"><span data-stu-id="6a401-155">Capture references to elements</span></span>

<span data-ttu-id="6a401-156">Certains [JavaScript interop](xref:razor-components/javascript-interop) scénarios requièrent des références aux éléments HTML.</span><span class="sxs-lookup"><span data-stu-id="6a401-156">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="6a401-157">Par exemple, une bibliothèque d’interface utilisateur peut nécessiter une référence d’élément pour l’initialisation, ou vous devrez peut-être appeler des API de type de commande sur un élément, telle que `focus` ou `play`.</span><span class="sxs-lookup"><span data-stu-id="6a401-157">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="6a401-158">Vous pouvez capturer des références aux éléments HTML dans un composant en ajoutant un `ref` d’attribut à l’élément HTML et en définissant un champ de type `ElementRef` dont le nom correspond à la valeur de la `ref` attribut.</span><span class="sxs-lookup"><span data-stu-id="6a401-158">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="6a401-159">L’exemple suivant montre la capture d’une référence à l’élément d’entrée de nom d’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="6a401-159">The following example shows capturing a reference to the username input element:</span></span>

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="6a401-160">Faire **pas** utiliser des références de l’élément capturé comme un moyen de remplir le modèle DOM.</span><span class="sxs-lookup"><span data-stu-id="6a401-160">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="6a401-161">Cela peut interférer avec le modèle déclaratif de rendu.</span><span class="sxs-lookup"><span data-stu-id="6a401-161">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="6a401-162">Autant que code .NET est concerné, un `ElementRef` est un handle opaque.</span><span class="sxs-lookup"><span data-stu-id="6a401-162">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="6a401-163">Le *uniquement* chose que vous pouvez en faire est de transmettre au code JavaScript grâce à l’interopérabilité de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6a401-163">The *only* thing you can do with it is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="6a401-164">Lorsque vous procédez ainsi, le code JavaScript côté reçoit un `HTMLElement` instance, il peut utiliser avec les API DOM normal.</span><span class="sxs-lookup"><span data-stu-id="6a401-164">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="6a401-165">Par exemple, le code suivant définit une méthode d’extension .NET qui permet de définir le focus sur un élément :</span><span class="sxs-lookup"><span data-stu-id="6a401-165">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="6a401-166">*myLib.js*:</span><span class="sxs-lookup"><span data-stu-id="6a401-166">*mylib.js*:</span></span>

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="6a401-167">*ElementRefExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a401-167">*ElementRefExtensions.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Blazor;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        public static Task Focus(this ElementRef elementRef)
        {
            return JSRuntime.Current.InvokeAsync<object>("myLib.focusElement", elementRef);
        }
    }
}
```

<span data-ttu-id="6a401-168">Vous pouvez désormais vous concentrer entrées dans un de vos composants :</span><span class="sxs-lookup"><span data-stu-id="6a401-168">Now you can focus inputs in any of your components:</span></span>

```cshtml
@using MyLib

<input ref="username" />
<button onclick="@SetFocus">Set focus</button>

@functions {
    ElementRef username;

    void SetFocus()
    {
        username.Focus();
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="6a401-169">Le `username` variable est renseignée uniquement une fois que le composant effectue le rendu et sa sortie inclut le `<input>` élément.</span><span class="sxs-lookup"><span data-stu-id="6a401-169">The `username` variable is only populated after the component renders and its output includes the `<input>` element.</span></span> <span data-ttu-id="6a401-170">Si vous essayez de transmettre un non remplie `ElementRef` au code JavaScript, reçoit le code JavaScript `null`.</span><span class="sxs-lookup"><span data-stu-id="6a401-170">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="6a401-171">Pour manipuler les références d’élément après que le composant a terminé l’utilisation de rendu (pour définir le focus initial sur un élément) le `OnAfterRenderAsync` ou `OnAfterRender` [méthodes de cycle de vie du composant](xref:razor-components/components#lifecycle-methods).</span><span class="sxs-lookup"><span data-stu-id="6a401-171">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="6a401-172">Appeler des méthodes .NET à partir de fonctions JavaScript</span><span class="sxs-lookup"><span data-stu-id="6a401-172">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="6a401-173">Appel de méthode .NET statique</span><span class="sxs-lookup"><span data-stu-id="6a401-173">Static .NET method call</span></span>

<span data-ttu-id="6a401-174">Pour appeler une méthode .NET statique à partir de JavaScript, utilisez le `DotNet.invokeMethod` ou `DotNet.invokeMethodAsync` fonctions.</span><span class="sxs-lookup"><span data-stu-id="6a401-174">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="6a401-175">Passez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et tous les arguments.</span><span class="sxs-lookup"><span data-stu-id="6a401-175">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="6a401-176">Là encore, la version asynchrone est préférable pour prendre en charge des scénarios côté serveur.</span><span class="sxs-lookup"><span data-stu-id="6a401-176">Again, the async version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="6a401-177">Pour être appelables à partir de JavaScript, la méthode .NET doit être publique et statique décorée avec `[JSInvokable]`.</span><span class="sxs-lookup"><span data-stu-id="6a401-177">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="6a401-178">Par défaut, l’identificateur de méthode est le nom de méthode, mais vous pouvez spécifier un autre identificateur à l’aide de la `JSInvokableAttribute` constructeur.</span><span class="sxs-lookup"><span data-stu-id="6a401-178">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="6a401-179">Appel de méthodes génériques ouvertes n’est pas actuellement pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6a401-179">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="6a401-180">L’exemple d’application inclut un C# méthode pour retourner un tableau de `int`s.</span><span class="sxs-lookup"><span data-stu-id="6a401-180">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="6a401-181">La méthode est décorée avec le `JSInvokable` attribut.</span><span class="sxs-lookup"><span data-stu-id="6a401-181">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="6a401-182">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6a401-182">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

<span data-ttu-id="6a401-183">JavaScript pris en charge pour le client appelle le C# méthode .NET.</span><span class="sxs-lookup"><span data-stu-id="6a401-183">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="6a401-184">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="6a401-184">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

<span data-ttu-id="6a401-185">Lorsque le **méthode statique .NET de déclencheur ReturnArrayAsync** est sélectionné, examinez la sortie de console dans les outils de développement du navigateur web :</span><span class="sxs-lookup"><span data-stu-id="6a401-185">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="6a401-186">La quatrième valeur de tableau est poussée vers le tableau (`data.push(4);`) retourné par `ReturnArrayAsync`.</span><span class="sxs-lookup"><span data-stu-id="6a401-186">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="6a401-187">Appel de méthode d’instance</span><span class="sxs-lookup"><span data-stu-id="6a401-187">Instance method call</span></span>

<span data-ttu-id="6a401-188">Vous pouvez également appeler des méthodes d’instance .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6a401-188">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="6a401-189">Pour appeler une méthode d’instance .NET à partir de JavaScript, tout d’abord passer l’instance de .NET à JavaScript en l’encapsulant dans un `DotNetObjectRef` instance.</span><span class="sxs-lookup"><span data-stu-id="6a401-189">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="6a401-190">L’instance de .NET est passé par référence à JavaScript, et vous pouvez appeler des méthodes d’instance de .NET sur l’instance à l’aide de la `invokeMethod` ou `invokeMethodAsync` fonctions.</span><span class="sxs-lookup"><span data-stu-id="6a401-190">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="6a401-191">L’instance de .NET peut également être passé en tant qu’argument lors de l’appel d’autres méthodes de .NET à partir de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6a401-191">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="6a401-192">L’exemple d’application consigne les messages dans la console du côté client.</span><span class="sxs-lookup"><span data-stu-id="6a401-192">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="6a401-193">Pour les exemples suivants, illustrées dans l’exemple d’application, examinez la sortie de la console du navigateur dans les outils de développement du navigateur.</span><span class="sxs-lookup"><span data-stu-id="6a401-193">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="6a401-194">Lorsque le **méthode d’instance déclencheur .NET HelloHelper.SayHello** bouton est sélectionné, `ExampleJsInterop.CallHelloHelperSayHello` est appelée et transmet un nom, `Blazor`, à la méthode.</span><span class="sxs-lookup"><span data-stu-id="6a401-194">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="6a401-195">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6a401-195">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

<span data-ttu-id="6a401-196">`CallHelloHelperSayHello` appelle la fonction JavaScript `sayHello` avec une nouvelle instance de `HelloHelper`.</span><span class="sxs-lookup"><span data-stu-id="6a401-196">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="6a401-197">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a401-197">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

<span data-ttu-id="6a401-198">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="6a401-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

<span data-ttu-id="6a401-199">Le nom est passé à `HelloHelper`du constructeur, ce qui affecte le `HelloHelper.Name` propriété.</span><span class="sxs-lookup"><span data-stu-id="6a401-199">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="6a401-200">Lorsque la fonction JavaScript `sayHello` est exécutée, `HelloHelper.SayHello` retourne le `Hello, {Name}!` message, ce qui est écrit dans la console par la fonction JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6a401-200">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="6a401-201">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a401-201">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="6a401-202">Console de sortie dans les outils de développement du navigateur web :</span><span class="sxs-lookup"><span data-stu-id="6a401-202">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="6a401-203">Partagez du code interop dans une bibliothèque de classes de composant de Razor</span><span class="sxs-lookup"><span data-stu-id="6a401-203">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="6a401-204">Code interop JavaScript peut être inclus dans une bibliothèque de classes de composant de Razor (`dotnet new blazorlib`), qui vous permet de partager le code dans un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="6a401-204">JavaScript interop code can be included in a Razor Component class library (`dotnet new blazorlib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="6a401-205">La bibliothèque de classes Razor composant gère les ressources JavaScript incorporation dans l’assembly généré.</span><span class="sxs-lookup"><span data-stu-id="6a401-205">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="6a401-206">Les fichiers JavaScript sont placés dans le *wwwroot* dossier et les outils s’occupe de l’incorporation de ces ressources lors de la génération de la bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="6a401-206">The JavaScript files are placed in the *wwwroot* folder, and the tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="6a401-207">Le package NuGet intégré est référencé dans le fichier de projet de l’application, tout comme n’importe quel package NuGet normal est référencé.</span><span class="sxs-lookup"><span data-stu-id="6a401-207">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="6a401-208">Une fois que l’application a été restaurée, code d’application peut appeler dans JavaScript comme s’il s’agissait C#.</span><span class="sxs-lookup"><span data-stu-id="6a401-208">After the app has been restored, app code can call into JavaScript as if it were C#.</span></span>
