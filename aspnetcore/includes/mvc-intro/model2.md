---
ms.openlocfilehash: f2c9cad82eb25d350426465b3632862761740abe
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040496"
---
<a name="dc"></a>

<span data-ttu-id="da2c4-101">Ajoutez la classe `MvcMovieContext` suivante au dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="da2c4-101">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]


<span data-ttu-id="da2c4-102">Le code précédent crée une propriété `DbSet` pour le jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="da2c4-102">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="da2c4-103">Dans la terminologie Entity Framework, un jeu d’entités correspond généralement à une table de base de données, et une entité correspond à une ligne dans la table.</span><span class="sxs-lookup"><span data-stu-id="da2c4-103">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="da2c4-104">Ajouter une chaîne de connexion de base de données</span><span class="sxs-lookup"><span data-stu-id="da2c4-104">Add a database connection string</span></span>

<span data-ttu-id="da2c4-105">Ajoutez une chaîne de connexion au fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="da2c4-105">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="da2c4-106">Ajouter les packages NuGet nécessaires</span><span class="sxs-lookup"><span data-stu-id="da2c4-106">Add required NuGet packages</span></span>

<span data-ttu-id="da2c4-107">Exécutez la commande CLI .NET Core suivante pour ajouter SQLite et CodeGeneration.Design au projet :</span><span class="sxs-lookup"><span data-stu-id="da2c4-107">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="da2c4-108">Le package `Microsoft.VisualStudio.Web.CodeGeneration.Design` est nécessaire à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="da2c4-108">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="da2c4-109">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="da2c4-109">Register the database context</span></span>

<span data-ttu-id="da2c4-110">En tête du fichier *Startup.cs*, ajoutez les instructions `using` suivantes :</span><span class="sxs-lookup"><span data-stu-id="da2c4-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="da2c4-111">Inscrivez le contexte de base de données auprès du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection) dans `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="da2c4-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="da2c4-112">Générez le projet en tant que vérification des erreurs.</span><span class="sxs-lookup"><span data-stu-id="da2c4-112">Build the project as a check for errors.</span></span>