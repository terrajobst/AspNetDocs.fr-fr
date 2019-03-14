---
ms.openlocfilehash: f60f934934ae9e16bbd1174959d99aecdbb5833d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025106"
---
--

[![Slack](https://bootstrap-slack.herokuapp.com/badge.svg)](https://bootstrap-slack.herokuapp.com)
![Version de Bower](https://img.shields.io/bower/v/bootstrap.svg)
[![Version de npm](https://img.shields.io/npm/v/bootstrap.svg)](https://www.npmjs.com/package/bootstrap)
[![État de la build](https://img.shields.io/travis/twbs/bootstrap/master.svg)](https://travis-ci.org/twbs/bootstrap)
[![État de devDependency](https://img.shields.io/david/dev/twbs/bootstrap.svg)](https://david-dm.org/twbs/bootstrap#info=devDependencies)
[![NuGet](https://img.shields.io/nuget/v/bootstrap.svg)](https://www.nuget.org/packages/Bootstrap)
[![État des tests Selenium](https://saucelabs.com/browser-matrix/bootstrap.svg)](https://saucelabs.com/u/bootstrap)

Bootstrap est une infrastructure frontale élégante, intuitive et puissante qui accélère et facilite le développement web. Créée par [Mark Otto](https://twitter.com/mdo) et [Jacob Thornton](https://twitter.com/fat), elle est géré par [l’équipe principale](https://github.com/orgs/twbs/people) avec l’aide et l’implication massive de la communauté.

Pour commencer, consultez <http://getbootstrap.com>.


## <a name="table-of-contents"></a>Table des matières

* [Démarrage rapide](#quick-start)
* [Bogues et demandes de fonctionnalités](#bugs-and-feature-requests)
* [Documentation](#documentation)
* [Contribution](#contributing)
* [Community](#community)
* [Gestion des versions](#versioning)
* [Créateurs](#creators)
* [Droits d’auteur et licence](#copyright-and-license)


## <a name="quick-start"></a>Démarrage rapide

Plusieurs options de démarrage rapide sont disponibles :

* [Téléchargez la dernière version](https://github.com/twbs/bootstrap/archive/v3.3.7.zip).
* Clonez le référentiel : `git clone https://github.com/twbs/bootstrap.git`.
* Installez avec [Bower](http://bower.io) : `bower install bootstrap`.
* Installez avec [npm](https://www.npmjs.com) : `npm install bootstrap@3`.
* Installez avec [Meteor](https://www.meteor.com) : `meteor add twbs:bootstrap`.
* Installez avec [Composer](https://getcomposer.org) : `composer require twbs/bootstrap`.

Lisez la [page Bien démarrer](http://getbootstrap.com/getting-started/) pour plus d’informations sur le contenu de l’infrastructure, des modèles et des exemples, etc.

### <a name="whats-included"></a>Contenu

Dans le téléchargement, vous trouverez les répertoires et fichiers suivants, qui groupent logiquement des ressources communes et proposent des variantes compilées et réduites. Cela devrait ressembler à ceci :

```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap.min.css.map
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   ├── bootstrap-theme.min.css
│   └── bootstrap-theme.min.css.map
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    ├── glyphicons-halflings-regular.woff
    └── glyphicons-halflings-regular.woff2
```

Nous fournissons du code CSS et JS compilé (`bootstrap.*`), ainsi que du code CSS et JS compilé et réduit (`bootstrap.min.*`). Les [mappages de sources](https://developer.chrome.com/devtools/docs/css-preprocessors) CSS (`bootstrap.*.map`) sont utilisables avec les outils de développement de certains navigateurs. Les polices Glyphicon sont incluses, de même que le thème Bootstrap facultatif.


## <a name="bugs-and-feature-requests"></a>Bogues et demandes de fonctionnalités

Vous rencontrez un bogue ou souhaitez soumettre une demande de fonctionnalité ? Lisez tout d’abord les [instructions relatives aux problèmes](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md#using-the-issue-tracker) et parcourez les problèmes existants et fermés. Si votre idée ou votre problème n’a pas encore été abordé, [ouvrez un nouveau problème](https://github.com/twbs/bootstrap/issues/new).

Remarque : Les **demandes de fonctionnalités doivent porter sur [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev)**, car Bootstrap v3 est maintenant en mode maintenance et fermé aux nouvelles fonctionnalités. Cela nous permet de concentrer nos efforts sur Bootstrap v4.


## <a name="documentation"></a>Documentation

La documentation de Bootstrap, incluse dans le répertoire racine de ce dépôt, est générée avec [Jekyll](http://jekyllrb.com) et est hébergée publiquement sur GitHub Pages à l’adresse <http://getbootstrap.com>. Les documents sont également exécutables en local.

### <a name="running-documentation-locally"></a>Exécution en local de la documentation

1. Si nécessaire, [installez Jekyll](http://jekyllrb.com/docs/installation) et d’autres dépendances Ruby avec `bundle install`.
   **Remarque pour les utilisateurs de Windows :** Lecture [ce guide non officiel](http://jekyll-windows.juthilo.com/) se Jekyll en cours d’exécution sans problème.
2. Dans le répertoire `/bootstrap` racine, exécutez `bundle exec jekyll serve` en ligne de commande.
4. Ouvrez `http://localhost:9001` dans votre navigateur : c’est fini.

Pour plus d’informations sur Jekyll, lisez sa [documentation](http://jekyllrb.com/docs/home/).

### <a name="documentation-for-previous-releases"></a>Documentation des versions antérieures

La documentation de la version 2.3.2 est actuellement disponible sur <http://getbootstrap.com/2.3.2/>, en attendant que tout le monde passe à Bootstrap 3.

Les [versions précédentes](https://github.com/twbs/bootstrap/releases) et leur documentation sont également disponibles au téléchargement.


## <a name="contributing"></a>Contribuer

Veuillez lire toutes nos [recommandations sur les contributions](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md). Vous y trouverez des instructions pour signaler des problèmes, ainsi que des normes de codage et des remarques sur le développement.

En outre, si votre demande de tirage (pull request) contient des correctifs ou des fonctionnalités JavaScript, vous devez inclure les [tests unitaires correspondants](https://github.com/twbs/bootstrap/tree/master/js/tests). Tout le code HTML et CSS doit être conforme au [Guide de code](https://github.com/mdo/code-guide), qui est géré par [Mark Otto](https://github.com/mdo).

**Bootstrap v3 est maintenant fermé aux nouvelles fonctionnalités.** Il est à présent en mode maintenance, ce qui nous permet de concentrer nos efforts sur [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev), l’avenir de l’infrastructure. Les demandes de tirage qui ajoutent de nouvelles fonctionnalités (et ne résolvent pas de bogues) doivent plutôt porter sur [Bootstrap v4 (la branche git `v4-dev`)](https://github.com/twbs/bootstrap/tree/v4-dev).

Les préférences en matière d’éditeurs sont disponibles dans le fichier [editor config](https://github.com/twbs/bootstrap/blob/master/.editorconfig) pour faciliter l’utilisation dans les éditeurs de texte courants. Pour en savoir plus et télécharger les plug-ins, consultez <http://editorconfig.org>.


## <a name="community"></a>Communauté

Restez au courant du développement de Bootstrap et dialoguez avec les mainteneurs du projet et les membres de la communauté.

* Suivez-nous [@getbootstrap sur Twitter](https://twitter.com/getbootstrap).
* Lisez et abonnez-vous au [Blog officiel de Bootstrap](http://blog.getbootstrap.com).
* Accédez à [la salle Slack officielle](https://bootstrap-slack.herokuapp.com).
* Discutez avec d’autres utilisateurs de Bootstrap sur IRC. Sur le canal `##bootstrap` du serveur `irc.freenode.net`.
* Vous trouverez l’implémentation sur Stack Overflow (avec la balise [`twitter-bootstrap-3`](https://stackoverflow.com/questions/tagged/twitter-bootstrap-3)).
* Les développeurs sont invités à utiliser le mot clé `bootstrap` sur les packages qui modifient ou ajoutent des fonctionnalités à Bootstrap lorsque la distribution passe par [npm](https://www.npmjs.com/browse/keyword/bootstrap) ou des mécanismes de remise similaires pour assurer une découvrabilité maximale.


## <a name="versioning"></a>Gestion de version

Pour des raisons de transparence dans notre cycle de publication et dans un effort pour assurer la compatibilité descendante, Bootstrap est maintenu sous [les instructions Gestion sémantique de version](http://semver.org/). Il arrive que nous nous trompions, mais nous nous efforçons de respecter ces règles dans la mesure du possible.

Consultez la [section Versions de notre projet GitHub](https://github.com/twbs/bootstrap/releases) : vous y trouverez le journal des modifications de chaque version de Bootstrap. Les messages d’annonce de versions publiés sur le [blog Bootstrap officiel](http://blog.getbootstrap.com) contiennent des résumés des principales modifications apportées à chaque version.


## <a name="creators"></a>Créateurs

**Mark Otto**

* <https://twitter.com/mdo>
* <https://github.com/mdo>

**Jacob Thornton**

* <https://twitter.com/fat>
* <https://github.com/fat>


## <a name="copyright-and-license"></a>Droits d’auteur et licence

Droits d’auteur du code et de la documentation 2011-2016 Twitter, Inc. Code sous [licence du MIT](https://github.com/twbs/bootstrap/blob/master/LICENSE). Documents publiés sous [Creative Commons](https://github.com/twbs/bootstrap/blob/master/docs/LICENSE).
