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
# <a name="razor-components-class-libraries"></a>Bibliothèques de classes de composants de Razor

Par [Simon Timms](https://github.com/stimms)

> [!NOTE]
> Le kit SDK .NET Core 3.0 Preview 2 n’inclut pas un modèle de projet pour les bibliothèques de classes de composant Razor, mais nous prévoyons d’ajouter un modèle dans une prochaine version d’évaluation. En attendant, vous pouvez utiliser le modèle de bibliothèque de classes de composant Blazor présenté dans cette rubrique.

Les composants peuvent être partagées dans les bibliothèques de composants entre projets. Les composants peuvent être inclus dans :

* Un autre projet dans la solution.
* Un package NuGet.
* Une bibliothèque .NET référencée.

Tout comme les composants sont des types .NET standards, les bibliothèques de composants sont des assemblys .NET normales.

Pour créer une nouvelle bibliothèque de composant, utilisez le `blazorlib` modèle avec le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande. Le modèle fait partie des modèles installés lorsque [configuration des composants de Razor](xref:razor-components/get-started).

```console
dotnet new blazorlib -o MyComponentLib1
```

Pour ajouter la bibliothèque à un projet existant, utilisez la [dotnet sln](/dotnet/core/tools/dotnet-sln) commande :

```console
dotnet sln add .\MyComponentLib1
```

Les bibliothèques de composants peuvent contenir des fichiers statiques, tels que des images, de JavaScript et de feuilles de style. Au moment de la génération, les fichiers statiques sont incorporés dans le fichier d’assembly généré (*.dll*), ce qui permet la consommation des composants sans avoir à vous soucier de l’inclusion de leurs ressources. Les fichiers inclus dans le `content` répertoire sont marquées comme ressource incorporée. 

## <a name="consume-a-library-component"></a>Utiliser un composant de bibliothèque

Pour consommer des composants définis dans une bibliothèque dans un autre projet, le [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) directive doit être utilisée. Les composants individuels peuvent être ajoutés par nom. Par exemple, la directive suivante ajoute `Component1` de `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Le format général de la directive est :

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

Toutefois, il est courant pour inclure tous les composants à partir d’un assembly à l’aide d’un caractère générique :

```cshtml
@addTagHelper *, MyComponentLib1
```

Le `@addTagHelper` directive peut être incluse dans *_ViewImport.cshtml* pour rendre les composants disponibles pour un projet entier ou appliqués à une seule page ou un ensemble de pages au sein d’un dossier. Avec la `@addTagHelper` directive en place, les composants de la bibliothèque de composant peuvent être consommés comme s’ils étaient dans le même assembly que l’application. 

## <a name="build-pack-and-ship-to-nuget"></a>Build, les packs et les expédier à NuGet

Étant donné que les bibliothèques de composants sont des bibliothèques .NET standard, empaquetage et leur transfert vers NuGet ne diffère pas d’emballage et d’expédition n’importe quelle bibliothèque à NuGet. Empaquetage est effectué à l’aide de la [dotnet pack](/dotnet/core/tools/dotnet-pack) commande :

```console
dotnet pack
```

Télécharger le package à l’aide de NuGet le [dotnet nuget publier](/dotnet/core/tools/dotnet-nuget-push) commande :

```console
dotnet nuget publish
```

Toutes les ressources statiques inclus sont inclus dans le package NuGet. Consommateurs de la bibliothèque reçoivent automatiquement des scripts et feuilles de style, les consommateurs ne sont pas nécessaires pour installer manuellement les ressources.
