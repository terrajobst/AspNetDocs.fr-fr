---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Création de mises en page avec des pages maîtresC#d’affichage () | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, vous allez apprendre à créer une mise en page commune pour plusieurs pages de votre application en tirant parti des pages maîtres de vue. Vous pouvez utiliser une...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 026e3efb4ebf84016aa0f6a5fda4af549fdadfcb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594160"
---
# <a name="creating-page-layouts-with-view-master-pages-c"></a>Création de dispositions de page avec des pages maîtres de vue (C#)

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> Dans ce didacticiel, vous allez apprendre à créer une mise en page commune pour plusieurs pages de votre application en tirant parti des pages maîtres de vue. Vous pouvez utiliser une page maître d’affichage, par exemple, pour définir une mise en page à deux colonnes et utiliser la disposition à deux colonnes pour toutes les pages de votre application Web.

## <a name="creating-page-layouts-with-view-master-pages"></a>Création de mises en page avec des pages maîtres de vue

Dans ce didacticiel, vous allez apprendre à créer une mise en page commune pour plusieurs pages de votre application en tirant parti des pages maîtres de vue. Vous pouvez utiliser une page maître d’affichage, par exemple, pour définir une mise en page à deux colonnes et utiliser la disposition à deux colonnes pour toutes les pages de votre application Web.

Vous pouvez également tirer parti des pages maîtres de vue pour partager du contenu commun sur plusieurs pages de votre application. Par exemple, vous pouvez placer le logo de votre site Web, les liens de navigation et les bannières de bannière dans une page maître d’affichage. De cette façon, chaque page de votre application affichera automatiquement ce contenu.

Dans ce didacticiel, vous allez apprendre à créer une page maître de vue et à créer une nouvelle page afficher le contenu basée sur la page maître.

### <a name="creating-a-view-master-page"></a>Création d’une page maître d’affichage

Commençons par créer une page maître de vue qui définit une disposition à deux colonnes. Vous ajoutez une nouvelle page maître d’affichage à un projet MVC en cliquant avec le bouton droit sur le dossier Views\Shared, en sélectionnant l’option de menu **Ajouter, nouvel élément**, puis en sélectionnant le modèle **MVC afficher la page maître** (voir la figure 1).

[![ajout d’une page maître de vue](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Figure 01**: ajout d’une page de vue maître ([cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))

Vous pouvez créer plusieurs pages maîtres d’affichage dans une application. Chaque page maître d’affichage peut définir une mise en page différente. Par exemple, vous souhaiterez peut-être que certaines pages aient une disposition à deux colonnes et que d’autres pages aient une disposition à trois colonnes.

Une page maître de vue ressemble beaucoup à une vue ASP.NET MVC standard. Toutefois, contrairement à un affichage normal, une page maître de vue contient une ou plusieurs balises `<asp:ContentPlaceHolder>`. Les balises `<contentplaceholder>` sont utilisées pour marquer les zones de la page maître qui peuvent être remplacées dans une page de contenu individuelle.

Par exemple, la page maître d’affichage de la liste 1 définit une disposition à deux colonnes. Il contient deux balises `<contentplaceholder>`. Une `<ContentPlaceHolder>` pour chaque colonne.

**Liste 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Le corps de la page maître d’affichage dans la liste 1 contient deux balises `<div>` qui correspondent aux deux colonnes. La classe de colonne de feuille de style en cascade est appliquée aux deux balises `<div>`. Cette classe est définie dans la feuille de style déclarée en haut de la page maître. Vous pouvez afficher un aperçu de la façon dont la page maître de la vue sera affichée en basculant sur Mode Création. Cliquez sur l’onglet conception en bas à gauche de l’éditeur de code source (voir figure 2).

[![de l’aperçu d’une page maître dans le concepteur](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Figure 02**: aperçu d’une page maître dans le concepteur ([cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))

### <a name="creating-a-view-content-page"></a>Création d’une page afficher le contenu

Après avoir créé une page maître d’affichage, vous pouvez créer une ou plusieurs pages de contenu d’affichage basées sur la page maître d’affichage. Par exemple, vous pouvez créer une page de contenu de vue d’index pour le contrôleur de démarrage en cliquant avec le bouton droit sur le dossier Views\Home, en sélectionnant **Ajouter, nouvel élément**, en sélectionnant le modèle **MVC afficher la page de contenu** , en entrant le nom index. aspx, puis en cliquant sur le bouton **Ajouter** (voir figure 3).

[![de l’ajout d’une page afficher le contenu](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Figure 03**: ajout d’une page afficher le contenu ([cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))

Une fois que vous avez cliqué sur le bouton Ajouter, une nouvelle boîte de dialogue s’affiche pour vous permettre de sélectionner une page maître d’affichage à associer à la page afficher le contenu (voir figure 4). Vous pouvez accéder à la page maître site. Master de la vue que nous avons créée dans la section précédente.

[![de la sélection d’une page maître](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Figure 04**: sélection d’une page maître ([cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))

Une fois que vous avez créé une nouvelle page afficher le contenu basée sur la page maître site. Master, vous pouvez obtenir le fichier dans la liste 2.

**Liste 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Notez que cette vue contient une balise `<asp:Content>` qui correspond à chacune des balises `<asp:ContentPlaceHolder>` dans la page maître de la vue. Chaque balise `<asp:Content>` comprend un attribut ContentPlaceHolderID qui pointe vers le `<asp:ContentPlaceHolder>` particulier qu’elle remplace.

Notez, en outre, que la page d’affichage de contenu de la liste 2 ne contient aucune des balises HTML d’ouverture et de fermeture normales. Par exemple, il ne contient pas les balises d’ouverture et de fermeture `<html>` ou `<head>`. Toutes les balises d’ouverture et de fermeture normales sont contenues dans la page maître de la vue.

Tout contenu que vous souhaitez afficher dans une page de contenu de la vue doit être placé dans une balise de `<asp:Content>`. Si vous placez du code HTML ou tout autre contenu en dehors de ces balises, vous obtiendrez une erreur lorsque vous tenterez d’afficher la page.

Vous n’avez pas besoin de remplacer chaque balise `<asp:ContentPlaceHolder>` d’une page maître dans une page d’affichage de contenu. Il vous suffit de substituer une balise `<asp:ContentPlaceHolder>` lorsque vous souhaitez remplacer la balise par du contenu particulier.

Par exemple, la vue d’index modifiée dans la liste 3 contient uniquement deux balises `<asp:Content>`. Chaque balise `<asp:Content>` comprend du texte.

**Liste 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Lorsque la vue de la liste 3 est demandée, elle affiche la page dans la figure 5. Notez que la vue affiche une page avec deux colonnes. Notez, en outre, que le contenu de la page afficher le contenu est fusionné avec le contenu de la page maître de la vue

[![la page de contenu de la vue d’index](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Figure 05**: page de contenu de la vue[d’index (cliquez pour afficher l’image en taille réelle](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))

### <a name="modifying-view-master-page-content"></a>Modification du contenu de la page maître d’affichage

L’un des problèmes que vous rencontrez presque immédiatement lorsque vous travaillez avec des pages maîtres de vue est le problème de la modification du contenu de la page maître d’affichage lorsque différentes pages de contenu de vue sont demandées. Par exemple, vous souhaitez que chaque page de votre application Web ait un titre unique. Toutefois, le titre est déclaré dans la page maître d’affichage et non dans la page afficher le contenu. Comment personnaliser le titre de chaque page afficher le contenu ?

Vous pouvez modifier le titre affiché par une page afficher le contenu de deux manières. Tout d’abord, vous pouvez assigner un titre de page à l’attribut title de la directive `<%@ page %>` déclaré en haut d’une page afficher le contenu. Par exemple, si vous souhaitez affecter le titre de page « Super Great Web » à la vue index, vous pouvez inclure la directive suivante en haut de la vue index :

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Lorsque la vue index est rendue dans le navigateur, le titre souhaité s’affiche dans la barre de titre du navigateur :

[barre de titre du navigateur ![](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)

Une page de vue principale doit remplir une condition importante pour que l’attribut de titre fonctionne. La page maître de la vue doit contenir une balise `<head runat="server">` au lieu d’une balise de `<head>` normale pour son en-tête. Si la balise `<head>` n’inclut pas l’attribut runat = "Server", le titre n’apparaît pas. La page maître d’affichage par défaut comprend la balise `<head runat="server">` requise.

Une autre approche pour modifier le contenu d’une page maître à partir d’une page de contenu de vue individuelle consiste à encapsuler la région que vous souhaitez modifier dans une balise de `<asp:ContentPlaceHolder>`. Par exemple, imaginez que vous souhaitez modifier non seulement le titre, mais également les balises méta, rendues par une page de vue principale. La page mode maître de la liste 4 contient une balise `<asp:ContentPlaceHolder>` dans sa balise `<head>`.

**Liste 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Notez que la balise `<asp:ContentPlaceHolder>` dans la liste 4 comprend le contenu par défaut : un titre par défaut et des balises méta par défaut. Si vous ne remplacez pas cette balise `<asp:ContentPlaceHolder>` dans une page de contenu de vue individuelle, le contenu par défaut s’affiche.

La page affichage de contenu dans le Listing 5 remplace la balise `<asp:ContentPlaceHolder>` pour afficher un titre personnalisé et des balises méta personnalisées.

**Liste 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Récapitulatif

Ce didacticiel vous a fourni une présentation de base pour afficher les pages maîtres et afficher les pages de contenu. Vous avez appris à créer des pages maîtres de vue et à créer des pages de contenu d’affichage basées sur celles-ci. Nous avons également examiné comment vous pouvez modifier le contenu d’une page maître d’affichage à partir d’une page de contenu de vue particulière.

> [!div class="step-by-step"]
> [Précédent](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Suivant](passing-data-to-view-master-pages-cs.md)
