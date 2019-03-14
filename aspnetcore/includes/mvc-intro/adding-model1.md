---
ms.openlocfilehash: 72e33ea44976963193d2560427fc418875be450e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038866"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="2262d-101">Ajouter un modèle dans une application ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="2262d-101">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="2262d-102">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2262d-102">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="2262d-103">Dans cette section, vous ajoutez des classes pour la gestion des films d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="2262d-103">In this section, you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="2262d-104">Ces classes constituent la partie « **M**odèle » de l’application **M**VC.</span><span class="sxs-lookup"><span data-stu-id="2262d-104">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="2262d-105">Vous utilisez ces classes avec [Entity Framework Core](/ef/core) (EF Core) pour travailler avec une base de données.</span><span class="sxs-lookup"><span data-stu-id="2262d-105">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="2262d-106">EF Core est un framework de mappage relationnel d’objets qui simplifie le code d’accès aux données à écrire.</span><span class="sxs-lookup"><span data-stu-id="2262d-106">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span> <span data-ttu-id="2262d-107">[EF Core prend en charge de nombreux moteurs de base de données](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="2262d-107">[EF Core supports many database engines](/ef/core/providers/).</span></span>

<span data-ttu-id="2262d-108">Les classes de modèle que vous créez portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core.</span><span class="sxs-lookup"><span data-stu-id="2262d-108">The model classes you'll create are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="2262d-109">Elles définissent simplement les propriétés des données stockées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="2262d-109">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="2262d-110">Dans ce didacticiel, vous écrivez d’abord les classes du modèle, puis EF Core crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="2262d-110">In this tutorial you'll write the model classes first, and EF Core will create the database.</span></span> <span data-ttu-id="2262d-111">Une autre approche que nous ne décrivons pas ici consiste à générer les classes du modèle à partir d’une base de données déjà existante.</span><span class="sxs-lookup"><span data-stu-id="2262d-111">An alternate approach not covered here is to generate model classes from an already-existing database.</span></span> <span data-ttu-id="2262d-112">Pour plus d’informations sur cette approche, consultez [ASP.NET Core - Base de données existante](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="2262d-112">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="2262d-113">Ajouter une classe de modèle de données</span><span class="sxs-lookup"><span data-stu-id="2262d-113">Add a data model class</span></span>
