---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054216"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[Open Iconic v1.1.1](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a>Open Iconic est l’équivalent open source [d’Iconic](http://useiconic.com). C’est une collection très lisible de 223 icônes de faible encombrement&mdash;prête à l’utilisation avec Bootstrap et Foundation. [Afficher la collection](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a>Que contient Open Iconic ?

* 223 icônes conçues pour être lisibles jusqu’à 8 pixels
* Fichiers SVG très légers - 61,8 pour le jeu entier 
* Sprite SVG&mdash;le remplacement moderne des polices d’icône
* Formats Webfont (EOT, OTF, SVG, TTF, WOFF), PNG et WebP
* Feuilles de style Webfont (notamment les versions pour Bootstrap et Foundation) dans les formats CSS, LESS, SCSS et Stylus
* Images raster PNG et WebP 8px, 16px, 24px, 32px, 48px et 64px.


## <a name="getting-started"></a>Prise en main

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a>Pour des exemples de code et tout ce dont vous avez besoin pour bien démarrer avec Open Iconic, consultez nos sections [Icons](http://useiconic.com/open#icons) et [Reference](http://useiconic.com/open#reference).

### <a name="general-usage"></a>Utilisation générale

#### <a name="using-open-iconics-svgs"></a>Utilisation des SVG d’Open Iconic

Nous aimons les SVG et pensons qu’il s’agit de la meilleure façon d’afficher des icônes sur le web. Étant donné que les SVG d’Open Iconic sont basiques, nous vous suggérons de les afficher comme n’importe quelle autre image (n’oubliez pas l’attribut `alt`).

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a>Utilisation de Sprite SVG d’Open Iconic

Open Iconic est également fourni dans un Sprite SVG qui vous permet d’afficher toutes les icônes dans le jeu à l’aide d’une seule requête. C’est comme une police d’icône, sans les complications.

L’ajout d’une icône à partir d’un Sprite SVG est un peu différent de ce à quoi vous êtes habitué, mais c’est quand même un jeu d’enfant. *Conseil : Pour que vos icônes soient facilement stylisables, nous vous suggérons d’ajouter une classe générale à la balise* `<svg>` *et un nom de classe unique pour chaque icône différents dans la balise* `<use>` *.*  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

Le dimensionnement des icônes n’a besoin que d’une CSS de base. Toutes les icônes sont sous forme de carré, donc définissez simplement la balise `<svg>` avec des dimensions de la largeur et de hauteur égales.

```
.icon {
  width: 16px;
  height: 16px;
}
```

La coloration des icônes est encore plus facile. Il vous suffit de définir la règle `fill` sur la balise `<use>`.

```
.icon-account-login {
  fill: #f00;
}
```

Pour en savoir plus sur les Sprite SVG, lisez le [guide de Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).

#### <a name="using-open-iconics-icon-font"></a>Utilisation de la police d’icône d’Open Iconic...


##### <a name="with-bootstrap"></a>…avec Bootstrap

Vous trouverez nos feuilles de style Bootstrap dans `font/css/open-iconic-bootstrap.{css, less, scss, styl}`


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a>…avec Foundation

Vous trouverez nos feuilles de style Foundation dans `font/css/open-iconic-foundation.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a>…seule

Vous trouverez nos feuilles de style par défaut dans `font/css/open-iconic.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a>Licence

### <a name="icons"></a>Icônes

Tout le code (notamment le balisage SVG) est sous [licence MIT](http://opensource.org/licenses/MIT).

### <a name="fonts"></a>Polices

Toutes les polices sont sous [licence SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).
