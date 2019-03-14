---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188716"
---
> <span data-ttu-id="7a589-101">Certaines commandes ne sont pas pris en charge si l’application utilise SQLite comme magasin de données d’identité.</span><span class="sxs-lookup"><span data-stu-id="7a589-101">Some commands aren't supported if the app uses SQLite as its Identity data store.</span></span> <span data-ttu-id="7a589-102">En raison des limitations dans le moteur de base de données, `Alter` commandes lèvent l’exception suivante :</span><span class="sxs-lookup"><span data-stu-id="7a589-102">Due to limitations in the database engine, `Alter` commands throw the following exception:</span></span>
>
> <span data-ttu-id="7a589-103">"System.NotSupportedException: SQLite ne prend pas en charge cette opération de migration. »</span><span class="sxs-lookup"><span data-stu-id="7a589-103">"System.NotSupportedException: SQLite does not support this migration operation."</span></span> 
>
> <span data-ttu-id="7a589-104">Pour contourner ce problème, exécutez les migrations Code First sur la base de données pour modifier les tables.</span><span class="sxs-lookup"><span data-stu-id="7a589-104">As a work around, run Code First migrations on the database to change the tables.</span></span>
