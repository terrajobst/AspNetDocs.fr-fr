---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064846"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[<span data-ttu-id="68a9f-101">Open Iconic v1.1.1</span><span class="sxs-lookup"><span data-stu-id="68a9f-101">Open Iconic v1.1.1</span></span>](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a><span data-ttu-id="68a9f-102">Open Iconic est l’équivalent open source [d’Iconic](http://useiconic.com).</span><span class="sxs-lookup"><span data-stu-id="68a9f-102">Open Iconic is the open source sibling of [Iconic](http://useiconic.com).</span></span> <span data-ttu-id="68a9f-103">C’est une collection très lisible de 223 icônes de faible encombrement&mdash;prête à l’utilisation avec Bootstrap et Foundation.</span><span class="sxs-lookup"><span data-stu-id="68a9f-103">It is a hyper-legible collection of 223 icons with a tiny footprint&mdash;ready to use with Bootstrap and Foundation.</span></span> [<span data-ttu-id="68a9f-104">Afficher la collection</span><span class="sxs-lookup"><span data-stu-id="68a9f-104">View the collection</span></span>](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a><span data-ttu-id="68a9f-105">Que contient Open Iconic ?</span><span class="sxs-lookup"><span data-stu-id="68a9f-105">What's in Open Iconic?</span></span>

* <span data-ttu-id="68a9f-106">223 icônes conçues pour être lisibles jusqu’à 8 pixels</span><span class="sxs-lookup"><span data-stu-id="68a9f-106">223 icons designed to be legible down to 8 pixels</span></span>
* <span data-ttu-id="68a9f-107">Fichiers SVG très légers - 61,8 pour le jeu entier</span><span class="sxs-lookup"><span data-stu-id="68a9f-107">Super-light SVG files - 61.8 for the entire set</span></span> 
* <span data-ttu-id="68a9f-108">Sprite SVG&mdash;le remplacement moderne des polices d’icône</span><span class="sxs-lookup"><span data-stu-id="68a9f-108">SVG sprite&mdash;the modern replacement for icon fonts</span></span>
* <span data-ttu-id="68a9f-109">Formats Webfont (EOT, OTF, SVG, TTF, WOFF), PNG et WebP</span><span class="sxs-lookup"><span data-stu-id="68a9f-109">Webfont (EOT, OTF, SVG, TTF, WOFF), PNG and WebP formats</span></span>
* <span data-ttu-id="68a9f-110">Feuilles de style Webfont (notamment les versions pour Bootstrap et Foundation) dans les formats CSS, LESS, SCSS et Stylus</span><span class="sxs-lookup"><span data-stu-id="68a9f-110">Webfont stylesheets (including versions for Bootstrap and Foundation) in CSS, LESS, SCSS and Stylus formats</span></span>
* <span data-ttu-id="68a9f-111">Images raster PNG et WebP 8px, 16px, 24px, 32px, 48px et 64px.</span><span class="sxs-lookup"><span data-stu-id="68a9f-111">PNG and WebP raster images in 8px, 16px, 24px, 32px, 48px and 64px.</span></span>


## <a name="getting-started"></a><span data-ttu-id="68a9f-112">Prise en main</span><span class="sxs-lookup"><span data-stu-id="68a9f-112">Getting Started</span></span>

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a><span data-ttu-id="68a9f-113">Pour des exemples de code et tout ce dont vous avez besoin pour bien démarrer avec Open Iconic, consultez nos sections [Icons](http://useiconic.com/open#icons) et [Reference](http://useiconic.com/open#reference).</span><span class="sxs-lookup"><span data-stu-id="68a9f-113">For code samples and everything else you need to get started with Open Iconic, check out our [Icons](http://useiconic.com/open#icons) and [Reference](http://useiconic.com/open#reference) sections.</span></span>

### <a name="general-usage"></a><span data-ttu-id="68a9f-114">Utilisation générale</span><span class="sxs-lookup"><span data-stu-id="68a9f-114">General Usage</span></span>

#### <a name="using-open-iconics-svgs"></a><span data-ttu-id="68a9f-115">Utilisation des SVG d’Open Iconic</span><span class="sxs-lookup"><span data-stu-id="68a9f-115">Using Open Iconic's SVGs</span></span>

<span data-ttu-id="68a9f-116">Nous aimons les SVG et pensons qu’il s’agit de la meilleure façon d’afficher des icônes sur le web.</span><span class="sxs-lookup"><span data-stu-id="68a9f-116">We like SVGs and we think they're the way to display icons on the web.</span></span> <span data-ttu-id="68a9f-117">Étant donné que les SVG d’Open Iconic sont basiques, nous vous suggérons de les afficher comme n’importe quelle autre image (n’oubliez pas l’attribut `alt`).</span><span class="sxs-lookup"><span data-stu-id="68a9f-117">Since Open Iconic are just basic SVGs, we suggest you display them like you would any other image (don't forget the `alt` attribute).</span></span>

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a><span data-ttu-id="68a9f-118">Utilisation de Sprite SVG d’Open Iconic</span><span class="sxs-lookup"><span data-stu-id="68a9f-118">Using Open Iconic's SVG Sprite</span></span>

<span data-ttu-id="68a9f-119">Open Iconic est également fourni dans un Sprite SVG qui vous permet d’afficher toutes les icônes dans le jeu à l’aide d’une seule requête.</span><span class="sxs-lookup"><span data-stu-id="68a9f-119">Open Iconic also comes in a SVG sprite which allows you to display all the icons in the set with a single request.</span></span> <span data-ttu-id="68a9f-120">C’est comme une police d’icône, sans les complications.</span><span class="sxs-lookup"><span data-stu-id="68a9f-120">It's like an icon font, without being a hack.</span></span>

<span data-ttu-id="68a9f-121">L’ajout d’une icône à partir d’un Sprite SVG est un peu différent de ce à quoi vous êtes habitué, mais c’est quand même un jeu d’enfant.</span><span class="sxs-lookup"><span data-stu-id="68a9f-121">Adding an icon from an SVG sprite is a little different than what you're used to, but it's still a piece of cake.</span></span> <span data-ttu-id="68a9f-122">*Conseil : Pour que vos icônes soient facilement stylisables, nous vous suggérons d’ajouter une classe générale à la balise* `<svg>` *et un nom de classe unique pour chaque icône différents dans la balise* `<use>` *.*</span><span class="sxs-lookup"><span data-stu-id="68a9f-122">*Tip: To make your icons easily style able, we suggest adding a general class to the* `<svg>` *tag and a unique class name for each different icon in the* `<use>` *tag.*</span></span>  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

<span data-ttu-id="68a9f-123">Le dimensionnement des icônes n’a besoin que d’une CSS de base.</span><span class="sxs-lookup"><span data-stu-id="68a9f-123">Sizing icons only needs basic CSS.</span></span> <span data-ttu-id="68a9f-124">Toutes les icônes sont sous forme de carré, donc définissez simplement la balise `<svg>` avec des dimensions de la largeur et de hauteur égales.</span><span class="sxs-lookup"><span data-stu-id="68a9f-124">All the icons are in a square format, so just set the `<svg>` tag with equal width and height dimensions.</span></span>

```
.icon {
  width: 16px;
  height: 16px;
}
```

<span data-ttu-id="68a9f-125">La coloration des icônes est encore plus facile.</span><span class="sxs-lookup"><span data-stu-id="68a9f-125">Coloring icons is even easier.</span></span> <span data-ttu-id="68a9f-126">Il vous suffit de définir la règle `fill` sur la balise `<use>`.</span><span class="sxs-lookup"><span data-stu-id="68a9f-126">All you need to do is set the `fill` rule on the `<use>` tag.</span></span>

```
.icon-account-login {
  fill: #f00;
}
```

<span data-ttu-id="68a9f-127">Pour en savoir plus sur les Sprite SVG, lisez le [guide de Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span><span class="sxs-lookup"><span data-stu-id="68a9f-127">To learn more about SVG Sprites, read [Chris Coyier's guide](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span></span>

#### <a name="using-open-iconics-icon-font"></a><span data-ttu-id="68a9f-128">Utilisation de la police d’icône d’Open Iconic...</span><span class="sxs-lookup"><span data-stu-id="68a9f-128">Using Open Iconic's Icon Font...</span></span>


##### <a name="with-bootstrap"></a><span data-ttu-id="68a9f-129">…avec Bootstrap</span><span class="sxs-lookup"><span data-stu-id="68a9f-129">…with Bootstrap</span></span>

<span data-ttu-id="68a9f-130">Vous trouverez nos feuilles de style Bootstrap dans `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="68a9f-130">You can find our Bootstrap stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span></span>


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a><span data-ttu-id="68a9f-131">…avec Foundation</span><span class="sxs-lookup"><span data-stu-id="68a9f-131">…with Foundation</span></span>

<span data-ttu-id="68a9f-132">Vous trouverez nos feuilles de style Foundation dans `font/css/open-iconic-foundation.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="68a9f-132">You can find our Foundation stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a><span data-ttu-id="68a9f-133">…seule</span><span class="sxs-lookup"><span data-stu-id="68a9f-133">…on its own</span></span>

<span data-ttu-id="68a9f-134">Vous trouverez nos feuilles de style par défaut dans `font/css/open-iconic.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="68a9f-134">You can find our default stylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a><span data-ttu-id="68a9f-135">Licence</span><span class="sxs-lookup"><span data-stu-id="68a9f-135">License</span></span>

### <a name="icons"></a><span data-ttu-id="68a9f-136">Icônes</span><span class="sxs-lookup"><span data-stu-id="68a9f-136">Icons</span></span>

<span data-ttu-id="68a9f-137">Tout le code (notamment le balisage SVG) est sous [licence MIT](http://opensource.org/licenses/MIT).</span><span class="sxs-lookup"><span data-stu-id="68a9f-137">All code (including SVG markup) is under the [MIT License](http://opensource.org/licenses/MIT).</span></span>

### <a name="fonts"></a><span data-ttu-id="68a9f-138">Polices</span><span class="sxs-lookup"><span data-stu-id="68a9f-138">Fonts</span></span>

<span data-ttu-id="68a9f-139">Toutes les polices sont sous [licence SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span><span class="sxs-lookup"><span data-stu-id="68a9f-139">All fonts are under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span></span>
