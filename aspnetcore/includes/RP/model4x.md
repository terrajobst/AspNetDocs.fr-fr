---
ms.openlocfilehash: d875717d480fe234ac5a328493fcec6a268e37a8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039516"
---
<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="93fbf-101">Effectuer la génération de modèles automatique pour le modèle Movie</span><span class="sxs-lookup"><span data-stu-id="93fbf-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="93fbf-102">Exécutez la commande suivante dans une ligne de commande (dans le répertoire du projet qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*) :</span><span class="sxs-lookup"><span data-stu-id="93fbf-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="93fbf-103">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="93fbf-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="93fbf-104">Ouvrez une interface de commande dans le répertoire du projet (répertoire qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="93fbf-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
