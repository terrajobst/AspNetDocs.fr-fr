---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188716"
---
> Certaines commandes ne sont pas pris en charge si l’application utilise SQLite comme magasin de données d’identité. En raison des limitations dans le moteur de base de données, `Alter` commandes lèvent l’exception suivante :
>
> "System.NotSupportedException: SQLite ne prend pas en charge cette opération de migration. » 
>
> Pour contourner ce problème, exécutez les migrations Code First sur la base de données pour modifier les tables.
