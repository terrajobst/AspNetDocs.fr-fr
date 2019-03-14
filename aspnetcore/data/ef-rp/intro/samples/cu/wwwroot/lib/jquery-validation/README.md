---
ms.openlocfilehash: 83a958fe6a8d61becf9f9062e8ad0ff69108b435
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064196"
---
<a name="jquery-validation-pluginhttpjqueryvalidationorg---form-validation-made-easy"></a>[Plug-in jQuery Validation](http://jqueryvalidation.org/) : simplifie la validation des formulaires
================================

[![État de la build](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![État devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)

Le plug-in jQuery Validation assure une validation immédiate des formulaires existants, et facilite tous types de personnalisation vis-à-vis des applications.

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[Aider le projet](http://pledgie.com/campaigns/18159)

[![Aider le projet](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)

Ce projet a besoin d’aide ! [Vous pouvez faire un don dans le cadre de la campagne Pledgie](http://pledgie.com/campaigns/18159) et en parler autour de vous. Si vous avez utilisé ou envisagez d’utiliser le plug-in, vous pouvez faire un don ; quelle que soit sa valeur, il sera utile.

Vous trouverez le plan des dépenses sur la [page de Pledgie](http://pledgie.com/campaigns/18159).

## <a name="getting-started"></a>Prise en main

### <a name="downloading-the-prebuilt-files"></a>Télécharger les fichiers prédéfinis

Les fichiers prédéfinis peuvent être téléchargés à partir de http://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Télécharger les dernières modifications

Pour récupérer les fichiers de développement non publiés, vous pouvez :

 1. [télécharger](https://github.com/jzaefferer/jquery-validation/archive/master.zip) ou dupliquer ce référentiel ;
 2. [configurer la build](CONTRIBUTING.md#build-setup) ;
 3. exécuter `grunt` pour créer les fichiers générés dans le répertoire « dist ».

### <a name="including-it-on-your-page"></a>Les ajouter à une page

Intégrez jQuery et le plug-in sur une page. Ensuite, sélectionnez un formulaire à valider et appelez la méthode `validate`.

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

Vous pouvez également inclure jQuery et le plug-in grâce à la commande requirejs dans votre module.

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

Pour plus d’informations sur la configuration des règles et des personnalisations, [consultez la documentation](http://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Signaler des problèmes et apporter une contribution au code

Consultez les [Recommandations de contribution](CONTRIBUTING.md) pour plus d’informations.

**REMARQUE IMPORTANTE À PROPOS DE LA VALIDATION DES E-MAILS**. Depuis la version 1.12.0, ce plug-in utilise l’expression régulière [recommandée par la spécification HTML5 pour les navigateurs](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Nous suivrons leur exemple et utiliserons la même vérification. Si vous pensez que la spécification est incorrecte, veuillez leur signaler le problème. Si vous avez des exigences différentes, vous pouvez [utiliser une méthode personnalisée](http://jqueryvalidation.org/jQuery.validator.addMethod/).

## <a name="license"></a>Licence
Copyright &copy; Jörn Zaefferer<br>
Sous licence du MIT.
