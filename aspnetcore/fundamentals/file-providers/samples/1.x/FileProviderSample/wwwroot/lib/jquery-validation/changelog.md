---
ms.openlocfilehash: b97b3f95a1ecd1e5802794043f848bda83a2f966
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039426"
---
<a name="1160--2016-12-01"></a>1.16.0 / 2016-12-01
==================

## <a name="additional"></a>Objets
  * Affiner les algorithmes cifES et nieES. Ferme #1826

## <a name="build"></a>Build
  * Inclure la version minimisée dans le package NPM
  * Faire passer les dépendances de développement aux versions plus récentes

## <a name="core"></a>Principal
  * Ajouter une liaison pour l’entrée avec le type de bouton. Ferme #1891
  * Prise en charge de jquery3. Ferme #1866
  * Modifier l’alias jQuery 'expr[":"]' en 'expr.pseudos'

## <a name="localisation"></a>Localisation
  * Ajouter une traduction en ourdou. Ferme #1873.

## <a name="localization"></a>Localisation
  * Extension de fichier erronée corrigée pour la traduction az. Ferme #1890.
  * Ajout de traduction manquante en pt-BR (ferme #1897)
  * Faute de frappe corrigée dans le fichier de langue arabe.

## <a name="tests"></a>Tests
  * Mettre à niveau QUnit vers la version 2.0.

## <a name="umd"></a>UMD
  * Meilleure prise en charge de CommonJS.

<a name="1151--2016-07-22"></a>1.15.1 / 2016-07-22
==================

## <a name="additional"></a>Objets
  * Correction de la validation de types mime multiples
  * IBAN nécessite au moins 5 caractères (ferme #1797, résout #1674)
  * Correction de la référence jQuery notEqualTo

## <a name="core"></a>Principal
  * Échec du test ajouté pour #1805
  * Correction de la validation de groupe avec 3 champs et plus
  * Correction des régressions introduites dans #1644 et #1657 (ferme #1800)
  * Mettre à jour la validation d’étape pour gérer correctement les virgules flottantes
  * Corriger l’erreur lors de l’appel de $.fn.rules() sur un élément autre qu’un formulaire
  * Appeler `errorPlacement` dans la portée du validateur
  * Correction d’un problème avec des éléments contenteditable dans les formulaires où les événements pour la validation d’entrée unique entraîneraient des exceptions

## <a name="demo"></a>Démonstration
  * Ajouter des liens vers les démonstrations Bootstrap et Semantic-UI dans le fichier index.html
  * Utiliser `.on()` à la place de `.validateDelegate()`

## <a name="localization"></a>Localisation
  * Langue Azéri ajoutée

## <a name="tests"></a>Tests
  * Tests unitaires de régression ajoutés pour PR #1760

<a name="1150--2016-02-24"></a>1.15.0 / 2016-02-24
==================

## <a name="all"></a>Tous
  * Problèmes de style de code corrigés

## <a name="core"></a>Principal
  * `resetForm` doit également supprimer la classe `valid` des éléments.
  * Suppression de la mise en surbrillance du champ s’il est déjà mis en surbrillance lors de l’utilisation d’une règle à distance.
  * Lier l’événement `blur` une seule fois dans la règle `equalTo`
  * Correction de l’erreur lors de l’appel de .rules() sur jquery vide défini.
  * Corriger la gestion des messages d’erreur avec des groupes d’entrée.
  * Corriger TypeError dans `showLabel` lors de l’utilisation des paramètres `groups`
  * Ajout d’une manière de passer le nom de la méthode à distance
  * Le déclenchement de la validation échoue lorsque le champ suivant est déjà rempli (corrige #1508)
  * La règle requise est prioritaire sur les règles de nombre et de chiffres
  * Erreur masquée mais classe d’erreur en entrée pas supprimée
  * La validation à distance utilise des messages d’erreur incorrects
  * Correction de la mise en surbrillance de champ avec la validation à distance.
  * Sélecteur `:filled` corrigé pour sélection de plusieurs éléments.
  * Référence de document ajoutée à jQuery.validator.methods
  * Déplacement du traitement du message de `formatAndAdd` vers `defaultMessage`
  * ErrorList doit contenir uniquement les erreurs requises
  * Extraire le nom de fichier sans inclure « C:\fakepath\"
  * Prise en charge de l’attribut d’étape HTML5. Corrige #1295
  * Prise en charge ajoutée pour classe « en attente » dans les demandes en attente
  * Normaliseur ajouté (#1602)
  * Méthode `creditcard` fractionnée
  * errorID d’échappement à utiliser dans expression régulière, pas pour générer aria-describedby
  * Guillemets simples d’échappement dans les noms pour éviter une erreur Sizzle
  * Au lieu d’utiliser la valeur du champ de validation pour ignorer l’appel d’API, utiliser l’objet de données sérialisées de la requête
  * Ajouter la prise en charge des balises contentEditable

## <a name="additional"></a>Objets
  * BIC : autoriser les chiffres 1 à 9 en deuxième place de l’emplacement
  * L’expression régulière de la méthode d’acceptation doit être correctement échappée.
  * Vérification ne respectant pas la casse pour BIC
  * PostalCodeCA correct pour exclure les combinaisons non valides
  * Rendre la méthode postalCodeCA plus modérée

## <a name="localization"></a>Localisation
  * Ajout de la localisation en Macédonien.
  * Message de modèle manquant ajouté en polonais (adamwojtkiewicz)
  * Traduction en persan corrigée du message min/max.
  * messages_sk.js mis à jour
  * Mettre à jour la traduction en malais
  * Messages inclus à partir de méthodes supplémentaires
  * Amélioration de la traduction en pt_BR et correction d’une faute de frappe sur la clé « cifES ».

<a name="1140--2015-06-30"></a>1.14.0 / 2015-06-30
==================

## <a name="core"></a>Principal
  * Suppression de la méthode removeAttrs inutilisée
  * Remplacement de l'expression régulière pour la méthode de l’url
  * Suppression du paramètre URL incorrect dans $.ajax, remplacé par $.extend
  * Gestion correcte du bouton d’envoi d'annulation imbriqué
  * Correction de la mise en retrait
  * Refactorisation de attributeRules et dataRules pour partager le normaliseur
  * Méthode dataRules pour convertir la valeur en nombre pour les entrées numériques
  * Mise à jour de la méthode URL pour autoriser les URL relatives à un protocole
  * Suppression de l’espace réservé $.format déconseillé
  * Utilisation de jQuery 1.7+ on/off, ajout de la méthode destroy
  * Remplacement de la compatibilité IE8 .indexOf par $.inArray
  * Définition des attributs de la valeur NaN sur indéfinis pour Opera Mini
  * Arrêt de la suppression de la valeur dans la méthode requise
  * Utilisation : sélecteur désactivé pour faire correspondre les éléments désactivés
  * Exclusion de certaines touches du clavier pour empêcher la revalidation du champ
  * Ne pas rechercher la totalité du DOM pour les éléments des cases d’option/cases à cocher
  * Envoi de meilleures erreurs pour les méthodes de règle incorrectes
  * Correction de l'erreur de validation de numéro
  * Correction de la référence à la spécification whatwg
  * Mise en focus d'un élément non valide lors de la validation d’un ensemble personnalisé d’entrées
  * Réinitialisation des styles d’éléments lors de l’utilisation des méthodes de mise en surbrillance personnalisée
  * Création une séquence d'échappement pour le signe dollar dans l'ID d'erreur
  * Rétablissement de "Ignorer les champs en lecture seule ainsi que les champs désactivés".
  * Mise à jour du lien dans le commentaire pour l’algorithme de Luhn

## <a name="additionals"></a>Compléments
  * Mise à jour de dateITA pour corriger le problème de fuseau horaire
  * Correction de la méthode d’extension pour la période de la méthode uniquement
  * Correction de la méthode d'acceptation pour faire correspondre la période uniquement
  * Mise à jour de la méthode de temps pour autoriser une heure à un seul chiffre
  * Arrêt du test incorrect pour la méthode notEqualTo
  * Ajout de la méthode notEqualTo
  * Utilisation d'une référence correcte jQuery via `$`
  * Suppression de la vérification d'expression régulière inutile dans la méthode iban
  * Numéro CPF brésilien

## <a name="localization"></a>Localisation
  * Mise à jour de messages_tr.js
  * Mise à jour de messages_sr_lat.js
  * Ajout de l'espagnol pour le Pérou (ES PE)
  * Ajout du géorgien (ქართული, ge)
  * Faute de frappe corrigée dans la traduction catalane
  * Amélioration de la traduction finnoise (fi)
  * Ajout des paramètres régionaux pour l'arménien (hy_AM)
  * Extension de la traduction italienne (it) avec la méthode de devise
  * Ajout des paramètres régionaux bn_BD
  * Mise à jour des paramètres régionaux zh
  * Suppression du point final à la fin des messages en italien

<a name="1131--2014-10-14"></a>1.13.1 / 2014-10-14
==================

## <a name="core"></a>Principal
  * Autorisation de 0 comme valeur pour autoCreateRanges
  * Application du paramètre ignore à tous les éléments validationTargetFor
  * Ne pas supprimer de valeur dans les méthodes min/max/rangelength
  * Création d'une séquence d'échappement pour l'ID/le nom avant de l’utiliser comme sélecteur dans errorsFor
  * Valeur explicite par défaut pour l’option focusCleanup
  * Correction de l'expression régulière incorrecte pour le détecteur describedby
  * Ignorer les champs en lecture seule ainsi que les champs désactivés
  * Amélioration de la séquence d’échappement de l'ID, stockage de l'ID en séquence d’échappement dans describedby
  * Utilisation de la valeur de retour submitHandler pour autoriser ou empêcher l’envoi de formulaire

## <a name="additionals"></a>Compléments
  * Ajout de la méthode postalcodeBR
  * Correction de la méthode de modèle lorsque le paramètre est une chaîne


<a name="1130--2014-07-01"></a>1.13.0 / 2014-07-01
==================

## <a name="all"></a>Tous
* Ajout du wrapper UMD du plug-in

## <a name="core"></a>Principal
* Respect de non-erreurs non-error aria-describedby et des erreurs masquées vides
* Amélioration de l'expression régulière dateISO
* Ajout d'une case d’option/case à cocher pour déléguer l’événement de clic
* Utilisation de aria-describedby pour les éléments sans étiquette
* Inscription de focusin, focusout et keyup sur la case d’option/case à cocher
* Correction de la normalisation de la valeur d’attribut rangelength
* Mise à jour de la méthode elementValue pour traiter les champs type="number"
* Utilisation de charAt au lieu de la notation de tableau pour les chaînes, afin de prendre en charge IE8 (?)

## <a name="localization"></a>Localisation
* Correction de la traduction sk de la méthode rangelength
* Ajout des méthodes finnoises
* Correction du message de validation de numéro GL
* Message de validation de méthode de numéro ES
* Ajout du galicien (GL)
* Correction des messages français pour les méthodes min et max

## <a name="additionals"></a>Compléments
* Ajout de la méthode statesUS
* Correction de la méthode dateITA pour traiter un bogue DST
* Ajout de la méthode de date perse
* Ajout de la méthode postalCodeCA
* Ajout de la méthode postalcodeIT

<a name="1120--2014-04-01"></a>1.12.0 / 2014-04-01
==================

* Ajout d'un test ARIA ([3d5658e](https://github.com/jzaefferer/jquery-validation/commit/3d5658e9e4825fab27198c256beed86f0bd12577))
* Ajout des messages de localisation es-AR. ([7b30beb](https://github.com/jzaefferer/jquery-validation/commit/7b30beb8ebd218c38a55d26a63d529e16035c7a2))
* Ajout des points manquants aux messages 'es' et 'es_AR'. ([a2a653c](https://github.com/jzaefferer/jquery-validation/commit/a2a653cb68926ca034b4b09d742d275db934d040))
* Ajout de la localisation indonésienne (ID) ([1d348bd](https://github.com/jzaefferer/jquery-validation/commit/1d348bdcb65807c71da8d0bfc13a97663631cd3a))
* Ajout de la validation des numéros de documents espagnols NIF, NIE et CIF ([#830](https://github.com/jzaefferer/jquery-validation/issues/830), [317c20f](https://github.com/jzaefferer/jquery-validation/commit/317c20fa9bb772770bb9b70d46c7081d7cfc6545))
* Ajout du formulaire actuel dans le contexte de la requête ajax à distance ([0a18ae6](https://github.com/jzaefferer/jquery-validation/commit/0a18ae65b9b6d877e3d15650a5c2617a9d2b11d5))
* Compléments : Mettre à jour de la méthode IBAN, suppression d’espaces blancs de fin ([#970](https://github.com/jzaefferer/jquery-validation/issues/970), [347b04a](https://github.com/jzaefferer/jquery-validation/commit/347b04a7d4e798227405246a5de3fc57451d52e1))
* {Méthode BIC : Améliorer l’expression régulière, {1} est toujours redondant. Ferme gh-744 ([5cad6b4](https://github.com/jzaefferer/jquery-validation/commit/5cad6b493575e8a9a82470d17e0900c881130873))
* Bower : Ajout de Bower.json pour l’inscription du package ([e86ccb0](https://github.com/jzaefferer/jquery-validation/commit/e86ccb06e301613172d472cf15dd4011ff71b398))
* Remplace les références en dollar par 'jQuery' pour la compatibilité avec jQuery.noConflict. Ferme gh-754 ([2049afe](https://github.com/jzaefferer/jquery-validation/commit/2049afe46c1be7b3b89b1d9f0690f5bebf4fbf68))
* Principal : Ajouter un champ de la « méthode » à l’entrée de liste d’erreurs ([89a15c7](https://github.com/jzaefferer/jquery-validation/commit/89a15c7a4b17fa2caaf4ff817f09b04c094c3884))
* Principal : Prise en charge pour les messages génériques via l’attribut data-msg ([5bebaa5](https://github.com/jzaefferer/jquery-validation/commit/5bebaa5c55c73f457c0e0181ec4e3b0c409e2a9d))
* Principal : Autorise les attributs ayant une valeur égale à zéro (par exemple min = '0') ([#854](https://github.com/jzaefferer/jquery-validation/issues/854), [9dc0d1d](https://github.com/jzaefferer/jquery-validation/commit/9dc0d1dd946b2c6178991fb16df0223c76162579))
* Principal : Élément $.format déconseillé de désactiver ([#755](https://github.com/jzaefferer/jquery-validation/issues/755), [bf3b350](https://github.com/jzaefferer/jquery-validation/commit/bf3b3509140ea8ab5d83d3ec58fd9f1d7822efc5))
* Principal : Corriger la prise en charge de plusieurs classes d’erreur ([c1f0baf](https://github.com/jzaefferer/jquery-validation/commit/c1f0baf36c21ca175bbc05fb9345e5b44b094821))
* Principal : Ignorer les événements sur les éléments ignorés ([#700](https://github.com/jzaefferer/jquery-validation/issues/700), [a864211](https://github.com/jzaefferer/jquery-validation/commit/a86421131ea69786ee9e0d23a68a54a7658ccdbf))
* Principal : Améliorer la méthode elementValue ([6c041ed](https://github.com/jzaefferer/jquery-validation/commit/6c041edd21af1425d12d06cdd1e6e32a78263e82))
* Principal : Que les éléments de handle ignoré element() correctement. ([3f464a8](https://github.com/jzaefferer/jquery-validation/commit/3f464a8da49dbb0e4881ada04165668e4a63cecb))
* Principal : Basculer l’analyse dataRules sur style spécification W3C HTML5 ([460fd22](https://github.com/jzaefferer/jquery-validation/commit/460fd22b6c84a74c825ce1fa860c0a9da20b56bb))
* Principal : Déclencher la réussite d’une opération facultative, mais la réussite des autres validateurs ([#851](https://github.com/jzaefferer/jquery-validation/issues/851), [f93e1de](https://github.com/jzaefferer/jquery-validation/commit/f93e1deb48ec8b3a8a54e946a37db2de42d3aa2a))
* Principal : Un élément brut au lieu d’utiliser non renvoi à nouveau l’élément ([03cd4c9](https://github.com/jzaefferer/jquery-validation/commit/03cd4c93069674db5415a0bf174a5870da47e5d2))
* Principal : vérification que l'opération à distance est exécutée en dernier ([#711](https://github.com/jzaefferer/jquery-validation/issues/711), [ad91b6f](https://github.com/jzaefferer/jquery-validation/commit/ad91b6f388b7fdfb03b74e78554cbab9fd8fca6f))
* Démonstration : Utilisez option appropriée dans la démo en plusieurs parties. ([#1025](https://github.com/jzaefferer/jquery-validation/issues/1025), [070edc7](https://github.com/jzaefferer/jquery-validation/commit/070edc7be4de564cb74cfa9ee4e3f40b6b70b76f))
* Correction de l’utilisation de $/ jQuery dans les autres méthodes. Corrige #839 ([#839](https://github.com/jzaefferer/jquery-validation/issues/839), [59bc899](https://github.com/jzaefferer/jquery-validation/commit/59bc899e4586255a4251903712e813c21d25b3e1))
* Amélioration des traductions chinoises ([1a0bfe3](https://github.com/jzaefferer/jquery-validation/commit/1a0bfe32b16f8912ddb57388882aa880fab04ffe))
* Implémentation initiale ARIA-Required ([bf3cfb2](https://github.com/jzaefferer/jquery-validation/commit/bf3cfb234ede2891d3f7e19df02894797dd7ba5e))
* Localisation : modification des valeurs acceptées en extension. Corrige #771, ferme gh-793. ([#771](https://github.com/jzaefferer/jquery-validation/issues/771), [12edec6](https://github.com/jzaefferer/jquery-validation/commit/12edec66eb30dc7e86756222d455d49b34016f65))
* Messages : Ajouter la localisation islandaise ([dc88575](https://github.com/jzaefferer/jquery-validation/commit/dc885753c8872044b0eaa1713cecd94c19d4c73d))
* Messages : Ajouter des points manquants aux messages « bg », « fr » et « sr ». ([adbc636](https://github.com/jzaefferer/jquery-validation/commit/adbc6361c377bf6b74c35df9782479b1115fbad7))
* Messages : Création de messages_sr_lat.js ([f2f9007](https://github.com/jzaefferer/jquery-validation/commit/f2f90076518014d98495c2a9afb9a35d45d184e6))
* Messages : Création de messages_tj.js ([de830b3](https://github.com/jzaefferer/jquery-validation/commit/de830b3fd8689a7384656c17565ee92c2878d8a5))
* Messages : Corriger la traduction sr_lat, ajouter un espace manquant ([880ba1c](https://github.com/jzaefferer/jquery-validation/commit/880ba1ca545903a41d8c5332fc4038a7e9a580bd))
* Messages : Mettre à jour de messages_sr.js, correction des espace manquant ([10313f4](https://github.com/jzaefferer/jquery-validation/commit/10313f418c18ea75f385248468c2d3600f136cfb))
* Méthodes : Ajouter une méthode supplémentaire pour la devise ([1a981b4](https://github.com/jzaefferer/jquery-validation/commit/1a981b440346620964c87ebdd0fa03246348390e))
* Méthodes : Ajout des guillemets à la suppression du signe de ponctuation de stripHTML ([aa0d624](https://github.com/jzaefferer/jquery-validation/commit/aa0d6241c3ea04663edc1e45ed6e6134630bdd2f))
* Méthodes : Corriger la méthode dateita pour éviter les erreurs ([279 b 932](https://github.com/jzaefferer/jquery-validation/commit/279b932c1267b7238e6652880b7846ba3bbd2084))
* Méthodes : Méthodes localisées pour la culture chilienne (es-CL) ([cf36b93](https://github.com/jzaefferer/jquery-validation/commit/cf36b933499e435196d951401221d533a4811810))
* Méthodes : Mettre à jour d’un e-mail à utiliser des expressions régulières HTML5, supprimer la méthode email2 ([#828](https://github.com/jzaefferer/jquery-validation/issues/828), [dd162ae](https://github.com/jzaefferer/jquery-validation/commit/dd162ae360639f73edd2dcf7a256710b2f5a4e64))
* Méthode de modèle : Supprimer des séparateurs, étant donné que les implémentations HTML5 ne les incluent pas. ([37992c1](https://github.com/jzaefferer/jquery-validation/commit/37992c1c9e2e0be8b315ccccc2acb74863439d3e))
* Restriction du validateur de carte de crédit pour inclure le contrôle de longueur. Ferme gh-772 ([f5f47c5](https://github.com/jzaefferer/jquery-validation/commit/f5f47c5c661da5b0c0c6d59d169e82230928a804))
* Mise à jour de messages_ko.js - ferme gh-715 ([5da3085](https://github.com/jzaefferer/jquery-validation/commit/5da3085ff02e0e6ecc955a8bfc3bb9a8d220581b))
* Mise à jour de messages_pt_BR.js. Ferme gh-782 ([4bf813b](https://github.com/jzaefferer/jquery-validation/commit/4bf813b751ce34fac3c04eaa2e80f75da3461124))
* Mise à jour de phonesUK et mobileUK pour accepter de nouveaux préfixes. Ferme gh-750 ([d447b41](https://github.com/jzaefferer/jquery-validation/commit/d447b41b830dee984be21d8281ec7b87a852001d))
* Vérification des codes postaux à neuf chiffres. Ferme gh-726 ([165005d](https://github.com/jzaefferer/jquery-validation/commit/165005d4b5780e22d13d13189d107940c622a76f))
* phoneUS : Ajouter des exclusions de N11. Ferme gh-861 ([519bbc6](https://github.com/jzaefferer/jquery-validation/commit/519bbc656bcb26e8aae5166d7b2e000014e0d12a))
* resetForm devrait effacer toutes les valeurs aria non valides ([4f8a631](https://github.com/jzaefferer/jquery-validation/commit/4f8a631cbe84f496ec66260ada52db2aa0bb3733))
* valid(): Vérifiez tous les éléments. Corrige #791 - valid() valide uniquement le premier élément (non valide) ([#791](https://github.com/jzaefferer/jquery-validation/issues/791), [6f26803](https://github.com/jzaefferer/jquery-validation/commit/6f268031afaf4e155424ee74dd11f6c47fbb8553))

<a name="1111--2013-03-22"></a>1.11.1 / 2013-03-22
==================

  * Rétablissement pour convertir également les paramètres de la méthode de plage en nombres. Ferme gh-702
  * Remplacement de l'utilisation principale de PHP par des gestionnaires mockjax. Nettoyage de la démo, mise à jour avec le plug-in d’entrée masqué le plus récent. Conservation de la démo captcha en PHP. Corrige #662
  * Suppression de la mise en surbrillance du code inligne à partir de la démo lait. Affichage de la source fonctionne correctement.
  * Correction de la démo des totaux dynamique en supprimant l'espace blanc du modèle de contenu avant de le transmettre au constructeur jQuery
  * Correction de la validation min/max. Ferme gh-666. Corrige #648
  * Corrige les messages qui s'affichent comme règle et provoquent une exception après avoir mis à jour via rules("add"). Ferme gh-670, corrige #624
  * Ajout de la localisation coréenne (ko). Ferme gh-671
  * Amélioration de la méthode de code postal du Royaume-Uni pour filtrer d'autres codes postaux non valides. Ferme #682
  * Mise à jour de messages_sv.js. Ferme #683
  * Changement du lien Grunt vers le site Web du projet. Ferme #684
  * Déplacement de la méthode de suppression vers le bas de la liste pour l'exécuter en dernier, après toutes les autres méthodes appliquées à un champ. Corrige #679
  * Mise à jour de la description de plugin.json, qui doit inclure le mot 'valider'
  * Correction des fautes de frappe
  * Correction du chargeur jQuery pour utiliser son propre chemin d’accès. Corrige les démos imbriquées.
  * Mise à jour de grunt-cotisation-qunit pour utiliser PhantomJS 1.8 lors de l’installation via le module de nœud 'phantomjs'
  * Conversion de valid() en valeur booléenne au lieu de 0 ou 1. Corrige #109 - valid() ne retourne pas une valeur booléenne

<a name="1110--2013-02-04"></a>1.11.0 / 2013-02-04
==================

  * Suppression de l’effacement sous forme des nombres des règles `min`, `max` et `range`. Corrige #455. Ferme gh-528.
  * Mise à jour des étiquettes préexistantes - corrige #430 ferme gh-436
  * Correction de $.validator.format afin d’éviter l’interpolation de groupe, où au moins IE8/9 remplace -bash par la correspondance. Corrige #614
  * Correction de l'expression régulière mimetype
  * Ajout d'un manifeste de plug-in et mise à jour des en-têtes pour la licence MIT uniquement, suppression des doubles licences inutiles (par exemple, jQuery).
  * Messages en hébreu : Suppression des points en fin de phrases - corrige gh-568
  * Traduction française pour la validation require_from_group. Corrige gh-573.
  * Autorisation des groupes sous forme de tableau ou de chaîne - corrige #479
  * Suppression des espaces avec plusieurs types MIME
  * Correction de certaines validations de date, erreurs de syntaxe JS.
  * Suppression de la prise en charge du plug-in de métadonnées, remplacement par les propriétés data-rule- et data-msg- (ajoutées dans 907467e8).
  * Sftp ajouté en tant que modèle URL valide
  * Ajout de la localisation malaise (my)
  * Mise à jour de localization/messages_hu.js
  * Suppression du polyfill focusin/focusout. Corrige #542 - L'inclusion de jquery.validate interfère avec les événements focusin et focusout dans IE9
  * Localisation : Faute de frappe corrigée dans la traduction finnoise
  * Correction de la démo RTM pour afficher une icône non valide lors du passage de valide à non valide
  * Correction du retour prématuré de la fonction distante qui empêche l’appel ajax si une entrée a été saisie trop rapidement. Garantit que la validation à distance valide toujours la valeur plus récente.
  * Annulation de la correction #244. Corrige #521 - la validation de l'e-mail se déclenche immédiatement lorsque le champ contient du texte.

<a name="1100--2012-09-07"></a>1.10.0 / 2012-09-07
===================

  * Correction des chaînes françaises contenant des espaces blancs, phoneUS, phoneUK et mobileUK en fonction des commentaires de la communauté.
  * Renommage des fichiers avec language_REGION conformément à la norme ISO_3166-1 (http://en.wikipedia.org/wiki/ISO_3166-1)), car la langue de Taïwan est le chinois (zh) et la région est Taïwan (TW)
  * Optimisation des modèles d’expression régulière, en particulier pour les numéros de téléphone du Royaume-Uni.
  * Ajout du nom de la langue pour chaque fichier, changement du code de langue en fonction de la norme ISO 639 pour l’estonien, le géorgien, l’ukrainien et le chinois (http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
  * Localisation croate (HR) ajoutée
  * Les traductions françaises existantes ont été modifiées et des traductions françaises pour d'autres méthodes supplémentaires ont été ajoutées.
  * Les modifications ont été fusionnées pour spécifier des messages d’erreur personnalisés dans les attributs de données
  * Mise à jour de l'expression régulière des numéros de téléphone mobiles du Royaume-Uni pour les nouveaux numéros. Corrige #154
  * Ajout de l’élément à l’appel réussi avec un test. Corrige #60
  * Correction de l'expression régulière pour une méthode supplémentaire de temps. Corrige #131
  * resetForm efface désormais l'ancienne valeur previousValue sur les éléments de formulaire. Corrige #312
  * Ajout d'une case à cocher de test aux valeurs require_from_group et require_from_group, et modification de la valeur elementValue. Corrige #359
  * Correction des problèmes de réponse de dataFilter dans jQuery 1.5.2+. Corrige #405
  * Ajout de la démo jQuery Mobile. Corrige #249
  * Annulation de l'optimisation de findByName pour la correction. Corrige #82 - $.validator.prototype.findByName s’arrête dans IE7
  * Ajout de la prise en charge du code postal des États-Unis et test. Corrige #90
  * Remplacement de lastElement par lastActive dans keyup, ignorer la validation sur l’onglet ou l'élément vide. Corrige #244
  * Suppression du numéro de stripHtml. Corrige #2
  * Correction du nombre non valide lors d'une validation à distance de valide à non valide. Corrige #286
  * Ajout d'un lien vers file_input pour une démo d'index
  * Déplacement de l’ancienne méthode d'acceptation vers la méthode supplémentaire d'extension, ajout d'une nouvelle méthode d'acceptation pour gérer le filtrage de type MIME standard du navigateur. Corrige #287 et remplace #369
  * Désactive l'événement flou lorsque onfocusout est défini sur false. Test ajouté.
  * Correction du problème de valeur pour les cases d’option et les cases à cocher. Corrige #363
  * Test ajouté pour rangeWords, expression régulière et liens corrigés dans la méthode. Corrige #308
  * Correction de la démo TinyMCE et ajout d’un lien sur la page de démo. Corrige #382
  * Modification du message de localisation pour min/max. Corrige #273
  * Ajout du sélecteur de pseudo pour les types d’entrée texte afin de résoudre un problème avec l’attribut de type vide par défaut. Ajout de tests et de certaines balises de test. Corrige #217
  * Correction du bogue délégué pour une démo des totaux dynamique. Corrige #51
  * Correction du message incorrect pour le validateur alphanumérique
  * Suppression de la fausse vérification incorrecte pour l’attribut requis
  * correctif d'attribut requis pour les navigateurs non HTML5. Corrige #301
  * Ajout des méthodes "require_from_group" et "skip_or_fill_minimum"
  * Utiliser le code iso correct pour le suédois
  * Mise à jour des fichiers HTML de démo pour utiliser le doctype HTML5
  * Correction du problème d'expression régulière pour les décimales sans zéros non significatifs. Ajout de nouvelles méthodes de test. Corrige #41
  * Introduction d'une méthode elementValue qui normalise uniquement les valeurs de chaîne (ne pas toucher la valeur de tableau de la multisélection). Corrige #116
  * Prise en charge des boutons d'envoi ajoutés de manière dynamique, et mise à jour du cas de test. Utilise validateDelegate. Code de PR #9
  * Correction du guillemet double incorrect dans les contextes de test
  * Correction de la méthode maxWords pour inclure la limite supérieure, ne pas l’exclure. Corrige #284
  * Correction d'une erreur de grammaire dans le message du validateur de plage en allemand. Corrige #315
  * Correction de la gestion de plusieurs noms de classe pour l’option d’errorClass. Test par Max Lynch. Corrige #280
  * Correction de l’utilisation de jQuery.format, doit être $.validator.format. Corrige #329
  * Méthodes pour 'tous' les numéros de téléphone du Royaume-Uni + codes postaux du Royaume-Uni
  * Méthode de modèle : Convertir le paramètre de chaîne expression régulière. Corrige le problème #223
  * Erreur de grammaire dans le fichier de localisation allemand
  * Localisation estonienne ajoutée pour les messages
  * Amélioration de la gestion des info-bulles dans la démo themerollered
  * Ajout de type="text" aux champs de saisie sans attribut de type pour qSA
  * Mise à jour de la démo themerollered pour utiliser une info-bulle afin d'afficher les erreurs par superposition.
  * Mise à jour de la démo themerollered pour utiliser la dernière IU jQuery (et la dernière version de jQuery). Déplacement du code pour accélérer le chargement de la page.
  * Correction du message d’erreur min tronqué en japonais.
  * Mise à jour du plug-in de formulaire avec la version la plus récente. Amélioration de la démo ajaxSubmit.
  * Suppression des méthodes dateDE et numberDE de classRuleSettings, restes du déplacement de ces éléments vers les méthodes localisées
  * Transmission de l'événement d'envoi au rappel submitHandler
  * Correction #219 - correction de valid() sur les éléments avec rappel de dépendance ou expression de dépendance.
  * Amélioration de la version pour supprimer le répertoire distant pour s'assurer que seule la version actuelle est compressée

<a name="190"></a>1.9.0
---
* Ajout de la localisation basque (Europe)
* Ajout de la localisation slovène (SL)
* Correction du problème #127 - les traductions finnoises contiennent un : au lieu d'un ;
* Correction de la localisation russe, problème de syntaxe mineur
* Ajout de la prise en charge des types d’entrées HTML5, corrige #97
* Prise en charge améliorée de HTML5 en définissant l’attribut novalidate sur le formulaire, et lecture de l’attribut de type.
* Correction de showLabel() pour supprimer toutes les classes de l'élément erroné. Suppression de settings.validClass uniquement. Corrige #151.
* Ajout de 'pattern' aux autres méthodes pour validation par rapport aux expressions régulières arbitraires.
* Méthode e-mail améliorée pour ne pas autoriser le point à la fin (valide ^pur RFC, mais non souhaitable ici). Corrige #143
* Correction des traductions suédoises et norvégiennes, permutation des messages min/max. Corrige #181
* Correction #184 - resetForm : doit annuler lastElement
* Correction #71 - amélioration de la méthode de temps existante et ajout de la méthode time12h pour le format d’heure 12h am/pm
* Correction #177 - correction de la validation d’une entrée de case d’option ou de case à cocher unique
* Correction #189 - les éléments masqués sont maintenant ignorés par défaut
* Correction 194 # - requise car l'attribut échoue si jQuery>=1,6 - utiliser .prop au lieu de .attr
* Corrections #47, #39, #32 - les numéros de carte de crédit peuvent contenir des espaces ainsi que des tirets (les espaces sont généralement saisis par les utilisateurs).

<a name="181"></a>1.8.1
---
* Ajout de la localisation thaï (TH), corrige #85
* Ajout de la localisation vietnamienne (VI), merci Ngoc
* Correction du problème #78. Le style d’erreur/de validation s’applique à toutes les cases d’option du même groupe pour la validation requise.
* N’utilisez pas form.elements car cet objet n'est plus pris en charge dans jQuery 1.6. Cet objet comporte beaucoup d'erreurs (IE6-8: form.elements === form).

<a name="180"></a>1.8.0
---
* Localisation NL améliorée (http://plugins.jquery.com/node/14120)
* Ajout de la localisation géorgienne (GE), merci Avtandil Kikabidze
* Ajout de la localisation serbe (SR), Merci Aleksandar Milovac
* Ajout d’ipv4 et ipv6 aux autres méthodes, merci Natal Ngétal
* Ajout de la localisation japonaise (JA), merci Bryan Meyerovich
* Ajout de la localisation catalane (CA), merci Xavier de Pedro
* Correction des instructions var manquantes dans les boucles for-in
* Correctif de la validation à distance où un message était compris (https://github.com/jzaefferer/jquery-validation/issues/11)
* Correctifs pour la compatibilité avec jQuery 1.5.1, avec conservation de la compatibilité descendante

<a name="17"></a>1.7
---
* Ajout de la localisation lituanienne (LT)
* Ajout de la localisation grecque (EL) (http://plugins.jquery.com/node/12319)
* Ajout de la localisation lettone (LV) (http://plugins.jquery.com/node/12349)
* Ajout de la localisation hébraïque (HE) (http://plugins.jquery.com/node/12039)
* Correction de la localisation espagnole (http://plugins.jquery.com/node/12696)
* Ajout de la démo themerolled jQuery UI
* Suppression de cmxform.js
* Correction des quatre points-virgules manquants (http://plugins.jquery.com/node/12639)
* phone-method renommé phoneUS dans additional-methods.js
* Ajout des méthodes phoneUK et mobileUK à additional-methods.js (http://plugins.jquery.com/node/12359)
* Options étendues approfondies pour éviter de modifier plusieurs formulaires en utilisant rules-method sur un élément unique (http://plugins.jquery.com/node/12411)
* Correctifs pour la compatibilité avec jQuery 1.4.2, avec conservation de la compatibilité descendante

<a name="16"></a>1.6
---
* Ajout des localisations arabe (AR), portugaise (PTPT), perse (FA), finnoise (FI) et bulgare (BR)
* Mise à jour de la localisation suédoise (SE) (certains caractères html iso manquants)
* Correction de $.validator.addMethod pour gérer correctement une chaîne vide ou non définie pour l’argument de message
* Correction de deux variables globales accidentelles
* Amélioration de max/min/rangeWords (dans additional-methods.js) pour supprimer html avant le comptage ; utile lors du décompte des mots dans un éditeur de texte enrichi
* Ajout de méthodes localisées pour DE, NL et PT, en supprimant les méthodes dateDE et numberDE (utilisez plutôt messages_de.js et methods_de.js avec les méthodes de date et de nombre)
* Correction de la synchronisation d'envoi d'un formulaire à distance, bravo Matas Petrikas !
* Amélioration de la validation de sélection interactive, désormais la validation sur clic (par le biais d’une option ou d’une sélection, incohérence dans les navigateurs) ; ne fonctionne pas dans Safari, un événement de clic ne se déclenche pas sur les éléments de sélection ; corrige http://plugins.jquery.com/node/11520
* Mise à jour vers la dernier plug-in de formulaire (2.36), corrigeant ainsi http://plugins.jquery.com/node/11487
* Liaison de l’événement blur pour la cible equalTo afin de revalider quand cette cible change, corrige http://plugins.jquery.com/node/11450
* Validation simplifiée de la sélection, délégation à la méthode jQuery’s val() pour obtenir la valeur de sélection ; devrait corriger http://plugins.jquery.com/node/11239
* Correction du message par défaut pour les chiffres (http://plugins.jquery.com/node/9853)
* Correction d’un problème avec le message distant mis en cache (http://plugins.jquery.com/node/11029 et http://plugins.jquery.com/node/9351)
* Correction d’un point-virgule manquant dans additional-methods.js (http://plugins.jquery.com/node/9233)
* Ajout de la détection automatique des paramètres de substitution dans les messages, supprimant ainsi la nécessité de fournir des fonctions de format (http://plugins.jquery.com/node/11195)
* Correction d’un problème avec :filled/:blank, causé par Sizzle (http://plugins.jquery.com/node/11144)
* Ajout d’une méthode entière à additional-methods.js (http://plugins.jquery.com/node/9612)
* Correction de la méthode errorsFor où for-attribute contient des caractères qui nécessitent un échappement pour être valides à l’intérieur d’un sélecteur (http://plugins.jquery.com/node/9611)

<a name="155"></a>1.5.5
---
* Correctif pour http://plugins.jquery.com/node/8659
* Correction de la virgule de fin dans messages_cs.js

<a name="154"></a>1.5.4
---
* Correction du bogue de méthode distante (http://plugins.jquery.com/node/8658)

<a name="153"></a>1.5.3
---
* Correction d’un bogue lié à l’option de wrapper, où tous les éléments ancêtres correspondant à l’option de wrapper étaient sélectionnés (http://plugins.jquery.com/node/7624)
* Mise à jour de la démo en plusieurs parties afin d'utiliser le dernier élément Accordion de l’interface utilisateur jQuery
* Ajout des méthodes dateNL et time à additionalMethods.js
* Ajout de la localisation en chinois traditionnel (Taïwan, tw) et kazakhe (cm)
* Déplacement de jQuery.format (anciennement String.format) vers jQuery.validator.format, jQuery.format est déprécié et sera supprimé dans la version 1.6 (pour obtenir des détails, consultez http://code.google.com/p/jquery-utils/issues/detail?id=15)
* Nettoyage de messages_pl.js et messages_ptbr.js (les messages définis pour min/max/rangeValue, qui ont été supprimés dans 1.4)
* Correction de la logique booléenne défectueuse dans valid-plugin-method pour plusieurs éléments ; maintenant, tous les éléments doivent être valides pour un résultat boolean-true (http://plugins.jquery.com/node/8481)
* Enhancement $.validator.addMethod: Un message de tiers non défini-argument ne remplace pas une (message existant http://plugins.jquery.com/node/8443)
* Amélioration de l’option de submitHandler : Lorsqu’il est utilisé, cliquez sur Envoyer des événements sur les boutons sont capturés et le bouton d’envoi est inséré dans le formulaire avant d’appeler submitHandler et supprimé par la suite ; conserve (intactes de boutons Envoyer http://plugins.jquery.com/node/7183#comment-3585)
* Ajout de l’option validClass, "valid" par défaut, qui ajoute cette classe à tous les éléments valides, après la validation (http://dev.jquery.com/ticket/2205)
* Ajout de la méthode creditcardtypes à additionalMethods.js, notamment des tests (par le biais de http://dev.jquery.com/ticket/3635)
* Amélioration de la méthode distante pour autoriser les messages côté serveur sous forme de chaîne, ou true pour valide, ou false pour non valide à l’aide du message défini côté client (http://dev.jquery.com/ticket/3807)
* Amélioration de la méthode d’acceptation pour également accepter une liste séparée par des virgules de type Drupal de valeurs (http://plugins.jquery.com/node/8580)

<a name="152"></a>1.5.2
---
* Correction des messages dans additional-methods.js pour maxWords, minWords et rangeWords afin d'inclure l’appel à $.format
* Valeur fixe transmise aux méthodes pour exclure le retour chariot (\r), comme le fait val() dans jQuery
* Ajout de la localisation slovaque (sk)
* Ajout de la démonstration pour l’intégration avec des onglets d’interface utilisateur jQuery
* Ajout d'un exemple selects-grouping aux onglets de démo (voir le deuxième onglet, champ de date de naissance)

<a name="151"></a>1.5.1
---
* Mise à jour de la démo marketo pour utiliser l'option invalidHandler au lieu de lier l’événement invalid-form
* Ajout d'un exemple d’intégration TinyMCE
* Ajout de la localisation ukrainienne (ua)
* Correction de la validation de longueur pour utiliser une valeur ajustée (régression de 1,5 avec ajustement global avant suppression de la validation)
* Divers petits correctifs pour la compatibilité avec 1.2.6 et 1.3

<a name="15"></a>1,5
---
* Amélioration de la démo de base, validation du champ confirm-password après modification du mot de passe
* Correction de la validation de base pour transmettre la valeur d’entrée non ajustée comme premier paramètre aux méthodes de validation, modification requise en conséquence ; interrompt la méthode personnalisée existante qui s’appuie sur l'ajustement
* Ajout des localisations norvégienne (no), italienne (it), hongroise (hu) et roumaine (ro)
* Correction #3195 : Deux failles dans la localisation suédoise
* Correction #3503 : Étendue Rules pour accepter les propriétés de messages : permet de spécifier ajouter des messages personnalisés à un élément via rules (« ajouter », {messages : {requis : « Required ! " } });
* Correction #3356 : Régression depuis #2908 lors de l’utilisation de meta-option
* Correction #3370 : Option ajouté ignoreTitle, définie pour ignorer la lecture des messages à partir de l’attribut de titre, permet d’éviter des problèmes avec la barre d’outils Google ; valeur par défaut est false pour la compatibilité
* Correction #3516 : Événement déclencheur invalid-form même lorsque la validation à distance est impliquée.
* Ajout de l’option invalidHandler comme raccourci vers bind("invalid-form", function() {})
* Correction du problème dans Safari pour d'indicateur de chargement dans ajaxSubmit-integration-demo (ajouter d'abord au corps, puis masquer)
* Ajout d'un test pour la validation de carte de crédit et message par défaut amélioré
* Amélioration de la validation à distance, en acceptant les options pour accéder à $.ajax en tant que paramètre (chaîne URL ou options, y compris la propriété url et tous les autres éléments pris en charge par $.ajax)

<a name="14"></a>1.4
---
* Correction #2931, valider les éléments dans l’ordre du document et ignorer les entrées type=image
* Correction de l'utilisation des variables $ et jQuery, désormais entièrement compatibles avec toutes les variations d’utilisation de noConflict
* Implémentation de #2908, activation de messages personnalisés via metadata ala class="{required:true,messages:{required:'required field'}}", ajout de demo/custom-messages-metadata-demo.html
* Suppression des méthodes déconseillées minValue (min), maxValue (max), rangeValue (rangevalue), minLength (minlength), maxLength (maxlength), rangeLength (rangelength)
* Correction de la régression #2215 : Suppression de la surbrillance uniquement pour les éléments en cours, pas tout
* Implémentation de 2989 #, permettant au bouton image d'annuler la validation
* Problème résolu où IE effectue une mauvaise validation avec maxlength=0
* Ajout de la localisation tchèque (cs)
* Réinitialisation de validator.submitted sur validator.resetForm(), permettant une réinitialisation complète lorsque cela est nécessaire
* Correction 3035 #, ignorer tous les attributs erronés lors de la lecture des règles (0, non défini, chaîne vide), suppression d'une partie de la solution de contournement maxlength (pour 0)
* Ajout de la localisation néerlandaise (nl) (#3201)

<a name="13"></a>1.3
---
* Correction de l'événement invalid-form, désormais uniquement déclenché lorsque le formulaire n’est pas valide
* Ajout des localisations espagnole (es), russe (ru), portugaise (Brésil) (ptbr), turque (tr) et polonaise (pl)
* Ajout du plug-in removeAttrs pour faciliter l’ajout et la suppression de plusieurs attributs
* Ajout de l'option groups afin d'afficher un message pour plusieurs éléments, via groups: { arbitraryGroupName: "fieldName1 fieldName2[, fieldNameN" }
* Amélioration de rules() pour ajouter et supprimer des règles (statiques) : rules("add", "method1[, methodN]"/{method1:param[, method_n:param]}) et rules("remove"[, "method1[, method_n]")
* Amélioration de rules-option, accepte les listes de chaînes séparées par des espaces dans les méthodes, par exemple {birthdate: "required date"}
* Validation de groupe de case à cocher fixe avec des règles inline : Tant que les règles sont spécifiées sur le premier élément, le groupe est désormais correctement validé sur cliquez sur
* Correction #2473, ignorer toutes les règles avec un paramètre explicite de type boolean-false, par exemple required:false revient à ne pas spécifier l'argument required (géré comme required:true jusqu'à présent)
* Correction #2424, avec un correctif modifié à #2473 : Les méthodes qui retournent une incompatibilité de dépendance ne s’arrêtent plus ; l’évaluation d’autres règles de Pourtant, réussite n’est pas appliquée pour les champs facultatifs
* Correction de la validation des URL et e-mails pour ne pas utiliser de valeurs ajustées
* Correction de la validation de carte de crédit pour accepter uniquement des chiffres et des tirets ("asdf" n’est pas un numéro de carte de crédit valide)
* Autorisation à la fois des éléments de bouton et d’entrée pour les boutons d'annulation (via class="cancel")
* Correction #2215 : Affichage des messages fixe pour appeler d’activation/désactivation dans le cadre de l’affichage et masquage des messages, des effets secondaires visual n’est plus lors de la vérification d’un élément et un validator.checkForm pour valider un formulaire sans interface utilisateur
* Réécriture des sélecteurs personnalisés (:blank, :filled, :unchecked) avec des fonctions pour la compatibilité avec AIR

<a name="121"></a>1.2.1
-----

* Plug-in du délégué fourni avec le plug-in de validation - toujours requis
* Amélioration de la validation à distance pour inclure des parties du plug-in ajaxQueue pour une bonne synchronisation (aucun plug-in supplémentaire requis)
* Correction de l'erreur stopRequest qui empêche pendingRequest < 0
* Ajout de la propriété jQuery.validator.autoCreateRanges, définie par défaut sur false, pour remplacer min/max par range et minlength/maxlength par rangelength ; corrige le problème introduit par la création automatique des plages dans la version 1.2
* Correction de optional-methods pour ne rien mettre en surbrillance si le champ est vide, autrement dit, ne déclenche aucune réussite
* Autorisation de false/null pour les options d'activation/désactivation de surbrillance au lieu de forcer un événement do-nothing-callback si aucun élément ne doit être mis en surbrillance
* Correction de l'appel validate() sans aucun élément sélectionné, retournant une valeur non définie au lieu de lever une erreur
* Amélioration de la démo, remplacement des métadonnées par des classes/attributs pour spécifier les règles
* Correction de l’erreur qui apparait quand aucun message personnalisé n’est utilisé pour la validation à distance
* Modification de la validation des e-mails et des URL pour exiger une étiquette de domaine et une étiquette supérieure
* Correction de la validation des URL et des e-mails pour exiger TLD (pour exiger une étiquette de domaine) ; la version 1.2 (TLD facultatif) est déplacée vers les compléments en tant que url2 et email2
* Correction de la démonstration dynamic-totals dans IE6/7 et amélioration de la création des modèles, en utilisant textarea pour stocker un modèle multiligne et une interpolation de chaîne
* Ajout d'un exemple de formulaire de connexion avec un lien d'envoi du mot de passe ("Email password") rendant le champ de mot de passe facultatif
* Amélioration de la démo dynamic-totals avec un exemple d’un message unique pour deux champs

<a name="12"></a>1.2
---

* Ajout d’un exemple de validation AJAX-captcha (basé sur http://psyrens.com/captcha/)
* Ajout de remember-the-milk-demo (merci à l’équipe RTM pour l’autorisation !)
* Ajout de marketo-demo (merci Glen Lipka !)
* Ajout de la prise en charge de ajax-validation, voir la méthode "remote" ; serverside retourne JSON, true pour les éléments valides, false ou une chaîne pour non valide, une chaîne est utilisée comme message
* Ajout d'options d'activation/désactivation de surbrillance, par défaut bascule errorClass sur element, permettant une mise en surbrillance personnalisée
* Ajout de la méthode valid()plugin pour faciliter la vérification par programmation des formulaires et champs sans avoir à utiliser l’API du validateur
* Ajout de la méthode rules()plugin pour lire et écrire des règles pour un élément (actuellement en lecture seule)
* Remplacement de l’expression régulière pour la méthode email, grâce à la participation de Scott Gonzalez (consultez http://projects.scottsplayground.com/email_address_validation/)
* Restructuration de l'architecture d'événement pour s’appuyer uniquement sur la délégation, ce qui améliore à la fois les performances et facilite l’utilisation pour le développeur (nécessite jquery.delegate.js)
* Déplacement de la documentation d’Inline vers http://docs.jquery.com/Plugins/Validation, notamment les exemples interactifs pour toutes les méthodes
* Suppression de la validation validator.refresh(), la validation est maintenant complètement dynamique
* minValue renommé en min, maxValue en max et rangeValue en range, les noms précédents sont déconseillés (à supprimer dans 1.3)
* minLength renommé en minlength, maxLength en maxlength et rangeLength en rangelength, les noms précédents sont déconseillés (à supprimer dans 1.3)
* Ajout de fonctionnalités supplémentaires pour fusionner min + max et range et minlength + maxlength dans rangelength
* Ajout de la prise en charge des paramètres de règle dynamique, permettant de spécifier une fonction en tant que paramètre, par exemple pour minlength, appel lors de la validation de l’élément
* Autorisation de la spécification d'une chaîne null ou vide en tant que message pour ne rien afficher (voir la démo marketo)
* Remaniement de règles : Maintenant prend en charge la combinaison de règles-option, les métadonnées, les classes (nouveau) et les attributs (nouveau), voir rules() pour plus d’informations

<a name="112"></a>1.1.2
---

* Remplacement de l’expression régulière pour la méthode URL, grâce à la participation de Scott Gonzalez (consultez http://projects.scottsplayground.com/iri/)
* Amélioration de la méthode email pour mieux gérer les caractères unicode
* Correction d'une erreur de conteneur pour masquer lorsque tous les éléments sont valides, pas uniquement lors de l'envoi du formulaire
* Correction de String.format en jQuery.format (déplacement vers l’espace de noms jQuery)
* Correction de la méthode accept pour accepter des extensions en majuscules et minuscules
* Correction de la méthode validate()plugin pour créer une seule instance du validateur pour un formulaire donné et toujours retourner une seule instance (évite de multiples liaisons d'événements )
* Modification du journal de la console debug-mode du niveau erreur ("error") au niveau avertissement ("warn")

<a name="111"></a>1.1.1
-----

* Correction de l'élément XHTML non valide, empêchant la création d'une étiquette d'erreur dans IE depuis jQuery 1.1.4
* String.format fixe et améliorées : Recherche globale et remplacer, améliore le traitement des arguments de tableau
* Correction de la gestion de cancel-button afin d'utiliser validator-object pour stocker l’état au lieu de l’élément de formulaire
* Correction des sélecteurs de nom pour gérer les noms « complexes », par exemple qui contient des crochets ("list[]")
* Ajout d'un bouton et désactivation des éléments à exclure de la validation
* Déplacement des gestionnaires d’événements d'élément à actualiser pour pouvoir ajouter des gestionnaires aux nouveaux éléments
* Correction de la validation d’e-mail pour autoriser les domaines longs de niveau supérieur (par exemple ".travel")
* Déplacement de showErrors() de valid() à form()
* Ajout de validator.size() : retourne le nombre d’erreurs actuelles
* Appel de submitHandler avec validateur comme étendue pour faciliter l’accès à ses méthodes, par exemple pour rechercher les étiquettes d’erreur à l’aide de errorsFor(Element)
* Compatible avec jQuery 1.1.x et 1.2.x

<a name="11"></a>1.1
---

* Ajout d'une validation sur les événements blur, keyup et click (pour les cases à cocher et les cases d'option). Remplace event-option.
* Correction de resetForm
* Correction de custom-methods-demo

<a name="10"></a>1.0
---

* Amélioration des méthodes number et numberDE pour rechercher les nombres décimaux corrects avec séparateurs
* Seuls les éléments avec des règles sont vérifiés (sinon success-option s'applique à tous les éléments)
* Ajout de la méthode de numéro de carte de crédit (merci Brian Klug !)
* Ajout de ignore-option, par exemple ignore: "[@type=hidden]", cette expression permet d'exclure les éléments à valider. Par défaut : aucune, même si les boutons submit et reset sont toujours ignorés
* Amélioration importante de Functions-as-messages grâce à une application d’assistance String.format flexible
* Acceptation de Functions en tant que messages, pour fournir runtime-custom-messages
* Correction de l'exclusion des éléments sans règles de successList
* Correction de custom-method-demo, remplacement de l’alerte par un message affichant le nombre d’erreurs
* Correction de form-submit-prevention lors de l’utilisation de submitHandler
* Suppression totale de la dépendance sur les ID d’éléments, même s'ils sont toujours utilisés (le cas échéant) pour lier des étiquettes d’erreur à des entrées. Résultat obtenu à l’aide d’un tableau avec {name, message, element} au lieu d’un objet avec des paires id:message pour l'élément errorList interne.
* Ajout de la prise en charge pour spécifier des règles simples comme chaînes simples, par exemple "required" équivaut à {required: true}
* Fonctionnalités supplémentaires : Ajouter errorClass à l’élément parent de champ non valide s, ce qui facilite le conteneur de champ d’étiquette ou de l’étiquette pour le champ de style.
* Ajout d'une fonctionnalité : focusCleanup - si activée, supprime errorClass des éléments non valides et masque tous les messages d’erreur chaque fois que le focus est mis sur l’élément.
* Ajout de l’option de réussite pour montrer qu'un champ a été correctement validé
* Correction de l'option select-issue Opera (pour éviter une collision d’attribut)
* Résolution des problèmes de mise en focus des éléments masqués dans IE
* Ajout d'une fonctionnalités pour ignorer la validation pour les boutons d'envoi avec la classe "cancel"
* Résolution des problèmes potentiels avec la barre d’outils Google en préférant les messages d’option de plug-in à l’attribut de titre
* submitHandler est appelé uniquement si un événement submit réel a été géré, validator.form() retourne la valeur false uniquement pour les formulaires non valides
* Les éléments non valides sont mis en focus uniquement avec l'événement submit ou via validator.focusInvalid(), ce qui évite tout problème de type focus-on-blur
* Résolution du problème de disposition du conteneur d'erreur IE6
* Personnalisation de l’élément d’erreur via l’option d’errorElement
* Ajout de validator.refresh() pour rechercher de nouvelles entrées dans le formulaire
* Ajout de la méthode de validation accept, vérifie les extensions de fichier
* Amélioration de la fonctionnalité de dépendance en ajoutant deux expressions personnalisées : ":blank" pour sélectionner les éléments avec une valeur vide, et ":filled" pour sélectionner les éléments avec une valeur, les deux cas excluant les espaces blancs
* Ajout d’une méthode resetForm() au validateur : Réinitialise chaque élément de formulaire (à l’aide du plug-in de formulaire, si disponible), supprime les classes des éléments non valides et masque tous les messages d’erreur
* Correction des docs pour validator.showErrors()
* Correction de la création d’étiquette d'erreur pour toujours utiliser html() au lieu de text(), ce qui permet au code HTML arbitraire d'être transmis sous forme de messages
* Correction de la création d’étiquettes d'erreur afin d'utiliser la classe d’erreur spécifiée
* Fonctionnalité de dépendance ajouté : La méthode Required accepte String (expressions jQuery) et fonctions comme argument
* Amélioration profonde de la personnalisation de l’affichage des messages d’erreur : Utiliser des messages normaux et afficher/masquer un conteneur supplémentaire ; Remplacer complètement l’affichage des messages avec un mécanisme propre (tout en la possibilité de déléguer au gestionnaire par défaut ; Personnaliser le placement des étiquettes générées (au lieu de l’élément par défaut sous)
* Correction de deux bogues majeurs dans IE (conteneurs d'erreur) et Opera (métadonnées)
* Modification des méthodes de validation pour accepter les champs vides comme champs valides (exception : les méthodes "required" et "equalTo" bien entendu)
* "min" renommé en "minLength", "max" en "maxLength", "length" en "rangeLength"
* Ajout de "minValue", "maxValue" et "rangeValue"
* API simplifiée pour la prise en charge de différents événements. La valeur par défaut, submit, peut être désactivée. Si un événement est spécifié, s'applique à chaque élément (plutôt qu'à l’intégralité du formulaire). La combinaison de keyup-validation avec submit-validation est maintenant très facile à configurer
* Ajout de la prise en charge pour one-message-per-rule lors de la définition de messages via les paramètres de plug-in
* Ajout de la prise en charge pour encapsuler les métadonnées dans un élément parent. Utile lorsque les métadonnées servent dans d'autres plug-ins.
* Tests refactorisés et démonstrations : Moins de fichiers, de meilleures démos
* Documentation améliorée : Plus d’exemples de méthodes, autres textes expliquer certaines notions de base de référence
