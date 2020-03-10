---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Prise en main de la boîte à outils de contrôle AJAX (VB) | Microsoft Docs
author: microsoft
description: Découvrez tout ce que vous devez savoir pour commencer à utiliser le kit d’outils de contrôle AJAX.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: ff614308555b31710b11f408e12e9a6fadbf98d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613483"
---
# <a name="get-started-with-the-ajax-control-toolkit-vb"></a>Bien démarrer avec AJAX Control Toolkit (VB)

par [Microsoft](https://github.com/microsoft)

> Découvrez tout ce que vous devez savoir pour commencer à utiliser le kit d’outils de contrôle AJAX.

Le kit d’outils de contrôle AJAX contient plus de 30 contrôles gratuits que vous pouvez utiliser dans vos applications ASP.NET. Dans ce didacticiel, vous allez apprendre à télécharger AJAX Control Toolkit et à ajouter les contrôles de boîte à outils à votre boîte à outils Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Téléchargement de la boîte à outils de contrôle AJAX

[AJAX Control Toolkit](http://devexpress.com/act) est un projet open source développé par les membres de la communauté ASP.net et de l’équipe ASP.net.

[![téléchargement de la boîte à outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Figure 01**: Téléchargement de la boîte à outils de contrôle AJAX ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))

Après avoir téléchargé le fichier, vous devez débloquer le fichier. Cliquez avec le bouton droit sur le fichier, sélectionnez Propriétés, puis cliquez sur le bouton **débloquer** (voir figure 2).

[![du déblocage du fichier ZIP du kit de commandes AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Figure 02**: déblocage du fichier zip du kit de commandes Ajax ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))

Une fois le fichier débloqué, vous pouvez décompresser le fichier : cliquez avec le bouton droit sur le fichier et sélectionnez l’option de menu **extraire tout** . Nous sommes maintenant prêts à ajouter le Toolkit à la boîte à outils Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Ajout de la boîte à outils de contrôle AJAX à la boîte à outils

Le moyen le plus simple d’utiliser le kit d’outils de contrôle AJAX consiste à ajouter le Toolkit à votre boîte à outils Visual Studio/Visual Web Developer (voir figure 3). De cette façon, vous pouvez simplement faire glisser un contrôle Toolkit sur une page lorsque vous souhaitez l’utiliser.

[![boîte à outils de contrôle AJAX s’affiche dans la boîte à outils](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Figure 03**: la boîte à outils de contrôle AJAX s’affiche dans la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))

Tout d’abord, vous devez ajouter un onglet AJAX Control Toolkit à la boîte à outils. Effectuez les opérations suivantes.

1. Créez un site Web ASP.NET en sélectionnant l’option de menu fichier, nouveau site Web. Double-cliquez sur le fichier default. aspx dans la fenêtre Explorateur de solutions pour ouvrir le fichier dans l’éditeur.
2. Cliquez avec le bouton droit sur la boîte à outils sous l’onglet général, puis sélectionnez l’option de menu **Ajouter un onglet** (voir figure 4).
3. Entrez un nouvel onglet nommé AJAX Control Toolkit.

[![ajout d’un nouvel onglet](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Figure 04**: ajout d’un nouvel onglet ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))

Ensuite, vous devez ajouter les contrôles de la boîte à outils du contrôle AJAX au nouvel onglet. procédez comme suit :

- Cliquez avec le bouton droit sous l’onglet AJAX Control Toolkit et sélectionnez l’option de menu **choisir des éléments (voir figure 5)** .
- Accédez à l’emplacement où vous avez décompressé la boîte à outils de contrôle AJAX et sélectionnez l’assembly AjaxControlToolkit. dll.

[![choisir les éléments à ajouter à la boîte à outils](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Figure 05**: sélectionner les éléments à ajouter à la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))

Une fois ces étapes terminées, tous les contrôles du kit d’outils s’affichent dans votre boîte à outils.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Mise à niveau vers une nouvelle version de la boîte à outils

Si vous utilisiez une version antérieure de la boîte à outils et que vous devez maintenant passer à une version ultérieure, Voici les étapes recommandées :

- Fichiers binaires : supprimez l’ancienne version de l’assembly AjaxControlToolkit. dll dans le dossier bin de votre site Web.
- Éléments de boîte à outils-supprimez l’onglet AJAX Control Toolkit et suivez les étapes ci-dessus pour recréer l’onglet avec la nouvelle version de l’assembly AjaxControlToolkit. dll.

> [!div class="step-by-step"]
> [Précédent](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Suivant](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
