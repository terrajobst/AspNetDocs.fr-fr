---
ms.openlocfilehash: 568fa161b27e554fd8b474670b8f9a7ec2f39f45
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047126"
---
Le tableau suivant détaille les paramètres du générateur de code ASP.NET Core :

| Paramètre               | Description|
| ----------------- | ------------ |
| -m  | Nom du modèle. |
| -dc  | Contexte de données. |
| -udl | Utiliser la disposition par défaut. |
| --relativeFolderPath | Chemin du dossier de sortie relatif pour créer les vues. |
| --useDefaultLayout | La disposition par défaut doit être utilisée pour les vues. |
| --referenceScriptLibraries | Ajoute `_ValidationScriptsPartial` aux pages Modifier et Créer. |

Utilisez le commutateur `h` pour obtenir de l’aide sur la commande `aspnet-codegenerator controller` :

```console
dotnet aspnet-codegenerator controller -h
```