---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048216"
---

> [!NOTE]
> <span data-ttu-id="4132a-101">Plusieurs opérations de modification de schéma ne sont pas prises en charge par le fournisseur EF Core SQLite.</span><span class="sxs-lookup"><span data-stu-id="4132a-101">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="4132a-102">Par exemple, l’ajout d’une colonne est pris en charge, mais pas sa suppression.</span><span class="sxs-lookup"><span data-stu-id="4132a-102">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="4132a-103">Si une migration est créée pour supprimer une colonne, le `ef migrations add` commande réussit mais la `ef database update` Échec d’une commande.</span><span class="sxs-lookup"><span data-stu-id="4132a-103">If a migration is created to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="4132a-104">Certaines de ces restrictions peuvent être surmontés en écrivant du code de migrations pour effectuer une reconstruction de la table manuellement.</span><span class="sxs-lookup"><span data-stu-id="4132a-104">Some of these limitations can be overcome by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="4132a-105">Implique la reconstruction de la table :</span><span class="sxs-lookup"><span data-stu-id="4132a-105">A table rebuild involves:</span></span>

>* <span data-ttu-id="4132a-106">Modification du nom de la table existante.</span><span class="sxs-lookup"><span data-stu-id="4132a-106">Renaming the existing table.</span></span>
>* <span data-ttu-id="4132a-107">Création d’une table.</span><span class="sxs-lookup"><span data-stu-id="4132a-107">Creating a new table.</span></span>
>* <span data-ttu-id="4132a-108">Copie de données à partir de l’ancienne table dans la nouvelle table.</span><span class="sxs-lookup"><span data-stu-id="4132a-108">Copying data from the old table to the new table.</span></span>
>* <span data-ttu-id="4132a-109">Suppression de l’ancienne table.</span><span class="sxs-lookup"><span data-stu-id="4132a-109">Dropping the old table.</span></span>

<span data-ttu-id="4132a-110">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="4132a-110">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="4132a-111">Limites d’un fournisseur de base de données EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="4132a-111">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="4132a-112">Personnaliser le code de migration</span><span class="sxs-lookup"><span data-stu-id="4132a-112">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="4132a-113">Amorçage des données</span><span class="sxs-lookup"><span data-stu-id="4132a-113">Data seeding</span></span>](/ef/core/modeling/data-seeding)