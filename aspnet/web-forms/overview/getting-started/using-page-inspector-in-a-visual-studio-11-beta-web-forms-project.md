---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Utilisation de Inspecteur de page pour Visual Studio 2012 dans ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Inspecteur de page pour Visual Studio 2012 est un outil de développement Web avec un navigateur intégré. Sélectionnez un élément dans le navigateur intégré, puis Inspecteur de page...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575921"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Utilisation de l’Inspecteur de page pour Visual Studio 2012 dans Web Forms ASP.NET

par Tim Ammann

> Inspecteur de page pour Visual Studio 2012 est un outil de développement Web avec un navigateur intégré. Sélectionnez un élément dans le navigateur intégré, puis Inspecteur de page met instantanément en surbrillance la source et le CSS de l’élément. Vous pouvez parcourir n’importe quelle page de votre application, rechercher rapidement les sources du balisage rendu et utiliser les outils de navigation directement dans l’environnement Visual Studio.
> 
> Ce didacticiel montre comment activer Mode d’inspection, puis Rechercher et modifier rapidement les règles CSS et le texte dans votre projet Web. Le didacticiel utilise un projet d’application Web Forms, mais vous pouvez également utiliser des Inspecteur de page pour les projets de site Web et les applications [MVC](https://go.microsoft.com/?linkid=9802002) .
> 
> Ce didacticiel contient les sections suivantes :
> 
> [Conditions préalables](#_1_prerequisites)
> 
> [Créer une application Web](#_2_creating_a)
> 
> [Utiliser Inspecteur de page pour afficher l’application](#_3_using_page)
> 
> [Activer Mode d’inspection](#_4_inspection_mode)
> 
> [Utiliser Inspecteur de page pour apporter des modifications au balisage](#_5_using_page)
> 
> [Mode d’inspection et la fenêtre HTML](#_6_inspection_mode)
> 
> [Aperçu des modifications CSS dans la fenêtre styles](#_7_previewing_css)
> 
> [Synchronisation automatique CSS](#css_auto_sync)
> 
> [Utilisation du sélecteur de couleurs CSS](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Conditions préalables requises

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Pour obtenir la dernière version de Inspecteur de page, utilisez [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) pour installer le kit de développement logiciel (SDK) Azure pour .NET 2,0.

Inspecteur de page est fourni avec Microsoft Web Developer Tools. La version la plus récente est 1,3. Pour vérifier la version que vous avez, exécutez Visual Studio et sélectionnez à **propos de Microsoft Visual Studio** dans le menu **aide** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Créer une application Web

Tout d’abord, vous allez créer une application Web que vous utiliserez Inspecteur de page avec. Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**. À gauche, développez **visuel C#** , sélectionnez **Web**, puis sélectionnez **ASP.NET Web Forms application**.

![Nouvelle Web Forms application](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Cliquez sur **OK**.

L’application s’ouvre en mode **source** .

![Nouvelle Web Forms application en mode Source](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Maintenant que vous disposez d’une application à utiliser, vous pouvez utiliser Inspecteur de page pour l’examiner et la modifier.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Utiliser Inspecteur de page pour afficher l’application

Ensuite, vous allez afficher l’application avec Inspecteur de page. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis choisissez **afficher dans inspecteur de page**.

![Afficher dans l'inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Par défaut, lorsque Inspecteur de page démarre pour la première fois, il est ancré sous la forme d’une fenêtre étroite sur le côté gauche de l’environnement Visual Studio. Laissez-le ancré sur le côté gauche et affectez-lui une largeur qui vous convient, ou ancrez-le dans l’une des zones d’outils en haut, en bas ou à droite :

![Inspecteur de page les positions d’ancrage](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Si vous détachez la fenêtre de Inspecteur de page, vous pouvez la placer en dehors de Visual Studio, ou même sur un deuxième écran, le cas échéant. Toutefois, pour pouvoir appuyer sur ALT + TAB entre Inspecteur de page et Visual Studio lorsque la fenêtre de Inspecteur de page n’est pas ancrée, accédez à **outils** &gt; **Options** &gt; **environnement** &gt; **onglets et fenêtres**, et sous **onglets**, désactivez la case à cocher appelée **fenêtres outil flottant toujours rester au-dessus de la fenêtre principale**:

![Désactivez la case à cocher fenêtres outil flottantes pour ALT + TAB entre Visual Studio et la fenêtre de Inspecteur de page désancrée.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Le volet supérieur de la fenêtre Inspecteur de page affiche la page actuelle dans une fenêtre de navigateur. Le volet inférieur affiche la page dans le balisage HTML à gauche, ainsi que certains onglets à droite qui vous permettent d’inspecter différents aspects de la page. Le volet inférieur est semblable au [outils de développement F12](https://msdn.microsoft.com/ie/aa740478) dans Internet Explorer. (Toutefois, contrairement aux outils de développement, vous pouvez utiliser Inspecteur de page directement dans Visual Studio.)

![Inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

Dans ce didacticiel, vous allez utiliser le volet navigateur Inspecteur de page, ainsi que les onglets **HTML** et **styles** pour vous aider à naviguer rapidement et à apporter des modifications à l’application.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Activer Mode d’inspection

Vous verrez ensuite comment le Mode d’inspection de Inspecteur de page fonctionne. Dans la fenêtre Inspecteur de page, cliquez sur le bouton **inspecter** .

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Pour voir le mode inspection en action, déplacez votre souris sur les différentes parties de la page dans la fenêtre du navigateur Inspecteur de page. Comme c’est le cas, le pointeur de la souris se transforme en un plus grand signe plus, et l’élément sous est mis en surbrillance :

![Pointage sur div. Content-Wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Au fur et à mesure que vous déplacez le pointeur de la souris, Notez que

- Le contenu en mode **source** change pour afficher le balisage correspondant à l’élément sélectionné sur la page. La balise appropriée est mise en surbrillance. Si la source se trouve dans un autre fichier, ce fichier est ouvert en mode Source avec le balisage approprié mis en surbrillance.

- Le balisage affiché sous l’onglet **HTML** dans inspecteur de page change également pour correspondre à l’élément sélectionné sur la page. Dans l’onglet **HTML** , le balisage approprié est décrit.

- L’onglet **styles** affiche les règles CSS qui s’appliquent à la sélection actuelle.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Utiliser Inspecteur de page pour apporter des modifications au balisage

À présent, vous verrez comment utiliser Inspecteur de page pour rechercher et apporter des modifications au balisage ou au texte dont l’emplacement peut ne pas être immédiatement évident.

Placez Inspecteur de page dans Mode d’inspection puis faites défiler vers le bas de la page d’hébergement.

Dès que vous entrez dans la zone de pied de page, Inspecteur de page ouvre le fichier de disposition *site. Master* en mode **source** dans un onglet temporaire à droite des autres onglets et met en surbrillance la section de la page maître que vous avez sélectionnée. Cela vous montre comment Inspecteur de page pouvez rechercher et afficher le contenu d’une page qui peut provenir d’un fichier différent de celui que vous avez ouvert à l’origine.

![Le pied de page met en surbrillance Mode d’inspection](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Dans la fenêtre du navigateur Inspecteur de page, déplacez le pointeur de la souris sur la ligne <a id="a"> </a>avec la mention de droits d’auteur.

Dans la page *site. Master* , la ligne correspondante est mise en surbrillance.

![Ligne de copyright du pied de page en surbrillance](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Ajoutez du texte à la fin de la ligne dans le fichier *site. Master* .

&lt;p&gt;&amp;copie ; &lt;% : DateTime. Now. Year%&gt;-My ASP.NET application Rocks !&lt;/p&gt;

Maintenant, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour pour afficher les résultats dans la fenêtre du navigateur Inspecteur de page.

![Mon application ASP.NET Rocks !](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Vous avez peut-être pensé que le pied de page était sur la page *default. aspx* , mais il se trouvait dans la page de disposition maître et inspecteur de page l’a trouvé pour vous.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Mode d’inspection et la fenêtre HTML

Ensuite, vous aurez un aperçu rapide de la fenêtre HTML et de la façon dont elle mappe les éléments pour vous.

Placez Inspecteur de page dans Mode d’inspection.

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Cliquez sur la partie supérieure de la page qui indique « votre logo ici ». Vous examinez un élément particulier plus en détail. par conséquent, l’affichage dans la fenêtre du navigateur ne change plus lorsque vous déplacez le pointeur de la souris.

Déplacez maintenant le pointeur de la souris vers la fenêtre **HTML** . Lorsque vous déplacez le pointeur de la souris, Inspecteur de page présente l’élément dans la fenêtre **HTML** et met en surbrillance l’élément correspondant dans la fenêtre du navigateur.

![Fenêtre HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Comme précédemment, Inspecteur de page ouvre le fichier *site. Master* pour vous dans un onglet temporaire. cliquez sur l’onglet site. Master et le balisage correspondant est mis en surbrillance dans la section &lt;en-tête&gt; :

![Balisage en surbrillance](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Aperçu des modifications CSS dans la fenêtre styles

Vous verrez ensuite comment vous pouvez utiliser la fenêtre Inspecteur de page **styles** pour afficher un aperçu des modifications apportées à CSS.

Cliquez sur le bouton **inspecter** pour placer Inspecteur de page dans mode d’inspection.

Dans la fenêtre du navigateur Inspecteur de page, déplacez le pointeur de la souris sur la section « page d’hébergement » jusqu’à ce que l’étiquette **div. Content-Wrapper** apparaisse.

![Pointage sur les éléments](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Cliquez une fois dans la section div. Content-Wrapper, puis placez le pointeur de la souris dans la fenêtre **styles** . Sous le sélecteur de classe. featured. Content-Wrapper, désactivez et activez la case à cocher de la propriété Background-color.

![Effacer la couleur d’arrière-plan](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Notez la manière dont les préversions changent instantanément dans la fenêtre du navigateur Inspecteur de page.

Activez à nouveau la case à cocher, puis double-cliquez sur la valeur de la propriété et remplacez-la par `red`. La modification apparaît immédiatement :

![Couleur d’arrière-plan rouge](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

La fenêtre **styles** facilite le test et l’aperçu des modifications CSS avant de valider les modifications apportées à la feuille de style elle-même.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Synchronisation automatique CSS

> [!NOTE]
> Cette fonctionnalité nécessite la version 1,3 de Inspecteur de page.

La fonctionnalité de synchronisation automatique CSS vous permet de modifier directement un fichier CSS et de voir immédiatement les modifications dans le navigateur Inspecteur de page.

Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection.

Dans le navigateur Inspecteur de page, déplacez le pointeur de la souris sur la section « page d’hébergement » jusqu’à ce que l’étiquette **div. Content-Wrapper** apparaisse. Cliquez une fois pour sélectionner cet élément.

La fenêtre **styles** affiche toutes les règles CSS pour cet élément. Faites défiler la liste pour rechercher le sélecteur de classe. featured. Content-wrapper. Cliquez sur « . featured. Content-wrapper ». Inspecteur de page ouvre le fichier CSS qui définit ce style (site. CSS) et met en surbrillance le style CSS correspondant.

![Fichier CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

À présent, remplacez la valeur de `background-color` par « Red ». La modification s’affiche immédiatement dans le navigateur Inspecteur de page.

![Navigateur de Inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Utilisation du sélecteur de couleurs CSS

Ensuite, vous allez apprendre à utiliser Inspecteur de page pour rechercher et modifier rapidement le CSS pour le texte mis en surbrillance dans l’application par défaut. Dans cet exemple, vous avez décidé que vous n’aimez pas la mise en surbrillance bleue et que vous souhaitez la remplacer par une autre couleur.

Cliquez sur le bouton **inspecter** .

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Dans la fenêtre du navigateur Inspecteur de page, déplacez le pointeur de la souris sur le texte « vidéos, didacticiels et exemples » en surbrillance pour afficher l’étiquette « marque » CSS.

![Pointage sur l’élément Mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Cliquez sur le texte pour le sélectionner. Le sélecteur de marque CSS correspondant apparaît en bas de la fenêtre **styles** .

![marquer le sélecteur dans la fenêtre styles](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Cliquez sur le sélecteur de marque. Cela ouvre le fichier *site. CSS* pour l’application Web. Cliquez sur l’onglet site. CSS pour mettre en surbrillance le CSS correspondant du sélecteur :

![marquer le sélecteur dans la feuille de style](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Sélectionnez et supprimez la ligne avec la propriété Background-color.

Vous allez maintenant utiliser le nouveau sélecteur de couleurs CSS de Visual Studio 2012 pour choisir une nouvelle couleur pour la propriété **marquer** la couleur d’arrière-plan.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Utilisation du sélecteur de couleurs CSS de Visual Studio 2012

L’éditeur CSS dans Visual Studio 2012 possède un sélecteur de couleurs qui facilite la sélection et l’insertion de couleurs. Il possède une barre de couleurs simple et un sélecteur « indépendant » qui offre un contrôle plus précis.

Le sélecteur de couleurs contient une palette de couleurs standard, prend en charge les couleurs standard, les codes de hachage, les couleurs RVB, RVBA, TSL et HSLA, et conserve une liste des couleurs que vous avez utilisées le plus récemment dans le document.

Sur la ligne où la propriété Background-color était, tapez « BC » et appuyez une fois sur la flèche bas.

Quand vous tapez le premier caractère de chaque mot dans une propriété séparée par un trait d’Union comme « background-color », IntelliSense filtre la liste pour afficher uniquement les propriétés correspondantes :

![Valeurs filtrées IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

À présent, tapez un signe deux-points. Dans ce cas, le nom complet de la propriété Background-color est inséré. Tapez **#** ou **RVB (** , et la barre du sélecteur de couleurs s’affiche :

![Barre du sélecteur de couleurs CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Pour voir comment fonctionne la barre du sélecteur de couleurs, cliquez sur ses couleurs à l’aide du pointeur de la souris, ou appuyez sur la touche de direction bas, puis utilisez les touches de direction gauche et droite pour parcourir les couleurs. Lorsque vous visitez une couleur, la valeur correspondante pour la propriété Background-color est aperçu :

![couleur d’arrière-plan-aperçu de la valeur de propriété](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

À ce stade, vous pouvez appuyer sur entrée pour sélectionner la valeur, puis un point-virgule (;) pour terminer l’entrée CSS. Pour le moment, passez à la section suivante pour voir comment fonctionne la fenêtre contextuelle du sélecteur de couleurs.

#### <a name="using-the-color-picker-pop-down"></a>Utilisation de la fenêtre contextuelle du sélecteur de couleurs

Si la barre de couleurs n’a pas la couleur exacte que vous recherchez, vous pouvez utiliser le menu contextuel du sélecteur de couleurs.

Pour l’ouvrir, cliquez sur le double Chevron à l’extrémité droite de la barre de couleurs, ou appuyez sur la flèche bas une fois ou deux fois sur le clavier.

![Menu contextuel du sélecteur de couleurs CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Cliquez sur une couleur à partir de la barre verticale à droite. Cela montre un dégradé pour cette couleur dans la fenêtre principale. Choisissez une couleur directement à partir de la barre verticale en appuyant sur entrée ou en cliquant sur n’importe quel point dans la fenêtre principale pour choisir avec une plus grande précision.

S’il y a une couleur sur l’écran de l’ordinateur que vous souhaitez utiliser (il n’est pas nécessaire qu’il se trouve à l’intérieur de l’interface utilisateur de Visual Studio), vous pouvez capturer sa valeur à l’aide de l’outil pipette situé en bas à droite.

Vous pouvez également modifier l’opacité d’une couleur en déplaçant le curseur en bas du sélecteur de couleurs. Cela modifie les valeurs de couleur en valeurs RVBA, car le format RVBA peut représenter l’opacité.

Une fois que vous avez choisi une couleur, appuyez sur entrée, puis tapez un point-virgule pour terminer l’entrée de couleur d’arrière-plan dans le fichier *site. CSS* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Barre de mise à jour Inspecteur de page

Inspecteur de page détecte immédiatement la modification apportée au fichier *site. CSS* (ou à n’importe quel fichier de l’application) et affiche une alerte dans une barre de mise à jour.

![Barre de mise à jour](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Pour enregistrer tous vos fichiers et actualiser le navigateur Inspecteur de page, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour. La modification de la couleur de surbrillance s’affiche dans le navigateur :

![Couleur de surbrillance modifiée](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Notez que vous avez facilement actualisé le Inspecteur de page navigateur à partir de l’environnement Visual Studio. L’utilisation de Inspecteur de page au lieu d’un navigateur externe vous permet de rester dans l’éditeur lorsque vous développez vos applications Web.

## <a name="see-also"></a>Voir aussi

[Présentation de inspecteur de page](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vidéo Channel 9)

[Inspecteur de page des messages d’erreur](https://go.microsoft.com/?linkid=9813062) (MSDN)
