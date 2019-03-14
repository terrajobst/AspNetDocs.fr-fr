---
ms.openlocfilehash: 4d6b6309e6e15c364542b5d3ae296d53c6ea4358
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037396"
---
<span data-ttu-id="2a073-101">Le code de base de données d’identité généré nécessite [Migrations Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="2a073-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="2a073-102">Créer une migration et mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="2a073-102">Create a migration and update the database.</span></span> <span data-ttu-id="2a073-103">Par exemple, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="2a073-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2a073-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a073-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2a073-105">Dans Visual Studio **Console du Gestionnaire de Package**:</span><span class="sxs-lookup"><span data-stu-id="2a073-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2a073-106">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a073-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="2a073-107">Le paramètre de nom de « CreateIdentitySchema » pour le `Add-Migration` commande est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="2a073-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="2a073-108">`"CreateIdentitySchema"` Décrit la migration.</span><span class="sxs-lookup"><span data-stu-id="2a073-108">`"CreateIdentitySchema"` describes the migration.</span></span>
