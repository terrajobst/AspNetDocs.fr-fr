---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028276"
---
# <a name="jquery"></a><span data-ttu-id="23176-101">jQuery</span><span class="sxs-lookup"><span data-stu-id="23176-101">jQuery</span></span>

> <span data-ttu-id="23176-102">jQuery est une bibliothèque JavaScript rapide, petite et riches en fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="23176-102">jQuery is a fast, small, and feature-rich JavaScript library.</span></span>

<span data-ttu-id="23176-103">Pour plus d’informations sur la prise en main et l’utilisation de jQuery, consultez la [documentation de jQuery](http://api.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="23176-103">For information on how to get started and how to use jQuery, please see [jQuery's documentation](http://api.jquery.com/).</span></span>
<span data-ttu-id="23176-104">Vous trouverez les fichiers sources et les problèmes sur le [référentiel jQuery](https://github.com/jquery/jquery).</span><span class="sxs-lookup"><span data-stu-id="23176-104">For source files and issues, please visit the [jQuery repo](https://github.com/jquery/jquery).</span></span>

<span data-ttu-id="23176-105">Si vous mettez à niveau, veuillez consulter le [billet de blog pour 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span><span class="sxs-lookup"><span data-stu-id="23176-105">If upgrading, please see the [blog post for 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span></span> <span data-ttu-id="23176-106">Il inclut les différences notables avec la version précédente et un journal des modifications plus lisible.</span><span class="sxs-lookup"><span data-stu-id="23176-106">This includes notable differences from the previous version and a more readable changelog.</span></span>

## <a name="including-jquery"></a><span data-ttu-id="23176-107">Inclure jQuery</span><span class="sxs-lookup"><span data-stu-id="23176-107">Including jQuery</span></span>

<span data-ttu-id="23176-108">Voici certaines des façons les plus courantes pour inclure jQuery.</span><span class="sxs-lookup"><span data-stu-id="23176-108">Below are some of the most common ways to include jQuery.</span></span>

### <a name="browser"></a><span data-ttu-id="23176-109">Visiteur</span><span class="sxs-lookup"><span data-stu-id="23176-109">Browser</span></span>

#### <a name="script-tag"></a><span data-ttu-id="23176-110">Balise de script</span><span class="sxs-lookup"><span data-stu-id="23176-110">Script tag</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a><span data-ttu-id="23176-111">Babel</span><span class="sxs-lookup"><span data-stu-id="23176-111">Babel</span></span>

<span data-ttu-id="23176-112">[Babel](http://babeljs.io/) est un compilateur JavaScript de génération suivante.</span><span class="sxs-lookup"><span data-stu-id="23176-112">[Babel](http://babeljs.io/) is a next generation JavaScript compiler.</span></span> <span data-ttu-id="23176-113">L’une des fonctionnalités est la possibilité d’utiliser des modules ES6/ES2015 maintenant, même si les navigateurs ne gèrent pas encore cette fonctionnalité en mode natif.</span><span class="sxs-lookup"><span data-stu-id="23176-113">One of the features is the ability to use ES6/ES2015 modules now, even though browsers do not yet support this feature natively.</span></span>

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a><span data-ttu-id="23176-114">Browserify/Webpack</span><span class="sxs-lookup"><span data-stu-id="23176-114">Browserify/Webpack</span></span>

<span data-ttu-id="23176-115">Il existe plusieurs façons d’utiliser [Browserify](http://browserify.org/) et [Webpack](https://webpack.github.io/).</span><span class="sxs-lookup"><span data-stu-id="23176-115">There are several ways to use [Browserify](http://browserify.org/) and [Webpack](https://webpack.github.io/).</span></span> <span data-ttu-id="23176-116">Pour plus d’informations sur l’utilisation de ces outils, reportez-vous à la documentation du projet correspondant.</span><span class="sxs-lookup"><span data-stu-id="23176-116">For more information on using these tools, please refer to the corresponding project's documention.</span></span> <span data-ttu-id="23176-117">Dans le script, inclure jQuery ressemble généralement à...</span><span class="sxs-lookup"><span data-stu-id="23176-117">In the script, including jQuery will usually look like this...</span></span>

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a><span data-ttu-id="23176-118">AMD (définition de module asynchrone)</span><span class="sxs-lookup"><span data-stu-id="23176-118">AMD (Asynchronous Module Definition)</span></span>

<span data-ttu-id="23176-119">AMD est un format de module conçu pour le navigateur.</span><span class="sxs-lookup"><span data-stu-id="23176-119">AMD is a module format built for the browser.</span></span> <span data-ttu-id="23176-120">Pour plus d’informations, nous vous recommandons de lire la [documentation de require.js](http://requirejs.org/docs/whyamd.html).</span><span class="sxs-lookup"><span data-stu-id="23176-120">For more information, we recommend [require.js' documentation](http://requirejs.org/docs/whyamd.html).</span></span>

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a><span data-ttu-id="23176-121">Nœud</span><span class="sxs-lookup"><span data-stu-id="23176-121">Node</span></span>

<span data-ttu-id="23176-122">Pour inclure jQuery dans [Node](nodejs.org), installez tout d’abord avec npm.</span><span class="sxs-lookup"><span data-stu-id="23176-122">To include jQuery in [Node](nodejs.org), first install with npm.</span></span>

```sh
npm install jquery
```

<span data-ttu-id="23176-123">Pour que jQuery fonctionne dans Node, une fenêtre avec un document est requise.</span><span class="sxs-lookup"><span data-stu-id="23176-123">For jQuery to work in Node, a window with a document is required.</span></span> <span data-ttu-id="23176-124">Dans la mesure où aucune fenêtre de ce type n’existe en mode natif dans Node, il est possible d’en simuler une avec des outils tels que [jsdom](https://github.com/tmpvar/jsdom).</span><span class="sxs-lookup"><span data-stu-id="23176-124">Since no such window exists natively in Node, one can be mocked by tools such as [jsdom](https://github.com/tmpvar/jsdom).</span></span> <span data-ttu-id="23176-125">Ceci peut être utile dans le cadre de tests.</span><span class="sxs-lookup"><span data-stu-id="23176-125">This can be useful for testing purposes.</span></span>

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
