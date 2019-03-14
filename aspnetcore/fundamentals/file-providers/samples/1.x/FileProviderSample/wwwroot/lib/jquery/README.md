---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028276"
---
# <a name="jquery"></a>jQuery

> jQuery est une bibliothèque JavaScript rapide, petite et riches en fonctionnalités.

Pour plus d’informations sur la prise en main et l’utilisation de jQuery, consultez la [documentation de jQuery](http://api.jquery.com/).
Vous trouverez les fichiers sources et les problèmes sur le [référentiel jQuery](https://github.com/jquery/jquery).

Si vous mettez à niveau, veuillez consulter le [billet de blog pour 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/). Il inclut les différences notables avec la version précédente et un journal des modifications plus lisible.

## <a name="including-jquery"></a>Inclure jQuery

Voici certaines des façons les plus courantes pour inclure jQuery.

### <a name="browser"></a>Visiteur

#### <a name="script-tag"></a>Balise de script

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a>Babel

[Babel](http://babeljs.io/) est un compilateur JavaScript de génération suivante. L’une des fonctionnalités est la possibilité d’utiliser des modules ES6/ES2015 maintenant, même si les navigateurs ne gèrent pas encore cette fonctionnalité en mode natif.

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a>Browserify/Webpack

Il existe plusieurs façons d’utiliser [Browserify](http://browserify.org/) et [Webpack](https://webpack.github.io/). Pour plus d’informations sur l’utilisation de ces outils, reportez-vous à la documentation du projet correspondant. Dans le script, inclure jQuery ressemble généralement à...

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a>AMD (définition de module asynchrone)

AMD est un format de module conçu pour le navigateur. Pour plus d’informations, nous vous recommandons de lire la [documentation de require.js](http://requirejs.org/docs/whyamd.html).

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a>Nœud

Pour inclure jQuery dans [Node](nodejs.org), installez tout d’abord avec npm.

```sh
npm install jquery
```

Pour que jQuery fonctionne dans Node, une fenêtre avec un document est requise. Dans la mesure où aucune fenêtre de ce type n’existe en mode natif dans Node, il est possible d’en simuler une avec des outils tels que [jsdom](https://github.com/tmpvar/jsdom). Ceci peut être utile dans le cadre de tests.

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
