---
ms.openlocfilehash: 939231583679d469d01724cc1a405bd45e46d2c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57175886"
---
```console
npm run release
```

Cette commande génère les ressources côté client à délivrer pendant l’exécution de l’application. Les ressources sont placées dans le dossier *wwwroot*.

Webpack a effectué les tâches suivantes :

* Purge du contenu du répertoire *wwwroot*.
* Conversion du code TypeScript en JavaScript&mdash;processus appelé *transpilation*.
* Troncation du code JavaScript généré pour réduire la taille de fichier&mdash;processus appelé *minimisation*.
* Copie des fichiers JavaScript, CSS et HTML traités à partir de *src* dans le répertoire *wwwroot*.
* Injection des éléments suivants dans le fichier *wwwroot/index.html* :
    * Une balise `<link>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css*. Cette balise est placée immédiatement avant la balise `</head>` de fermeture.
    * Une balise `<script>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css* minimisé. Cette balise est placée immédiatement avant la balise `</body>` de fermeture.
