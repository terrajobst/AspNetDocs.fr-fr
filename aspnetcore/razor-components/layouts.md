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
# <a name="razor-components-layouts"></a>Dispositions de composants de Razor

Par [Rainer Stropek](https://www.timecockpit.com)

En général, les applications contiennent plusieurs pages. Les éléments de disposition, tels que les menus, les messages de droits d’auteur et les logos, doivent être présents sur toutes les pages. Copier le code de ces éléments de disposition dans toutes les pages d’une application n’est pas une solution efficace. Toute duplication est difficile à entretenir et probablement mène au contenu incohérent au fil du temps. *Dispositions* résoudre ce problème.

Techniquement, une disposition est simplement un autre composant. Une disposition est définie dans un modèle Razor ou dans C# de code et peut contenir la liaison de données, l’injection de dépendances et autres fonctionnalités ordinaires des composants. Deux aspects supplémentaires activer un *composant* dans un *disposition*:

* Le composant de mise en page doit hériter de `BlazorLayoutComponent`. `BlazorLayoutComponent` définit un `Body` propriété qui contient le contenu à restituer à l’intérieur de la disposition.
* Le composant de mise en page utilise le `Body` propriété pour spécifier où le contenu du corps doit être restitué à l’aide de la syntaxe Razor `@Body`. Lors de la génération `@Body` est remplacé par le contenu de la mise en page.

L’exemple de code suivant montre le modèle d’un composant de mise en page Razor. Notez l’utilisation de `BlazorLayoutComponent` et `@Body`:

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

## <a name="use-a-layout-in-a-component"></a>Utiliser une disposition dans un composant

Utilisez la directive Razor `@layout` pour appliquer une mise en page à un composant. Le compilateur convertit cette directive dans une `LayoutAttribute`, qui est appliqué à la classe de composant.

L’exemple de code suivant illustre le concept. Le contenu de ce composant est inséré dans le *MasterLayout* à la position de `@Body`:

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a>Sélection de configuration centralisé

Tous les dossiers d’une une application peut éventuellement contenir un fichier de modèle nommé *_ViewImports.cshtml*. Le compilateur inclut les directives spécifiées dans le fichier d’importations de vue dans tous les modèles Razor dans le même dossier et dans tous ses sous-dossiers de manière récursive. Par conséquent, un *_ViewImports.cshtml* fichier contenant `@layout MainLayout` garantit que tous les composants dans un dossier, utilisez le *MainLayout* mise en page. Il est inutile d’ajouter à plusieurs reprises `@layout` à tous les  *\*.cshtml* fichiers.

Notez que le modèle par défaut utilise le *_ViewImports.cshtml* mécanisme pour la sélection de la mise en page. Une application qui vient d’être créée contient la *_ViewImports.cshtml* de fichiers dans le *Pages* dossier.

## <a name="nested-layouts"></a>Dispositions imbriquées

Peut s’agir de dispositions imbriquées. Un composant peut faire référence à une disposition qui à son tour fait référence à une autre disposition. Par exemple, les dispositions d’imbrication peuvent être utilisées afin de refléter une structure de menu à plusieurs niveaux.

Exemples de code suivants montrent comment utiliser des dispositions imbriquées. Le *CustomersComponent.cshtml* fichier est le composant à afficher. Notez que le composant fait référence à la disposition `MasterDataLayout`.

*CustomersComponent.cshtml*:

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

Le *MasterDataLayout.cshtml* fichier fournit le `MasterDataLayout`. La mise en page fait référence à une autre disposition, `MainLayout`, où elle va être incorporé.

*MasterDataLayout.cshtml*:

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

Enfin, `MainLayout` contient les éléments de disposition de niveau supérieur, tels que l’en-tête, pied de page et menu principal.

*MainLayout.cshtml*:

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
