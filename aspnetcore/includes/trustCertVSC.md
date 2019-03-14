---
ms.openlocfilehash: 476580d4bc2435f73ef37c5344ab7fea4b67b9b8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032496"
---
Approuvez le certificat de développement HTTPS en exécutant la commande suivante :

```console
dotnet dev-certs https --trust
```

La commande précédente affiche la boîte de dialogue suivante :

![Boîte de dialogue Avertissement de sécurité](~/getting-started/_static/cert.png)

Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.

Pour plus d’informations, consultez [Approuver le certificat de développement HTTPS ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).