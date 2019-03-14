---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049106"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="fba2f-101">L’échantillon de données utilisateur sécurisée d’exécution/de build</span><span class="sxs-lookup"><span data-stu-id="fba2f-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="fba2f-102">Définir le mot de passe avec l’outil Secret Manager :</span><span class="sxs-lookup"><span data-stu-id="fba2f-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="fba2f-103">Mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="fba2f-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="fba2f-104">Activer HTTPS dans le projet</span><span class="sxs-lookup"><span data-stu-id="fba2f-104">Enable HTTPS in the project</span></span>
