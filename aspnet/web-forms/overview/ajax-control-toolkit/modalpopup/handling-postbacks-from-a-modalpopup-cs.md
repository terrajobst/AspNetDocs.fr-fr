---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Gestion des publications (postback) àC#partir d’un ModalPopup () | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Une attention particulière doit être prise lorsqu’un POS...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599085"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a>Gestion des publications (postback) à partir d’un ModalPopup (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Une attention particulière doit être prise lorsqu’une publication (postback) est créée à partir de la fenêtre contextuelle.

## <a name="overview"></a>Vue d'ensemble de

Le contrôle ModalPopup dans la boîte à outils de contrôle AJAX offre un moyen simple de créer une fenêtre contextuelle modale à l’aide de moyens côté client. Une attention particulière doit être prise lorsqu’une publication (postback) est créée à partir de la fenêtre contextuelle.

## <a name="steps"></a>Étapes

Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Ensuite, ajoutez un panneau qui sert de fenêtre contextuelle modale. À partir de là, l’utilisateur peut entrer un nom et une adresse de messagerie. Un bouton est utilisé pour fermer la fenêtre contextuelle et enregistrer les informations. Notez que l’attribut `OnClick` est défini de telle sorte qu’une publication (postback) se produit lorsque vous cliquez sur ce bouton :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

La page elle-même se compose de deux étiquettes pour exactement les mêmes informations : nom et adresse de messagerie. Un bouton est utilisé pour déclencher la fenêtre contextuelle modale :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Pour que la fenêtre contextuelle s’affiche, ajoutez le contrôle `ModalPopupExtender`. Affectez à l’attribut `PopupControlID` l’ID du panneau et `TargetControlID` à l’ID du bouton :

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Désormais, chaque fois que le bouton `Save` dans la fenêtre contextuelle modale est cliqué, la méthode `SaveData()` côté serveur est exécutée. À partir de là, vous pouvez enregistrer les données entrées dans un magasin de données. Par souci de simplicité, les nouvelles données sont simplement sorties dans l’étiquette :

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

En outre, les contrôles TextBox dans la fenêtre contextuelle modale doivent être remplis avec le nom et l’adresse de messagerie actuels. Toutefois, cela n’est nécessaire que si aucune publication (postback) ne se produit. S’il existe une publication (postback), la fonctionnalité ASP.NET ViewState remplit automatiquement les zones de texte avec les valeurs appropriées.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

[![la fenêtre contextuelle modale entraîne une publication (postback)](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

La fenêtre contextuelle modale entraîne une publication ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-modalpopup-with-a-repeater-control-cs.md)
> [Suivant](positioning-a-modalpopup-cs.md)
