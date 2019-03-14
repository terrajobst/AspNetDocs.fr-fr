---
ms.openlocfilehash: f323482d6f8bfaebf7bf6673d5fb91608430760a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044076"
---
## <a name="add-initial-migration-and-update-the-database"></a>Ajoutez une migration initiale et de mettre à jour de la base de données

* Ouvrez une invite de commandes et accédez au répertoire du projet. (Le répertoire contenant le *Startup.cs* fichier).

* À l’invite de commandes, exécutez les commandes suivantes :

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](/dotnet/core/tools/index) est une implémentation multiplateforme de .NET. Voici ce que faire ces commandes :

  * [restauration de dotnet](/dotnet/core/tools/dotnet-restore): Télécharge les packages NuGet spécifiés dans le *.csproj* fichier.
  * `dotnet ef migrations add Initial` Exécute la commande de migrations Entity Framework .NET Core CLI et crée la migration initiale. Le paramètre après « ajouter » est un nom que vous attribuez à la migration. Ici vous nommez la migration « Initial » car il s’agit de la migration de base de données initiale. Cette opération crée le *Migrations de données / /\<date-heure > _Initial.cs* fichier contenant les commandes de migration pour ajouter le *film* table à la base de données.
  * `dotnet ef database update`  Met à jour la base de données avec la migration que nous venons de créer.

Vous allez en savoir plus sur la chaîne de connexion et de la base de données dans le didacticiel suivant. Vous allez découvrir les modifications du modèle de données dans le [ajouter un champ](xref:tutorials/first-mvc-app/new-field) didacticiel.
