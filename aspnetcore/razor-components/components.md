---
title: Créer et utiliser des composants de Razor
author: guardrex
description: Découvrez comment créer et utiliser des composants de Razor, y compris comment lier à des données, gérer les événements et gérer les cycles de vie de composant.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: razor-components/components
ms.openlocfilehash: 1533587f9f11e99f24d860c02f0efb6713119308
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031096"
---
# <a name="create-and-use-razor-components"></a><span data-ttu-id="32128-103">Créer et utiliser des composants de Razor</span><span class="sxs-lookup"><span data-stu-id="32128-103">Create and use Razor Components</span></span>

<span data-ttu-id="32128-104">Par [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), et [Morné Zaayman](https://github.com/MorneZaayman)</span><span class="sxs-lookup"><span data-stu-id="32128-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Morné Zaayman](https://github.com/MorneZaayman)</span></span>

<span data-ttu-id="32128-105">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="32128-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="32128-106">Consultez la rubrique [Bien démarrer](xref:razor-components/get-started) pour connaître les prérequis.</span><span class="sxs-lookup"><span data-stu-id="32128-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="32128-107">Les applications de composants de Razor sont créées à l’aide de *composants*.</span><span class="sxs-lookup"><span data-stu-id="32128-107">Razor Components apps are built using *components*.</span></span> <span data-ttu-id="32128-108">Un composant est un bloc autonome de l’interface utilisateur (IU), par exemple une page, une boîte de dialogue ou un formulaire.</span><span class="sxs-lookup"><span data-stu-id="32128-108">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="32128-109">Un composant inclut un balisage HTML et la logique de traitement nécessaire pour injecter des données ou de répondre aux événements d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32128-109">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="32128-110">Les composants sont flexibles et léger.</span><span class="sxs-lookup"><span data-stu-id="32128-110">Components are flexible and lightweight.</span></span> <span data-ttu-id="32128-111">Ils peuvent être imbriqués, réutilisés et partagés entre des projets.</span><span class="sxs-lookup"><span data-stu-id="32128-111">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="32128-112">Classes de composant</span><span class="sxs-lookup"><span data-stu-id="32128-112">Component classes</span></span>

<span data-ttu-id="32128-113">Les composants sont généralement implémentées dans *.cshtml* de fichiers en utilisant une combinaison de C# et le balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="32128-113">Components are typically implemented in *.cshtml* files using a combination of C# and HTML markup.</span></span> <span data-ttu-id="32128-114">L’interface utilisateur pour un composant est défini à l’aide de HTML.</span><span class="sxs-lookup"><span data-stu-id="32128-114">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="32128-115">La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="32128-115">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="32128-116">Lors de la compilation d’une application de composants de Razor, le balisage HTML et C# logique de rendu sont converties en une classe de composant.</span><span class="sxs-lookup"><span data-stu-id="32128-116">When a Razor Components app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="32128-117">Le nom de la classe générée correspond au nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="32128-117">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="32128-118">Membres de la classe de composant sont définies dans un `@functions` bloc (plusieurs `@functions` bloc est autorisé).</span><span class="sxs-lookup"><span data-stu-id="32128-118">Members of the component class are defined in a `@functions` block (more than one `@functions` block is permissible).</span></span> <span data-ttu-id="32128-119">Dans le `@functions` bloc, l’état du composant (propriétés, champs) est spécifié, ainsi que des méthodes pour la gestion des événements ou pour définir une autre logique de composant.</span><span class="sxs-lookup"><span data-stu-id="32128-119">In the `@functions` block, component state (properties, fields) is specified along with methods for event handling or for defining other component logic.</span></span>

<span data-ttu-id="32128-120">Membres composant peuvent ensuite être utilisées comme partie du composant de rendu à l’aide de la logique C# expressions qui commencent par `@`.</span><span class="sxs-lookup"><span data-stu-id="32128-120">Component members can then be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="32128-121">Par exemple, un C# champ est restitué en préfixant `@` au nom du champ.</span><span class="sxs-lookup"><span data-stu-id="32128-121">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="32128-122">L’exemple suivant évalue et effectue le rendu :</span><span class="sxs-lookup"><span data-stu-id="32128-122">The following example evaluates and renders:</span></span>

* <span data-ttu-id="32128-123">`_headingFontStyle` la valeur de propriété CSS pour `font-style`.</span><span class="sxs-lookup"><span data-stu-id="32128-123">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="32128-124">`_headingText` pour le contenu de la `<h1>` élément.</span><span class="sxs-lookup"><span data-stu-id="32128-124">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="32128-125">Une fois que le composant est initialement affichée, le composant régénère son arborescence rendu en réponse aux événements.</span><span class="sxs-lookup"><span data-stu-id="32128-125">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="32128-126">Composants de Razor compare la nouvelle arborescence de rendu par rapport à le, puis applique toutes les modifications pour du navigateur modèle DOM (Document Object).</span><span class="sxs-lookup"><span data-stu-id="32128-126">Razor Components then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

## <a name="using-components"></a><span data-ttu-id="32128-127">À l’aide de composants</span><span class="sxs-lookup"><span data-stu-id="32128-127">Using components</span></span>

<span data-ttu-id="32128-128">Les composants peuvent inclure d’autres composants en les déclarant à l’aide de la syntaxe d’élément HTML.</span><span class="sxs-lookup"><span data-stu-id="32128-128">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="32128-129">Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.</span><span class="sxs-lookup"><span data-stu-id="32128-129">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="32128-130">Restitue le balisage suivant un `HeadingComponent` (*HeadingComponent.cshtml*) instance :</span><span class="sxs-lookup"><span data-stu-id="32128-130">The following markup renders a `HeadingComponent` (*HeadingComponent.cshtml*) instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a><span data-ttu-id="32128-131">Paramètres de composant</span><span class="sxs-lookup"><span data-stu-id="32128-131">Component parameters</span></span>

<span data-ttu-id="32128-132">Composants peuvent avoir *composant paramètres*, qui sont définis à l’aide de *non public* assorties de propriétés de la classe de composant `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="32128-132">Components can have *component parameters*, which are defined using *non-public* properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="32128-133">Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="32128-133">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="32128-134">Dans l’exemple suivant, le `ParentComponent` définit la valeur de la `Title` propriété de la `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="32128-134">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`:</span></span>

<span data-ttu-id="32128-135">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-135">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

<span data-ttu-id="32128-136">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-136">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a><span data-ttu-id="32128-137">Contenu enfant</span><span class="sxs-lookup"><span data-stu-id="32128-137">Child content</span></span>

<span data-ttu-id="32128-138">Composants peuvent définir le contenu d’un autre composant.</span><span class="sxs-lookup"><span data-stu-id="32128-138">Components can set the content of another component.</span></span> <span data-ttu-id="32128-139">Le composant affectation fournit le contenu entre les balises qui spécifient le composant de réception.</span><span class="sxs-lookup"><span data-stu-id="32128-139">The assigning component provides the content between the tags that specify the receiving component.</span></span> <span data-ttu-id="32128-140">Par exemple, un `ParentComponent` peut fournir des contenus pour le rendu par un composant enfant en plaçant le contenu à l’intérieur `<ChildComponent>` balises.</span><span class="sxs-lookup"><span data-stu-id="32128-140">For example, a `ParentComponent` can provide content for rendering by a Child component by placing the content inside `<ChildComponent>` tags.</span></span>

<span data-ttu-id="32128-141">*ParentComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-141">*ParentComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

<span data-ttu-id="32128-142">Le composant enfant a un `ChildContent` propriété qui représente un `RenderFragment`.</span><span class="sxs-lookup"><span data-stu-id="32128-142">The Child component has a `ChildContent` property that represents a `RenderFragment`.</span></span> <span data-ttu-id="32128-143">La valeur de `ChildContent` est placé dans le balisage du composant enfant où le contenu doit être restitué.</span><span class="sxs-lookup"><span data-stu-id="32128-143">The value of `ChildContent` is positioned in the child component's markup where the content should be rendered.</span></span> <span data-ttu-id="32128-144">Dans l’exemple suivant, la valeur de `ChildContent` est reçu à partir du composant parent et restituée dans le panneau de configuration d’amorçage `panel-body`.</span><span class="sxs-lookup"><span data-stu-id="32128-144">In the following example, the value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="32128-145">*ChildComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-145">*ChildComponent.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> <span data-ttu-id="32128-146">La réception de la propriété le `RenderFragment` contenu doit être nommé `ChildContent` par convention.</span><span class="sxs-lookup"><span data-stu-id="32128-146">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

## <a name="data-binding"></a><span data-ttu-id="32128-147">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="32128-147">Data binding</span></span>

<span data-ttu-id="32128-148">Liaison de données à la fois des composants et des éléments DOM est effectuée avec la `bind` attribut.</span><span class="sxs-lookup"><span data-stu-id="32128-148">Data binding to both components and DOM elements is accomplished with the `bind` attribute.</span></span> <span data-ttu-id="32128-149">L’exemple suivant lie la `ItalicsCheck` état activé de propriété à la case à cocher :</span><span class="sxs-lookup"><span data-stu-id="32128-149">The following example binds the `ItalicsCheck` property to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck" />
```

<span data-ttu-id="32128-150">Lorsque la case à cocher est activée et désactivée, la valeur de propriété est mis à jour vers `true` et `false`, respectivement.</span><span class="sxs-lookup"><span data-stu-id="32128-150">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="32128-151">La case à cocher est mis à jour dans l’interface utilisateur uniquement lorsque le composant est rendu, pas en réponse à l’évolution de la valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="32128-151">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="32128-152">Dans la mesure où les composants s’afficher après l’exécution de code de gestionnaire d’événements, mises à jour de la propriété sont généralement pas immédiatement dans l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32128-152">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="32128-153">À l’aide de `bind` avec un `CurrentValue` propriété (`<input bind="@CurrentValue" />`) est fondamentalement équivalent à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="32128-153">Using `bind` with a `CurrentValue` property (`<input bind="@CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="32128-154">Lorsque le composant est affiché, le `value` de l’élément d’entrée provient de la `CurrentValue` propriété.</span><span class="sxs-lookup"><span data-stu-id="32128-154">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="32128-155">Lorsque l’utilisateur tape dans la zone de texte, le `onchange` événement est déclenché et le `CurrentValue` propriété est définie sur la valeur modifiée.</span><span class="sxs-lookup"><span data-stu-id="32128-155">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="32128-156">En réalité, la génération de code est un peu plus complexe, car `bind` gère des quelques cas où les conversions de type sont effectuées.</span><span class="sxs-lookup"><span data-stu-id="32128-156">In reality, the code generation is a little more complex because `bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="32128-157">En principe, `bind` associe la valeur actuelle d’une expression avec un `value` attribut et gère des modifications à l’aide du gestionnaire enregistré.</span><span class="sxs-lookup"><span data-stu-id="32128-157">In principle, `bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="32128-158">**Chaînes de format**</span><span class="sxs-lookup"><span data-stu-id="32128-158">**Format strings**</span></span>

<span data-ttu-id="32128-159">Liaison de données fonctionne avec <xref:System.DateTime> chaînes de format.</span><span class="sxs-lookup"><span data-stu-id="32128-159">Data binding works with <xref:System.DateTime> format strings.</span></span> <span data-ttu-id="32128-160">Autres expressions de format, telles que les devises ou les formats de nombre, ne sont pas disponibles pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="32128-160">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd" />

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="32128-161">Le `format-value` attribut spécifie le format de date à appliquer à la `value` de la `input` élément.</span><span class="sxs-lookup"><span data-stu-id="32128-161">The `format-value` attribute specifies the date format to apply to the `value` of the `input` element.</span></span> <span data-ttu-id="32128-162">Le format est également utilisé pour analyser la valeur quand un `onchange` événement se produit.</span><span class="sxs-lookup"><span data-stu-id="32128-162">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="32128-163">**Paramètres du composant**</span><span class="sxs-lookup"><span data-stu-id="32128-163">**Component parameters**</span></span>

<span data-ttu-id="32128-164">Liaison reconnaît également les paramètres du composant, où `bind-{property}` peut lier une valeur de propriété entre plusieurs composants.</span><span class="sxs-lookup"><span data-stu-id="32128-164">Binding also recognizes component parameters, where `bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="32128-165">Utilise le composant suivant `ChildComponent` et lie le `ParentYear` paramètre du parent de le `Year` paramètre sur le composant enfant :</span><span class="sxs-lookup"><span data-stu-id="32128-165">The following component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

<span data-ttu-id="32128-166">Composant parent :</span><span class="sxs-lookup"><span data-stu-id="32128-166">Parent component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="32128-167">Composant enfant :</span><span class="sxs-lookup"><span data-stu-id="32128-167">Child component:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

<span data-ttu-id="32128-168">Le `Year` paramètre est peut être liée, car il possède un accompagnement `YearChanged` événement qui correspond au type de le `Year` paramètre.</span><span class="sxs-lookup"><span data-stu-id="32128-168">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="32128-169">Chargement de la `ParentComponent` génère le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="32128-169">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="32128-170">Si la valeur de la `ParentYear` propriété est modifiée en sélectionnant le bouton dans le `ParentComponent`, le `Year` propriété de la `ChildComponent` est mis à jour.</span><span class="sxs-lookup"><span data-stu-id="32128-170">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="32128-171">La nouvelle valeur de `Year` est affiché dans l’interface utilisateur lorsque le `ParentComponent` est un nouvel affichage :</span><span class="sxs-lookup"><span data-stu-id="32128-171">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

## <a name="event-handling"></a><span data-ttu-id="32128-172">Gestion des événements</span><span class="sxs-lookup"><span data-stu-id="32128-172">Event handling</span></span>

<span data-ttu-id="32128-173">Composants de Razor fournissent des fonctionnalités de gestion des événements.</span><span class="sxs-lookup"><span data-stu-id="32128-173">Razor Components provide event handling features.</span></span> <span data-ttu-id="32128-174">Pour un attribut d’élément HTML nommé `on<event>` (par exemple, `onclick`, `onsubmit`) avec une valeur typée de délégué, les composants de Razor traite la valeur de l’attribut comme gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="32128-174">For an HTML element attribute named `on<event>` (for example, `onclick`, `onsubmit`) with a delegate-typed value, Razor Components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="32128-175">Le nom d’attribut commence toujours par `on`.</span><span class="sxs-lookup"><span data-stu-id="32128-175">The attribute's name always starts with `on`.</span></span>

<span data-ttu-id="32128-176">Le code suivant appelle la `UpdateHeading` méthode lorsque le bouton est sélectionné dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="32128-176">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="32128-177">Le code suivant appelle la `CheckboxChanged` méthode lorsque la case à cocher est modifié dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="32128-177">The following code calls the `CheckboxChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged" />

@functions {
    void CheckboxChanged()
    {
        ...
    }
}
```

<span data-ttu-id="32128-178">Gestionnaires d’événements peuvent également être asynchrone et retour un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="32128-178">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="32128-179">Il est inutile d’appeler manuellement `StateHasChanged()`.</span><span class="sxs-lookup"><span data-stu-id="32128-179">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="32128-180">Exceptions sont enregistrées lorsqu’ils se produisent.</span><span class="sxs-lookup"><span data-stu-id="32128-180">Exceptions are logged when they occur.</span></span>

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="32128-181">Pour certains événements, types d’arguments spécifiques à l’événement événement sont autorisées.</span><span class="sxs-lookup"><span data-stu-id="32128-181">For some events, event-specific event argument types are permitted.</span></span> <span data-ttu-id="32128-182">Si l’accès à l’un de ces types d’événements n’est pas nécessaire, elle n’est pas requise dans l’appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="32128-182">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="32128-183">La liste d’arguments d’événement pris en charge est :</span><span class="sxs-lookup"><span data-stu-id="32128-183">The list of supported event arguments is:</span></span>

* <span data-ttu-id="32128-184">UIEventArgs</span><span class="sxs-lookup"><span data-stu-id="32128-184">UIEventArgs</span></span>
* <span data-ttu-id="32128-185">UIChangeEventArgs</span><span class="sxs-lookup"><span data-stu-id="32128-185">UIChangeEventArgs</span></span>
* <span data-ttu-id="32128-186">UIKeyboardEventArgs</span><span class="sxs-lookup"><span data-stu-id="32128-186">UIKeyboardEventArgs</span></span>
* <span data-ttu-id="32128-187">UIMouseEventArgs</span><span class="sxs-lookup"><span data-stu-id="32128-187">UIMouseEventArgs</span></span>

<span data-ttu-id="32128-188">Expressions lambda peuvent également être utilisées :</span><span class="sxs-lookup"><span data-stu-id="32128-188">Lambda expressions can also be used:</span></span>

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="32128-189">Il est souvent utile de fermer sur des valeurs supplémentaires, comme lors de l’itération sur un jeu d’éléments.</span><span class="sxs-lookup"><span data-stu-id="32128-189">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="32128-190">L’exemple suivant crée trois boutons, chacune appelant `UpdateHeading` en passant un argument d’événement (`UIMouseEventArgs`) et son numéro de bouton (`buttonNumber`) lorsque sélectionné dans l’interface utilisateur :</span><span class="sxs-lookup"><span data-stu-id="32128-190">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary" 
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    string message = "Select a button to learn its position.";

    void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="32128-191">Capturer les références aux composants</span><span class="sxs-lookup"><span data-stu-id="32128-191">Capture references to components</span></span>

<span data-ttu-id="32128-192">Références de composants fournissent un moyen de get une référence à une instance du composant afin que vous pouvez émettre des commandes à cette instance, tels que `Show` ou `Reset`.</span><span class="sxs-lookup"><span data-stu-id="32128-192">Component references provide a way get a reference to a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="32128-193">Pour capturer une référence de composant, ajoutez un `ref` attribut au composant enfant et puis définir un champ avec le même nom et le même type que celui du composant enfant.</span><span class="sxs-lookup"><span data-stu-id="32128-193">To capture a component reference, add a `ref` attribute to the child component and then define a field with the same name and the same type as the child component.</span></span>

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    MyLoginDialog loginDialog;

    void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="32128-194">Lorsque le composant est affiché, le `loginDialog` champ est renseigné avec la `MyLoginDialog` instance du composant enfant.</span><span class="sxs-lookup"><span data-stu-id="32128-194">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="32128-195">Vous pouvez ensuite appeler les méthodes de .NET sur l’instance du composant.</span><span class="sxs-lookup"><span data-stu-id="32128-195">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32128-196">Le `loginDialog` variable est renseignée uniquement une fois que le composant est restitué et sa sortie inclut le `MyLoginDialog` élément.</span><span class="sxs-lookup"><span data-stu-id="32128-196">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="32128-197">Jusqu'à ce point, il n’a rien à faire référence.</span><span class="sxs-lookup"><span data-stu-id="32128-197">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="32128-198">Pour manipuler les références de composants une fois que le composant a terminé le rendu, utilisez la `OnAfterRenderAsync` ou `OnAfterRender` méthodes.</span><span class="sxs-lookup"><span data-stu-id="32128-198">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<span data-ttu-id="32128-199">Tandis que les références de composant de capture utilise une syntaxe similaire à [capture les références d’élément](xref:razor-components/javascript-interop#capture-references-to-elements), il n’est pas un [JavaScript interop](xref:razor-components/javascript-interop) fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="32128-199">While capturing component references uses a similar syntax to [capturing element references](xref:razor-components/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:razor-components/javascript-interop) feature.</span></span> <span data-ttu-id="32128-200">Références de composants ne sont pas transmis au code JavaScript ; ils sont utilisés uniquement dans le code .NET.</span><span class="sxs-lookup"><span data-stu-id="32128-200">Component references aren't passed to JavaScript code; they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="32128-201">Faire **pas** utilisent des références de composant pour muter l’état des composants de l’enfant.</span><span class="sxs-lookup"><span data-stu-id="32128-201">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="32128-202">Utilisez plutôt les paramètres déclaratifs normales pour passer des données à des composants enfants.</span><span class="sxs-lookup"><span data-stu-id="32128-202">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="32128-203">Cela entraîne des composants enfants de restituer automatiquement aux moments appropriés à.</span><span class="sxs-lookup"><span data-stu-id="32128-203">This causes child components to rerender at the correct times automatically.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="32128-204">Méthodes de cycle de vie</span><span class="sxs-lookup"><span data-stu-id="32128-204">Lifecycle methods</span></span>

<span data-ttu-id="32128-205">`OnInitAsync` et `OnInit` exécuter du code pour initialiser le composant.</span><span class="sxs-lookup"><span data-stu-id="32128-205">`OnInitAsync` and `OnInit` execute code to initialize the component.</span></span> <span data-ttu-id="32128-206">Pour effectuer une opération asynchrone, utilisez `OnInitAsync` et le `await` mot clé sur l’opération :</span><span class="sxs-lookup"><span data-stu-id="32128-206">To perform an asynchronous operation, use `OnInitAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

<span data-ttu-id="32128-207">Pour une opération synchrone, utilisez `OnInit`:</span><span class="sxs-lookup"><span data-stu-id="32128-207">For a synchronous operation, use `OnInit`:</span></span>

```csharp
protected override void OnInit()
{
    ...
}
```

<span data-ttu-id="32128-208">`OnParametersSetAsync` et `OnParametersSet` sont appelés quand un composant a reçu les paramètres de son parent et les valeurs sont affectées aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="32128-208">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="32128-209">Ces méthodes sont exécutées après l’initialisation du composant et ensuite chaque fois que le composant est rendu :</span><span class="sxs-lookup"><span data-stu-id="32128-209">These methods are executed after component initialization and then each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="32128-210">`OnAfterRenderAsync` et `OnAfterRender` sont appelées après un composant a terminé le rendu.</span><span class="sxs-lookup"><span data-stu-id="32128-210">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="32128-211">Références de composant et d’élément sont renseignées à ce stade.</span><span class="sxs-lookup"><span data-stu-id="32128-211">Element and component references are populated at this point.</span></span> <span data-ttu-id="32128-212">Cette étape permet d’effectuer les étapes d’initialisation supplémentaires à l’aide du contenu affiché, telles que l’activation des bibliothèques JavaScript tierces qui opèrent sur les éléments DOM rendus.</span><span class="sxs-lookup"><span data-stu-id="32128-212">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

<span data-ttu-id="32128-213">`SetParameters` peut être substituée pour exécuter du code avant de définir des paramètres :</span><span class="sxs-lookup"><span data-stu-id="32128-213">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="32128-214">Si `base.SetParameters` n’est pas appelé, le code personnalisé peut interpréter la valeur des paramètres entrants dans une façon requis.</span><span class="sxs-lookup"><span data-stu-id="32128-214">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="32128-215">Par exemple, les paramètres entrants ne sont pas nécessaire d’affecter aux propriétés sur la classe.</span><span class="sxs-lookup"><span data-stu-id="32128-215">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

<span data-ttu-id="32128-216">`ShouldRender` peut être substituée pour supprimer l’actualisation de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32128-216">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="32128-217">Si l’implémentation retourne `true`, l’interface utilisateur est actualisée.</span><span class="sxs-lookup"><span data-stu-id="32128-217">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="32128-218">Même si `ShouldRender` est substitué, le composant est toujours affichée à l’origine.</span><span class="sxs-lookup"><span data-stu-id="32128-218">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="32128-219">Élimination de composant avec IDisposable</span><span class="sxs-lookup"><span data-stu-id="32128-219">Component disposal with IDisposable</span></span>

<span data-ttu-id="32128-220">Si un composant implémente <xref:System.IDisposable>, le [Dispose, méthode](/dotnet/standard/garbage-collection/implementing-dispose) est appelée lorsque le composant est supprimé de l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32128-220">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="32128-221">Utilise le composant suivant `@implements IDisposable` et `Dispose` méthode :</span><span class="sxs-lookup"><span data-stu-id="32128-221">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="32128-222">Routage</span><span class="sxs-lookup"><span data-stu-id="32128-222">Routing</span></span>

<span data-ttu-id="32128-223">Routage dans les composants de Razor est obtenu en fournissant un modèle d’itinéraire pour chaque composant accessible dans l’application.</span><span class="sxs-lookup"><span data-stu-id="32128-223">Routing in Razor Components is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="32128-224">Quand un *.cshtml* de fichiers avec un `@page` directive est compilée, la classe générée est donnée un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> spécifiant le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="32128-224">When a *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="32128-225">Lors de l’exécution, le routeur recherche de classes de composant avec un `RouteAttribute` et restitue le composant a un modèle d’itinéraire qui correspond à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="32128-225">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="32128-226">Plusieurs modèles d’itinéraire peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="32128-226">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="32128-227">Le composant suivant répond aux demandes de `/BlazorRoute` et `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="32128-227">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="32128-228">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="32128-228">Route parameters</span></span>

<span data-ttu-id="32128-229">Composants peuvent recevoir des paramètres d’itinéraire à partir du modèle d’itinéraire fourni dans la `@page` directive.</span><span class="sxs-lookup"><span data-stu-id="32128-229">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="32128-230">Le routeur utilise les paramètres d’itinéraire pour remplir les paramètres de composant correspondants.</span><span class="sxs-lookup"><span data-stu-id="32128-230">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="32128-231">*RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-231">*RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

<span data-ttu-id="32128-232">Paramètres facultatifs ne sont pas pris en charge, par conséquent, deux `@page` directives sont appliquées dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="32128-232">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="32128-233">La première permet la navigation vers le composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="32128-233">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="32128-234">La seconde `@page` directive prend le `{text}` paramètre d’itinéraire et affecte la valeur à la `Text` propriété.</span><span class="sxs-lookup"><span data-stu-id="32128-234">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="32128-235">Héritage de classe de base pour une expérience « code-behind »</span><span class="sxs-lookup"><span data-stu-id="32128-235">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="32128-236">Fichiers composants (*.cshtml*) mélanger le balisage HTML et C# code dans le même fichier de traitement.</span><span class="sxs-lookup"><span data-stu-id="32128-236">Component files (*.cshtml*) mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="32128-237">Le `@inherits` directive peut être utilisée pour fournir une expérience de « code-behind » qui sépare le balisage de composant à partir du code de traitement des applications de composants de Razor.</span><span class="sxs-lookup"><span data-stu-id="32128-237">The `@inherits` directive can be used to provide Razor Components apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="32128-238">Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) montre comment un composant peut hériter d’une classe de base, `BlazorRocksBase`, pour fournir les propriétés et les méthodes du composant.</span><span class="sxs-lookup"><span data-stu-id="32128-238">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="32128-239">*BlazorRocks.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-239">*BlazorRocks.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

<span data-ttu-id="32128-240">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="32128-240">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="32128-241">La classe de base doit dériver de `BlazorComponent`.</span><span class="sxs-lookup"><span data-stu-id="32128-241">The base class should derive from `BlazorComponent`.</span></span>

## <a name="razor-support"></a><span data-ttu-id="32128-242">Prise en charge de Razor</span><span class="sxs-lookup"><span data-stu-id="32128-242">Razor support</span></span>

<span data-ttu-id="32128-243">**Directives Razor**</span><span class="sxs-lookup"><span data-stu-id="32128-243">**Razor directives**</span></span>

<span data-ttu-id="32128-244">Directives Razor sont affichés dans le tableau suivant.</span><span class="sxs-lookup"><span data-stu-id="32128-244">Razor directives are shown in the following table.</span></span>

| <span data-ttu-id="32128-245">Directive</span><span class="sxs-lookup"><span data-stu-id="32128-245">Directive</span></span> | <span data-ttu-id="32128-246">Description</span><span class="sxs-lookup"><span data-stu-id="32128-246">Description</span></span> |
| --------- | ----------- |
| [<span data-ttu-id="32128-247">\@functions</span><span class="sxs-lookup"><span data-stu-id="32128-247">\@functions</span></span>](xref:mvc/views/razor#section-5) | <span data-ttu-id="32128-248">Ajoute un C# bloc de code pour un composant.</span><span class="sxs-lookup"><span data-stu-id="32128-248">Adds a C# code block to a component.</span></span> |
| `@implements` | <span data-ttu-id="32128-249">Implémente une interface pour la classe de composant généré.</span><span class="sxs-lookup"><span data-stu-id="32128-249">Implements an interface for the generated component class.</span></span> |
| [<span data-ttu-id="32128-250">\@inherits</span><span class="sxs-lookup"><span data-stu-id="32128-250">\@inherits</span></span>](xref:mvc/views/razor#section-3) | <span data-ttu-id="32128-251">Fournit un contrôle complet de la classe qui hérite du composant.</span><span class="sxs-lookup"><span data-stu-id="32128-251">Provides full control of the class that the component inherits.</span></span> |
| [<span data-ttu-id="32128-252">\@inject</span><span class="sxs-lookup"><span data-stu-id="32128-252">\@inject</span></span>](xref:mvc/views/razor#section-4) | <span data-ttu-id="32128-253">Permet l’injection de code à partir de service le [conteneur de service](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="32128-253">Enables service injection from the [service container](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="32128-254">Pour plus d’informations, consultez [Injection de dépendances dans les vues](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="32128-254">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span> |
| `@layout` | <span data-ttu-id="32128-255">Spécifie un composant de mise en forme.</span><span class="sxs-lookup"><span data-stu-id="32128-255">Specifies a layout component.</span></span> <span data-ttu-id="32128-256">Composants de mise en page sont utilisés afin d’éviter des incohérences et la duplication de code.</span><span class="sxs-lookup"><span data-stu-id="32128-256">Layout components are used to avoid code duplication and inconsistency.</span></span> |
| [<span data-ttu-id="32128-257">\@page</span><span class="sxs-lookup"><span data-stu-id="32128-257">\@page</span></span>](xref:razor-pages/index#razor-pages) | <span data-ttu-id="32128-258">Spécifie que le composant doit gérer les demandes directement.</span><span class="sxs-lookup"><span data-stu-id="32128-258">Specifies that the component should handle requests directly.</span></span> <span data-ttu-id="32128-259">Le `@page` directive peut être spécifiée avec un itinéraire et des paramètres facultatifs.</span><span class="sxs-lookup"><span data-stu-id="32128-259">The `@page` directive can be specified with a route and optional parameters.</span></span> <span data-ttu-id="32128-260">Contrairement aux Pages Razor, le `@page` directive n’a pas besoin être la première directive en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="32128-260">Unlike Razor Pages, the `@page` directive doesn't need to be the first directive at the top of the file.</span></span> <span data-ttu-id="32128-261">Pour plus d’informations, consultez [Routage](xref:razor-components/routing).</span><span class="sxs-lookup"><span data-stu-id="32128-261">For more information, see [Routing](xref:razor-components/routing).</span></span> |
| [<span data-ttu-id="32128-262">\@using</span><span class="sxs-lookup"><span data-stu-id="32128-262">\@using</span></span>](xref:mvc/views/razor#using) | <span data-ttu-id="32128-263">Ajoute le C# `using` directive à la classe de composant généré.</span><span class="sxs-lookup"><span data-stu-id="32128-263">Adds the C# `using` directive to the generated component class.</span></span> |
| [<span data-ttu-id="32128-264">\@addTagHelper</span><span class="sxs-lookup"><span data-stu-id="32128-264">\@addTagHelper</span></span>](xref:mvc/views/razor#tag-helpers) | <span data-ttu-id="32128-265">Utiliser `@addTagHelper` pour utiliser un composant dans un autre assembly que l’assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="32128-265">Use `@addTagHelper` to use a component in a different assembly than the app's assembly.</span></span> |

<span data-ttu-id="32128-266">**Attributs conditionnels**</span><span class="sxs-lookup"><span data-stu-id="32128-266">**Conditional attributes**</span></span>

<span data-ttu-id="32128-267">Les attributs sont rendus conditionnelle basée sur la valeur .NET.</span><span class="sxs-lookup"><span data-stu-id="32128-267">Attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="32128-268">Si la valeur est `false` ou `null`, l’attribut n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="32128-268">If the value is `false` or `null`,  the attribute isn't rendered.</span></span> <span data-ttu-id="32128-269">Si la valeur est `true`, l’attribut est rendu réduite.</span><span class="sxs-lookup"><span data-stu-id="32128-269">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="32128-270">Dans l’exemple suivant, `IsCompleted` détermine si `checked` est restitué dans le balisage du contrôle :</span><span class="sxs-lookup"><span data-stu-id="32128-270">In the following example, `IsCompleted` determines if `checked` is rendered in the control's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

<span data-ttu-id="32128-271">Si `IsCompleted` est `true`, la case à cocher est rendue en tant que :</span><span class="sxs-lookup"><span data-stu-id="32128-271">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="32128-272">Si `IsCompleted` est `false`, la case à cocher est rendue en tant que :</span><span class="sxs-lookup"><span data-stu-id="32128-272">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="32128-273">**Informations supplémentaires sur Razor**</span><span class="sxs-lookup"><span data-stu-id="32128-273">**Additional information on Razor**</span></span>

<span data-ttu-id="32128-274">Pour plus d’informations sur Razor, consultez le [référence de la syntaxe Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="32128-274">For more information on Razor, see the [Razor syntax reference](xref:mvc/views/razor).</span></span>

## <a name="raw-html"></a><span data-ttu-id="32128-275">HTML brut</span><span class="sxs-lookup"><span data-stu-id="32128-275">Raw HTML</span></span>

<span data-ttu-id="32128-276">Les chaînes sont normalement restitués à l’aide de nœuds de texte DOM, ce qui signifie que tout balisage, qu'elles peuvent contenir est ignorée et traitée en tant que texte littéral.</span><span class="sxs-lookup"><span data-stu-id="32128-276">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="32128-277">Pour afficher le HTML brut, encapsuler le contenu HTML dans un `MarkupString` valeur.</span><span class="sxs-lookup"><span data-stu-id="32128-277">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="32128-278">La valeur est analysée en tant que HTML ou SVG et insérée dans le DOM.</span><span class="sxs-lookup"><span data-stu-id="32128-278">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="32128-279">Rendu HTML brut construit à partir d’une non fiables source est un **risque de sécurité** et doit être évitée !</span><span class="sxs-lookup"><span data-stu-id="32128-279">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="32128-280">L’exemple suivant illustre l’utilisation du `MarkupString` type à ajouter un bloc de contenu HTML statique à la sortie rendue d’un composant :</span><span class="sxs-lookup"><span data-stu-id="32128-280">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@functions {
    string myMarkup = "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="32128-281">Composants basés sur des modèles</span><span class="sxs-lookup"><span data-stu-id="32128-281">Templated components</span></span>

<span data-ttu-id="32128-282">Composants basés sur des modèles sont des composants qui acceptent un ou plusieurs modèles d’interface utilisateur en tant que paramètres, ce qui peuvent ensuite être utilisés comme partie de la logique de rendu du composant.</span><span class="sxs-lookup"><span data-stu-id="32128-282">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="32128-283">Composants basés sur des modèles vous permettent de vous montre comment créer des composants de haut niveau qui sont plus facilement réutilisables que les composants standards.</span><span class="sxs-lookup"><span data-stu-id="32128-283">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="32128-284">Voici deux exemples sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="32128-284">A couple of examples include:</span></span>

* <span data-ttu-id="32128-285">Un composant de table qui permet à un utilisateur de spécifier des modèles pour l’en-tête de la table, de lignes et de pied de page.</span><span class="sxs-lookup"><span data-stu-id="32128-285">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="32128-286">Un composant de liste qui permet à un utilisateur de spécifier un modèle pour le rendu des éléments dans une liste.</span><span class="sxs-lookup"><span data-stu-id="32128-286">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="32128-287">Paramètres de modèle</span><span class="sxs-lookup"><span data-stu-id="32128-287">Template parameters</span></span>

<span data-ttu-id="32128-288">Un composant basé sur un modèle est défini en spécifiant un ou plusieurs paramètres de composant de type `RenderFragment` ou `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="32128-288">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="32128-289">Un fragment de rendu représente un segment de l’interface utilisateur qui est restitué par le composant.</span><span class="sxs-lookup"><span data-stu-id="32128-289">A render fragment represents a segment of UI that is rendered by the component.</span></span> <span data-ttu-id="32128-290">Un fragment de rendu prend un paramètre qui peut être spécifié lorsque le fragment de rendu est appelé.</span><span class="sxs-lookup"><span data-stu-id="32128-290">A render fragment optionally takes a parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="32128-291">*Components/TableTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-291">*Components/TableTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

<span data-ttu-id="32128-292">Lorsque vous utilisez un composant basé sur un modèle, les paramètres du modèle peuvent être spécifiés à l’aide des éléments enfants qui correspondent aux noms des paramètres (`TableHeader` et `RowTemplate` dans l’exemple suivant) :</span><span class="sxs-lookup"><span data-stu-id="32128-292">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="32128-293">Paramètres de contexte de modèle</span><span class="sxs-lookup"><span data-stu-id="32128-293">Template context parameters</span></span>

<span data-ttu-id="32128-294">Arguments de composant de type `RenderFragment<T>` passé comme éléments ont un paramètre implicit nommé `context` (par exemple à partir de l’exemple de code précédent, `@context.PetId`), mais vous pouvez modifier le nom de paramètre en utilisant le `Context` attribut sur l’enfant élément.</span><span class="sxs-lookup"><span data-stu-id="32128-294">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="32128-295">Dans l’exemple suivant, le `RowTemplate` l’élément `Context` attribut spécifie le `pet` paramètre :</span><span class="sxs-lookup"><span data-stu-id="32128-295">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="32128-296">Vous pouvez également spécifier le `Context` attribut sur l’élément de composant.</span><span class="sxs-lookup"><span data-stu-id="32128-296">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="32128-297">Spécifié `Context` attribut s’applique à tous les paramètres de modèle spécifié.</span><span class="sxs-lookup"><span data-stu-id="32128-297">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="32128-298">Cela peut être utile lorsque vous souhaitez spécifier le nom du paramètre de contenu pour le contenu enfant implicite (sans habillage du n’importe quel élément enfant).</span><span class="sxs-lookup"><span data-stu-id="32128-298">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="32128-299">Dans l’exemple suivant, le `Context` attribut apparaît sur le `TableTemplate` élément et s’applique à tous les paramètres de modèle :</span><span class="sxs-lookup"><span data-stu-id="32128-299">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="32128-300">Composants typées de génériques</span><span class="sxs-lookup"><span data-stu-id="32128-300">Generic-typed components</span></span>

<span data-ttu-id="32128-301">Composants basés sur des modèles sont souvent de manière générique de type.</span><span class="sxs-lookup"><span data-stu-id="32128-301">Templated components are often generically typed.</span></span> <span data-ttu-id="32128-302">Par exemple, un composant de modèle de vue de liste générique peut être utilisé pour restituer `IEnumerable<T>` valeurs.</span><span class="sxs-lookup"><span data-stu-id="32128-302">For example, a generic List View Template component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="32128-303">Pour définir un composant générique, utilisez la `@typeparam` directive pour spécifier les paramètres de type.</span><span class="sxs-lookup"><span data-stu-id="32128-303">To define a generic component, use the `@typeparam` directive to specify type parameters.</span></span>

<span data-ttu-id="32128-304">*Components/ListViewTemplate.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-304">*Components/ListViewTemplate.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml?highlight=1)]

<span data-ttu-id="32128-305">Lorsque vous utilisez des composants typées de génériques, le paramètre de type est déduit si possible :</span><span class="sxs-lookup"><span data-stu-id="32128-305">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="32128-306">Sinon, le paramètre de type doit être explicitement spécifié à l’aide d’un attribut qui correspond au nom du paramètre de type.</span><span class="sxs-lookup"><span data-stu-id="32128-306">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="32128-307">Dans l’exemple suivant, `TItem="Pet"` Spécifie le type :</span><span class="sxs-lookup"><span data-stu-id="32128-307">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="32128-308">Paramètres et valeurs en cascade</span><span class="sxs-lookup"><span data-stu-id="32128-308">Cascading values and parameters</span></span>

<span data-ttu-id="32128-309">Dans certains scénarios, il est peu pratique pour transmettre les données à partir d’un composant d’ancêtre à un composant descendant à l’aide [paramètres du composant](#component-parameters), en particulier lorsqu’il existe plusieurs couches de composant.</span><span class="sxs-lookup"><span data-stu-id="32128-309">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="32128-310">Les valeurs et les paramètres en cascade résolvent ce problème en fournissant un moyen pratique d’un composant d’ancêtre à fournir une valeur pour tous ses composants descendants.</span><span class="sxs-lookup"><span data-stu-id="32128-310">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="32128-311">Les valeurs et les paramètres en cascade fournissent également une approche pour coordonner des composants.</span><span class="sxs-lookup"><span data-stu-id="32128-311">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="32128-312">Exemple de thème</span><span class="sxs-lookup"><span data-stu-id="32128-312">Theme example</span></span>

<span data-ttu-id="32128-313">Dans l’exemple suivant *thème* exemple à partir de l’exemple d’application, le `ThemeInfo` classe spécifie les informations de thème pour le flux vers le bas de la hiérarchie des composants afin que tous les boutons dans une partie donnée de l’application partagent le même style.</span><span class="sxs-lookup"><span data-stu-id="32128-313">In the following *Theme* example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="32128-314">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="32128-314">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="32128-315">Un composant ancêtre peut fournir une valeur en cascade à l’aide du composant de valeur en cascade.</span><span class="sxs-lookup"><span data-stu-id="32128-315">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="32128-316">Le composant de valeur en cascade encapsule une sous-arborescence de la hiérarchie des composants et fournit une valeur unique à tous les composants dans cette sous-arborescence.</span><span class="sxs-lookup"><span data-stu-id="32128-316">The Cascading Value component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="32128-317">Par exemple, l’exemple d’application spécifie les informations de thème (`ThemeInfo`) dans une des dispositions de l’application comme un paramètre en cascade pour tous les composants qui composent le corps de la disposition de la `@Body` propriété.</span><span class="sxs-lookup"><span data-stu-id="32128-317">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="32128-318">`ButtonClass` reçoit une valeur de `btn-success` dans le composant de mise en page.</span><span class="sxs-lookup"><span data-stu-id="32128-318">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="32128-319">N’importe quel composant descendant peut consommer de cette propriété via le `ThemeInfo` objet en cascade.</span><span class="sxs-lookup"><span data-stu-id="32128-319">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="32128-320">*Shared/CascadingValuesParametersLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-320">*Shared/CascadingValuesParametersLayout.cshtml*:</span></span>

```cshtml
@inherits BlazorLayoutComponent
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@functions {
    ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="32128-321">Pour rendre des valeurs en cascade, les composants déclarer des paramètres en cascade à l’aide de la `[CascadingParameter]` attribut ou selon une valeur de nom de chaîne :</span><span class="sxs-lookup"><span data-stu-id="32128-321">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")] PermInfo Permissions { get; set; }
```

<span data-ttu-id="32128-322">Liaison avec une valeur de nom de chaîne s’applique si vous avez plusieurs valeurs en cascade du même type et que vous avez besoin pour les différencier dans le même sous-arbre.</span><span class="sxs-lookup"><span data-stu-id="32128-322">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="32128-323">Valeurs en cascade sont liés aux paramètres en cascade par type.</span><span class="sxs-lookup"><span data-stu-id="32128-323">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="32128-324">Dans l’exemple d’application, le composant de thème des paramètres de valeurs en cascade est lié à la `ThemeInfo` valeur en cascade à un paramètre en cascade.</span><span class="sxs-lookup"><span data-stu-id="32128-324">In the sample app, the Cascading Values Parameters Theme component binds to the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="32128-325">Le paramètre est utilisé pour définir la classe CSS pour un des boutons affichés par le composant.</span><span class="sxs-lookup"><span data-stu-id="32128-325">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="32128-326">*Pages/CascadingValuesParametersTheme.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-326">*Pages/CascadingValuesParametersTheme.cshtml*:</span></span>

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a><span data-ttu-id="32128-327">Exemple de TabSet</span><span class="sxs-lookup"><span data-stu-id="32128-327">TabSet example</span></span>

<span data-ttu-id="32128-328">Paramètres en cascade permettent également aux composants de collaborer au travers de la hiérarchie des composants.</span><span class="sxs-lookup"><span data-stu-id="32128-328">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="32128-329">Par exemple, considérez les éléments suivants *TabSet* exemple dans l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="32128-329">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="32128-330">L’exemple d’application a une `ITab` interface qui implémentent des onglets :</span><span class="sxs-lookup"><span data-stu-id="32128-330">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="32128-331">Le composant TabSet des paramètres de valeurs en cascade utilise le composant ensemble d’onglets, qui contient plusieurs composants d’onglet :</span><span class="sxs-lookup"><span data-stu-id="32128-331">The Cascading Values Parameters TabSet component uses the Tab Set component, which contains several Tab components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

<span data-ttu-id="32128-332">Les composants d’onglet enfants ne sont pas explicitement transmis en tant que paramètres à l’ensemble de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="32128-332">The child Tab components aren't explicitly passed as parameters to the Tab Set.</span></span> <span data-ttu-id="32128-333">Au lieu de cela, les composants d’onglet enfants font partie du contenu enfant de l’ensemble de l’onglet.</span><span class="sxs-lookup"><span data-stu-id="32128-333">Instead, the child Tab components are part of the child content of the Tab Set.</span></span> <span data-ttu-id="32128-334">Toutefois, l’ensemble de l’onglet doit tout savoir sur chaque composant de l’onglet afin qu’il puisse afficher les en-têtes et l’onglet actif. Pour activer cette coordination sans nécessiter du code supplémentaire, le composant de l’ensemble d’onglets *peut fournir lui-même comme une valeur en cascade* qui est ensuite récupéré par les composants d’onglet descendants.</span><span class="sxs-lookup"><span data-stu-id="32128-334">However, the Tab Set still needs to know about each Tab component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the Tab Set component *can provide itself as a cascading value* that is then picked up by the descendent Tab components.</span></span>

<span data-ttu-id="32128-335">*Components/TabSet.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-335">*Components/TabSet.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

<span data-ttu-id="32128-336">La capture de composants onglet descendante l’ensemble d’onglets contenant comme un paramètre en cascade, afin que les composants de l’onglet ajoutent eux-mêmes à l’ensemble d’onglets et les coordonnées sur l’onglet est actif.</span><span class="sxs-lookup"><span data-stu-id="32128-336">The descendent Tab components capture the containing Tab Set as a cascading parameter, so the Tab components add themselves to the Tab Set and coordinate on which tab is active.</span></span>

<span data-ttu-id="32128-337">*Components/Tab.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-337">*Components/Tab.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a><span data-ttu-id="32128-338">Modèles Razor</span><span class="sxs-lookup"><span data-stu-id="32128-338">Razor templates</span></span>

<span data-ttu-id="32128-339">Restituer fragments peuvent être définies à l’aide de la syntaxe du modèle Razor.</span><span class="sxs-lookup"><span data-stu-id="32128-339">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="32128-340">Les modèles Razor sont un moyen de définir un extrait de code de l’interface utilisateur et le format suivant :</span><span class="sxs-lookup"><span data-stu-id="32128-340">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="32128-341">L’exemple suivant illustre comment spécifier `RenderFragment` et `RenderFragment<T>` valeurs.</span><span class="sxs-lookup"><span data-stu-id="32128-341">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values.</span></span>

<span data-ttu-id="32128-342">*RazorTemplates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="32128-342">*RazorTemplates.cshtml*:</span></span>

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

<span data-ttu-id="32128-343">Restituer des fragments définis à l’aide de Razor modèles peuvent être passés comme arguments à des composants basés sur des modèles ou restitués directement.</span><span class="sxs-lookup"><span data-stu-id="32128-343">Render fragments defined using Razor templates can be passed as arguments to templated components or rendered directly.</span></span> <span data-ttu-id="32128-344">Par exemple, les modèles précédentes sont directement restitués avec le balisage Razor suivant :</span><span class="sxs-lookup"><span data-stu-id="32128-344">For example, the previous templates are directly rendered with the following Razor markup:</span></span>

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

<span data-ttu-id="32128-345">Sortie rendue :</span><span class="sxs-lookup"><span data-stu-id="32128-345">Rendered output:</span></span>

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```
