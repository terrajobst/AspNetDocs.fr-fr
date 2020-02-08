---
uid: single-page-application/overview/templates/hottowel-template
title: Modèle d’essuie à chaud | Microsoft Docs
author: madskristensen
description: Modèle HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075058"
---
# <a name="hot-towel-template"></a>Modèle Hot Towel

par [Mads Kristensen](https://github.com/madskristensen)

> Le modèle MVC de la serviette chaude est écrit par John Papa
> 
> Choisissez la version à télécharger :
> 
> [Modèle MVC pour les serviettes à chaud pour Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Modèle MVC pour les serviettes à chaud pour Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Serviette en chaud : parce que vous ne souhaitez pas accéder au SPA sans l’utiliser !

Vous souhaitez créer un SPA, mais vous ne pouvez pas décider où commencer ? Utilisez une serviette et, en quelques secondes, vous disposerez d’un SPA et de tous les outils dont vous avez besoin pour créer vos images !

La serviette constitue un excellent point de départ pour la création d’une application à page unique (SPA) avec ASP.NET. Vous pouvez dès à présent disposer d’une structure modulaire pour votre code, de la navigation dans l’affichage, de la liaison de données, de la gestion de données enrichie et du style simple mais élégant. La serviette à chaud fournit tout ce dont vous avez besoin pour créer un SPA, ce qui vous permet de vous concentrer sur votre application, et non sur la plomberie.

> En savoir plus sur la création d’un SPA à partir des [vidéos, didacticiels et cours Pluralsight de John Papa](http://johnpapa.net/spa?vsix).

## <a name="application-structure"></a>Structure d'application

Le SPA à chaud fournit un dossier d’application qui contient les fichiers JavaScript et HTML qui définissent votre application.

Dans le dossier de l’application :

- Durandal
- services
- ViewModels
- views

Le dossier d’application contient une collection de modules. Ces modules encapsulent les fonctionnalités et déclarent des dépendances sur d’autres modules. Le dossier views contient le code HTML de votre application et le dossier ViewModels contient la logique de présentation des vues (modèle MVVM courant). Le dossier services est idéal pour héberger tous les services courants dont votre application peut avoir besoin, tels que l’extraction de données HTTP ou l’interaction de stockage local. Il est courant que plusieurs ViewModels réutilisent le code des modules de service.

## <a name="aspnet-mvc-server-side-application-structure"></a>Structure d’application côté serveur ASP.NET MVC

La serviette chaude repose sur la structure ASP.NET MVC familière et puissante.

- Démarrage de l’application\_
- Contenu
- Controllers
- Modèles
- Scripts
- Les vues

## <a name="featured-libraries"></a>Bibliothèques proposées

- ASP.NET MVC
- API Web ASP.NET
- Optimisation Web ASP.NET-regroupement et minimisation
- [Breeze. js](http://Breezejs.com) -gestion de données enrichie
- [Durandal. js](http://Durandaljs.com) -navigation et vue de la composition
- [Knockout. js](http://Knockoutjs.com) -liaisons de données
- [Exiger. js](http://requirejs.org) -modularité avec AMD et optimisation
- [Toastr. js](http://jpapa.me/c7toastr) -messages contextuels
- [Démarrage Twitter](https://twitter.github.com/bootstrap/) -style CSS robuste

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Installation via le modèle SPA de l’essuie à chaud Visual Studio 2012

La serviette peut être installée en tant que modèle Visual Studio 2012. Cliquez simplement sur `File` | `New Project`, puis sélectionnez `ASP.NET MVC 4 Web Application`. Sélectionnez ensuite le modèle « application à une seule page » et exécutez !

## <a name="installing-via-the-nuget-package"></a>Installation via le package NuGet

La serviette à chaud est également un package NuGet qui augmente un projet MVC ASP.NET vide existant. Installez simplement à l’aide de NuGet, puis exécutez !

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Comment créer une serviette à chaud ?

Commencez simplement à ajouter du code !

1. Ajoutez votre propre code côté serveur, de préférence Entity Framework et WebAPI (ce qui est vraiment brillant avec Breeze. js).
2. Ajouter des vues au dossier `App/views`
3. Ajouter ViewModels au dossier `App/viewmodels`
4. Ajouter des liaisons de données HTML et Knockout à vos nouvelles vues
5. Mettre à jour les itinéraires de navigation dans `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Procédure pas à pas du HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index. cshtml est l’itinéraire et la vue de départ pour l’application MVC. Il contient toutes les balises méta standard, les liens CSS et les références JavaScript que vous attendez. Le corps contient un `<div>` unique, où tout le contenu (vos vues) sera placé lorsqu’il sera demandé. Le `@Scripts.Render` utilise Require. js pour exécuter le point d’entrée pour le code de l’application, contenu dans le fichier `main.js`. Un écran de démarrage est fourni pour montrer comment créer un écran de démarrage pendant le chargement de votre application.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main. js

Le fichier `main.js` contient le code qui s’exécutera dès que votre application sera chargée. Il s’agit de l’emplacement où vous souhaitez définir vos itinéraires de navigation, définir vos vues de démarrage et effectuer les paramétrages et les amorçages, tels que l’amorçage des données de votre application.

Le fichier `main.js` définit plusieurs modules de Durandal pour aider l’application à démarrer. L’instruction define aide à résoudre les dépendances de modules afin qu’elles soient disponibles pour la fonction. Tout d’abord, les messages de débogage sont activés, ce qui permet d’envoyer des messages sur les événements que l’application exécute dans la fenêtre de console. Le code App. Start indique à Durandal Framework de démarrer l’application. Les conventions sont définies de sorte que Durandal sache que tous les affichages et ViewModels sont contenus dans les mêmes dossiers nommés, respectivement. Enfin, le `app.setRoot` lance le chargement du `shell` à l’aide d’une animation `entrance` prédéfinie.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Les vues

Les vues se trouvent dans le dossier `App/views`.

### <a name="shellhtml"></a>shell.html

Le `shell.html` contient la disposition maître de votre code HTML. Tous vos autres affichages sont composés à l’emplacement de votre vue de `shell`. La serviette à chaud fournit une `shell` avec trois régions de ce type : un en-tête, une zone de contenu et un pied de page. Chacune de ces régions est chargée avec des contenus à partir d’autres vues quand elles sont demandées.

Les liaisons `compose` pour l’en-tête et le pied de page sont codées en dur dans une serviette chaude pour pointer vers les vues `nav` et `footer`, respectivement. La liaison compose pour la section `#content` est liée à l’élément actif du module `router`. En d’autres termes, lorsque vous cliquez sur un lien de navigation, la vue correspondante est chargée dans cette zone.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

Le `nav.html` contient les liens de navigation pour le SPA. C’est ici que la structure de menu peut être placée, par exemple. Il s’agit souvent d’une liaison de données (à l’aide de Knockout) au module `router` pour afficher la navigation que vous avez définie dans la `shell.js`. Knockout recherche les attributs de liaison de données et les lie au ViewModel `shell` pour afficher les itinéraires de navigation et afficher un ProgressBar (à l’aide de l’amorçage Twitter) si le module `router` est en train de naviguer d’une vue à l’autre (voir `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>fichier. html et details. html

Ces vues contiennent du code HTML pour les vues personnalisées. Lorsque l’utilisateur clique sur le lien `home` dans le menu de l’affichage de `nav`, la vue `home` est placée dans la zone de contenu de la vue `shell`. Ces vues peuvent être augmentées ou remplacées par vos propres vues personnalisées.

### <a name="footerhtml"></a>pied de page. html

Le `footer.html` contient du code HTML qui apparaît dans le pied de page, en bas de la vue de `shell`.

## <a name="viewmodels"></a>ViewModels

ViewModels se trouvent dans le dossier `App/viewmodels`.

### <a name="shelljs"></a>Shell. js

Le ViewModel `shell` contient des propriétés et des fonctions liées à la vue de `shell`. C’est souvent là que les liaisons de navigation de menu sont trouvées (voir la logique `router.mapNav`).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Page d’informations. js et details. js

Ces ViewModels contiennent les propriétés et les fonctions liées à la vue `home`. Il contient également la logique de présentation de la vue, et est le collage entre les données et la vue.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Services

Les services se trouvent dans le dossier app/services. Dans l’idéal, vos futurs services tels qu’un module DataService, qui est responsable de l’obtention et de la publication de données distantes, peuvent être placés.

### <a name="loggerjs"></a>Logger. js

La serviette à chaud fournit un module `logger` dans le dossier services. Le module `logger` est idéal pour la journalisation des messages dans la console et pour l’utilisateur dans les toasts publicitaires.
