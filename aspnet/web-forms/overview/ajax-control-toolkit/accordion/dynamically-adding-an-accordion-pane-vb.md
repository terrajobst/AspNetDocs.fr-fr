---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Ajout dynamique d’un volet accordéon (VB) | Microsoft Docs
author: wenz
description: Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: be48db5ea3de4af46b0f864cc9e73d2f518294a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607213"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a>Ajout dynamique d’un volet accordéon (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.

## <a name="steps"></a>Étapes

Le contrôle accordéon expose toutes les propriétés importantes au code côté serveur. Entre autres choses, la propriété `Panes` accorde l’accès à la collection de volets qui composent l’accordéon. Chaque volet est de type `AccordionPane`. Il est donc trivial de créer un tel volet :

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

La propriété `HeaderContainer` de `AccordionPane` permet d’accéder aux contrôles ASP.NET dans la section d’en-tête du volet. la propriété `ContentContainer` de `AccordionPane` fait de même pour la section content du volet. Cela permet au code ASP.NET d’ajouter du contenu aux volets :

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Enfin, le ou les volets doivent être ajoutés à la collection `Panes` de l’accordéon :

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Voici un code complet côté serveur qui ajoute deux volets à un contrôle accordéon :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Le seul élément manquant est l’accordéon lui-même, qui dépend de la présence du contrôle de `ScriptManager` ASP.NET :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Pour terminer l’exemple, les deux classes CSS référencées dans le contrôle accordéon fournissent des informations de style pour le navigateur :

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

[![les données de l’accordéon ont été ajoutées dynamiquement par le code côté serveur](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Les données de l’accordéon ont été ajoutées dynamiquement par le code côté serveur ([cliquez pour afficher l’image en taille réelle](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](databinding-to-an-accordion-vb.md)
