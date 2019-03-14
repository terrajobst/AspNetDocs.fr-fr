---
ms.openlocfilehash: 1b563a8c0674d51f893415221ca8fab574b78265
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032946"
---
<a name="--"></a>--
================================

<span data-ttu-id="68a7c-101">[![État de la build](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![État devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="68a7c-101">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="68a7c-102">Le plug-in jQuery Validation assure une validation immédiate des formulaires existants, et facilite tous types de personnalisation vis-à-vis des applications.</span><span class="sxs-lookup"><span data-stu-id="68a7c-102">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="68a7c-103">Aider le projet</span><span class="sxs-lookup"><span data-stu-id="68a7c-103">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="68a7c-104">[![Aider le projet](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="68a7c-104">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="68a7c-105">Ce projet a besoin d’aide !</span><span class="sxs-lookup"><span data-stu-id="68a7c-105">This project is looking for help!</span></span> <span data-ttu-id="68a7c-106">[Vous pouvez faire un don dans le cadre de la campagne Pledgie](http://pledgie.com/campaigns/18159) et en parler autour de vous.</span><span class="sxs-lookup"><span data-stu-id="68a7c-106">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="68a7c-107">Si vous avez utilisé ou envisagez d’utiliser le plug-in, vous pouvez faire un don ; quelle que soit sa valeur, il sera utile.</span><span class="sxs-lookup"><span data-stu-id="68a7c-107">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="68a7c-108">Vous trouverez le plan des dépenses sur la [page de Pledgie](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="68a7c-108">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="get-started"></a><span data-ttu-id="68a7c-109">Bien démarrer</span><span class="sxs-lookup"><span data-stu-id="68a7c-109">Get Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="68a7c-110">Télécharger les fichiers prédéfinis</span><span class="sxs-lookup"><span data-stu-id="68a7c-110">Downloading the prebuilt files</span></span>

<span data-ttu-id="68a7c-111">Les fichiers prédéfinis peuvent être téléchargés à partir de http://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="68a7c-111">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="68a7c-112">Télécharger les dernières modifications</span><span class="sxs-lookup"><span data-stu-id="68a7c-112">Downloading the latest changes</span></span>

<span data-ttu-id="68a7c-113">Pour récupérer les fichiers de développement non publiés, vous pouvez :</span><span class="sxs-lookup"><span data-stu-id="68a7c-113">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="68a7c-114">[télécharger](https://github.com/jzaefferer/jquery-validation/archive/master.zip) ou dupliquer ce référentiel ;</span><span class="sxs-lookup"><span data-stu-id="68a7c-114">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. <span data-ttu-id="68a7c-115">[configurer la build](CONTRIBUTING.md#build-setup) ;</span><span class="sxs-lookup"><span data-stu-id="68a7c-115">[Setup the build](CONTRIBUTING.md#build-setup)</span></span>
 3. <span data-ttu-id="68a7c-116">exécuter `grunt` pour créer les fichiers générés dans le répertoire « dist ».</span><span class="sxs-lookup"><span data-stu-id="68a7c-116">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="68a7c-117">Les ajouter à une page</span><span class="sxs-lookup"><span data-stu-id="68a7c-117">Including it on your page</span></span>

<span data-ttu-id="68a7c-118">Intégrez jQuery et le plug-in sur une page.</span><span class="sxs-lookup"><span data-stu-id="68a7c-118">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="68a7c-119">Ensuite, sélectionnez un formulaire à valider et appelez la méthode `validate`.</span><span class="sxs-lookup"><span data-stu-id="68a7c-119">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="68a7c-120">Vous pouvez également inclure jQuery et le plug-in grâce à la commande requirejs dans votre module.</span><span class="sxs-lookup"><span data-stu-id="68a7c-120">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="68a7c-121">Pour plus d’informations sur la configuration des règles et des personnalisations, [consultez la documentation](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="68a7c-121">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="68a7c-122">Signaler des problèmes et apporter une contribution au code</span><span class="sxs-lookup"><span data-stu-id="68a7c-122">Reporting issues and contributing code</span></span>

<span data-ttu-id="68a7c-123">Consultez les [Recommandations de contribution](CONTRIBUTING.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="68a7c-123">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="68a7c-124">**REMARQUE IMPORTANTE À PROPOS DE LA VALIDATION DES E-MAILS**.</span><span class="sxs-lookup"><span data-stu-id="68a7c-124">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="68a7c-125">Depuis la version 1.12.0, ce plug-in utilise l’expression régulière [recommandée par la spécification HTML5 pour les navigateurs](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="68a7c-125">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="68a7c-126">Nous suivrons leur exemple et utiliserons la même vérification.</span><span class="sxs-lookup"><span data-stu-id="68a7c-126">We will follow their lead and use the same check.</span></span> <span data-ttu-id="68a7c-127">Si vous pensez que la spécification est incorrecte, veuillez leur signaler le problème.</span><span class="sxs-lookup"><span data-stu-id="68a7c-127">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="68a7c-128">Si vous avez des exigences différentes, vous pouvez [utiliser une méthode personnalisée](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="68a7c-128">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="68a7c-129">Licence</span><span class="sxs-lookup"><span data-stu-id="68a7c-129">License</span></span>
<span data-ttu-id="68a7c-130">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="68a7c-130">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="68a7c-131">Sous licence du MIT.</span><span class="sxs-lookup"><span data-stu-id="68a7c-131">Licensed under the MIT license.</span></span>
