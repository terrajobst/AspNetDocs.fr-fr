---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Utilisation de la classe TagBuilder pour générer des applications auxiliaires HTML (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther présente une classe utilitaire utile dans l’infrastructure MVC ASP.NET, nommée la classe TagBuilder. Vous pouvez utiliser la classe TagBuilder pour facilement...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: c8eaea9932a30c744b9a69861619ce9458b5a23a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600029"
---
# <a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>Utilisation de la classe TagBuilder pour générer des applications auxiliaires HTML (C#)

par [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther présente une classe utilitaire utile dans l’infrastructure MVC ASP.NET, nommée la classe TagBuilder. Vous pouvez utiliser la classe TagBuilder pour créer facilement des balises HTML.

L’infrastructure MVC ASP.NET comprend une classe utilitaire utile nommée la classe TagBuilder que vous pouvez utiliser lors de la création d’applications auxiliaires HTML. La classe TagBuilder, comme le nom de la classe suggère, vous permet de créer facilement des balises HTML. Dans ce bref didacticiel, vous disposez d’une vue d’ensemble de la classe TagBuilder et vous apprenez à utiliser cette classe lors de la création d’une application auxiliaire HTML simple qui restitue les balises HTML &lt;img&gt;.

## <a name="overview-of-the-tagbuilder-class"></a>Vue d’ensemble de la classe TagBuilder

La classe TagBuilder est contenue dans l’espace de noms System. Web. Mvc. Il a cinq méthodes :

- AddCssClass ()-vous permet d’ajouter un nouvel attribut *Class = ""* à une balise.
- GenerateId ()-vous permet d’ajouter un attribut ID à une balise. Cette méthode remplace automatiquement les points de l’ID (par défaut, les points sont remplacés par des traits de soulignement).
- MergeAttribute ()-vous permet d’ajouter des attributs à une balise. Il existe plusieurs surcharges de cette méthode.
- SetInnerText () : vous permet de définir le texte interne de la balise. Le texte interne est automatiquement encodé en HTML.
- ToString () : vous permet de restituer la balise. Vous pouvez spécifier si vous souhaitez créer une balise normale, une balise de début, une balise de fin ou une balise de fermeture automatique.

La classe TagBuilder a quatre propriétés importantes :

- Attributes : représente tous les attributs de la balise.
- IdAttributeDotReplacement : représente le caractère utilisé par la méthode GenerateId () pour remplacer des points (la valeur par défaut est un trait de soulignement).
- InnerHTML : représente le contenu interne de la balise. L’assignation d’une chaîne à cette propriété *n’encode pas* le code HTML de la chaîne.
- TagName-représente le nom de la balise.

Ces méthodes et propriétés vous donnent toutes les méthodes et propriétés de base dont vous avez besoin pour créer une balise HTML. Vous n’avez pas vraiment besoin d’utiliser la classe TagBuilder. Vous pouvez utiliser une classe StringBuilder à la place. Toutefois, la classe TagBuilder facilite votre vie.

## <a name="creating-an-image-html-helper"></a>Création d’une image du programme d’assistance HTML

Lorsque vous créez une instance de la classe TagBuilder, vous transmettez le nom de la balise que vous souhaitez générer au constructeur TagBuilder. Ensuite, vous pouvez appeler des méthodes telles que les méthodes AddCssClass et MergeAttribute () pour modifier les attributs de la balise. Enfin, vous appelez la méthode ToString () pour afficher la balise.

Par exemple, la liste 1 contient une image Helper HTML. L’application Helper d’image est implémentée en interne avec un TagBuilder qui représente une balise HTML &lt;img&gt;.

**Liste 1-Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

La classe de la liste 1 contient deux méthodes surchargées statiques nommées image. Quand vous appelez la méthode image (), vous pouvez passer un objet qui représente un jeu d’attributs HTML ou non.

Notez que la méthode TagBuilder. MergeAttribute () est utilisée pour ajouter des attributs individuels tels que l’attribut SRC à TagBuilder. Notez, en outre, comment la méthode TagBuilder. MergeAttributes () est utilisée pour ajouter une collection d’attributs à TagBuilder. La méthode MergeAttributes () accepte un dictionnaire&lt;chaîne, objet&gt; paramètre. La classe RouteValueDictionary est utilisée pour convertir l’objet représentant la collection d’attributs dans un dictionnaire&lt;chaîne, objet&gt;.

Après avoir créé le programme d’assistance d’image, vous pouvez utiliser le programme d’assistance dans vos vues MVC ASP.NET comme n’importe quel autre programme d’assistance HTML standard. La vue de la liste 2 utilise le programme d’assistance d’image pour afficher la même image d’une Xbox à deux reprises (voir figure 1). L’assistance image () est appelée avec et sans collection d’attributs HTML.

**Liste 2-Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]

[![la boîte de dialogue Nouveau projet](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**Figure 01**: utilisation du programme d’assistance d’image ([cliquez pour afficher l’image en taille réelle](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))

Notez que vous devez importer l’espace de noms associé à l’application auxiliaire d’image en haut de la vue index. aspx. L’application auxiliaire est importée avec la directive suivante :

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [Précédent](creating-custom-html-helpers-cs.md)
> [Suivant](creating-page-layouts-with-view-master-pages-cs.md)
