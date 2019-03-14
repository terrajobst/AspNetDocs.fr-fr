---
ms.openlocfilehash: 96cd8c78178b02ad3eb4a471540950b4581af583
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033376"
---
# <a name="gdpr-sample"></a><span data-ttu-id="ee000-101">Exemple de RGPD</span><span class="sxs-lookup"><span data-stu-id="ee000-101">GDPR Sample</span></span>

* <span data-ttu-id="ee000-102">Dans *appsettings.json*, affectez la valeur `CheckNotConsentNeeded` à `false` pour demander le consentement ; sinon la valeur est true, ou omettre.</span><span class="sxs-lookup"><span data-stu-id="ee000-102">In *appsettings.json*, set `CheckNotConsentNeeded` to `false` to require consent; otherwise set to true or omit.</span></span> <span data-ttu-id="ee000-103">Testez l’application avec `CheckNotConsentNeeded` définie sur `false` et la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="ee000-103">Test the app with `CheckNotConsentNeeded` set to `false` and set to `true`.</span></span>
* <span data-ttu-id="ee000-104">Créer des cookies essentielles et les utilisations non vitales avec chaque variation de `CheckConsentNeeded` et consentement accordé.</span><span class="sxs-lookup"><span data-stu-id="ee000-104">Create essential and non-essential cookies with each variation of `CheckConsentNeeded` and consent granted.</span></span>
* <span data-ttu-id="ee000-105">Inscrire un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ee000-105">Register a user.</span></span>
* <span data-ttu-id="ee000-106">Supprimer les cookies.</span><span class="sxs-lookup"><span data-stu-id="ee000-106">Delete cookies.</span></span>
