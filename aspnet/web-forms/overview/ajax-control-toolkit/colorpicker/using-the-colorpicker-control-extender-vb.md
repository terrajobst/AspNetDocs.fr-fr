---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Utilisation de l’extendeur de contrôle ColorPicker (VB) | Microsoft Docs
author: microsoft
description: ColorPicker est un extendeur AJAX ASP.NET qui fournit des fonctionnalités de sélection de couleurs côté client avec l’interface utilisateur dans un contrôle Popup. Il peut être attaché à n’importe quel ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 77e2e3bc61a5e1498570959ca40acff83dc3fc82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554501"
---
# <a name="using-the-colorpicker-control-extender-vb"></a>Utilisation de l’extendeur de contrôle ColorPicker (VB)

par [Microsoft](https://github.com/microsoft)

> ColorPicker est un extendeur AJAX ASP.NET qui fournit des fonctionnalités de sélection de couleurs côté client avec l’interface utilisateur dans un contrôle Popup. Il peut être attaché à n’importe quel contrôle de zone de texte ASP.NET. Tel.

L’objectif de ce didacticiel est d’expliquer comment vous pouvez utiliser l’extendeur de contrôle ColorPicker d’un kit d’outils de contrôle AJAX. L’extendeur de contrôle ColorPicker affiche une boîte de dialogue contextuelle qui vous permet de sélectionner une couleur. Le composant ColorPicker est utile lorsque vous souhaitez fournir une interface utilisateur intuitive permettant à un utilisateur de choisir une couleur.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Extension d’un contrôle TextBox avec l’extendeur de contrôle ColorPicker

Imaginez, par exemple, que vous souhaitiez créer un site Web qui permet aux visiteurs de créer des cartes de visite personnalisées. Les visiteurs peuvent saisir le texte d’une carte de visite et choisir la couleur. La page ASP.NET de la liste 1 contient deux contrôles TextBox nommés txtCardText et txtCardColor. Lorsque vous envoyez le formulaire, les valeurs sélectionnées sont affichées (voir la figure 1).

[formulaire ![simple pour la création d’une carte de visite](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Figure 01**: formulaire simple pour la création d’une carte de visite ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-vb/_static/image2.png))

**Liste 1-CreateCard. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

Le formulaire de la liste 1 fonctionne, mais il ne fournit pas d’expérience utilisateur exceptionnelle. L’utilisateur doit taper une couleur dans la zone de texte. Si l’utilisateur souhaite une couleur spécialisée, par exemple, juste l’ombre droite de PEA vert, alors l’utilisateur doit déterminer le code de couleur HTML sans aucune aide.

Vous pouvez utiliser l’extendeur de contrôle ColorPicker pour améliorer l’expérience utilisateur. Le composant ColorPicker affiche une boîte de dialogue de couleur lorsque vous déplacez le focus sur un contrôle TextBox (voir figure 2).

[![l’extendeur de contrôle ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Figure 02**: extendeur de contrôle ColorPicker ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-vb/_static/image4.png))

Vous devez effectuer deux étapes pour utiliser l’extendeur de contrôle ColorPicker avec le formulaire dans la liste 1 :

1. Ajouter un contrôle ScriptManager à la page
2. Ajouter l’extendeur de contrôle ColorPicker à la page

Avant de pouvoir utiliser le composant ColorPicker, vous devez ajouter un ScriptManager à votre page. Un bon emplacement pour ajouter le ScriptManager se trouve juste en dessous de la balise d'&gt; de &lt;de formulaire d’ouverture. Vous pouvez faire glisser le ScriptManager sur la page à partir de la boîte à outils (le ScriptManager se trouve sous l’onglet Extensions AJAX). Vous pouvez également taper la balise suivante en mode source sous la balise de formulaire d’ouverture côté serveur :

&lt;asp : ScriptManager ID = "ScriptManager1" runat = "Server"/&gt;

Le moyen le plus simple d’ajouter l’extendeur de contrôle ColorPicker à la page est en mode Design. Si vous pointez le curseur de la souris sur la zone de texte txtCardColor, une option de tâche intelligente s’affiche pour vous permettre d’ajouter un extendeur (voir figure 3). Si vous choisissez cette option, l’Assistant extendeur s’affiche (voir figure 4).

[![ajout d’un extendeur](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Figure 03**: ajout d’un extendeur ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-vb/_static/image6.png))

[![sélection d’un extendeur de contrôle à l’aide de l’Assistant extendeur](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Figure 04**: sélection d’un extendeur de contrôle à l’aide de l’Assistant extendeur ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-vb/_static/image8.png))

Vous pouvez choisir l’extendeur ColorPicker pour étendre la zone de texte txtCardColor avec l’extendeur ColorPicker. Cliquez sur OK pour fermer la boîte de dialogue.

Une fois ces modifications effectuées, la source de la page ressemble à la liste 2.

**Liste 2-CreateCard. aspx (avec ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Notez que la page contient maintenant un contrôle ColorPickerExtender qui apparaît directement sous le contrôle TextBox txtCardColor. Le contrôle ColorPickerExtender étend le contrôle txtCardColor afin qu’il affiche une boîte de dialogue de sélecteur de couleurs.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Utilisation d’un bouton pour lancer la boîte de dialogue Sélecteur de couleurs

L’extendeur ColorPicker prend en charge les propriétés suivantes :

- PopupButtonId : ID d’un bouton sur la page qui provoque l’affichage de la boîte de dialogue du sélecteur de couleurs.
- PopupPosition : position, par rapport au contrôle cible, de la boîte de dialogue Sélecteur de couleurs. Les valeurs possibles sont Absolute, Center, BottomLeft, BottomRight, Left, seright, Right et Left (la valeur par défaut est BottomLeft).
- SampleControlId : ID d’un contrôle qui affiche la couleur sélectionnée.
- SelectedColor : couleur initiale sélectionnée par le composant ColorPicker.

Vous pouvez utiliser ces propriétés pour personnaliser l’affichage de la boîte de dialogue Sélecteur de couleurs et l’affichage de la couleur sélectionnée. La page de la liste 3 montre comment vous pouvez utiliser plusieurs de ces propriétés.

**Liste 3-CreateCardButton. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

La page de la liste 3 comprend un bouton choisir une couleur (voir figure 5). Lorsque vous cliquez sur ce bouton, la boîte de dialogue Sélecteur de couleurs s’affiche au-dessus de la zone de texte. Si vous sélectionnez une couleur dans la boîte de dialogue, la couleur sélectionnée apparaît comme couleur d’arrière-plan du contrôle Label lblSample.

La propriété ColorPicker PopupButtonID est utilisée pour associer le bouton choisir la couleur à l’extendeur ColorPicker. Lorsque vous fournissez une valeur pour la propriété PopupButtonID, la boîte de dialogue Sélecteur de couleurs ne s’affiche plus lorsque le contrôle cible a le focus. Vous devez cliquer sur le bouton pour afficher la boîte de dialogue.

La propriété SampleControlID est utilisée pour associer un contrôle qui affiche la couleur sélectionnée au composant ColorPicker. Le composant ColorPicker change la couleur d’arrière-plan de ce contrôle en couleur actuellement sélectionnée.

[![affichage de la boîte de dialogue Sélecteur de couleurs avec un bouton](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Figure 05**: affichage de la boîte de dialogue Sélecteur de couleurs avec un bouton ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-vb/_static/image10.png))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à utiliser l’extendeur de contrôle ColorPicker pour afficher une boîte de dialogue de sélecteur de couleurs contextuelle. Tout d’abord, nous avons examiné comment vous pouvez afficher la boîte de dialogue lorsque le focus est déplacé vers un contrôle TextBox. Ensuite, vous avez appris à créer un bouton qui affiche la boîte de dialogue Sélecteur de couleurs lorsque l’utilisateur clique sur le bouton.

> [!div class="step-by-step"]
> [Précédent](using-the-colorpicker-control-extender-cs.md)
