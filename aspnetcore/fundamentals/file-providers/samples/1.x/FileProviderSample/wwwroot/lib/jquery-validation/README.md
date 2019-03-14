---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029986"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a>[Plug-in jQuery Validation](https://jqueryvalidation.org/) : simplifie la validation des formulaires
================================

[![État de la build](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![État devDependency](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)

Le plug-in jQuery Validation assure une validation immédiate des formulaires existants, et facilite tous types de personnalisation vis-à-vis des applications.

## <a name="getting-started"></a>Prise en main

### <a name="downloading-the-prebuilt-files"></a>Télécharger les fichiers prédéfinis

Les fichiers prédéfinis peuvent être téléchargés à partir de https://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Télécharger les dernières modifications

Pour récupérer les fichiers de développement non publiés, vous pouvez :

 1. [télécharger](https://github.com/jquery-validation/jquery-validation/archive/master.zip) ou dupliquer ce référentiel ;
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

Pour plus d’informations sur la configuration des règles et des personnalisations, [consultez la documentation](https://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Signaler des problèmes et apporter une contribution au code

Consultez les [Recommandations de contribution](CONTRIBUTING.md) pour plus d’informations.

**REMARQUE IMPORTANTE À PROPOS DE LA VALIDATION DES E-MAILS**. Depuis la version 1.12.0, ce plug-in utilise l’expression régulière [recommandée par la spécification HTML5 pour les navigateurs](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Nous suivrons leur exemple et utiliserons la même vérification. Si vous pensez que la spécification est incorrecte, veuillez leur signaler le problème. Si vous avez des exigences différentes, vous pouvez [utiliser une méthode personnalisée](https://jqueryvalidation.org/jQuery.validator.addMethod/).
Au cas où vous deviez ajuster les modèles d’expression régulière de validation intégrés, veuillez [suivre la documentation](https://jqueryvalidation.org/jQuery.validator.methods/).

**REMARQUE IMPORTANTE SUR LA MÉTHODE REQUIRED**. Depuis la version 1.14.0, ce plug-in arrête de supprimer les espaces blancs dans la valeur de l’élément attaché. Si vous souhaitez obtenir le même résultat, vous pouvez utiliser [`normalizer`](https://jqueryvalidation.org/normalizer/) qui peut être utilisé pour transformer la valeur d’un élément avant la validation. Cette fonctionnalité était disponible depuis `v1.15.0`. En d’autres termes, vous pouvez procéder comme suit :
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

## <a name="license"></a>Licence
Copyright &copy; Jörn Zaefferer<br>
Sous licence du MIT.
