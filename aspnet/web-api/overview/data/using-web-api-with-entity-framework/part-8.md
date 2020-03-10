---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Afficher les détails de l’élément | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557322"
---
# <a name="display-item-details"></a>Afficher les détails des éléments

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez ajouter la possibilité d’afficher les détails de chaque livre. Dans App. js, ajoutez le code suivant au modèle de vue :

[!code-javascript[Main](part-8/samples/sample1.js)]

Dans views/domotique/index. cshtml, ajoutez un élément de liaison de données au lien Détails :

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Cela lie le gestionnaire de clics pour le &lt;un élément&gt; à la fonction `getBookDetail` sur le modèle de vue.

Dans le même fichier, remplacez la marque suivante :

[!code-html[Main](part-8/samples/sample3.html)]

par le code :

[!code-html[Main](part-8/samples/sample4.html)]

Ce balisage crée une table qui est liée aux données des propriétés du `detail` observable dans le modèle de vue.

La syntaxe «&lt;!--Ko--&gt;&quot; vous permet d’inclure une liaison Knockout en dehors d’un élément DOM. Dans ce cas, la liaison de `if` entraîne l’affichage de cette section du balisage uniquement lorsque `details` n’est pas null.

[!code-html[Main](part-8/samples/sample5.html)]

Maintenant, si vous exécutez l’application et cliquez sur l’une des &quot;détails&quot; les liens, l’application affiche les détails du livre.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Précédent](part-7.md)
> [Suivant](part-9.md)
