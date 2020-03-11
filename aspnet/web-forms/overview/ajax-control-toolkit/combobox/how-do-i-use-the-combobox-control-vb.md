---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Comment faire utiliser le contrôle ComboBox ? (VB) | Microsoft Docs
author: microsoft
description: ComboBox est un contrôle ASP.NET AJAX qui combine la flexibilité d’une zone de texte avec une liste d’options à partir desquelles les utilisateurs peuvent choisir.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 468063a72253cce55a02bfaef1219bff03d06418
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554487"
---
# <a name="how-do-i-use-the-combobox-control-vb"></a>Comment faire utiliser le contrôle ComboBox ? (VB)

par [Microsoft](https://github.com/microsoft)

> ComboBox est un contrôle ASP.NET AJAX qui combine la flexibilité d’une zone de texte avec une liste d’options à partir desquelles les utilisateurs peuvent choisir.

L’objectif de ce didacticiel est d’expliquer le contrôle de liste déroulante de la boîte à outils de contrôle AJAX. La zone de liste déroulante fonctionne comme une combinaison entre un contrôle ASP.NET DropDownList standard et un contrôle TextBox. Vous pouvez sélectionner une liste d’éléments préexistante ou entrer un nouvel élément.

La zone de liste déroulante est semblable à l’extendeur de contrôle de saisie semi-automatique, mais les contrôles sont utilisés dans différents scénarios. L’extendeur AutoComplete interroge un service Web pour obtenir les entrées correspondantes. Le contrôle ComboBox, en revanche, est initialisé avec un ensemble d’éléments. L’utilisation de l’extendeur AutoComplete est utile lorsque vous travaillez avec un grand ensemble de données (des millions de pièces de voiture) tout en utilisant le contrôle de liste déroulante est logique lorsque vous travaillez avec un petit ensemble de données (des douzaines de pièces de voiture).

## <a name="selecting-from-a-static-list-of-items"></a>Sélection à partir d’une liste statique d’éléments

Commençons par un exemple simple d’utilisation du contrôle ComboBox. Imaginez que vous souhaitez afficher une liste statique d’éléments dans une liste déroulante. Toutefois, vous souhaitez conserver la possibilité que la liste ne soit pas complète. Vous souhaitez autoriser un utilisateur à entrer une valeur personnalisée dans la liste.

Nous allons créer une page de Web Forms ASP.NET et utiliser le contrôle ComboBox dans la page. Ajoutez la nouvelle page ASP.NET à votre projet et basculez vers Mode Création.

Si vous souhaitez utiliser le contrôle ComboBox dans la page, vous devez ajouter un contrôle ScriptManager à la page. Faites glisser le contrôle ScriptManager situé sous l’onglet Extensions AJAX sur l’aire du concepteur. Vous devez ajouter le contrôle ScriptManager en haut de la page ; vous pouvez l’ajouter immédiatement en dessous de la balise de&gt; &lt;de l’ouverture côté serveur.

Ensuite, faites glisser le contrôle ComboBox sur la page. Vous pouvez trouver le contrôle ComboBox dans la boîte à outils avec les autres contrôles et extendeurs de contrôle de l’outil AJAX Control Toolkit (voir figure 1).

[formulaire ![simple pour la création d’une carte de visite](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Figure 01**: sélection du contrôle ComboBox à partir de la boîte à outils ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image2.png))

Nous allons utiliser le contrôle ComboBox pour afficher une liste statique de choix. L’utilisateur peut sélectionner un niveau particulier de spiciness pour ses aliments dans une liste de trois choix : modéré, moyen et chaud (voir figure 2).

[![sélection à partir d’une liste statique d’éléments](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Figure 02**: sélection à partir d’une liste statique d’éléments ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image4.png))

Il existe deux façons d’ajouter ces choix au contrôle ComboBox. Tout d’abord, vous sélectionnez l’option de tâche modifier les options lorsque vous pointez votre souris sur le contrôle dans Mode Création et que vous ouvrez l’éditeur d’élément (voir figure 3).

[![modifier des éléments de liste déroulante](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Figure 03**: modification des éléments de ComboBox ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image6.png))

La deuxième option consiste à ajouter la liste d’éléments entre les balises d’ouverture et de fermeture &lt;asp : ComboBox&gt; en mode Source. La page de la liste 1 contient la liste déroulante mise à jour qui contient la liste des éléments.

**Liste 1-static. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Lorsque vous ouvrez la page dans le Listing 1, vous pouvez sélectionner l’une des options préexistantes dans la liste déroulante. En d’autres termes, la zone de liste déroulante fonctionne comme un contrôle DropDownList.

Toutefois, vous avez également la possibilité d’entrer un nouveau choix (par exemple, Super corsée) qui ne figure pas dans la liste existante. Par conséquent, la zone de liste déroulante fonctionne également comme un contrôle TextBox.

Que vous ayez choisi un élément préexistant ou que vous entriez un élément personnalisé, lorsque vous envoyez le formulaire, votre choix s’affiche dans le contrôle étiquette. Lorsque vous envoyez le formulaire, le gestionnaire de btnSubmit\_Click s’exécute et met à jour l’étiquette (voir figure 4).

[![de l’affichage de l’élément sélectionné](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Figure 04**: affichage de l’élément sélectionné ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image8.png))

La zone de liste déroulante prend en charge les mêmes propriétés que le contrôle DropDownList pour récupérer l’élément sélectionné après l’envoi d’un formulaire :

- SelectedItem. Text : affiche la valeur de la propriété Text de l’élément sélectionné.
- SelectedItem. value : affiche la valeur de la propriété Value de l’élément sélectionné ou affiche le texte tapé dans la zone de liste déroulante.
- SelectedValue-identique à SelectedItem. Value sauf que cette propriété vous permet de spécifier l’élément sélectionné (initial) par défaut.

Si vous tapez un choix personnalisé dans la zone de liste déroulante, le choix personnalisé est assigné aux propriétés SelectedItem. Text et SelectedItem. Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Sélection de la liste des éléments de la base de données

Vous pouvez récupérer la liste des éléments que le contrôle ComboBox affiche à partir d’une base de données. Par exemple, vous pouvez lier la zone de liste déroulante à un contrôle SqlDataSource, un contrôle ObjectDataSource, un LinqDataSource ou un EntityDataSource.

Imaginez que vous souhaitez afficher une liste de films dans une zone de liste déroulante. Vous souhaitez récupérer la liste des films à partir de la table de base de données de films. Procédez comme suit :

1. Créer une page nommée movies. aspx
2. Ajoutez un contrôle ScriptManager à la page en faisant glisser le ScriptManager sous l’onglet Extensions AJAX de la boîte à outils vers la page.
3. Ajoutez un contrôle ComboBox à la page en faisant glisser la zone de liste déroulante sur la page.
4. Dans Mode Création, pointez votre souris sur le contrôle ComboBox et sélectionnez l’option de tâche **choisir la source de données** (voir figure 5). L’Assistant Configuration de source de données est lancé.
5. Dans l’étape **choisir une source de données** , sélectionnez l’option &lt;nouvelle&gt; de source de données.
6. Dans l’étape **choisir un type de source de données** , sélectionnez base de données.
7. Dans l’étape **choisir votre connexion de données** , sélectionnez votre base de données (par exemple, MoviesDB. mdf).
8. Dans l’étape **enregistrer la chaîne de connexion dans le fichier de configuration de l’application** , sélectionnez l’option permettant d’enregistrer votre chaîne de connexion.
9. Dans l’étape de configuration de l' **instruction SELECT** , sélectionnez la table de base de données movies, puis sélectionnez toutes les colonnes.
10. À l’étape **tester la requête** , cliquez sur le bouton Terminer.
11. De retour à l’étape **choisir la source de données** , sélectionnez la colonne titre pour le champ à afficher et la colonne ID du champ de données (voir la figure).
12. Cliquez sur le bouton OK pour fermer l’Assistant.

[![choix d’une source de données](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Figure 05**: choix d’une source de données ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image10.png))

[![choix des champs de texte et de valeur de données](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Figure 06**: choix des champs texte de données et valeur ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image12.png))

Une fois que vous avez effectué les étapes ci-dessus, la zone de liste déroulante est liée à un contrôle SqlDataSource qui représente les films de la table de base de données movies. La source de la page ressemble à la liste 2 (j’ai nettoyé un peu le formatage).

**Liste 2-movies. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Notez que le contrôle ComboBox a une propriété DataSourceID qui pointe vers le contrôle SqlDataSource. Lorsque vous ouvrez la page dans un navigateur, la liste des films de la base de données s’affiche (voir la figure 7). Vous pouvez choisir un film dans la liste ou entrer un nouveau film en tapant le film dans la liste déroulante.

[![affichage d’une liste de films](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Figure 07**: affichage d’une liste de films ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image14.png))

## <a name="setting-the-dropdownstyle"></a>Définition de DropDownStyle

Vous pouvez utiliser la propriété ComboBox DropDownStyle pour modifier le comportement de la zone de liste déroulante. Cette propriété accepte les valeurs possibles :

- DropDown-(valeur par défaut) la zone de liste déroulante affiche une liste déroulante lorsque vous cliquez sur la flèche et vous pouvez entrer une valeur personnalisée.
- Simple : la zone de liste déroulante affiche automatiquement une liste déroulante et vous pouvez entrer une valeur personnalisée.
- DropDownList : la zone de liste déroulante fonctionne comme un contrôle DropDownList.

La différence entre DropDown et simple est lorsque la liste des éléments est affichée. Dans le cas d’un simple, la liste s’affiche immédiatement lorsque vous déplacez le focus sur la zone de liste déroulante. Dans le cas de la liste déroulante, vous devez cliquer sur la flèche pour afficher la liste des éléments.

La valeur DropDownList permet au contrôle de zone de liste déroulante de fonctionner comme un contrôle DropDownList standard. Toutefois, il existe une différence importante ici. Les versions antérieures d’Internet Explorer affichent un contrôle DropDownList avec un index z infini, de sorte que le contrôle s’affiche devant tout contrôle placé devant lui. Étant donné que la zone de liste déroulante restitue une balise HTML &lt;div&gt; à la place d’une balise HTML &lt;sélectionner&gt;, la zone de liste déroulante respecte correctement l’ordre de plan.

## <a name="setting-the-autocompletemode"></a>Définition de AutoCompleteMode

Vous utilisez la propriété ComboBox AutoCompleteMode pour spécifier ce qui se produit quand un utilisateur tape du texte dans la zone de liste déroulante. Cette propriété accepte les valeurs possibles suivantes :

- None-(valeur par défaut) la zone de liste déroulante ne fournit pas de comportement de saisie semi-automatique.
- Suggérer : la liste déroulante affiche la liste et met en surbrillance l’élément correspondant dans la liste (voir figure 8).
- Append : la zone de liste déroulante n’affiche pas la liste et ajoute l’élément correspondant de la liste sur ce que vous avez tapé (voir la figure 9).
- SuggestAppend : la liste déroulante affiche la liste et ajoute l’élément correspondant de la liste sur ce que vous avez tapé (voir la figure 10).

[![la zone de liste déroulante fait une suggestion](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Figure 08**: la zone de liste déroulante fait une suggestion ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image16.png))

[![zone de liste déroulante ajoute le texte correspondant](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Figure 09**: le contrôle ComboBox ajoute le texte correspondant ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image18.png))

[![la zone de liste déroulante suggère et ajoute](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Figure 10**: la zone de liste déroulante suggère et ajoute ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image20.png))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à utiliser le contrôle ComboBox pour afficher un ensemble fixe d’éléments. Nous avons lié le contrôle ComboBox à la fois à un ensemble statique d’éléments et à une table de base de données. Enfin, vous avez appris à modifier le comportement de la zone de liste déroulante en définissant ses propriétés DropDownStyle et AutoCompleteMode.

> [!div class="step-by-step"]
> [Précédent](how-do-i-use-the-combobox-control-cs.md)
