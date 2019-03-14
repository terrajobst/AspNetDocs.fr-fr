---
ms.openlocfilehash: 85fa9f8b18dc7dfb3e9d37703549dc5c28dc7a4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062596"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Effectuer la génération de modèles automatique pour le modèle Movie

* Exécutez la commande suivante dans une ligne de commande (dans le répertoire du projet qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*) :

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Si vous obtenez l’erreur :
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

L’erreur précédente se produit quand vous êtes dans le mauvais répertoire. Ouvrez une interface de commande dans le répertoire du projet (répertoire contenant les fichiers *Program.cs*, *Startup.cs* et *.csproj*), puis exécutez la commande précédente.

Si vous obtenez l’erreur :
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Quittez Visual Studio et réexécutez la commande.
