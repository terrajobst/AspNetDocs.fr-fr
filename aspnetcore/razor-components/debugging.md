---
title: Déboguer des composants de Razor
author: guardrex
description: Découvrez comment déboguer des applications Blazor et composants de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/debug
ms.openlocfilehash: fb7ddcf3ae40ec28a372adf724a293b375be28a1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034116"
---
# <a name="debug-razor-components"></a>Déboguer des composants de Razor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Composants de Razor a certaines *très précoce* prise en charge pour le débogage côté client Blazor applications s’exécutant sur WebAssembly dans Chrome. Bien que cette prise en charge initiale de la méthode de débogage est très limitée et incomplète, il montre l’infrastructure de débogage base assemblent.

Pour déboguer une application de Blazor côté client dans Chrome :

* Créer une application Blazor en `Debug` configuration (la valeur par défaut pour les applications non publiées).
* Exécutez l’application Blazor dans Chrome (version 70 ou version ultérieure).
* Avec le focus clavier sur l’application (pas dans le volet d’outils de développement, dont vous devez probablement fermer pour une expérience de débogage plus claire), sélectionnez le raccourci clavier Blazor spécifiques suivant :
  * `Shift+Alt+D` sur Windows/Linux
  * `Shift+Cmd+D` sur macOS

Exécutez Chrome avec le débogage distant activé pour déboguer l’application Blazor. Si le débogage à distance est désactivé, une page d’erreur est générée par Chrome. La page d’erreur contient des instructions pour l’exécution de Chrome avec le port de débogage ouvert afin que le proxy de débogage Blazor peut se connecter à l’application. *Fermez toutes les instances de Chrome* et redémarrez Chrome comme indiqué.

![Page d’erreur débogage Blazor](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

Une fois que Chrome est en cours d’exécution avec débogage à distance est activé, le raccourci clavier de débogage s’ouvre un nouvel onglet du débogueur. Après quelques instants, le *Sources* onglet affiche une liste des assemblys .NET dans l’application. Développez chaque assembly, puis recherchez le *.cs*/*.cshtml* disponible pour le débogage de fichiers sources. Ensemble des points d’arrêt, revenez à l’onglet de l’application et les points d’arrêt sont atteints. Après un point d’arrêt est atteint, la seule étape (`F10`) ou reprendre (`F8`) normalement.

![Débogage Blazor](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

Blazor fournit un proxy de débogage qui implémente le [protocole Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) et complète le protocole avec. Informations spécifiques à NET. Lorsque vous appuyez sur débogage raccourci clavier, Blazor pointe le Chrome DevTools au niveau du proxy. Le proxy se connecte à la fenêtre du navigateur que vous recherchez à déboguer (par conséquent, la nécessité d’activer le débogage à distance).

Vous vous demandez peut-être pourquoi nous n’utilisons pas simplement les mappages de sources de navigateur. Mappages de sources autorise le navigateur à mapper les fichiers compilés vers leurs fichiers source d’origine. Toutefois, Blazor ne mappe pas C# directement à JS/WASM (au moins pas encore). Au lieu de cela, Blazor effectue interprétation de langage intermédiaire dans le navigateur, par conséquent, les mappages de sources ne sont pas pertinentes.

Notez que les fonctionnalités du débogueur sont **très limitée**. Vous pouvez actuellement uniquement :

* Définir et supprimer des points d’arrêt.
* Étape unique via le code ou la reprise (`F8`).
* Dans le *variables locales* afficher, examinez les valeurs des variables locales de type `int`, `string`, et `bool`.
* Consultez la pile des appels, y compris les chaînes d’appels qui vont à partir de JavaScript dans .NET et à partir de .NET à JavaScript.

Vous *ne peut pas*:

* Examinez les valeurs de n’importe quel variables locales qui ne sont pas un `int`, `string`, ou `bool`.
* Examinez les valeurs des propriétés de la classe du ou des champs.
* Placez le curseur sur les variables pour voir leurs valeurs
* Évaluer des expressions dans la console.
* Étape entre les appels asynchrones.
* Effectuer la plupart des autres scénarios de débogage ordinaires.

Développement de scénarios de débogage supplémentaire est un focus en cours de l’équipe d’ingénierie.

## <a name="troubleshooting-tip"></a>Conseil de dépannage

Si vous rencontrez des erreurs, l’info-bulle suivante peut-être vous aider à :

Dans le **débogueur** onglet, ouvrez les outils de développement dans votre navigateur. Dans la console, exécutez `localStorage.clear()` pour supprimer les points d’arrêt.
