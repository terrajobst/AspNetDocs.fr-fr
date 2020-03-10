---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Création d’une contrainte de route (VB) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont les demandes de navigateur correspondent aux itinéraires en créant des contraintes d’itinéraire avec des expressions régulières.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 205742dd8f866c8828008c8aac7ab3f98b173ceb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601387"
---
# <a name="creating-a-route-constraint-vb"></a>Création d’une contrainte de route (VB)

par [Stephen Walther](https://github.com/StephenWalther)

> Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont les demandes de navigateur correspondent aux itinéraires en créant des contraintes d’itinéraire avec des expressions régulières.

Vous utilisez des contraintes de routage pour restreindre les demandes de navigateur qui correspondent à un itinéraire donné. Vous pouvez utiliser une expression régulière pour spécifier une contrainte d’itinéraire.

Par exemple, imaginez que vous avez défini l’itinéraire dans la liste 1 de votre fichier global. asax.

**Liste 1-global. asax. vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

La liste 1 contient un itinéraire nommé Product. Vous pouvez utiliser l’itinéraire du produit pour mapper les demandes de navigateur aux ProductController contenues dans la liste 2.

**Liste 2-Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Notez que l’action Details () exposée par le contrôleur de produit accepte un seul paramètre nommé productId. Ce paramètre est un paramètre entier.

L’itinéraire défini dans la liste 1 correspond à l’une des URL suivantes :

- /Product/23
- /Product/7

Malheureusement, l’itinéraire correspondra également aux URL suivantes :

- /Product/blah
- /Product/apple

Étant donné que l’action Details () attend un paramètre entier, une requête qui contient autre chose qu’une valeur entière génère une erreur. Par exemple, si vous tapez l’URL/Product/Apple dans votre navigateur, vous obtiendrez la page d’erreur dans la figure 1.

[![la boîte de dialogue Nouveau projet](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Figure 01**: affichage d’une explosion de page ([cliquez pour afficher l’image en taille réelle](creating-a-route-constraint-vb/_static/image2.png))

Ce que vous voulez vraiment faire, c’est uniquement les URL qui contiennent un entier productId correct. Vous pouvez utiliser une contrainte lors de la définition d’un itinéraire pour limiter les URL qui correspondent à l’itinéraire. L’itinéraire du produit modifié dans la liste 3 contient une contrainte d’expression régulière qui correspond uniquement à des entiers.

**Liste 3-global. asax. vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

L’expression régulière \d + correspond à un ou plusieurs entiers. Cette contrainte fait en sorte que l’itinéraire du produit corresponde aux URL suivantes :

- /Product/3
- /Product/8999

Mais pas les URL suivantes :

- /Product/apple
- /Product

Ces requêtes de navigateur seront gérées par un autre itinéraire ou, s’il n’y a pas d’itinéraires correspondants, *l’erreur la ressource est introuvable* est retournée.

> [!div class="step-by-step"]
> [Précédent](creating-custom-routes-vb.md)
> [Suivant](creating-a-custom-route-constraint-vb.md)
