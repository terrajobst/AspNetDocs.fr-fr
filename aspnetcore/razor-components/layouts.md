---
title: Dispositions de composants de Razor
author: guardrex
description: Découvrez comment créer des composants réutilisables de disposition pour les applications Blazor et composants de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: 23d8f441c0b3bbde7a73717f6257013831617ec0
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039036"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="f83a1-103">Dispositions de composants de Razor</span><span class="sxs-lookup"><span data-stu-id="f83a1-103">Razor Components layouts</span></span>

<span data-ttu-id="f83a1-104">Par [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="f83a1-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="f83a1-105">En général, les applications contiennent plusieurs pages.</span><span class="sxs-lookup"><span data-stu-id="f83a1-105">Apps typically contain more than one page.</span></span> <span data-ttu-id="f83a1-106">Les éléments de disposition, tels que les menus, les messages de droits d’auteur et les logos, doivent être présents sur toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="f83a1-106">Layout elements, such as menus, copyright messages, and logos, must be present on all pages.</span></span> <span data-ttu-id="f83a1-107">Copier le code de ces éléments de disposition dans toutes les pages d’une application n’est pas une solution efficace.</span><span class="sxs-lookup"><span data-stu-id="f83a1-107">Copying the code of these layout elements into all of the pages of an app isn't an efficient solution.</span></span> <span data-ttu-id="f83a1-108">Toute duplication est difficile à entretenir et probablement mène au contenu incohérent au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="f83a1-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="f83a1-109">*Dispositions* résoudre ce problème.</span><span class="sxs-lookup"><span data-stu-id="f83a1-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="f83a1-110">Techniquement, une disposition est simplement un autre composant.</span><span class="sxs-lookup"><span data-stu-id="f83a1-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="f83a1-111">Une disposition est définie dans un modèle Razor ou dans C# de code et peut contenir la liaison de données, l’injection de dépendances et autres fonctionnalités ordinaires des composants.</span><span class="sxs-lookup"><span data-stu-id="f83a1-111">A layout is defined in a Razor template or in C# code and can contain data binding, dependency injection, and other ordinary features of components.</span></span> <span data-ttu-id="f83a1-112">Deux aspects supplémentaires activer un *composant* dans un *disposition*:</span><span class="sxs-lookup"><span data-stu-id="f83a1-112">Two additional aspects turn a *component* into a *layout*:</span></span>

* <span data-ttu-id="f83a1-113">Le composant de mise en page doit hériter de `BlazorLayoutComponent`.</span><span class="sxs-lookup"><span data-stu-id="f83a1-113">The layout component must inherit from `BlazorLayoutComponent`.</span></span> <span data-ttu-id="f83a1-114">`BlazorLayoutComponent` définit un `Body` propriété qui contient le contenu à restituer à l’intérieur de la disposition.</span><span class="sxs-lookup"><span data-stu-id="f83a1-114">`BlazorLayoutComponent` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="f83a1-115">Le composant de mise en page utilise le `Body` propriété pour spécifier où le contenu du corps doit être restitué à l’aide de la syntaxe Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="f83a1-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="f83a1-116">Lors de la génération `@Body` est remplacé par le contenu de la mise en page.</span><span class="sxs-lookup"><span data-stu-id="f83a1-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="f83a1-117">L’exemple de code suivant montre le modèle d’un composant de mise en page Razor.</span><span class="sxs-lookup"><span data-stu-id="f83a1-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="f83a1-118">Notez l’utilisation de `BlazorLayoutComponent` et `@Body`:</span><span class="sxs-lookup"><span data-stu-id="f83a1-118">Note the use of `BlazorLayoutComponent` and `@Body`:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="f83a1-119">Utiliser une disposition dans un composant</span><span class="sxs-lookup"><span data-stu-id="f83a1-119">Use a layout in a component</span></span>

<span data-ttu-id="f83a1-120">Utilisez la directive Razor `@layout` pour appliquer une mise en page à un composant.</span><span class="sxs-lookup"><span data-stu-id="f83a1-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="f83a1-121">Le compilateur convertit cette directive dans une `LayoutAttribute`, qui est appliqué à la classe de composant.</span><span class="sxs-lookup"><span data-stu-id="f83a1-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="f83a1-122">L’exemple de code suivant illustre le concept.</span><span class="sxs-lookup"><span data-stu-id="f83a1-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="f83a1-123">Le contenu de ce composant est inséré dans le *MasterLayout* à la position de `@Body`:</span><span class="sxs-lookup"><span data-stu-id="f83a1-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="f83a1-124">Sélection de configuration centralisé</span><span class="sxs-lookup"><span data-stu-id="f83a1-124">Centralized layout selection</span></span>

<span data-ttu-id="f83a1-125">Tous les dossiers d’une une application peut éventuellement contenir un fichier de modèle nommé *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f83a1-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="f83a1-126">Le compilateur inclut les directives spécifiées dans le fichier d’importations de vue dans tous les modèles Razor dans le même dossier et dans tous ses sous-dossiers de manière récursive.</span><span class="sxs-lookup"><span data-stu-id="f83a1-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="f83a1-127">Par conséquent, un *_ViewImports.cshtml* fichier contenant `@layout MainLayout` garantit que tous les composants dans un dossier, utilisez le *MainLayout* mise en page.</span><span class="sxs-lookup"><span data-stu-id="f83a1-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="f83a1-128">Il est inutile d’ajouter à plusieurs reprises `@layout` à tous les  *\*.cshtml* fichiers.</span><span class="sxs-lookup"><span data-stu-id="f83a1-128">There's no need to repeatedly add `@layout` to all of the *\*.cshtml* files.</span></span>

<span data-ttu-id="f83a1-129">Notez que le modèle par défaut utilise le *_ViewImports.cshtml* mécanisme pour la sélection de la mise en page.</span><span class="sxs-lookup"><span data-stu-id="f83a1-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="f83a1-130">Une application qui vient d’être créée contient la *_ViewImports.cshtml* de fichiers dans le *Pages* dossier.</span><span class="sxs-lookup"><span data-stu-id="f83a1-130">A newly created app contains the *_ViewImports.cshtml* file in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="f83a1-131">Dispositions imbriquées</span><span class="sxs-lookup"><span data-stu-id="f83a1-131">Nested layouts</span></span>

<span data-ttu-id="f83a1-132">Peut s’agir de dispositions imbriquées.</span><span class="sxs-lookup"><span data-stu-id="f83a1-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="f83a1-133">Un composant peut faire référence à une disposition qui à son tour fait référence à une autre disposition.</span><span class="sxs-lookup"><span data-stu-id="f83a1-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="f83a1-134">Par exemple, les dispositions d’imbrication peuvent être utilisées afin de refléter une structure de menu à plusieurs niveaux.</span><span class="sxs-lookup"><span data-stu-id="f83a1-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="f83a1-135">Exemples de code suivants montrent comment utiliser des dispositions imbriquées.</span><span class="sxs-lookup"><span data-stu-id="f83a1-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="f83a1-136">Le *CustomersComponent.cshtml* fichier est le composant à afficher.</span><span class="sxs-lookup"><span data-stu-id="f83a1-136">The *CustomersComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="f83a1-137">Notez que le composant fait référence à la disposition `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="f83a1-137">Note that the component references the layout `MasterDataLayout`.</span></span>

<span data-ttu-id="f83a1-138">*CustomersComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f83a1-138">*CustomersComponent.cshtml*:</span></span>

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

<span data-ttu-id="f83a1-139">Le *MasterDataLayout.cshtml* fichier fournit le `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="f83a1-139">The *MasterDataLayout.cshtml* file provides the `MasterDataLayout`.</span></span> <span data-ttu-id="f83a1-140">La mise en page fait référence à une autre disposition, `MainLayout`, où elle va être incorporé.</span><span class="sxs-lookup"><span data-stu-id="f83a1-140">The layout references another layout, `MainLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="f83a1-141">*MasterDataLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f83a1-141">*MasterDataLayout.cshtml*:</span></span>

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

<span data-ttu-id="f83a1-142">Enfin, `MainLayout` contient les éléments de disposition de niveau supérieur, tels que l’en-tête, pied de page et menu principal.</span><span class="sxs-lookup"><span data-stu-id="f83a1-142">Finally, `MainLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="f83a1-143">*MainLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f83a1-143">*MainLayout.cshtml*:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
