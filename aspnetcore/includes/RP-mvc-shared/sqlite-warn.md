---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048216"
---

> [!NOTE]
> Plusieurs opérations de modification de schéma ne sont pas prises en charge par le fournisseur EF Core SQLite. Par exemple, l’ajout d’une colonne est pris en charge, mais pas sa suppression. Si une migration est créée pour supprimer une colonne, le `ef migrations add` commande réussit mais la `ef database update` Échec d’une commande. Certaines de ces restrictions peuvent être surmontés en écrivant du code de migrations pour effectuer une reconstruction de la table manuellement. Implique la reconstruction de la table :

>* Modification du nom de la table existante.
>* Création d’une table.
>* Copie de données à partir de l’ancienne table dans la nouvelle table.
>* Suppression de l’ancienne table.

Pour plus d'informations, reportez-vous aux ressources suivantes :
> * [Limites d’un fournisseur de base de données EF Core SQLite](/ef/core/providers/sqlite/limitations)
> * [Personnaliser le code de migration](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Amorçage des données](/ef/core/modeling/data-seeding)