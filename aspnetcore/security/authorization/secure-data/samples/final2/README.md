---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049106"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>L’échantillon de données utilisateur sécurisée d’exécution/de build

* Définir le mot de passe avec l’outil Secret Manager :

  `dotnet user-secrets set SeedUserPW <pw>`

* Mettre à jour la base de données :

    `dotnet ef database update`

* Activer HTTPS dans le projet
