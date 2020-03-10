---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Créer la vue (IU) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557301"
---
# <a name="create-the-view-ui"></a>Créer la vue (Interface utilisateur)

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez commencer à définir le code HTML de l’application et ajouter la liaison de données entre le code HTML et le modèle de vue.

Ouvrez le fichier views/orig/index. cshtml. Remplacez l’intégralité du contenu de ce fichier par le code suivant.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

La plupart des éléments de `div` sont là pour le style de [démarrage](http://getbootstrap.com/) . Les éléments importants sont ceux avec `data-bind` attributs. Cet attribut lie le code HTML au modèle de vue.

Exemple :

[!code-html[Main](part-7/samples/sample2.html)]

Dans cet exemple, le &quot;`text`&quot; la liaison fait en sorte que l’élément `<p>` affiche la valeur de la propriété `error` à partir du modèle de vue. Rappelez-vous que `error` a été déclaré comme `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Chaque fois qu’une nouvelle valeur est assignée à `error`, Knockout met à jour le texte dans l’élément `<p>`.

La liaison de `foreach` indique à Knockout de parcourir en boucle le contenu du tableau `books`. Pour chaque élément du tableau, Knockout crée un nouvel élément &lt;Li&gt;. Les liaisons dans le contexte de la `foreach` font référence aux propriétés sur l’élément de tableau. Exemple :

[!code-html[Main](part-7/samples/sample4.html)]

Ici, la liaison `text` lit la propriété auteur de chaque livre.

Si vous exécutez l’application maintenant, elle doit ressembler à ceci :

![](part-7/_static/image1.png)

La liste de livres est chargée de façon asynchrone, après le chargement de la page. Pour le moment, les détails de la &quot;&quot; les liens ne sont pas fonctionnels. Nous allons ajouter cette fonctionnalité dans la section suivante.

> [!div class="step-by-step"]
> [Précédent](part-6.md)
> [Suivant](part-8.md)
