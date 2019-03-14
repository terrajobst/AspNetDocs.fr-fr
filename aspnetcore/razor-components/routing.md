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
# <a name="razor-components-routing"></a>Composants de Razor routage

Par [Luke Latham](https://github.com/guardrex)

Découvrez comment acheminer les demandes dans les applications et sur le composant NavLink.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)). Consultez la rubrique [Bien démarrer](xref:razor-components/get-started) pour connaître les prérequis.

## <a name="route-templates"></a>Modèles de routes

Le `<Router>` composant Active le routage, et un modèle d’itinéraire est fourni pour chaque composant accessible. Le `<Router>` composant apparaît dans le *App.cshtml* fichier :

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

Quand un  *\*.cshtml* de fichiers avec un `@page` directive est compilée, la classe générée est donnée un [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) spécifiant le modèle d’itinéraire. Lors de l’exécution, le routeur recherche de classes de composant avec un `RouteAttribute` et restitue le composant a un modèle d’itinéraire qui correspond à l’URL demandée.

Plusieurs modèles d’itinéraire peuvent être appliqués à un composant. Dans le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), le composant suivant répond aux demandes de `/BlazorRoute` et `/DifferentBlazorRoute`.

*Pages/BlazorRoute.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

`<Router>` prend en charge la définition d’un composant de secours pour le rendu lorsqu’un itinéraire demandé n’est pas résolu. Activer ce scénario participer en définissant le `FallbackComponent` paramètre vers le type de la classe de composant de secours.

L’exemple suivant définit un composant défini dans *Pages/MyFallbackRazorComponent.cshtml* en tant que le composant de secours pour un `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Pour générer des itinéraires correctement, l’application doit inclure un `<base>` balise dans son *wwwroot/index.HTML* fichier avec le chemin de base application spécifié dans le `href` attribut (`<base href="/" />`). Pour plus d’informations, consultez [hôte et le déploiement : Chemin d’accès de base application](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="route-parameters"></a>Paramètres d’itinéraire

Le routeur utilise les paramètres d’itinéraire pour remplir les paramètres correspondants de composant portant le même nom (non respect de la casse).

*Pages/RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

Paramètres facultatifs ne sont pas pris en charge encore, par conséquent, deux `@page` directives sont appliquées dans l’exemple ci-dessus. La première permet la navigation vers le composant sans paramètre. La seconde `@page` directive prend le `{text}` paramètre d’itinéraire et affecte la valeur à la `Text` propriété.

## <a name="route-constraints"></a>Contraintes de routage

Une contrainte de route applique la mise en correspondance sur un segment de routage à un composant de type.

Dans l’exemple suivant, l’itinéraire vers le composant utilisateurs correspond uniquement si :

* Un `Id` segment de routage est présent sur l’URL de requête.
* Le `Id` segment est un entier (`int`).

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

Les contraintes d’itinéraire indiqués dans le tableau suivant sont disponibles pour utilisation. Pour les contraintes d’itinéraire qui correspondent à la culture dite indifférente, consultez l’avertissement le tableau ci-dessous pour plus d’informations.

| Contrainte | Exemple           | Exemples de correspondances                                                                  | Invariant<br>culture<br>correspondance |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Aucune                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Oui                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Oui                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Oui                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Oui                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Aucune                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Oui                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Oui                              |

> [!WARNING]
> Les contraintes de routage qui vérifient que l’URL peut être convertie en type CLR (comme `int` ou `DateTime`) utilisent toujours la culture invariant. ces contraintes partent du principe que l’URL n’est pas localisable.

## <a name="navlink-component"></a>Composant de NavLink

Utiliser un composant NavLink à la place de HTML  **\<un >** éléments lors de la création de liens de navigation. Un composant NavLink se comporte comme un  **\<un >** élément, à ceci près qu’il active ou désactive un `active` classe CSS en fonction de son `href` correspond à l’URL actuelle. Le `active` classe permet à un utilisateur de comprendre quelle page est la page active parmi les liens de navigation affichées.

Le composant NavMenu dans le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) crée un [Bootstrap](https://getbootstrap.com/docs/) barre de navigation qui montre comment utiliser les composants NavLink. Le balisage suivant montre les deux premières NavLinks dans le *Shared/NavMenu.cshtml* fichier.

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

Il existe deux `NavLinkMatch` options :

* `NavLinkMatch.All` &ndash; Spécifie que le NavLink doit être actif lorsqu’il correspond à l’URL entière en cours.
* `NavLinkMatch.Prefix` &ndash; Spécifie que le NavLink doit être actif lorsqu’il correspond à n’importe quel préfixe de l’URL actuelle.

Dans l’exemple précédent, le NavLink accueil (`href=""`) correspond à toutes les URL et reçoit toujours la `active` classe CSS. Le deuxième NavLink reçoit uniquement le `active` classe lorsque l’utilisateur visite le composant BlazorRoute (`href="BlazorRoute"`).
