---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165870"
---
<span data-ttu-id="65ddf-101">**Avertissement**: Le code suivant utilise `GetTempFileName`, qui lève une `IOException` si les fichiers de plus de 65 535 sont créés sans supprimer les fichiers temporaires antérieurs.</span><span class="sxs-lookup"><span data-stu-id="65ddf-101">**Warning**: The following code uses `GetTempFileName`, which throws an `IOException` if more than 65535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="65ddf-102">Une application réelle doit supprimer les fichiers temporaires, ou utiliser `GetTempPath` et `GetRandomFileName` pour créer des noms de fichiers temporaires.</span><span class="sxs-lookup"><span data-stu-id="65ddf-102">A real app should either delete temporary files or use `GetTempPath` and `GetRandomFileName` to create temporary file names.</span></span> <span data-ttu-id="65ddf-103">Comme la limite de 65 535 fichiers est par serveur, une autre application sur le serveur peut utiliser la totalité des 65535 fichiers.</span><span class="sxs-lookup"><span data-stu-id="65ddf-103">The 65535 files limit is per server, so another app on the server can use up all 65535 files.</span></span> 
