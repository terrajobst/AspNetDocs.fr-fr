---
title: Composants de Razor routage
author: guardrex
description: Découvrez comment acheminer les demandes dans les applications et sur le composant NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/01/2019
uid: razor-components/routing
ms.openlocfilehash: 5c648ba1bb3846f5baa515e808a98a3b33f81438
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031606"
---
# <a name="razor-components-routing"></a><span data-ttu-id="67bf0-103">Composants de Razor routage</span><span class="sxs-lookup"><span data-stu-id="67bf0-103">Razor Components routing</span></span>

<span data-ttu-id="67bf0-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="67bf0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="67bf0-105">Découvrez comment acheminer les demandes dans les applications et sur le composant NavLink.</span><span class="sxs-lookup"><span data-stu-id="67bf0-105">Learn how to route requests in apps and about the NavLink component.</span></span>

<span data-ttu-id="67bf0-106">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="67bf0-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="67bf0-107">Consultez la rubrique [Bien démarrer](xref:razor-components/get-started) pour connaître les prérequis.</span><span class="sxs-lookup"><span data-stu-id="67bf0-107">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

## <a name="route-templates"></a><span data-ttu-id="67bf0-108">Modèles de routes</span><span class="sxs-lookup"><span data-stu-id="67bf0-108">Route templates</span></span>

<span data-ttu-id="67bf0-109">Le `<Router>` composant Active le routage, et un modèle d’itinéraire est fourni pour chaque composant accessible.</span><span class="sxs-lookup"><span data-stu-id="67bf0-109">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="67bf0-110">Le `<Router>` composant apparaît dans le *App.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="67bf0-110">The `<Router>` component appears in the *App.cshtml* file:</span></span>

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

<span data-ttu-id="67bf0-111">Quand un  *\*.cshtml* de fichiers avec un `@page` directive est compilée, la classe générée est donnée un [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) spécifiant le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="67bf0-111">When a *\*.cshtml* file with an `@page` directive is compiled, the generated class is given a [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) specifying the route template.</span></span> <span data-ttu-id="67bf0-112">Lors de l’exécution, le routeur recherche de classes de composant avec un `RouteAttribute` et restitue le composant a un modèle d’itinéraire qui correspond à l’URL demandée.</span><span class="sxs-lookup"><span data-stu-id="67bf0-112">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="67bf0-113">Plusieurs modèles d’itinéraire peuvent être appliqués à un composant.</span><span class="sxs-lookup"><span data-stu-id="67bf0-113">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="67bf0-114">Dans le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), le composant suivant répond aux demandes de `/BlazorRoute` et `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="67bf0-114">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), the following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="67bf0-115">*Pages/BlazorRoute.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="67bf0-115">*Pages/BlazorRoute.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

<span data-ttu-id="67bf0-116">`<Router>` prend en charge la définition d’un composant de secours pour le rendu lorsqu’un itinéraire demandé n’est pas résolu.</span><span class="sxs-lookup"><span data-stu-id="67bf0-116">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="67bf0-117">Activer ce scénario participer en définissant le `FallbackComponent` paramètre vers le type de la classe de composant de secours.</span><span class="sxs-lookup"><span data-stu-id="67bf0-117">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="67bf0-118">L’exemple suivant définit un composant défini dans *Pages/MyFallbackRazorComponent.cshtml* en tant que le composant de secours pour un `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="67bf0-118">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="67bf0-119">Pour générer des itinéraires correctement, l’application doit inclure un `<base>` balise dans son *wwwroot/index.HTML* fichier avec le chemin de base application spécifié dans le `href` attribut (`<base href="/" />`).</span><span class="sxs-lookup"><span data-stu-id="67bf0-119">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/" />`).</span></span> <span data-ttu-id="67bf0-120">Pour plus d’informations, consultez [hôte et le déploiement : Chemin d’accès de base application](xref:host-and-deploy/razor-components/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="67bf0-120">For more information, see [Host and deploy: App base path](xref:host-and-deploy/razor-components/index#app-base-path).</span></span>

## <a name="route-parameters"></a><span data-ttu-id="67bf0-121">Paramètres d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="67bf0-121">Route parameters</span></span>

<span data-ttu-id="67bf0-122">Le routeur utilise les paramètres d’itinéraire pour remplir les paramètres correspondants de composant portant le même nom (non respect de la casse).</span><span class="sxs-lookup"><span data-stu-id="67bf0-122">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive).</span></span>

<span data-ttu-id="67bf0-123">*Pages/RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="67bf0-123">*Pages/RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

<span data-ttu-id="67bf0-124">Paramètres facultatifs ne sont pas pris en charge encore, par conséquent, deux `@page` directives sont appliquées dans l’exemple ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="67bf0-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="67bf0-125">La première permet la navigation vers le composant sans paramètre.</span><span class="sxs-lookup"><span data-stu-id="67bf0-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="67bf0-126">La seconde `@page` directive prend le `{text}` paramètre d’itinéraire et affecte la valeur à la `Text` propriété.</span><span class="sxs-lookup"><span data-stu-id="67bf0-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="67bf0-127">Contraintes de routage</span><span class="sxs-lookup"><span data-stu-id="67bf0-127">Route constraints</span></span>

<span data-ttu-id="67bf0-128">Une contrainte de route applique la mise en correspondance sur un segment de routage à un composant de type.</span><span class="sxs-lookup"><span data-stu-id="67bf0-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="67bf0-129">Dans l’exemple suivant, l’itinéraire vers le composant utilisateurs correspond uniquement si :</span><span class="sxs-lookup"><span data-stu-id="67bf0-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="67bf0-130">Un `Id` segment de routage est présent sur l’URL de requête.</span><span class="sxs-lookup"><span data-stu-id="67bf0-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="67bf0-131">Le `Id` segment est un entier (`int`).</span><span class="sxs-lookup"><span data-stu-id="67bf0-131">The `Id` segment is an integer (`int`).</span></span>

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

<span data-ttu-id="67bf0-132">Les contraintes d’itinéraire indiqués dans le tableau suivant sont disponibles pour utilisation.</span><span class="sxs-lookup"><span data-stu-id="67bf0-132">The route constraints shown in the following table are available for use.</span></span> <span data-ttu-id="67bf0-133">Pour les contraintes d’itinéraire qui correspondent à la culture dite indifférente, consultez l’avertissement le tableau ci-dessous pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="67bf0-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="67bf0-134">Contrainte</span><span class="sxs-lookup"><span data-stu-id="67bf0-134">Constraint</span></span> | <span data-ttu-id="67bf0-135">Exemple</span><span class="sxs-lookup"><span data-stu-id="67bf0-135">Example</span></span>           | <span data-ttu-id="67bf0-136">Exemples de correspondances</span><span class="sxs-lookup"><span data-stu-id="67bf0-136">Example Matches</span></span>                                                                  | <span data-ttu-id="67bf0-137">Invariant</span><span class="sxs-lookup"><span data-stu-id="67bf0-137">Invariant</span></span><br><span data-ttu-id="67bf0-138">culture</span><span class="sxs-lookup"><span data-stu-id="67bf0-138">culture</span></span><br><span data-ttu-id="67bf0-139">correspondance</span><span class="sxs-lookup"><span data-stu-id="67bf0-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="67bf0-140">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="67bf0-140">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="67bf0-141">Aucune</span><span class="sxs-lookup"><span data-stu-id="67bf0-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="67bf0-142">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="67bf0-142">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="67bf0-143">Oui</span><span class="sxs-lookup"><span data-stu-id="67bf0-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="67bf0-144">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="67bf0-144">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="67bf0-145">Oui</span><span class="sxs-lookup"><span data-stu-id="67bf0-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="67bf0-146">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="67bf0-146">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="67bf0-147">Oui</span><span class="sxs-lookup"><span data-stu-id="67bf0-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="67bf0-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="67bf0-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="67bf0-149">Oui</span><span class="sxs-lookup"><span data-stu-id="67bf0-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="67bf0-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="67bf0-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="67bf0-151">Aucune</span><span class="sxs-lookup"><span data-stu-id="67bf0-151">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="67bf0-152">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="67bf0-152">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="67bf0-153">Oui</span><span class="sxs-lookup"><span data-stu-id="67bf0-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="67bf0-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="67bf0-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="67bf0-155">Oui</span><span class="sxs-lookup"><span data-stu-id="67bf0-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="67bf0-156">Les contraintes de routage qui vérifient que l’URL peut être convertie en type CLR (comme `int` ou `DateTime`) utilisent toujours la culture invariant.</span><span class="sxs-lookup"><span data-stu-id="67bf0-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="67bf0-157">ces contraintes partent du principe que l’URL n’est pas localisable.</span><span class="sxs-lookup"><span data-stu-id="67bf0-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="67bf0-158">Composant de NavLink</span><span class="sxs-lookup"><span data-stu-id="67bf0-158">NavLink component</span></span>

<span data-ttu-id="67bf0-159">Utiliser un composant NavLink à la place de HTML  **\<un >** éléments lors de la création de liens de navigation.</span><span class="sxs-lookup"><span data-stu-id="67bf0-159">Use a NavLink component in place of HTML **\<a>** elements when creating navigation links.</span></span> <span data-ttu-id="67bf0-160">Un composant NavLink se comporte comme un  **\<un >** élément, à ceci près qu’il active ou désactive un `active` classe CSS en fonction de son `href` correspond à l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="67bf0-160">A NavLink component behaves like an **\<a>** element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="67bf0-161">Le `active` classe permet à un utilisateur de comprendre quelle page est la page active parmi les liens de navigation affichées.</span><span class="sxs-lookup"><span data-stu-id="67bf0-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="67bf0-162">Le composant NavMenu dans le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) crée un [Bootstrap](https://getbootstrap.com/docs/) barre de navigation qui montre comment utiliser les composants NavLink.</span><span class="sxs-lookup"><span data-stu-id="67bf0-162">The NavMenu component in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) creates a [Bootstrap](https://getbootstrap.com/docs/) nav bar that demonstrates how to use NavLink components.</span></span> <span data-ttu-id="67bf0-163">Le balisage suivant montre les deux premières NavLinks dans le *Shared/NavMenu.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="67bf0-163">The following markup shows the first two NavLinks in the *Shared/NavMenu.cshtml* file.</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

<span data-ttu-id="67bf0-164">Il existe deux `NavLinkMatch` options :</span><span class="sxs-lookup"><span data-stu-id="67bf0-164">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="67bf0-165">`NavLinkMatch.All` &ndash; Spécifie que le NavLink doit être actif lorsqu’il correspond à l’URL entière en cours.</span><span class="sxs-lookup"><span data-stu-id="67bf0-165">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="67bf0-166">`NavLinkMatch.Prefix` &ndash; Spécifie que le NavLink doit être actif lorsqu’il correspond à n’importe quel préfixe de l’URL actuelle.</span><span class="sxs-lookup"><span data-stu-id="67bf0-166">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="67bf0-167">Dans l’exemple précédent, le NavLink accueil (`href=""`) correspond à toutes les URL et reçoit toujours la `active` classe CSS.</span><span class="sxs-lookup"><span data-stu-id="67bf0-167">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="67bf0-168">Le deuxième NavLink reçoit uniquement le `active` classe lorsque l’utilisateur visite le composant BlazorRoute (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="67bf0-168">The second NavLink only receives the `active` class when the user visits the BlazorRoute component (`href="BlazorRoute"`).</span></span>
