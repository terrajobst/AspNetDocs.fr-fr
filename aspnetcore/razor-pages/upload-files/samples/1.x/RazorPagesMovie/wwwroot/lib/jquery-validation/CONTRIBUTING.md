---
ms.openlocfilehash: 3b33bb9bcab1aa7e5438c6fe3f2d7ba0833e7756
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054726"
---
--

## <a name="reporting-an-issue"></a>Signaler un problème

1. Vérifiez que le problème en question est reproductible.
2. Utilisez http://jsbin.com ou http://jsfiddle.net pour fournir une page de test.
3. Précisez sur quels navigateurs il est possible de reproduire le problème. **Remarque : Problèmes liés au mode compatibilité d’Internet Explorer ne seront pas traités. Veillez à effectuer vos tests dans un vrai navigateur !**
4. La version du plug-in dans laquelle le problème peut être reproduit. Se reproduit-il après mise à jour vers la dernière version ?

Les problèmes de documentation sont également suivis dans le traqueur de problèmes [jQuery Validation](https://github.com/jzaefferer/jquery-validation/issues).
Les demandes de tirage (pull requests) visant à améliorer les documents sont en revanche les bienvenues dans le référentiel [Documents jQuery Validation](https://github.com/jzaefferer/validation-content).

**REMARQUE IMPORTANTE À PROPOS DE LA VALIDATION DES E-MAILS**. Depuis la version 1.12.0, ce plug-in utilise l’expression régulière [recommandée par la spécification HTML5 pour les navigateurs](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Nous suivrons leur exemple et utiliserons la même vérification. Si vous pensez que la spécification est incorrecte, veuillez leur signaler le problème. Si vous avez des exigences différentes, vous pouvez [utiliser une méthode personnalisée](http://jqueryvalidation.org/jQuery.validator.addMethod/).

## <a name="contributing-code"></a>Contribution au code

Merci pour votre contribution ! Voici quelques recommandations qui vous aideront à la faire valider.

1. Vérifiez que le problème en question est reproductible. Utilisez jsbin.com ou jsfiddle.net pour fournir une page de test.
2. Suivez le [guide de style de jQuery](http://contribute.jquery.com/style-guides/js).
3. Ajoutez ou mettez à jour les tests unitaires en même temps que votre correctif. Exécutez-les dans au moins un navigateur (voir ci-dessous).
4. Exécutez `grunt` (voir ci-dessous) pour effectuer la vérification lint et autres.
5. Décrivez la modification dans votre message de validation et faites référence au ticket, comme suit : « Démonstrations : Correction du bogue délégué pour une démo des totaux dynamique. Correctifs #51 ». Si vous ajoutez un nouveau fichier de localisation, utilisez quelque chose comme ceci : « Localisation : Localisation de l’ajout croate (HR) »

## <a name="build-setup"></a>Configuration de la build

1. Installez [Node.js](http://nodejs.org).
2. Installez l’interface de ligne de commande Grunt pour effectuer l’installation en exécutant `npm install -g grunt-cli`. Pour plus d’informations, consultez le site web http://gruntjs.com/getting-started.
3. Installez les dépendances npm en exécutant `npm install`.
4. Il est maintenant possible d’appeler la build en exécutant `grunt`.

## <a name="creating-a-new-additional-method"></a>Création d’une nouvelle méthode supplémentaire

Si vous souhaitez ajouter à additional-methods.js des méthodes personnalisées que vous avez écrites :

1. Créer une branche
2. Ajoutez la méthode en tant que nouveau fichier dans `src/additional`.
3. (Facultatif) Ajoutez des traductions à `src/localization`.
4. Envoyez une demande de tirage à la branche maîtresse.

## <a name="unit-tests"></a>Tests unitaires

Pour exécuter des tests unitaires, il suffit d’ouvrir `test/index.html` dans un navigateur. Veillez à exécuter `npm install` au préalable afin que toutes les dépendances requises soient disponibles.
Commencez par un seul navigateur lorsque vous développez le correctif, puis exécutez-le sur d’autres navigateurs avant de le valider. En général, la dernière version de Chrome, Firefox, Safari et Opera et quelques versions d’Internet Explorer.

## <a name="documentation"></a>Documentation

Veuillez signaler les problèmes de documentation dans le traqueur de problèmes [jQuery Validation](https://github.com/jzaefferer/jquery-validation/issues).
Si votre demande de tirage implémente ou modifie une API publique, l’idéal serait que vous fournissiez une demande de tirage sur le référentiel [Documents jQuery Validation](https://github.com/jzaefferer/validation-content).

## <a name="linting"></a>Vérification lint

Pour exécuter JSHint et d’autres outils, utilisez `grunt`.
