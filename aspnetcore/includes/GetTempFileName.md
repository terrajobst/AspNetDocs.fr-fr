---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165870"
---
**Avertissement**: Le code suivant utilise `GetTempFileName`, qui lève une `IOException` si les fichiers de plus de 65 535 sont créés sans supprimer les fichiers temporaires antérieurs. Une application réelle doit supprimer les fichiers temporaires, ou utiliser `GetTempPath` et `GetRandomFileName` pour créer des noms de fichiers temporaires. Comme la limite de 65 535 fichiers est par serveur, une autre application sur le serveur peut utiliser la totalité des 65535 fichiers. 
