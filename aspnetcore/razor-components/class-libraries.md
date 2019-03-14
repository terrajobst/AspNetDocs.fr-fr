---
title: Bibliothèques de classes de composants de Razor
author: guardrex
description: Découvrez comment les composants peuvent être inclus dans les composants de Razor des applications à partir d’une bibliothèque de composants externes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 0e644627178bae2b8880760335860b3e0ebef156
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037406"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="022fb-103">Bibliothèques de classes de composants de Razor</span><span class="sxs-lookup"><span data-stu-id="022fb-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="022fb-104">Par [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="022fb-104">By [Simon Timms](https://github.com/stimms)</span></span>

> [!NOTE]
> <span data-ttu-id="022fb-105">Le kit SDK .NET Core 3.0 Preview 2 n’inclut pas un modèle de projet pour les bibliothèques de classes de composant Razor, mais nous prévoyons d’ajouter un modèle dans une prochaine version d’évaluation.</span><span class="sxs-lookup"><span data-stu-id="022fb-105">The .NET Core 3.0 Preview 2 SDK doesn't include a project template for Razor Component Class Libraries, but we expect to add a template in a future preview.</span></span> <span data-ttu-id="022fb-106">En attendant, vous pouvez utiliser le modèle de bibliothèque de classes de composant Blazor présenté dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="022fb-106">In meantime, you can use the Blazor Component Class Library template explained in this topic.</span></span>

<span data-ttu-id="022fb-107">Les composants peuvent être partagées dans les bibliothèques de composants entre projets.</span><span class="sxs-lookup"><span data-stu-id="022fb-107">Components can be shared in component libraries across projects.</span></span> <span data-ttu-id="022fb-108">Les composants peuvent être inclus dans :</span><span class="sxs-lookup"><span data-stu-id="022fb-108">Components can be included from:</span></span>

* <span data-ttu-id="022fb-109">Un autre projet dans la solution.</span><span class="sxs-lookup"><span data-stu-id="022fb-109">Another project in the solution.</span></span>
* <span data-ttu-id="022fb-110">Un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="022fb-110">A NuGet package.</span></span>
* <span data-ttu-id="022fb-111">Une bibliothèque .NET référencée.</span><span class="sxs-lookup"><span data-stu-id="022fb-111">A referenced .NET library.</span></span>

<span data-ttu-id="022fb-112">Tout comme les composants sont des types .NET standards, les bibliothèques de composants sont des assemblys .NET normales.</span><span class="sxs-lookup"><span data-stu-id="022fb-112">Just as components are regular .NET types, component libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="022fb-113">Pour créer une nouvelle bibliothèque de composant, utilisez le `blazorlib` modèle avec le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande.</span><span class="sxs-lookup"><span data-stu-id="022fb-113">To create a new component library, use the `blazorlib` template with the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="022fb-114">Le modèle fait partie des modèles installés lorsque [configuration des composants de Razor](xref:razor-components/get-started).</span><span class="sxs-lookup"><span data-stu-id="022fb-114">The template is part of the templates installed when [setting up Razor Components](xref:razor-components/get-started).</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="022fb-115">Pour ajouter la bibliothèque à un projet existant, utilisez la [dotnet sln](/dotnet/core/tools/dotnet-sln) commande :</span><span class="sxs-lookup"><span data-stu-id="022fb-115">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

```console
dotnet sln add .\MyComponentLib1
```

<span data-ttu-id="022fb-116">Les bibliothèques de composants peuvent contenir des fichiers statiques, tels que des images, de JavaScript et de feuilles de style.</span><span class="sxs-lookup"><span data-stu-id="022fb-116">Component libraries may contain static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="022fb-117">Au moment de la génération, les fichiers statiques sont incorporés dans le fichier d’assembly généré (*.dll*), ce qui permet la consommation des composants sans avoir à vous soucier de l’inclusion de leurs ressources.</span><span class="sxs-lookup"><span data-stu-id="022fb-117">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="022fb-118">Les fichiers inclus dans le `content` répertoire sont marquées comme ressource incorporée.</span><span class="sxs-lookup"><span data-stu-id="022fb-118">Any files included in the `content` directory are marked as an embedded resource.</span></span> 

## <a name="consume-a-library-component"></a><span data-ttu-id="022fb-119">Utiliser un composant de bibliothèque</span><span class="sxs-lookup"><span data-stu-id="022fb-119">Consume a library component</span></span>

<span data-ttu-id="022fb-120">Pour consommer des composants définis dans une bibliothèque dans un autre projet, le [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) directive doit être utilisée.</span><span class="sxs-lookup"><span data-stu-id="022fb-120">In order to consume components defined in a library in another project, the [@addTagHelper](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="022fb-121">Les composants individuels peuvent être ajoutés par nom.</span><span class="sxs-lookup"><span data-stu-id="022fb-121">Individual components may be added by name.</span></span> <span data-ttu-id="022fb-122">Par exemple, la directive suivante ajoute `Component1` de `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="022fb-122">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="022fb-123">Le format général de la directive est :</span><span class="sxs-lookup"><span data-stu-id="022fb-123">The general format of the directive is:</span></span>

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

<span data-ttu-id="022fb-124">Toutefois, il est courant pour inclure tous les composants à partir d’un assembly à l’aide d’un caractère générique :</span><span class="sxs-lookup"><span data-stu-id="022fb-124">However, it's common to include all of the components from an assembly using a wildcard:</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="022fb-125">Le `@addTagHelper` directive peut être incluse dans *_ViewImport.cshtml* pour rendre les composants disponibles pour un projet entier ou appliqués à une seule page ou un ensemble de pages au sein d’un dossier.</span><span class="sxs-lookup"><span data-stu-id="022fb-125">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="022fb-126">Avec la `@addTagHelper` directive en place, les composants de la bibliothèque de composant peuvent être consommés comme s’ils étaient dans le même assembly que l’application.</span><span class="sxs-lookup"><span data-stu-id="022fb-126">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span> 

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="022fb-127">Build, les packs et les expédier à NuGet</span><span class="sxs-lookup"><span data-stu-id="022fb-127">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="022fb-128">Étant donné que les bibliothèques de composants sont des bibliothèques .NET standard, empaquetage et leur transfert vers NuGet ne diffère pas d’emballage et d’expédition n’importe quelle bibliothèque à NuGet.</span><span class="sxs-lookup"><span data-stu-id="022fb-128">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="022fb-129">Empaquetage est effectué à l’aide de la [dotnet pack](/dotnet/core/tools/dotnet-pack) commande :</span><span class="sxs-lookup"><span data-stu-id="022fb-129">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="022fb-130">Télécharger le package à l’aide de NuGet le [dotnet nuget publier](/dotnet/core/tools/dotnet-nuget-push) commande :</span><span class="sxs-lookup"><span data-stu-id="022fb-130">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="022fb-131">Toutes les ressources statiques inclus sont inclus dans le package NuGet.</span><span class="sxs-lookup"><span data-stu-id="022fb-131">Any included static resources are included in the NuGet package.</span></span> <span data-ttu-id="022fb-132">Consommateurs de la bibliothèque reçoivent automatiquement des scripts et feuilles de style, les consommateurs ne sont pas nécessaires pour installer manuellement les ressources.</span><span class="sxs-lookup"><span data-stu-id="022fb-132">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
