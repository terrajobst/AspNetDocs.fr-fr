---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Création d’un extendeur de contrôle AJAX Control Toolkit personnalisé (VB) | Microsoft Docs
author: microsoft
description: Les extendeurs personnalisés vous permettent de personnaliser et d’étendre les fonctionnalités des contrôles ASP.NET sans avoir à créer de nouvelles classes.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 0849fa6c13679e0cd01bb20a4067a097acbce298
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578161"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Création d’un extendeur de contrôle AJAX Control Toolkit personnalisé (VB)

par [Microsoft](https://github.com/microsoft)

> Les extendeurs personnalisés vous permettent de personnaliser et d’étendre les fonctionnalités des contrôles ASP.NET sans avoir à créer de nouvelles classes.

Dans ce didacticiel, vous allez apprendre à créer un extendeur de contrôle AJAX Control Toolkit personnalisé. Nous créons un nouvel extendeur simple, mais utile, qui modifie l’état d’un bouton de désactivé à activé quand vous tapez du texte dans une zone de texte. Après avoir lu ce didacticiel, vous pourrez étendre ASP.NET AJAX Toolkit avec vos propres extendeurs de contrôle.

Vous pouvez créer des extendeurs de contrôle personnalisés à l’aide de Visual Studio ou de Visual Web Developer (Assurez-vous que vous disposez de la dernière version de Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Vue d’ensemble de l’extendeur DisabledButton

Notre nouvel extendeur de contrôle porte le nom d’extendeur DisabledButton. Cet extendeur aura trois propriétés :

- TargetControlID : zone de texte étendue par le contrôle.
- TargetButtonIID : bouton désactivé ou activé.
- DisabledText : texte initialement affiché dans le bouton. Lorsque vous commencez à taper, le bouton affiche la valeur de la propriété texte du bouton.

Vous raccordez l’extendeur DisabledButton à un contrôle TextBox et Button. Avant de taper du texte, le bouton est désactivé et la zone de texte et le bouton se présentent comme suit :

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))

Une fois que vous avez commencé à taper du texte, le bouton est activé et la zone de texte et le bouton se présentent comme suit :

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))

Pour créer notre extendeur de contrôle, nous devons créer les trois fichiers suivants :

- DisabledButtonExtender. vb : ce fichier est la classe de contrôle côté serveur qui gère la création de votre extendeur et vous permet de définir les propriétés au moment de la conception. Il définit également les propriétés qui peuvent être définies sur votre extendeur. Ces propriétés sont accessibles par le biais du code et au moment de la conception et correspondent aux propriétés définies dans le fichier DisableButtonBehavior. js.
- DisabledButtonBehavior. js--ce fichier est l’emplacement où vous allez ajouter l’ensemble de votre logique de script client.
- DisabledButtonDesigner. vb : cette classe active les fonctionnalités au moment du Design. Vous avez besoin de cette classe si vous souhaitez que l’extendeur de contrôle fonctionne correctement avec Visual Studio/Visual Web Developer designer.

Ainsi, un extendeur de contrôle se compose d’un contrôle côté serveur, d’un comportement côté client et d’une classe de concepteur côté serveur. Vous allez apprendre à créer les trois fichiers dans les sections suivantes.

## <a name="creating-the-custom-extender-website-and-project"></a>Création du site Web et du projet d’extendeur personnalisé

La première étape consiste à créer un projet de bibliothèque de classes et un site Web dans Visual Studio/Visual Web Developer. Nous allons créer l’extendeur personnalisé dans le projet de bibliothèque de classes et tester l’extendeur personnalisé dans le site Web.

Commençons par le site Web. Pour créer le site Web, procédez comme suit :

1. Sélectionnez l’option de menu **fichier, nouveau site Web**.
2. Sélectionnez le modèle **site Web ASP.net** .
3. Nommez le nouveau site Web *Website1*.
4. Cliquez sur le bouton **OK** .

Ensuite, nous devons créer le projet de bibliothèque de classes qui contiendra le code de l’extendeur de contrôle :

1. Sélectionnez l’option de menu **fichier, ajouter, nouveau projet**.
2. Sélectionnez le modèle **bibliothèque de classes** .
3. Nommez la nouvelle bibliothèque de classes portant le nom **CustomExtenders**.
4. Cliquez sur le bouton **OK** .

Une fois ces étapes terminées, la fenêtre de Explorateur de solutions doit ressembler à la figure 1.

[![solution avec un site Web et un projet de bibliothèque de classes](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Figure 01**: solution avec le site Web et le projet de bibliothèque de classes ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))

Ensuite, vous devez ajouter toutes les références d’assembly nécessaires au projet de bibliothèque de classes :

1. Cliquez avec le bouton droit sur le projet CustomExtenders et sélectionnez l’option de menu **Ajouter une référence**.
2. Sélectionnez l'onglet .NET.
3. Ajoutez des références aux assemblys suivants :

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Sélectionnez l’onglet Parcourir.
5. Ajoutez une référence à l’assembly AjaxControlToolkit. dll. Cet assembly se trouve dans le dossier où vous avez téléchargé le kit d’outils de contrôle AJAX.

Vous pouvez vérifier que vous avez ajouté toutes les références appropriées en cliquant avec le bouton droit sur votre projet, en sélectionnant Propriétés, puis en cliquant sur l’onglet Références (voir figure 2).

[![le dossier références avec les références requises](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Figure 02**: référence le dossier avec les références requises ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))

## <a name="creating-the-custom-control-extender"></a>Création de l’extendeur de contrôle personnalisé

Maintenant que nous avons notre bibliothèque de classes, nous pouvons commencer à créer notre contrôle d’extendeur. Commençons par les peines d’une classe de contrôle d’extendeur personnalisée (voir la liste 1).

**Liste 1-MyCustomExtender. vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Il existe plusieurs choses que vous remarquez sur la classe d’extendeur de contrôle dans la liste 1. Tout d’abord, Notez que la classe hérite de la classe de base ExtenderControlBase. Tous les contrôles d’extendeur du kit d’outils de contrôle AJAX dérivent de cette classe de base. Par exemple, la classe de base comprend la propriété TargetID qui est une propriété obligatoire de chaque extendeur de contrôle.

Ensuite, remarquez que la classe comprend les deux attributs suivants liés au script client :

- Resource : entraîne l’inclusion d’un fichier en tant que ressource incorporée dans un assembly.
- ClientScriptResource-provoque la récupération d’une ressource de script à partir d’un assembly.

L’attribut Resource est utilisé pour incorporer le fichier JavaScript MyControlBehavior. js dans l’assembly lorsque l’extendeur personnalisé est compilé. L’attribut ClientScriptResource est utilisé pour récupérer le script MyControlBehavior. js à partir de l’assembly lorsque l’extendeur personnalisé est utilisé dans une page Web.

Pour que les attributs Resource et ClientScriptResource fonctionnent, vous devez compiler le fichier JavaScript en tant que ressource incorporée. Sélectionnez le fichier dans la fenêtre Explorateur de solutions, ouvrez la feuille de propriétés et affectez la valeur *ressource incorporée* à la propriété **action de génération** .

Notez que l’extendeur de contrôle comprend également un attribut TargetControlType. Cet attribut est utilisé pour spécifier le type de contrôle qui est étendu par l’extendeur de contrôle. Dans le cas de la liste 1, l’extendeur de contrôle est utilisé pour étendre une zone de texte.

Enfin, Notez que l’extendeur personnalisé comprend une propriété nommée MyProperty. La propriété est marquée avec l’attribut ExtenderControlProperty. Les méthodes GetPropertyValue () et SetPropertyValue () sont utilisées pour passer la valeur de la propriété de l’extendeur de contrôle côté serveur au comportement côté client.

Commençons par implémenter le code pour notre extendeur DisabledButton. Vous trouverez le code de cet extendeur dans la liste 2.

**Liste 2-DisabledButtonExtender. vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

L’extendeur DisabledButton dans la liste 2 a deux propriétés nommées TargetButtonID et DisabledText. Le IDReferenceProperty appliqué à la propriété TargetButtonID vous empêche d’assigner tout autre chose que l’ID d’un contrôle bouton à cette propriété.

Les attributs Resource et ClientScriptResource associent un comportement côté client situé dans un fichier nommé DisabledButtonBehavior. js à cet extendeur. Nous aborderons ce fichier JavaScript dans la section suivante.

## <a name="creating-the-custom-extender-behavior"></a>Création du comportement d’extendeur personnalisé

Le composant côté client d’un extendeur de contrôle est appelé un comportement. La logique réelle pour désactiver et activer le bouton est contenue dans le comportement DisabledButton. Le code JavaScript pour le comportement est inclus dans le Listing 3.

**Liste 3-DisabledButton. js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

Le fichier JavaScript de la liste 3 contient une classe côté client nommée DisabledButtonBehavior. Cette classe, comme sa représentation côté serveur, comprend deux propriétés nommées TargetButtonID et DisabledText, auxquelles vous pouvez accéder à l’aide de l'\_TargetButtonID/Set\_TargetButtonID et de l'\_DisabledText/Set\_DisabledText.

La méthode Initialize () associe un gestionnaire d’événements KeyUp à l’élément Target pour le comportement. Chaque fois que vous tapez une lettre dans la zone de texte associée à ce comportement, le gestionnaire KeyUp s’exécute. Le gestionnaire KeyUp active ou désactive le bouton, selon que la zone de texte associée au comportement contient du texte ou non.

N’oubliez pas que vous devez compiler le fichier JavaScript de la liste 3 en tant que ressource incorporée. Sélectionnez le fichier dans la fenêtre Explorateur de solutions, ouvrez la feuille de propriétés et affectez la valeur *ressource incorporée* à la propriété **action de génération** (voir figure 3). Cette option est disponible dans Visual Studio et Visual Web Developer.

[![ajout d’un fichier JavaScript en tant que ressource incorporée](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Figure 03**: ajout d’un fichier JavaScript en tant que ressource incorporée ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))

## <a name="creating-the-custom-extender-designer"></a>Création du concepteur d’extendeurs personnalisé

Il existe une dernière classe que nous devons créer pour compléter notre extendeur. Nous devons créer la classe de concepteur dans la liste 4. Cette classe est requise pour que l’extendeur se comporte correctement avec Visual Studio/Visual Web Developer designer.

**Liste 4-DisabledButtonDesigner. vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Vous associez le concepteur de la liste 4 à l’extendeur DisabledButton avec l’attribut designer. Vous devez appliquer l’attribut designer à la classe DisabledButtonExtender comme suit :

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Utilisation de l’extendeur personnalisé

Maintenant que nous avons terminé de créer l’extendeur de contrôle DisabledButton, il est temps de l’utiliser sur notre site Web ASP.NET. Tout d’abord, nous devons ajouter l’extendeur personnalisé à la boîte à outils. Procédez comme suit :

1. Ouvrez une page ASP.NET en double-cliquant sur la page dans la fenêtre Explorateur de solutions.
2. Cliquez avec le bouton droit sur la boîte à outils, puis sélectionnez l’option de menu **choisir les éléments**.
3. Dans la boîte de dialogue choisir des éléments de boîte à outils, accédez à l’assembly CustomExtenders. dll.
4. Cliquez sur le bouton **OK** pour fermer la boîte de dialogue.

Une fois ces étapes terminées, l’extendeur de contrôle DisabledButton doit apparaître dans la boîte à outils (voir figure 4).

[![DisabledButton dans la boîte à outils](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Figure 04**: DisabledButton dans la boîte à outils ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))

Ensuite, nous devons créer une nouvelle page ASP.NET. Procédez comme suit :

1. Créez une nouvelle page ASP.NET nommée ShowDisabledButton. aspx.
2. Faites glisser un ScriptManager sur la page.
3. Faites glisser un contrôle TextBox sur la page.
4. Faites glisser un contrôle Button sur la page.
5. Dans la Fenêtre Propriétés, remplacez la valeur de la propriété de l’ID du bouton par la valeur <em>btnSave</em> et la propriété Text par la valeur *Save\** .

Nous avons créé une page avec un contrôle TextBox et Button standard ASP.NET.

Ensuite, nous devons étendre le contrôle TextBox avec l’extendeur DisabledButton :

1. Sélectionnez l’option Ajouter une tâche d' **extendeur** pour ouvrir la boîte de dialogue de l’Assistant extendeur (voir figure 5). Notez que la boîte de dialogue contient notre extendeur DisabledButton personnalisé.
2. Sélectionnez l’extendeur DisabledButton, puis cliquez sur le bouton **OK** .

[![de la boîte de dialogue de l’Assistant extendeur](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Figure 05**: boîte de dialogue de l’Assistant extendeur ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))

Enfin, nous pouvons définir les propriétés de l’extendeur DisabledButton. Vous pouvez modifier les propriétés de l’extendeur DisabledButton en modifiant les propriétés du contrôle TextBox :

1. Sélectionnez la zone de texte dans le concepteur.
2. Dans le Fenêtre Propriétés, développez le nœud extendeurs (voir figure 6).
3. Affectez la valeur *Save* à la propriété DisabledText et la valeur *btnSave* à la propriété TargetButtonID.

[Propriétés de l’extendeur de définition de ![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Figure 06**: définition des propriétés de l’extendeur ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))

Quand vous exécutez la page (en appuyant sur F5), le contrôle de bouton est initialement désactivé. Dès que vous commencez à entrer du texte dans la zone de texte, le contrôle de bouton est activé (voir la figure 7).

[![l’extendeur DisabledButton en action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Figure 07**: l’extendeur DisabledButton en action ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était d’expliquer comment vous pouvez étendre la boîte à outils de contrôle AJAX avec des contrôles d’extendeur personnalisés. Dans ce didacticiel, nous avons créé un extendeur de contrôle DisabledButton simple. Nous avons implémenté cet extendeur en créant une classe DisabledButtonExtender, un comportement JavaScript DisabledButtonBehavior et une classe DisabledButtonDesigner. Vous suivez un ensemble d’étapes similaires chaque fois que vous créez un extendeur de contrôle personnalisé.

> [!div class="step-by-step"]
> [Précédent](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
