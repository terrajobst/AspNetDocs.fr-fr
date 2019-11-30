---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Création d’applications auxiliaires HTMLC#personnalisées () | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel est de vous montrer comment créer des applications auxiliaires HTML personnalisées que vous pouvez utiliser dans vos vues MVC. En tirant parti du programme d’assistance HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 264ff9850bad397826b45649d52fbfefafc53a01
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594529"
---
# <a name="creating-custom-html-helpers-c"></a>Création de helpers HTML personnalisés (C#)

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> L’objectif de ce didacticiel est de vous montrer comment créer des applications auxiliaires HTML personnalisées que vous pouvez utiliser dans vos vues MVC. En tirant parti des applications auxiliaires HTML, vous pouvez réduire la quantité de frappe fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.

L’objectif de ce didacticiel est de vous montrer comment créer des applications auxiliaires HTML personnalisées que vous pouvez utiliser dans vos vues MVC. En tirant parti des applications auxiliaires HTML, vous pouvez réduire la quantité de frappe fastidieuse des balises HTML que vous devez effectuer pour créer une page HTML standard.

Dans la première partie de ce didacticiel, je décrirai certaines des applications auxiliaires HTML existantes incluses dans l’infrastructure MVC ASP.NET. Je décrirai ensuite deux méthodes pour créer des applications auxiliaires HTML personnalisées : j’explique comment créer des applications auxiliaires HTML personnalisées en créant une méthode statique et en créant une méthode d’extension.

## <a name="understanding-html-helpers"></a>Fonctionnement des applications auxiliaires HTML

Une application auxiliaire HTML est simplement une méthode qui retourne une chaîne. La chaîne peut représenter n’importe quel type de contenu souhaité. Par exemple, vous pouvez utiliser des applications auxiliaires HTML pour restituer des balises HTML standard, telles que des balises HTML `<input>` et `<img>`. Vous pouvez également utiliser des applications auxiliaires HTML pour restituer un contenu plus complexe, tel qu’une bande d’onglets ou une table HTML de données de base de données.

L’infrastructure MVC ASP.NET comprend l’ensemble suivant de programme d’assistance HTML standard (il ne s’agit pas d’une liste complète) :

- Html. ActionLink ()
- Html. BeginForm ()
- Html. CheckBox ()
- Html. DropDownList ()
- Html. EndForm ()
- Html. Hidden ()
- Html. ListBox ()
- Html. mot de passe ()
- Html. RadioButton ()
- Html. TextArea ()
- Html. TextBox ()

Par exemple, prenons le formulaire de la liste 1. Ce formulaire est rendu à l’aide de deux des applications auxiliaires HTML standard (voir la figure 1). Ce formulaire utilise les méthodes d’assistance `Html.BeginForm()` et `Html.TextBox()` pour restituer un formulaire HTML simple.

[Page ![affichée avec les applications auxiliaires HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figure 01**: page affichée avec les applications auxiliaires html ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image3.png))

**Liste 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

La méthode d’assistance HTML. BeginForm () est utilisée pour créer les balises d’ouverture et de fermeture de `<form>` HTML. Notez que la méthode `Html.BeginForm()` est appelée dans une instruction using. L’instruction Using garantit que la balise `<form>` est fermée à la fin du bloc using.

Si vous préférez, au lieu de créer un bloc using, vous pouvez appeler la méthode d’assistance HTML. EndForm () pour fermer la balise `<form>`. Utilisez l’approche la plus simple pour créer une balise d’ouverture et de fermeture `<form>` qui semble la plus intuitive pour vous.

Les méthodes d’assistance `Html.TextBox()` sont utilisées dans la liste 1 pour afficher les balises de `<input>` HTML. Si vous sélectionnez Afficher la source dans votre navigateur, vous voyez la source HTML dans le Listing 2. Notez que la source contient des balises HTML standard.

> [!IMPORTANT]
> Notez que le programme d’assistance `Html.TextBox()`-HTML est rendu avec des balises `<%= %>` au lieu de balises `<% %>`. Si vous n’incluez pas le signe égal, rien n’est restitué dans le navigateur.

L’infrastructure MVC ASP.NET contient un petit ensemble d’applications d’assistance. Il est très probable que vous deviez étendre l’infrastructure MVC avec des applications auxiliaires HTML personnalisées. Dans le reste de ce didacticiel, vous apprendrez deux méthodes pour créer des applications auxiliaires HTML personnalisées.

**Liste 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Création d’applications auxiliaires HTML avec des méthodes statiques

Le moyen le plus simple de créer une nouvelle application d’assistance HTML consiste à créer une méthode statique qui retourne une chaîne. Imaginez, par exemple, que vous décidiez de créer un programme d’assistance HTML qui restitue une balise de `<label>` HTML. Vous pouvez utiliser la classe de la liste 2 pour afficher un `<label>`.

**Liste 2 – `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Il n’y a rien de spécial concernant la classe dans la liste 2. La méthode `Label()` retourne simplement une chaîne.

La vue d’index modifiée dans la liste 3 utilise la `LabelHelper` pour afficher les balises de `<label>` HTML. Notez que la vue comprend une directive `<%@ imports %>` qui importe l’espace de noms `Application1.Helpers`.

**Liste 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Création d’applications auxiliaires HTML à l’aide de méthodes d’extension

Si vous souhaitez créer des applications auxiliaires HTML qui fonctionnent comme les applications auxiliaires HTML standard incluses dans l’infrastructure MVC ASP.NET, vous devez créer des méthodes d’extension. Les méthodes d’extension vous permettent d’ajouter de nouvelles méthodes à une classe existante. Lorsque vous créez une méthode d’assistance HTML, vous ajoutez de nouvelles méthodes à la classe HtmlHelper représentée par la propriété HTML d’une vue.

La classe de la liste 3 ajoute une méthode d’extension à la classe `HtmlHelper` nommée `Label()`. Il y a deux choses que vous devez remarquer à propos de cette classe. Tout d’abord, Notez que la classe est une classe statique. Vous devez définir une méthode d’extension avec une classe statique.

Deuxièmement, Notez que le premier paramètre de la méthode `Label()` est précédé du mot clé `this`. Le premier paramètre d’une méthode d’extension indique la classe que la méthode d’extension étend.

**Liste 3 – `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Après avoir créé une méthode d’extension et correctement généré votre application, la méthode d’extension apparaît dans Visual Studio IntelliSense, comme toutes les autres méthodes d’une classe (voir la figure 2). La seule différence est que les méthodes d’extension s’affichent avec un symbole spécial en regard de celles-ci (une icône représentant une flèche vers le bas).

[![à l’aide de la méthode d’extension html. Label ()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figure 02**: utilisation de la méthode d’extension html. Label () ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image6.png))

La vue d’index modifiée de la liste 4 utilise la méthode d’extension html. Label () pour afficher toutes ses balises de `<label>`.

**Liste 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris deux méthodes pour créer des applications auxiliaires HTML personnalisées. Tout d’abord, vous avez appris à créer un programme d’assistance HTML `Label()` personnalisé en créant une méthode statique qui retourne une chaîne. Ensuite, vous avez appris à créer une méthode d’assistance HTML `Label()` personnalisée en créant une méthode d’extension sur la classe `HtmlHelper`.

Dans ce didacticiel, je me suis concentré sur la création d’une méthode d’assistance HTML extrêmement simple. Sachez qu’une application auxiliaire HTML peut être aussi compliquée que vous le souhaitez. Vous pouvez générer des applications auxiliaires HTML qui restituent du contenu riche, comme des arborescences, des menus ou des tables de données de base de données.

> [!div class="step-by-step"]
> [Précédent](asp-net-mvc-views-overview-cs.md)
> [Suivant](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
