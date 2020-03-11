---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Utilisation des contrôles et des extendeurs de contrôle AJAXC#Control Toolkit () | Microsoft Docs
author: microsoft
description: Découvrez comment ajouter des contrôles et des extendeurs AJAX Control Toolkit à vos pages ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 1acafaadaf373b488b9e85b7ba31f08cd3b53e85
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535412"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>Utilisation de contrôles AJAX Control Toolkit et extendeurs de contrôle (C#)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment ajouter des contrôles et des extendeurs AJAX Control Toolkit à vos pages ASP.NET.

Le kit d’outils de contrôle AJAX contient un ensemble de contrôles et d’extendeurs de contrôle. Dans ce bref didacticiel, vous allez apprendre à ajouter des contrôles et des extendeurs de contrôle à une page ASP.NET.

> [!NOTE] 
> 
> Pour obtenir des instructions sur l’installation d’AJAX Control Toolkit et l’ajout d’AJAX Control Toolkit à la boîte à outils Visual Studio/Visual Web Developer, consultez le didacticiel [prise en main de la](get-started-with-the-ajax-control-toolkit-cs.md)boîte à outils de contrôle AJAX.

## <a name="using-ajax-control-toolkit-controls"></a>Utilisation des contrôles d’outils de contrôle AJAX

Un contrôle de la boîte à outils de contrôle AJAX fonctionne comme un contrôle ASP.NET normal. Vous pouvez faire glisser le contrôle de la boîte à outils vers une page ASP.NET. Vous pouvez ajouter le contrôle à la page en Mode Création ou en mode Source.

Il existe une exigence spéciale lors de l’utilisation des contrôles à partir de la boîte à outils de contrôle AJAX. La page doit contenir un contrôle ScriptManager. Le contrôle ScriptManager est chargé d’inclure tous les scripts JavaScript nécessaires pour les contrôles du kit d’outils de contrôle AJAX.

Par exemple, l’onglet AJAX Control Toolkit comprend un contrôle nommé le contrôle Editor. Ce contrôle affiche un éditeur HTML enrichi. Pour ajouter le contrôle Editor à une page, procédez comme suit :

1. Créer une nouvelle page ASP.NET nommée ShowEditor. aspx
2. Sélectionnez le contrôle ScriptManager sous l’onglet Extensions AJAX de la boîte à outils et faites glisser le contrôle sur la page.
3. Sélectionnez le contrôle de l’éditeur sous l’onglet AJAX Control Toolkit de la boîte à outils, puis faites glisser le contrôle sur la page (voir figure 1). Le concepteur doit ressembler à la figure 2.
4. Exécutez le site Web en sélectionnant l’option de menu **Déboguer, démarrer le débogage ou en** appuyant sur la touche F5.
5. La page de la figure 3 doit s’afficher.

[![sélection du contrôle de l’éditeur HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Figure 01**: sélection du contrôle de l’éditeur HTML ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))

[![le concepteur Visual Studio avec ScriptManager et le contrôle d’édition](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Figure 02**: concepteur Visual Studio avec ScriptManager et le contrôle d’édition ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))

[![la page DisplayEditor. aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Figure 03**: page DisplayEditor. aspx ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))

## <a name="using-ajax-control-toolkit-control-extenders"></a>Utilisation d’extendeurs de contrôle du kit d’outils de contrôle AJAX

La boîte à outils de contrôle AJAX contient également des extendeurs de contrôle. Comme son nom l’indique, un extendeur de contrôle étend les fonctionnalités d’un contrôle existant. Par exemple, l’extendeur de contrôle ConfirmButton étend le contrôle de bouton ASP.NET standard. L’extendeur modifie le comportement des contrôles de bouton afin que le bouton affiche une boîte de dialogue de confirmation lorsque vous cliquez dessus.

Un extendeur de contrôle, tout comme un contrôle de boîte à outils de contrôle AJAX, requiert un contrôle ScriptManager. Vous devez ajouter un contrôle ScriptManager à une page avant de commencer à utiliser les extendeurs de contrôle dans la page.

Procédez comme suit pour utiliser l’extendeur de contrôle ConfirmButton :

1. Créer une nouvelle page ASP.NET nommée ShowConfirmButton. aspx
2. Ajoutez un contrôle ScriptManager à la page en faisant glisser le contrôle sur la page à partir de sous l’onglet Extensions AJAX.
3. Ajoutez un contrôle bouton standard à la page en faisant glisser le bouton situé sous l’onglet standard de la boîte à outils vers l’aire du concepteur.
4. Cliquez sur l’option Ajouter une tâche d' **extendeur** (voir figure 4).
5. Dans la boîte de dialogue Choisir un extendeur, sélectionnez ConfirmButtonExtender (voir la figure 5), puis cliquez sur le bouton OK.
6. Sélectionnez le contrôle Button dans le concepteur et développez le nœud extendeurs, button1\_ConfirmButtonExtender dans le Fenêtre Propriétés (voir figure 6). Affectez *vraiment* la valeur à la propriété ConfirmText.
7. Exécutez la page en sélectionnant l’option de menu **Déboguer, démarrer le débogage** ou appuyez sur la touche F5.

[![l’option Ajouter une tâche d’extendeur](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Figure 04**: option de tâche Ajouter un extendeur ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))

[![sélection de l’extendeur de contrôle ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Figure 05**: sélection de l’extendeur de contrôle ConfirmButton ([Cliquer pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))

[![définition d’une propriété ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Figure 06**: définition d’une propriété ConfirmButton ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))

Lorsque la page s’ouvre, vous devez voir un bouton. Lorsque vous cliquez sur le bouton, vous recevez la boîte de dialogue de confirmation de la figure 7.

[![affichage de la boîte de dialogue de confirmation](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Figure 07**: affichage de la boîte de dialogue de confirmation ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))

Notez que vous ne faites pas normalement glisser un extendeur de contrôle sur une page. Au lieu de cela, vous utilisez l’option de tâche **Ajouter un extendeur** pour ajouter un extendeur à un contrôle que vous avez déjà ajouté à une page. Notez, en outre, que vous définissez les propriétés d’extendeur de contrôle en ouvrant la feuille de propriétés du contrôle qui est étendu.

Un seul contrôle ASP.NET peut être étendu par plusieurs extendeurs de contrôle. La feuille de propriétés du contrôle qui est étendu répertorie tous les extendeurs de contrôle associés au contrôle.

> [!div class="step-by-step"]
> [Précédent](get-started-with-the-ajax-control-toolkit-cs.md)
> [Suivant](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
