---
ms.openlocfilehash: 96cd8c78178b02ad3eb4a471540950b4581af583
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033376"
---
# <a name="gdpr-sample"></a>Exemple de RGPD

* Dans *appsettings.json*, affectez la valeur `CheckNotConsentNeeded` à `false` pour demander le consentement ; sinon la valeur est true, ou omettre. Testez l’application avec `CheckNotConsentNeeded` définie sur `false` et la valeur `true`.
* Créer des cookies essentielles et les utilisations non vitales avec chaque variation de `CheckConsentNeeded` et consentement accordé.
* Inscrire un utilisateur.
* Supprimer les cookies.
