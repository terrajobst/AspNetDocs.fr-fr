---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029986"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="0b035-101">[Plug-in jQuery Validation](https://jqueryvalidation.org/) : simplifie la validation des formulaires</span><span class="sxs-lookup"><span data-stu-id="0b035-101">[jQuery Validation Plugin](https://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="0b035-102">[![État de la build](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![État devDependency](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="0b035-102">[![Build Status](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency Status](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="0b035-103">Le plug-in jQuery Validation assure une validation immédiate des formulaires existants, et facilite tous types de personnalisation vis-à-vis des applications.</span><span class="sxs-lookup"><span data-stu-id="0b035-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="getting-started"></a><span data-ttu-id="0b035-104">Prise en main</span><span class="sxs-lookup"><span data-stu-id="0b035-104">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="0b035-105">Télécharger les fichiers prédéfinis</span><span class="sxs-lookup"><span data-stu-id="0b035-105">Downloading the prebuilt files</span></span>

<span data-ttu-id="0b035-106">Les fichiers prédéfinis peuvent être téléchargés à partir de https://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="0b035-106">Prebuilt files can be downloaded from https://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="0b035-107">Télécharger les dernières modifications</span><span class="sxs-lookup"><span data-stu-id="0b035-107">Downloading the latest changes</span></span>

<span data-ttu-id="0b035-108">Pour récupérer les fichiers de développement non publiés, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="0b035-108">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="0b035-109">[télécharger](https://github.com/jquery-validation/jquery-validation/archive/master.zip) ou dupliquer ce référentiel ;</span><span class="sxs-lookup"><span data-stu-id="0b035-109">[Downloading](https://github.com/jquery-validation/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. <span data-ttu-id="0b035-110">[configurer la build](CONTRIBUTING.md#build-setup) ;</span><span class="sxs-lookup"><span data-stu-id="0b035-110">[Setup the build](CONTRIBUTING.md#build-setup)</span></span>
 3. <span data-ttu-id="0b035-111">exécuter `grunt` pour créer les fichiers générés dans le répertoire « dist ».</span><span class="sxs-lookup"><span data-stu-id="0b035-111">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="0b035-112">Les ajouter à une page</span><span class="sxs-lookup"><span data-stu-id="0b035-112">Including it on your page</span></span>

<span data-ttu-id="0b035-113">Intégrez jQuery et le plug-in sur une page.</span><span class="sxs-lookup"><span data-stu-id="0b035-113">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="0b035-114">Ensuite, sélectionnez un formulaire à valider et appelez la méthode `validate`.</span><span class="sxs-lookup"><span data-stu-id="0b035-114">Then select a form to validate and call the `validate` method.</span></span>

```html
<form>
    <input required>
</form>
<script src="jquery.js"></script>
<script src="jquery.validate.js"></script>
<script>
$("form").validate();
</script>
```

<span data-ttu-id="0b035-115">Vous pouvez également inclure jQuery et le plug-in grâce à la commande requirejs dans votre module.</span><span class="sxs-lookup"><span data-stu-id="0b035-115">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="0b035-116">Pour plus d’informations sur la configuration des règles et des personnalisations, [consultez la documentation](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="0b035-116">For more information on how to setup a rules and customizations, [check the documentation](https://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="0b035-117">Signaler des problèmes et apporter une contribution au code</span><span class="sxs-lookup"><span data-stu-id="0b035-117">Reporting issues and contributing code</span></span>

<span data-ttu-id="0b035-118">Consultez les [Recommandations de contribution](CONTRIBUTING.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="0b035-118">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="0b035-119">**REMARQUE IMPORTANTE À PROPOS DE LA VALIDATION DES E-MAILS**.</span><span class="sxs-lookup"><span data-stu-id="0b035-119">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="0b035-120">Depuis la version 1.12.0, ce plug-in utilise l’expression régulière [recommandée par la spécification HTML5 pour les navigateurs](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="0b035-120">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="0b035-121">Nous suivrons leur exemple et utiliserons la même vérification.</span><span class="sxs-lookup"><span data-stu-id="0b035-121">We will follow their lead and use the same check.</span></span> <span data-ttu-id="0b035-122">Si vous pensez que la spécification est incorrecte, veuillez leur signaler le problème.</span><span class="sxs-lookup"><span data-stu-id="0b035-122">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="0b035-123">Si vous avez des exigences différentes, vous pouvez [utiliser une méthode personnalisée](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="0b035-123">If you have different requirements, consider [using a custom method](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>
<span data-ttu-id="0b035-124">Au cas où vous deviez ajuster les modèles d’expression régulière de validation intégrés, veuillez [suivre la documentation](https://jqueryvalidation.org/jQuery.validator.methods/).</span><span class="sxs-lookup"><span data-stu-id="0b035-124">In case you need to adjust the built-in validation regular expression patterns, please [follow the documentation](https://jqueryvalidation.org/jQuery.validator.methods/).</span></span>

<span data-ttu-id="0b035-125">**REMARQUE IMPORTANTE SUR LA MÉTHODE REQUIRED**.</span><span class="sxs-lookup"><span data-stu-id="0b035-125">**IMPORTANT NOTE ABOUT REQUIRED METHOD**.</span></span> <span data-ttu-id="0b035-126">Depuis la version 1.14.0, ce plug-in arrête de supprimer les espaces blancs dans la valeur de l’élément attaché.</span><span class="sxs-lookup"><span data-stu-id="0b035-126">As of version 1.14.0 this plugin stops trimming white spaces from the value of the attached element.</span></span> <span data-ttu-id="0b035-127">Si vous souhaitez obtenir le même résultat, vous pouvez utiliser [`normalizer`](https://jqueryvalidation.org/normalizer/) qui peut être utilisé pour transformer la valeur d’un élément avant la validation.</span><span class="sxs-lookup"><span data-stu-id="0b035-127">If you want to achieve the same result, you can use the [`normalizer`](https://jqueryvalidation.org/normalizer/) that can be used to transform the value of an element before validation.</span></span> <span data-ttu-id="0b035-128">Cette fonctionnalité était disponible depuis `v1.15.0`.</span><span class="sxs-lookup"><span data-stu-id="0b035-128">This feature was available since `v1.15.0`.</span></span> <span data-ttu-id="0b035-129">En d’autres termes, vous pouvez procéder comme suit :</span><span class="sxs-lookup"><span data-stu-id="0b035-129">In other words, you can do something like this:</span></span>
``` js
$("#myForm").validate({
    rules: {
        username: {
            required: true,
            // Using the normalizer to trim the value of the element
            // before validating it.
            //
            // The value of `this` inside the `normalizer` is the corresponding
            // DOMElement. In this example, `this` references the `username` element.
            normalizer: function(value) {
                return $.trim(value);
            }
        }
    }
});
```

## <a name="license"></a><span data-ttu-id="0b035-130">Licence</span><span class="sxs-lookup"><span data-stu-id="0b035-130">License</span></span>
<span data-ttu-id="0b035-131">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="0b035-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="0b035-132">Sous licence du MIT.</span><span class="sxs-lookup"><span data-stu-id="0b035-132">Licensed under the MIT license.</span></span>
