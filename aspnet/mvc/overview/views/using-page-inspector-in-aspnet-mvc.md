---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Utilisation de Inspecteur de page dans ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Inspecteur de page dans Visual Studio 2012 est un outil de développement Web avec un navigateur intégré. Sélectionnez un élément dans le navigateur intégré, puis Inspecteur de page i...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538016"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a>Utilisation de l'Inspecteur de page dans ASP.NET MVC

par Tim Ammann

> Inspecteur de page dans Visual Studio 2012 est un outil de développement Web avec un navigateur intégré. Sélectionnez un élément dans le navigateur intégré, puis Inspecteur de page met instantanément en surbrillance la source et le CSS de l’élément. Vous pouvez parcourir n’importe quelle vue MVC, rechercher rapidement les sources du balisage rendu et utiliser les outils de navigation directement dans l’environnement Visual Studio.
> 
> [Regarder la vidéo](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Ce didacticiel montre comment activer Mode d’inspection, puis Rechercher et modifier rapidement le balisage et CSS dans votre projet Web. Le didacticiel utilise un projet MVC, mais vous pouvez également utiliser Inspecteur de page pour [Web Forms](https://go.microsoft.com/?linkid=9802001) et d’autres applications ASP.net.
> 
> Ce didacticiel contient les sections suivantes :
> 
> - [Conditions préalables](#_1_prerequisites)
> - [Créer une application Web](#_2_creating_a)
> - [Utiliser Inspecteur de page pour accéder à une vue](#_3_using_page)
> - [Activer Mode d’inspection](#_4_inspection_mode)
> - [Utiliser Inspecteur de page pour apporter des modifications au balisage](#_5_using_page)
> - [Mode d’inspection et la fenêtre HTML](#_6_inspection_mode)
> - [Aperçu des modifications CSS dans la fenêtre styles](#_7_previewing_css)
> - [Synchronisation automatique CSS](#css_auto_sync)
> - [Utilisation du sélecteur de couleurs CSS](#css_color_picker)
> - [Mappage d’éléments de page dynamiques à JavaScript](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Conditions préalables requises

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Pour obtenir la dernière version de Inspecteur de page, utilisez [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) pour installer le kit de développement logiciel (SDK) Windows Azure pour .NET 2,0.

Inspecteur de page est fourni avec Microsoft Web Developer Tools. La version la plus récente est 1,3. Pour vérifier la version que vous avez, exécutez Visual Studio et sélectionnez à **propos de Microsoft Visual Studio** dans le menu **aide** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Créer une application Web

Tout d’abord, créez une application Web que vous utiliserez Inspecteur de page avec. Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**. À gauche, développez **visuel C#** , sélectionnez **Web**, puis sélectionnez **application Web ASP.net MVC4**.

![Nouvelle application ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Cliquez sur **OK**.

Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez **application Internet**. Laissez **Razor** comme moteur d’affichage par défaut.

![Nouveau projet MVC ASP.NET-application Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

L’application s’ouvre en mode **source** .

![Nouvelle application ASP.NET MVC en mode Source](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Maintenant que vous disposez d’une application à utiliser, vous pouvez utiliser Inspecteur de page pour l’examiner et la modifier.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Utiliser Inspecteur de page pour accéder à une vue

Dans Visual Studio 2012, vous pouvez cliquer avec le bouton droit sur n’importe quelle vue de votre projet, sélectionner **afficher dans inspecteur de page**, et inspecteur de page déterminer l’itinéraire et afficher la page.

Dans **Explorateur de solutions**, développez le dossier **views** , puis le dossier de **démarrage** . Cliquez avec le bouton droit sur le fichier index. cshtml, puis choisissez **afficher dans inspecteur de page**.

![Afficher index. cshtml dans Inspecteur de page](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Par défaut, Inspecteur de page est ancré sous la forme d’une fenêtre sur le côté gauche de l’environnement Visual Studio. Si vous préférez, vous pouvez l’ancrer ailleurs ou détacher la fenêtre. Consultez [Comment : organiser et ancrer des fenêtres](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Le volet supérieur de la fenêtre Inspecteur de page affiche la page actuelle dans une fenêtre de navigateur. Le volet inférieur affiche la page dans le balisage HTML, ainsi que certains onglets qui vous permettent d’inspecter différents aspects de la page. Le volet inférieur est semblable au [outils de développement F12](https://msdn.microsoft.com/ie/aa740478) dans Internet Explorer.

![Application ASP.NET MVC dans Inspecteur de page](using-page-inspector-in-aspnet-mvc/_static/image10.png)

Dans ce didacticiel, vous allez utiliser les onglets **HTML** et **styles** pour naviguer rapidement et apporter des modifications à l’application.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Mode EnableInspection

Pour placer Inspecteur de page dans Mode d’inspection, cliquez sur le bouton **inspecter** . Dans Mode d’inspection, lorsque vous maintenez le pointeur de la souris sur une partie de la page rendue, le balisage ou le code source correspondant est mis en surbrillance.

![Mode d’inspection bascule](using-page-inspector-in-aspnet-mvc/_static/image12.png)

À présent, déplacez votre souris sur les différentes parties de la page dans Inspecteur de page. Comme c’est le cas, le pointeur de la souris se transforme en un plus grand signe plus, et l’élément sous est mis en surbrillance :

![Pointage sur div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Lorsque vous déplacez le pointeur de la souris, Visual Studio met en surbrillance les syntaxe Razor correspondants dans le fichier source. Si l’élément HTML provient d’un autre fichier source, Visual Studio ouvre automatiquement le fichier.

Dans Inspecteur de page, l’onglet **HTML** affiche le code HTML généré à partir du syntaxe Razor. Lorsque vous déplacez le pointeur de la souris, les éléments HTML sont mis en surbrillance. L’onglet **styles** affiche les règles CSS pour l’élément.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Utiliser Inspecteur de page pour apporter des modifications au balisage

Inspecteur de page vous permet de trouver le balisage dont l’emplacement n’est peut-être pas évident. Vous pouvez ensuite modifier le balisage et afficher les modifications qui en résultent.

Pour le voir, cliquez sur **inspecter** , puis faites défiler la page jusqu’en bas de la page dans la fenêtre de inspecteur de page.

Lorsque vous déplacez le pointeur de la souris dans la zone de pied de page, Inspecteur de page ouvre le fichier \_Layout. cshtml et met en surbrillance la section de la page de disposition que vous avez sélectionnée. Comme vous pouvez le voir, le pied de page est défini dans le fichier de disposition, et non dans la vue elle-même.

![Pied de page](using-page-inspector-in-aspnet-mvc/_static/image16.png)

À présent, déplacez le pointeur de la souris sur la <a id="a"> </a>ligne avec la mention de droits d’auteur. Dans la page disposition. cshtml \_, la ligne correspondante est mise en surbrillance.

![Ligne de copyright du pied de page en surbrillance](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Ajoutez du texte à la fin de la ligne dans le fichier \_Layout. cshtml.

&lt;p&gt;&amp;copie ; @DateTime.Now.Year-mon application ASP.NET MVC Rocks !&lt;/p&gt;

Maintenant, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour pour afficher les résultats dans la fenêtre du navigateur Inspecteur de page.

![Mon application ASP.NET Rocks !](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Vous avez peut-être pensé que le pied de page est défini dans index. cshtml, mais il s’est avéré dans le \_Layout. cshtml, et Inspecteur de page l’a trouvé pour vous.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Mode d’inspection et la fenêtre HTML

Ensuite, vous aurez un aperçu rapide de la fenêtre HTML et de la façon dont elle mappe les éléments pour vous.

Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection.

Cliquez sur la partie supérieure de la page qui indique « votre logo ici ». Vous examinez un élément particulier plus en détail. par conséquent, l’affichage dans la fenêtre du navigateur ne change plus lorsque vous déplacez le pointeur de la souris.

Déplacez maintenant le pointeur de la souris vers la fenêtre **HTML** . Lorsque vous déplacez le pointeur de la souris, Inspecteur de page présente l’élément dans la fenêtre **HTML** et met en surbrillance l’élément correspondant dans la fenêtre du navigateur.

![Fenêtre HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Comme précédemment, Inspecteur de page ouvre le fichier \_Layout. cshtml pour vous dans un onglet temporaire. cliquez sur l’onglet \_Layout. cshtml temporaire, et le balisage correspondant sera mis en surbrillance dans la section&gt; d’en-tête de &lt;pour vous :

![Balisage en surbrillance](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Aperçu des modifications CSS dans la fenêtre styles

Ensuite, vous allez utiliser la fenêtre Inspecteur de page **styles** pour afficher un aperçu des modifications apportées à CSS.

Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection.

Dans la fenêtre du navigateur Inspecteur de page, déplacez le pointeur de la souris sur la section « page d’hébergement » jusqu’à ce que l’étiquette **div. Content-Wrapper** apparaisse.

![Pointage sur div. Content-Wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Cliquez une fois dans la section div. Content-Wrapper, puis placez le pointeur de la souris dans la fenêtre **styles** . La fenêtre **styles** affiche toutes les règles CSS pour cet élément. Faites défiler la liste pour rechercher le sélecteur de classe. featured. Content-wrapper. Maintenant, désactivez la case à cocher de la propriété Background-color.

![Effacer la couleur d’arrière-plan](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Notez la manière dont les préversions changent instantanément dans la fenêtre du navigateur Inspecteur de page.

Activez à nouveau la case à cocher, puis double-cliquez sur la valeur de la propriété et remplacez-la par la valeur rouge. La modification apparaît immédiatement :

![Couleur d’arrière-plan rouge](using-page-inspector-in-aspnet-mvc/_static/image30.png)

La fenêtre **styles** facilite le test et l’aperçu des modifications CSS avant de valider les modifications apportées à la feuille de style elle-même.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Synchronisation automatique CSS

> [!NOTE]
> Cette fonctionnalité nécessite la version 1,3 de Inspecteur de page.

La fonctionnalité de synchronisation automatique CSS vous permet de modifier directement un fichier CSS et de voir immédiatement les modifications dans le navigateur Inspecteur de page.

Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection.

Dans le navigateur Inspecteur de page, déplacez le pointeur de la souris sur la section « page d’hébergement » jusqu’à ce que l’étiquette **div. Content-Wrapper** apparaisse. Cliquez une fois pour sélectionner cet élément.

La fenêtre **styles** affiche toutes les règles CSS pour cet élément. Faites défiler la liste pour rechercher le sélecteur de classe. featured. Content-wrapper. Cliquez sur « . featured. Content-wrapper ». Inspecteur de page ouvre le fichier CSS qui définit ce style (site. CSS) et met en surbrillance le style CSS correspondant.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

À présent, remplacez la valeur de `background-color` par « Red ». La modification s’affiche immédiatement dans le navigateur Inspecteur de page.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Utilisation du sélecteur de couleurs CSS

L’éditeur CSS dans Visual Studio 2012 possède un sélecteur de couleurs qui facilite la sélection et l’insertion de couleurs. Le sélecteur de couleurs contient une palette de couleurs standard, prend en charge les couleurs standard, les codes de hachage, les couleurs RVB, RVBA, TSL et HSLA, et conserve une liste des couleurs que vous avez utilisées le plus récemment dans le document.

Dans la section précédente, vous avez modifié la valeur de la propriété `background-color`. Pour appeler le sélecteur de couleurs, placez le point d’insertion après le nom de la propriété et tapez **#** ou **RVB (** .

![Barre du sélecteur de couleurs CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Cliquez sur une couleur pour la sélectionner, ou appuyez sur la touche de direction bas, puis utilisez les touches de direction gauche et droite pour parcourir les couleurs. Lorsque vous visitez une couleur, la valeur hexadécimale correspondante est prévisualisée :

![couleur d’arrière-plan-aperçu de la valeur de propriété](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Si la barre de couleurs n’a pas la couleur exacte que vous souhaitez, vous pouvez utiliser le menu contextuel du sélecteur de couleurs. Pour l’ouvrir, cliquez sur le double Chevron à l’extrémité droite de la barre de couleurs, ou appuyez sur la flèche bas une fois ou deux fois sur le clavier.

![Menu contextuel du sélecteur de couleurs CSS](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Cliquez sur une couleur à partir de la barre verticale à droite. Cela montre un dégradé pour cette couleur dans la fenêtre principale. Choisissez une couleur directement à partir de la barre verticale en appuyant sur entrée ou en cliquant sur n’importe quel point dans la fenêtre principale pour choisir avec une plus grande précision.

S’il y a une couleur sur l’écran de l’ordinateur que vous souhaitez utiliser (il n’est pas nécessaire qu’il se trouve à l’intérieur de l’interface utilisateur de Visual Studio), vous pouvez capturer sa valeur à l’aide de l’outil pipette situé en bas à droite.

Vous pouvez également modifier l’opacité d’une couleur en déplaçant le curseur en bas du sélecteur de couleurs. Cela modifie les valeurs de couleur en valeurs RVBA, car le format RVBA peut représenter l’opacité.

Une fois que vous avez choisi une couleur, appuyez sur entrée, puis tapez un point-virgule pour terminer l’entrée de couleur d’arrière-plan dans le fichier *site. CSS* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Barre de mise à jour Inspecteur de page

Inspecteur de page détecte immédiatement la modification apportée au fichier *site. CSS* et affiche une alerte dans une barre de mise à jour.

![Barre de mise à jour](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Pour enregistrer tous vos fichiers et actualiser le navigateur Inspecteur de page, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour. La modification de la couleur de surbrillance s’affiche dans le navigateur.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Mappage d’éléments de page dynamiques à JavaScript

Dans les applications Web modernes, les éléments de la page sont souvent générés de manière dynamique avec JavaScript. Cela signifie qu’il n’y a pas de balisage statique (HTML ou Razor) qui correspond à ces éléments de page.

Avec la version 1,3, Inspecteur de page pouvez maintenant mapper des éléments qui ont été ajoutés dynamiquement à la page vers le code JavaScript correspondant. Pour illustrer cette fonctionnalité, nous allons utiliser le [modèle d’application à page unique (Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Le modèle SPA requiert la mise à jour [ASP.NET et Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .

Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**. À gauche, développez **visuel C#** , sélectionnez **Web**, puis sélectionnez **application Web ASP.net MVC4**. Cliquez sur **OK**.

Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez **application à page unique**.

Dans Explorateur de solutions, développez le dossier **views** , puis le dossier de **démarrage** . Cliquez avec le bouton droit sur le fichier index. cshtml, puis choisissez **afficher dans inspecteur de page**.

La première chose affichée dans le navigateur Inspecteur de page est une page de connexion. Cliquez sur « s’inscrire » et créez un nom d’utilisateur et un mot de passe. Une fois inscrit, l’application vous connecte et crée une liste des tâches avec quelques exemples d’éléments.

Cliquez sur **inspecter** pour placer Inspecteur de page dans mode d’inspection. Dans le navigateur Inspecteur de page, cliquez sur l’un des éléments de tâche. Notez qu’au lieu d’être mis en surbrillance en bleu, l’élément est mis en surbrillance en orange, avec « JS » en regard du nom de l’élément. Cela indique que l’élément a été créé dynamiquement par le biais d’un script.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

En outre, un trait de soulignement orange apparaît sous l’onglet **pile des appels** . Cela indique que le volet **pile des appels** contient plus d’informations sur l’élément.

Cliquez sur l’onglet **pile des appels** . Le volet **pile des appels** affiche la pile des appels pour l’appel JavaScript qui a créé l’élément. Les appels à des bibliothèques externes telles que jQuery sont réduits, afin que vous puissiez facilement voir les appels à votre script d’application.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Pour voir la pile complète, y compris les appels aux bibliothèques externes, vous pouvez développer les nœuds étiquetés « bibliothèques externes » :

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Si vous cliquez sur un élément dans la pile des appels, Visual Studio ouvre le fichier de code et met en surbrillance le script correspondant.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Voir aussi

[Introduction à ASP.NET MVC 4 avec Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (site Web ASP.net)

[Présentation de inspecteur de page](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vidéo Channel 9)

[Inspecteur de page des messages d’erreur](https://go.microsoft.com/?linkid=9813062) (MSDN)
