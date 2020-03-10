---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Nouveautés dans ASP.NET et le développement Web dans Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: La nouvelle version de Visual Studio introduit un certain nombre d’améliorations axées sur l’amélioration de l’expérience et des performances lors de l’utilisation des technologies Web...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 80c77ec65ed86b06e417d3f6ba608e404c46768b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525934"
---
# <a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Nouveautés du développement ASP.NET et web dans Visual Studio 2012

par l' [équipe Web camps](https://twitter.com/webcamps)

> La nouvelle version de Visual Studio introduit un certain nombre d’améliorations axées sur l’amélioration de l’expérience et des performances lors de l’utilisation des technologies Web. Les éditeurs Visual Studio pour CSS, JavaScript et HTML ont été entièrement remaniés pour inclure de nombreuses aides au code à la demande, telles que IntelliSense et la mise en retrait automatique. En ce qui concerne les performances, le regroupement et la minimisation sont maintenant intégrés en tant que fonctionnalités intégrées pour réduire facilement le temps de chargement des pages.
> 
> Visual Studio vous permet d’utiliser les dernières technologies de site Web. Vous pouvez utiliser des extraits de code CSS3 multinavigateurs pour vous assurer que votre site fonctionne indépendamment de la plateforme cliente tout en tirant parti des nouveaux éléments et fonctionnalités HTML5.
> 
> L’écriture et le profilage du code JavaScript doit être plus facile avec cette version de Visual Studio. Les listes IntelliSense, la documentation XML intégrée et les fonctionnalités de navigation sont désormais disponibles pour le code JavaScript. Vous avez maintenant le catalogue JavaScript à portée de main. En outre, vous pouvez vérifier la conformité ECMAScript5 avec vos scripts et détecter les erreurs de syntaxe à un moment précoce.
> 
> Enfin, cette version de Visual Studio implémente le regroupement intégré et la minimisation. Vos fichiers de script et feuilles de style seront empaquetés et compressés pour que le site fonctionne plus rapidement.
> 
> Ce laboratoire vous guide tout au long des améliorations et des nouvelles fonctionnalités décrites précédemment en appliquant des modifications mineures à un exemple d’application Web fournie dans le dossier source.
> 
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Utiliser les nouvelles fonctionnalités et améliorations de l’éditeur CSS
- Utiliser les nouvelles fonctionnalités et améliorations de l’éditeur HTML
- Utiliser les nouvelles fonctionnalités et améliorations de l’éditeur JavaScript
- Configurer et utiliser le regroupement et la minimisation

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (pour les scripts d’installation-déjà installés sur Windows 8 et windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) ou navigateur compatible avec HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique comprend les exercices suivants :

1. [Exercice 1 : nouveautés de l’éditeur CSS](#Exercise1)
2. [Exercice 2 : nouveautés de l’éditeur HTML](#Exercise2)
3. [Exercice 3 : nouveautés de l’éditeur JavaScript](#Exercise3)
4. [Exercice 4 : regroupement et minimisation](#Exercise4)

Durée estimée pour effectuer ce laboratoire : **60 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Exercice 1 : nouveautés de l’éditeur CSS

Les développeurs Web doivent être familiarisés avec la plupart des difficultés liées à l’édition CSS. L’un des plus grands problèmes de style CSS est la compatibilité entre navigateurs. Il arrive souvent que, après avoir appliqué des styles à votre site, vous remarquez qu’il semble différent si vous l’ouvrez dans un autre navigateur ou appareil. Par conséquent, vous pouvez consacrer beaucoup de temps à la résolution de ces problèmes visuels pour vous rendre compte que, lorsque vous le faites pour le faire fonctionner dans un navigateur, il est rompu dans les autres.

Visual Studio propose désormais des fonctionnalités qui aident les développeurs à accéder à, à travailler et à organiser les feuilles de style CSS de manière efficace. Tout au long de cet exercice, vous allez rencontrer les nouvelles fonctionnalités d’une organisation et d’une édition efficaces, ainsi que les extraits de code CSS3 pour la compatibilité entre les navigateurs.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Tâche 1-nouvelles fonctionnalités de l’éditeur

Au cours de cette tâche, vous allez découvrir les nouvelles fonctionnalités de l’éditeur CSS. Ce nouvel éditeur vous permet d’accroître votre productivité en tirant parti de la nouvelle mise en retrait intelligente, des commentaires de code améliorés et de la liste IntelliSense améliorée.

1. Démarrez **Visual Studio** et ouvrez la solution **WhatsNewASPNET. sln** qui se trouve dans le dossier **Source\WhatsNewASPNET** de ce Lab.
2. Dans Explorateur de solutions, ouvrez le fichier **site. CSS** situé sous le dossier **styles** . Assurez-vous que les outils **éditeur de texte** sont visibles dans la barre d’outils. Pour ce faire, sélectionnez l’option de menu **afficher** | les **barres d’outils** , puis vérifiez les options de l' **éditeur de texte** . Vous remarquerez que, depuis cette nouvelle version, le bouton **Commentaire** (![bouton commentaire](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) et le bouton Annuler les **marques** de commentaire (![bouton supprimer les marques de commentaire](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) sont également activés pour l’éditeur CSS.

    ![Activation de l’éditeur et des outils CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Activation de l’éditeur et des outils CSS")

    *Activation de l’éditeur et des outils CSS*
3. Faites défiler le code et sélectionnez une définition de classe CSS. Cliquez sur le bouton **Commentaire** (![bouton commentaire](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) pour commenter les lignes sélectionnées. Ensuite, cliquez sur le bouton Annuler les **marques de commentaire** (![bouton supprimer les marques de commentaire](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png)) pour annuler les modifications.
4. Cliquez sur le bouton **réduire** (![réduire](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) et **développez** les boutons (![développer](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) situés dans la marge de gauche du texte. Notez que vous pouvez maintenant masquer les styles que vous n’utilisez pas pour obtenir une vue plus propre.

    ![Réduction des classes CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Réduction des classes CSS")

    *Réduction des classes CSS*
5. Assurez-vous que la fonctionnalité de mise en retrait intelligente est activée. Sélectionnez l’option de menu **outils** | **options** , puis sélectionnez l' **éditeur de texte** | page **mise en forme** **CSS** | dans le volet gauche de l’écran. Activez l’option de **mise en retrait hiérarchique** .

    ![Activation de la mise en retrait hiérarchique](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Activation de la mise en retrait hiérarchique")

    *Activation de la mise en retrait hiérarchique*
6. Localisez la définition de classe principale (. main) et ajoutez un style aux éléments div. Vous remarquerez que le code s’aligne automatiquement, aidant ainsi les utilisateurs à trouver les classes parentes en un clin d’œil.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Alignement hiérarchique en CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Alignement hiérarchique en CSS")

    *Alignement hiérarchique en CSS*
7. À l’intérieur de la classe **div principale** , localisez le curseur à la fin de **Border : 0px ;** et appuyez sur **entrée** pour afficher la liste IntelliSense. Commencez à taper **Top** et notez la façon dont la liste est filtrée au fur et à mesure que vous tapez. La liste affiche les éléments qui contiennent **Top** dans une partie du mot (dans les versions antérieures de Visual Studio, la liste est filtrée par les éléments qui *commencent* par le terme).

    ![Améliorations d’IntelliSense dans CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "Améliorations d’IntelliSense dans CSS")

    *Améliorations d’IntelliSense dans CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Tâche 2 : sélecteur de couleurs

Au cours de cette tâche, vous allez découvrir le nouveau sélecteur de couleurs CSS intégré à Visual Studio IntelliSense.

1. Dans **site. CSS,** Localisez la définition de classe d’en-tête (. Header) et placez le curseur en regard de l’attribut **de couleur d’arrière-plan** , entre les caractères &quot;:&quot; et &quot;#&quot; sur cette ligne de code **.**

    ![Recherche du curseur](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Recherche du curseur")

    *Recherche du curseur*
2. Supprimez le **signe deux-points** ( :) et écrivez-le à nouveau pour afficher le sélecteur de couleurs. Notez que les couleurs les plus fréquentes de votre site s’affichent dans les premières couleurs. Si vous cliquez sur la couleur blanche, son code de couleur HTML (#fff) remplace le code de couleur actuel dans la feuille de style.

    ![Sélecteur de couleurs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Sélecteur de couleurs")

    *Sélecteur de couleurs*
3. Appuyez sur le bouton **développer** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png)) du sélecteur de couleurs pour afficher le dégradé de couleur, puis faites glisser le curseur dégradé pour sélectionner une couleur différente. Après cela, cliquez sur le bouton **pipette** et sélectionnez une couleur à partir de l’écran. Notez que la valeur de couleur d’arrière-plan change de manière dynamique pendant que vous déplacez le curseur.

    ![Dégradé du sélecteur de couleurs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Dégradé du sélecteur de couleurs")

    *Dégradé du sélecteur de couleurs*
4. Dans le curseur **opacité** , déplacez le sélecteur vers le centre de la barre pour réduire l’opacité. Notez que la valeur de la couleur d’arrière-plan modifie désormais son échelle en RVBA.

    ![Opacité du sélecteur de couleurs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Opacité du sélecteur de couleurs")

    *Opacité du sélecteur de couleurs*

    > [!NOTE]
    > La définition de couleur RVBA (rouge, vert, bleu, alpha) dans CSS3 vous permet de définir la valeur d’opacité de couleur pour un seul élément. Contrairement **à l’opacité :** un attribut CSS similaire **-** couleurs RVBA sont également compatibles avec les navigateurs les plus récents.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Tâche 3 : extraits de code compatibles CSS

Au cours de cette tâche, vous allez apprendre à utiliser des extraits de code CSS3 compatibles entre navigateurs pour implémenter certaines fonctionnalités de votre site Web.

1. Dans le fichier **site. CSS** , localisez la définition de classe CSS d' **en-** tête (. Header) et placez le curseur sous le **/\*\*de bordure RADIUS /** espace réservé pour ajouter un nouvel extrait de code. Appuyez sur **entrée** pour afficher la liste IntelliSense et tapez **RADIUS** pour filtrer la liste. Sélectionnez l’option **border-radius** dans la liste avec un double-clic, puis appuyez sur la touche **Tab** pour insérer l’extrait de code. Ensuite, tapez une taille de rayon en pixels, puis appuyez sur **entrée**. Par exemple, tapez **15px**.

    Les attributs CSS3 ajoutés par l’extrait de code affichent des bordures arrondies dans la plupart des navigateurs de conformité HTML5, y compris des navigateurs basés sur Mozilla et WebKit.

    ![Utilisation d’un extrait de code frontière-RADIUS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Utilisation d’un extrait de code frontière-RADIUS")

    *Utilisation d’un extrait de code frontière-RADIUS*
2. Appliquez les mêmes extraits de **bordure** dans le style de page (. page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Appuyez sur **F5** pour exécuter la solution. Notez que chaque page a maintenant des bordures arrondies.

    ![Angles arrondis](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Angles arrondis")

    *Angles arrondis*
4. Fermez le navigateur et revenez à Visual Studio.
5. Ouvrez le fichier **. CSS personnalisé** situé sous le dossier **styles** , puis placez le curseur à l’intérieur de la définition de classe **img UL Li. images** .
6. Appuyez sur entrée pour afficher la liste IntelliSense, tapez **box-shadow** , puis appuyez deux fois sur la touche **Tab** pour insérer l’extrait de code Shadow par défaut à l’intérieur de la définition de classe. Définissez les valeurs d’ombre sur **10px 10px 5px #888**. Ensuite, tapez **border-radius** et insérez l’extrait de code. Tapez **15px** pour définir la taille du rayon et appuyez sur **entrée**.

    ![Angles arrondis avec ombre](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Angles arrondis avec ombre")

    *Angles arrondis avec ombre*

    > [!NOTE]
    > À ce stade, l’attribut Shadow est inséré avec le préfixe correspondant (moz, WebKit, o) pour prendre en charge les navigateurs Mozilla et WebKit (chrome, Safari, Konkeror).
7. Créez une nouvelle classe **div. images UL Li img : Placez** le curseur sous la définition de classe **img Li. images UL Li** et placez le curseur à l’intérieur des crochets **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Tapez **Transform** , puis appuyez deux fois sur la touche **Tab** pour insérer l’extrait de transformation. Ensuite, entrez **Rotate (-15deg)** pour modifier la valeur de l’angle de rotation lorsque les images sont survolées.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Appuyez sur **F5** pour exécuter la solution et accéder à la page CSS3. Notez que les images ont des angles arrondis et des ombres de zone. Pointez la souris sur les images et regardez-les pivoter.

    ![Extrait de transformation permettant de faire pivoter une image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Extrait de transformation permettant de faire pivoter une image")

    *Extrait de transformation permettant de faire pivoter une image*

    > [!NOTE]
    > Si vous utilisez Internet Explorer 10 et que vous ne voyez pas les ombres, assurez-vous que le mode de document est défini sur normes IE10. Appuyez sur **F12** pour ouvrir les outils de développement Internet Explorer, puis cliquez sur **mode de document** pour passer aux normes IE10.

    ![à propos de-US](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Exercice 2 : nouveautés de l’éditeur HTML

Visual Studio dispose d’un éditeur HTML amélioré. Certaines des améliorations apportées à cette version sont la mise en retrait intelligente dans les documents HTML, les extraits HTML5, la correspondance des balises de début et de fin HTML et la validation HTML. Tout au long de cet exercice, vous verrez comment ces modifications améliorent votre aisance quand vous travaillez dans le balisage de site Web.

À l’instar de l’éditeur CSS, l’éditeur HTML a également été amélioré. La plupart de ces améliorations sont de petits éléments qui facilitent la vie des développeurs Web. Parmi ces améliorations, vous pouvez obtenir des extraits de code supplémentaires pour HTML5, une mise en retrait intelligente, des balises de début et de fin correspondantes lors de la modification et de la validation ciblant le document HTML DOCTYPE.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Tâche 1 : validation de DOCTYPE améliorée

L’éditeur HTML a désormais la possibilité de vérifier le DOCTYPE de votre page, même si la définition peut figurer dans la page maître. En fonction du DOCTYPE de votre page, l’éditeur HTML effectue une validation avec l’ensemble de règles approprié et filtre la liste IntelliSense en tenant compte des éléments DOCTYPE.

Dans cette tâche, vous allez modifier le DOCTYPE d’une page pour voir comment le comportement de l’éditeur HTML change en conséquence.

1. S’il n’est pas déjà ouvert, démarrez **Visual Studio** et ouvrez la solution **WhatsNewASPNET. sln** qui se trouve dans le dossier **Source\WhatsNewASPNET** de ce Lab.
2. Ouvrez la page **site. Master** .
3. Notez le schéma cible pour la barre d’outils validation. Le comportement de l’éditeur HTML (validation, IntelliSense, etc.) change correctement pour s’adapter à la déclaration DOCTYPE sélectionnée.

    ![Utiliser DOCTYPE dans la barre d’outils modification de la source HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Utiliser DOCTYPE dans la barre d’outils modification de la source HTML")

    *Utiliser DOCTYPE dans la barre d’outils modification de la source HTML*
4. Remplacez le schéma cible par HTML 4,01.

    ![Modification de DOCTYPE dans la barre d’outils modification de la source HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Modification de DOCTYPE dans la barre d’outils modification de la source HTML")

    *Modification de DOCTYPE dans la barre d’outils modification de la source HTML*
5. Placez le curseur sous l’élément **Body** et commencez à taper le nom d’un élément HTML5 (par exemple, **vidéo**). Notez que l’élément n’est pas disponible dans la liste IntelliSense.

    ![Éléments HTML5 non listés](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "Éléments HTML5 non listés")

    *Éléments HTML5 non listés*
6. Annulez les modifications apportées au schéma cible pour la barre d’outils validation, en sélectionnant DOCTYPE : XHTML5 dans la liste déroulante.

    ![Utiliser DOCTYPE dans la barre d’outils modification de la source HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Utiliser DOCTYPE dans la barre d’outils modification de la source HTML")

    *Réinitialiser DOCTYPE dans la barre d’outils modification de la source HTML*
7. Placez le curseur sous l’élément **Body** et commencez à taper un élément HTML5 (par exemple, comme une **vidéo**). Notez que les éléments HTML5 sont désormais disponibles dans la liste IntelliSense.

    ![Éléments HTML5 en cours de liste](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "Éléments HTML5 en cours de liste")

    *Éléments HTML5 en cours de liste*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Tâche 2 : mise à jour automatique des balises de début/fin

Visual Studio met désormais à jour les balises d’ouverture ou de fermeture HTML de l’élément que vous modifiez pour qu’elles correspondent les unes aux autres. Cette nouvelle fonctionnalité améliore votre productivité lors de la modification des balises HTML.

1. Sur la page **default. aspx** , ajoutez un élément **H3** avec un titre (par exemple, Visual Studio 2012 Rocks !).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Modifiez la balise **H3** et tapez **H2** ou **H1.**

    Notez que la balise de fin est automatiquement mise à jour. Vous pouvez également modifier la balise de fin pour voir que la balise de début est mise à jour en conséquence.

    ![Mise à jour automatique de la balise de fin](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Mise à jour automatique de la balise de fin")

    *Mise à jour automatique de la balise de fin*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Tâche 3 : nouveaux extraits de code HTML5

Visual Studio comprend désormais plusieurs extraits de code HTML5. Dans cette tâche, vous allez utiliser certains de ces extraits.

1. Ajoutez un nouveau dossier nommé **audio** à la racine du dossier du site Web. Ouvrez l’Explorateur Windows et copiez tout fichier audio dans le dossier **audio** de la solution **WhatsNewASPNET. sln** .
2. Dans la page **default. aspx** , localisez le curseur sous la Web11 Rocks !! En-tête. Tapez **audio** , puis appuyez sur la touche Tab.

    Le nouvel éditeur HTML comprend des extraits de code pour le contenu HTML5. N’oubliez pas d’utiliser la définition DOCTYPE appropriée pour activer les extraits HTML5.

    ![Insertion d’extraits de code HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Insertion d’extraits de code HTML5")

    *Insertion d’extraits de code HTML5*
3. Mettez à jour la source audio pour pointer vers un fichier audio existant.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Vous devez ajouter le fichier audio à la solution.
4. Appuyez sur **F5** pour exécuter le site et lire l’audio.

    ![Exécution du contrôle audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Exécution du contrôle audio")

    *Exécution du contrôle audio*

    > [!NOTE]
    > Vous pouvez également essayer d’autres extraits de code inclus dans Visual Studio, tels que des vidéos, des figures, etc.
5. À présent, essayez d’insérer un contrôle dans une partie de la page. Par exemple, essayez d’insérer un contrôle **GridView** , mais au lieu de taper **&lt;Juster,** commencez à taper **&lt;GV**. Notez que la liste IntelliSense affiche le contrôle **asp : GridView** .

    IntelliSense dans l’éditeur HTML fournit désormais une recherche par titre-respect de la casse, ainsi qu’une correspondance partielle (récupérant tous les éléments qui contiennent le terme).

    ![Insertion d’un GridView avec des listes IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Insertion d’un GridView avec des listes IntelliSense")

    *Insertion d’un GridView avec des listes IntelliSense*

    Si vous tapez **&lt;grille** , vous obtiendrez tous les éléments qui correspondent au terme, mais Visual Studio suggère le contrôle **GridView** :

    ![Insertion d’un GridView avec des listes IntelliSense et une correspondance partielle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Insertion d’un GridView avec des listes IntelliSense et une correspondance partielle")

    *Insertion d’un GridView avec des listes IntelliSense et une correspondance partielle*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Tâche 4 : balises actives de l’éditeur HTML

Une autre amélioration apportée à l’éditeur HTML est la fonctionnalité de balises actives. Les balises actives facilitent l’exécution de tâches de développement courantes ou répétitives par contrôle. Cette fonctionnalité était déjà disponible dans le Concepteur HTML, mais pas dans l’éditeur HTML.

1. Ouvrez **site. Master** et recherchez l’élément **asp : menu** . Placez le curseur sur la balise de début et notez que le petit glyphe affiché en bas de l’élément-cliquez dessus pour ouvrir le menu tâches intelligentes. Notez que vous pouvez accéder rapidement à certaines tâches relatives au contrôle Menu.

    ![Tâches intelligentes pour le contrôle Menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Tâches intelligentes pour le contrôle Menu")

    *Tâches intelligentes pour le contrôle Menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Tâche 5-retrait intelligent

L’une des meilleures pratiques en HTML consiste à mettre en retrait les éléments imbriqués pour que le code reste lisible. Dans Visual Studio 2012, vous remarquerez que l’éditeur met automatiquement en retrait les éléments lors de l’écriture du code.

> [!NOTE]
> Dans la version précédente de Visual Studio, la mise en retrait intelligente était disponible dans l’éditeur XML, mais pas dans l’éditeur HTML.

1. Assurez-vous que la configuration de mise en retrait de l’éditeur HTML est définie sur mise en retrait intelligente. Pour ce faire, sélectionnez **Outils |** Option du menu options, puis sélectionnez l' **éditeur de texte | HTML | Page tabulations** dans le volet gauche de l’écran. Sélectionnez l’option mise en retrait intelligente.

    ![Paramètres de l’éditeur HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "Paramètres de l’éditeur HTML")

    *Paramètres de l’éditeur HTML*
2. Sur la page **default. aspx** , supprimez tout le contenu sous l’élément audio.
3. Placez le curseur à la fin de l’élément **audio** ouvrant et appuyez sur **entrée**.

    Notez que la nouvelle position du curseur a un niveau de mise en retrait supplémentaire.

    ![Mise en retrait intelligente dans l’éditeur HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Mise en retrait intelligente dans l’éditeur HTML")

    *Mise en retrait intelligente dans l’éditeur HTML*
4. Restaurez la balise audio avec le contenu que vous avez supprimé, ou fermez **default. aspx** sans enregistrer les modifications.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Tâche 6 : extraire vers le contrôle utilisateur

Les outils de refactorisation inclus dans Visual Studio, tels que l’extraction d’une partie de code dans une fonction, sont de formidables fonctionnalités qui facilitent l’amélioration et la refactorisation du code existant. L’équivalent pour les pages ASP.NET est l’extraction du code HTML vers un contrôle utilisateur. Le faire manuellement implique plusieurs étapes, telles que la création d’un nouveau contrôle utilisateur, le déplacement de la section de code vers le contrôle utilisateur, l’inscription d’un préfixe de balise pour le contrôle utilisateur et, enfin, l’instanciation du contrôle utilisateur sur les pages. Désormais, le nouvel outil d' *extraction vers le contrôle utilisateur* effectue automatiquement toutes ces étapes pour vous.

Dans cette tâche, vous allez utiliser la nouvelle opération contextuelle Extract to User Control pour générer un nouveau contrôle utilisateur à partir du code sélectionné.

1. Sur la page **default. aspx** , sélectionnez les éléments **H2** et **audio** .
2. Cliquez avec le bouton droit et sélectionnez **extraire vers le contrôle utilisateur**.

    ![Option de menu extraire vers le contrôle utilisateur](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Option de menu extraire vers le contrôle utilisateur")

    *Option de menu extraire vers le contrôle utilisateur*
3. Tapez un nom pour le nouveau contrôle utilisateur. Par exemple, **Jukebox. ascx**, puis cliquez sur **OK**.

    ![Enregistrement du contrôle utilisateur extrait](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Enregistrement du contrôle utilisateur extrait")

    *Enregistrement du contrôle utilisateur extrait*
4. Notez que le code sélectionné a été extrait vers un contrôle utilisateur et que l’emplacement d’origine du code sélectionné a été remplacé par une instance du nouveau contrôle utilisateur.

    ![Page mise à jour automatiquement pour utiliser le nouveau contrôle utilisateur](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page mise à jour automatiquement pour utiliser le nouveau contrôle utilisateur")

    *Page mise à jour automatiquement pour utiliser le nouveau contrôle utilisateur*
5. Appuyez sur **F5** pour exécuter la page et vérifier que le contrôle fonctionne.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Exercice 3 : nouveautés de l’éditeur JavaScript

L’écriture ou la modification de code JavaScript n’est pas une tâche facile, surtout lorsque la taille de votre application commence à croître et que vous vous contentez de gérer des fichiers longs et des centaines de fonctions. Les développeurs de script doivent généralement effectuer des tâches supplémentaires pour maintenir la lisibilité du code et naviguer entre les fichiers. Avec l’inclusion de bibliothèques JavaScript telles que jQuery, la navigation dans les scripts est devenue un véritable défi en raison de la longueur du code.

Visual Studio a renouvelé l’éditeur JavaScript avec la promesse de rendre le mode Code accessible et organisé. De nombreuses fonctionnalités de Visual Studio qui existaient C# déjà dans ou les éditeurs VB sont désormais implémentées dans l’éditeur JavaScript : atteindre la définition, mise en retrait automatique, documentation et validation lors de l’écriture. Avec la liste IntelliSense renouvelée, vous disposerez du catalogue de fonctions JavaScript à portée de main.

Dans cet exercice, vous allez découvrir quelques-unes des nouvelles fonctionnalités et améliorations de l’éditeur JavaScript. Vous allez parcourir les fichiers d’exemple et découvrir chacune des nouvelles caractéristiques qui faciliteront l’efficacité de la programmation JavaScript dans Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Tâche 1 : éditeur JavaScript nouvelles fonctionnalités

Cette tâche vous présente quelques-unes des nouvelles fonctionnalités de l’éditeur JavaScript, qui se concentrent sur l’organisation de votre code et l’amélioration de l’expérience utilisateur.

1. S’il n’est pas déjà ouvert, démarrez **Visual Studio** et ouvrez la solution **WhatsNewASPNET. sln** qui se trouve dans le dossier **Source\WhatsNewASPNET** de ce Lab.
2. Appuyez sur **F5** pour exécuter l’application, puis cliquez sur le lien JavaScript dans la barre de navigation. Actualisez la page plusieurs fois et vérifiez comment le compteur s’incrémente.

    ![Compteur de pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Compteur de pages")

    *Compteur de pages*
3. Fermez le navigateur et revenez à Visual Studio.
4. Ouvrez la page **JavaScript. aspx** et recherchez le **&lt;** bloc de&gt;de script (illustré ci-dessous).

    Le code suivant utilise le stockage local HTML5 pour stocker une variable *pageLoadCount* qui stocke le nombre de fois que la page a été visitée par l’utilisateur actuel. Le stockage local est une base de données de valeurs de clé côté client introduite avec la norme HTML5. Les données sont enregistrées sur l’ordinateur local, à l’intérieur du navigateur de l’utilisateur.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Vérifiez que la déclaration DOCTYPE est définie sur XHTML5 avant de passer aux étapes suivantes.
5. Modifiez le code et Notez qu’IntelliSense pour JavaScript comprend des fonctionnalités HTML5, telles que le stockage local et leurs méthodes internes.

    ![Fonctionnalités JavaScript HTML5 dans JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "Fonctionnalités JavaScript HTML5 dans JavaScript")

    *Fonctionnalités JavaScript HTML5 dans JavaScript*
6. Cliquez sur n’importe quel crochet ouvrant ( **{** ) à partir du code de script et notez que les crochets sont mis en surbrillance.

    ![Les crochets sont mis en surbrillance](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Les crochets sont mis en surbrillance")

    *Les crochets sont mis en surbrillance*
7. Supprimez les marques de commentaire de la fonction **testAutoAlign ()** (sélectionnez les trois lignes et vous pouvez utiliser **CTRL** + **K**; **CTRL** + **U**) et localiser le curseur à l’intérieur du code de la fonction. Appuyez sur entrée pour ajouter une deuxième ligne. Notez que le code est maintenant **aligné** et **mis en retrait automatique**.

    ![Le code JavaScript est aligné automatiquement](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "Le code JavaScript est aligné automatiquement")

    *Le code JavaScript est aligné automatiquement*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Tâche 2 : validation de JavaScript

Dans cette tâche, vous allez découvrir la nouvelle validation JavaScript pour la norme ECMAScript5. Cette fonctionnalité vous aide à écrire du code JavaScript conforme, tout en empêchant les problèmes de script avant le déploiement de site.

> [!NOTE]
> Visual Studio 2010 a implémenté la conformité ECMAStript3, tandis que Visual Studio 2012 fournit la conformité ECMAScript5.

1. Ouvrez **ECMA5script5. js** situé dans le dossier du projet **Scripts\custom** . Vous allez maintenant tester la validation de la norme ECMAScript5.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Vous pouvez extraire le &quot; **utiliser le strict** &quot; sens de la première ligne du fichier, ce qui active le **mode strict**ECMAScript5. Ce mode se compose d’un sous-ensemble du langage qui clarifie les ambiguïtés de l’édition précédente et ajoute de nouvelles fonctionnalités, telles que les getters et les Setters, la prise en charge de la bibliothèque pour JSON et une réflexion plus complète sur les propriétés de l’objet.
2. Ouvrir le **liste d’erreurs** s’il n’est pas déjà ouvert (menu**affichage** | **Liste d’erreurs**). Notez que la déclaration de **fonction** est soulignée. En effet, dans les fonctions ECMA5 standard ne peuvent pas être imbriquées dans des structures de langage. Les détails de l’avertissement s’affichent dans la liste d’erreurs ci-dessous.

    ![Message d’erreur de validation JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "Message d’erreur de validation JavaScript")

    *Message d’erreur de validation JavaScript*
3. Commentez l' **&quot;utiliser une&quot;stricte** et remarquez que les erreurs disparaissent, mais que les avertissements sont conservés.
4. Dans la dernière ligne du fichier, écrivez une chaîne comme **&quot;&quot;de test** (en incluant les guillemets pour indiquer qu’il s’agit d’une chaîne). Écrivez un point en regard de la chaîne pour afficher la liste IntelliSense, puis sélectionnez l’option de **suppression** .

    Dans ECMAScript5 standard, les valeurs de chaîne et les variables ont également des méthodes de chaîne définies, comme Trim, UpperCase, Search et replace.

    ![Liste IntelliSense dans JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "Liste IntelliSense dans JavaScript")

    *Liste IntelliSense dans JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Tâche 3 : documentation XML pour JavaScript

Dans cette tâche, vous allez explorer les fonctionnalités de Visual Studio documentation XML dans JavaScript. Vous verrez que la liste JavaScript IntelliSense affiche désormais la documentation XML de chaque fonction. En outre, vous découvrirez la fonctionnalité de navigation dans JavaScript.

1. Ouvrez le fichier **xmlDoc. js** situé dans le dossier **scripts/projet personnalisé** . Ce fichier contient une documentation XML sur chacune des fonctions JavaScript.

    ![Documentation XML JavaScript intégrée à IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "Documentation XML JavaScript intégrée à IntelliSense")

    *Documentation XML JavaScript intégrée à IntelliSense*
2. Ci-dessous, **Ajoutez** une fonction dans le fichier **xmlDoc. js** , puis créez une nouvelle fonction nommée **test**.
3. Dans la fonction de **test** , appelez la fonction **Multiply** qui reçoit deux paramètres. Notez que la zone info-bulle indique la documentation de la fonction **multiplier** .

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Documentation XML pour les fonctions JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "Documentation XML pour les fonctions JavaScript")

    *Documentation XML pour les fonctions JavaScript*
4. Complétez l’instruction de l’appel de fonction et tapez un *point* pour ouvrir la liste IntelliSense sur la valeur retournée. Notez que Visual Studio détecte la valeur de **retour** dans la documentation, en traitant la valeur comme un nombre.

    ![Documentation XML pour les types de retour](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "Documentation XML pour les types de retour")

    *Documentation XML pour les types de retour*
5. À présent, insérez un appel pour ajouter une fonction. Notez que l’éditeur JavaScript prend désormais en charge les surcharges de fonction. Lorsque vous écrivez un nom de fonction, vous pouvez sélectionner l’une des surcharges disponibles spécifiées dans la documentation.

    ![Documentation XML pour les surcharges](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "Documentation XML pour les surcharges")

    *Documentation XML pour les surcharges*
6. Ouvrez le fichier **GotoDefinition. js** et recherchez l’appel de fonction **$ (). html ()** . Localisez le curseur sur **HTML**.
7. Appuyez sur **F12** et accédez à la définition. Notez que vous pouvez désormais accéder à votre code JavaScript et le parcourir sans utiliser l’outil de **recherche** .
8. Localisez le curseur sur l’instruction jQuery avant le bloc de signature en bas du fichier de code. Appuyez sur **F12**. Vous allez accéder au fichier de bibliothèque jQuery. Notez que vous pouvez également naviguer dans les fichiers jQuery à l’aide de la **touche F12**.

    ![Navigation vers les définitions jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigation vers les définitions jQuery")

    *Navigation vers les définitions jQuery*

> [!NOTE]
> Assurez-vous que GotoDefinition. js n’a pas d’erreurs de syntaxe avant d’enregistrer le fichier.

<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Exercice 4 : regroupement et minimisation

Combien de fois vos sites Web incluent-ils plusieurs fichiers JavaScript ou CSS ? Il s’agit d’un scénario très courant dans lequel le regroupement et la minimisation peuvent aider à réduire la taille du fichier et accélérer l’exécution du site. La nouvelle fonctionnalité de regroupement dans ASP.NET 4,5 compresse un ensemble de fichiers JS ou CSS en un seul élément et réduit sa taille en minimisation le contenu (c’est-à-dire en supprimant les espaces vides, en supprimant les commentaires et en réduisant les identificateurs).

Le regroupement et la minimisation dans ASP.NET 4,5 sont effectués au moment de l’exécution, ce qui permet au processus d’identifier l’agent utilisateur (par exemple, IE, Mozilla, etc.) et, par conséquent, d’améliorer la compression en ciblant le navigateur de l’utilisateur (par exemple, en supprimant des éléments qui sont spécifiques à Mozilla). Lorsque la demande provient d’IE).

Dans cet exercice, vous allez apprendre à activer et à utiliser les différents types de regroupement et de minimisation dans ASP.NET 4,5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Tâche 1 : installation du package de regroupement et de minimisation à partir de NuGet

1. S’il n’est pas déjà ouvert, démarrez **Visual Studio** et ouvrez la solution **WhatsNewASPNET. sln** qui se trouve dans le dossier **Source\WhatsNewASPNET** de ce Lab.
2. Ouvrez la console du gestionnaire de package NuGet. Pour ce faire, utilisez la **vue** de menu | autre **console du gestionnaire de package** **Windows** | .

    ![Ouverture du gestionnaire de package file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Ouverture de la console du gestionnaire de package")

    *Ouverture de la console du gestionnaire de package*
3. Dans la **console du gestionnaire de package,** tapez **install-package Microsoft. Web. Optimization** , puis appuyez sur **entrée**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Tâche 2 : offres groupées par défaut

La façon la plus simple d’utiliser le regroupement et la minimisation consiste à activer les regroupements par défaut. Cette méthode utilise des conventions pour vous permettre de référencer la version regroupée et minimisés pour les fichiers JS et CSS dans un dossier.

Au cours de cette tâche, vous allez apprendre à activer et à référencer les fichiers JS et CSS regroupés et minimisés et à afficher le résultat obtenu.

1. S’il n’est pas déjà ouvert, démarrez **Visual Studio** et ouvrez la solution **WhatsNewASPNET. sln** qui se trouve dans le dossier **Source\WhatsNewASPNET** de ce Lab.
2. Dans le **Explorateur de solutions**, développez les dossiers **styles**, **Scripts\custom** et **Scripts\bundle** .

    Notez que l’application utilise plusieurs fichiers CSS et JS.

    ![Plusieurs feuilles de style et fichiers JavaScript dans l’application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Plusieurs feuilles de style et fichiers JavaScript dans l’application")

    *Plusieurs feuilles de style et fichiers JavaScript dans l’application*
3. Ouvrez le fichier **global.asax.cs** .

    Notez que le nouvel espace de noms **Microsoft. Web. Optimization** est mis en commentaire au début du fichier. Supprimez les marques de commentaire de la directive using pour inclure les fonctionnalités de regroupement et de minimisation.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Localisez la méthode de démarrage de l' **Application\_** .

    Dans cette méthode, supprimez les marques de commentaire de l’appel EnableDefaultBundles comme indiqué dans l’extrait de code ci-dessous. Cela nous permet de référencer une collection groupée de fichiers CSS dans un dossier à l’aide du chemin d’accès à ce dossier, plus le &quot;CSS&quot; ou le suffixe de&quot; &quot;JS.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Ouvrez le fichier **Optimization. aspx** et recherchez le contrôle de contenu pour **HeadContent**.

    Notez que les fichiers CSS et JS ont une seule balise référencée.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Ce code est fourni à des fins de démonstration. Dans l’idéal, vous allez référencer les regroupements dans le fichier site. Master. Dans cet exemple de code, vous constaterez que certains des fichiers regroupés sont également référencés par le fichier site. Master, ce qui rend cette dernière référence redondante.
6. Notez que les liens utilisent les conventions de regroupement dans l’attribut **href** pour obtenir tous les fichiers CSS ou js du dossier styles et Scripts\custom, respectivement.

    Vous pouvez utiliser le chemin **scripts/personnalisé/js** comme indiqué ci-dessous pour regrouper et réduire tous les fichiers js à l’intérieur d’un dossier **scripts/personnalisé** . Il s’agit du comportement par défaut avec les regroupements par défaut.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Ouvrez le fichier **Styles\Site.CSS** .

    Notez que le fichier CSS d’origine contient du code en retrait, des espaces vides et des commentaires qui agrandissent le fichier. (Le fichier JavaScript contient également des espaces et des commentaires vides).

    ![Un des fichiers CSS d’origine dans le dossier scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "Un des fichiers CSS d’origine dans le dossier scripts")

    *Un des fichiers CSS d’origine dans le dossier scripts*
8. Appuyez sur **F5** pour exécuter l’application et accéder à la page d' **optimisation** .
9. Cliquez sur le lien d' **ensemble CSS** pour télécharger et ouvrir le fichier.

    Consultez le fichier regroupé minimisés. Vous remarquerez que tous les espaces vides, les commentaires et les caractères de mise en retrait ont été supprimés, générant un fichier plus petit.

    ![Fichiers CSS regroupés](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Fichiers CSS regroupés")

    *Fichiers CSS regroupés*
10. Cliquez maintenant sur le lien de **regroupement js** pour ouvrir le fichier groupé JavaScript. Vous pouvez ignorer sans risque l’avertissement de l’Explorateur. Notez que les fichiers JavaScript dans le dossier **personnalisé** sont également regroupés et minimisés.

    ![Fichiers JavaScript regroupés](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Fichiers JavaScript regroupés")

    *Fichiers JavaScript regroupés*

    L’activation de la compression pour les fichiers CSS ou JS était bien plus compliquée dans la version précédente de ASP.NET. Comme vous l’avez vu, il vous suffit d’ajouter une ligne dans le fichier *global. asax* pour activer le regroupement, puis de référencer les fichiers regroupés à partir de votre site.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Tâche 3 : bottes statiques

L’approche de regroupement statique vous permet de personnaliser l’ensemble de fichiers à regrouper, la référence et la méthode de minimisation qui sera utilisée.

Dans cette tâche, vous allez configurer un bundle statique pour définir un ensemble spécifique de fichiers à regrouper et réduire.

1. Fermez le navigateur.
2. Ouvrez le fichier **global.asax.cs** et recherchez l' **application\_méthode Start** .
3. Supprimez les marques de commentaire du code de Bundle statique comme indiqué dans le code ci-dessous.

    Vous définissez un bundle statique qui sera référencé avec le &quot; **~/StaticBundle**&quot; chemin d’accès virtuel et utilisez **JsMinify** pour la minimisation de tous les fichiers spécifiés avec la méthode **AddFile** . Enfin, vous ajoutez le bundle statique à **BundleTable** et l’activez.

    Notez que les fichiers ne se trouvent pas dans le même emplacement ; Il s’agit d’un autre avantage par rapport au regroupement par défaut.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Ouvrez le fichier **Optimization. aspx** .

    Notez que le lien vers le **Bundle js statique** utilise le chemin d’accès que vous avez déclaré lorsque vous avez configuré le bundle statique dans le fichier global.asax.cs : **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Appuyez sur **F5** pour exécuter l’application, puis accédez à la page **optimisation** .
6. Cliquez sur le lien de l' **offre groupée js statique** pour ouvrir le fichier.

    Notez que le fichier JavaScript groupé minimisés est la sortie de tous les fichiers JavaScript configurés dans le fichier de Bundle statique sous le chemin d’accès &quot;/StaticBundle&quot;.

    ![Bundle de fichiers JavaScript statiques](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Bundle de fichiers JavaScript statiques")

    *Bundle de fichiers JavaScript statiques*
7. Fermez le navigateur et revenez à Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Tâche 4 : lots de dossiers dynamiques

Dans cette tâche, vous allez apprendre à configurer des groupes de dossiers dynamiques. La puissance du regroupement dynamique est que vous pouvez inclure du JavaScript statique, ainsi que d’autres fichiers dans des langages qui se compilent en JavaScript, et donc exiger un traitement avant l’exécution du regroupement.

Dans cet exemple, vous allez apprendre à utiliser la classe **DynamicFolderBundle** pour créer un bundle dynamique pour les fichiers écrits dans CofeeScript. CofeeScript est un langage de programmation qui se compile en JavaScript et fournit une syntaxe plus simple pour l’écriture de code JavaScript, en améliorant la concision et la lisibilité de JavaScript.

1. Ouvrez le fichier **global.asax.cs** et recherchez l' **application\_méthode Start** .
2. Supprimez les marques de commentaire du code de Bundle dynamique comme indiqué dans le code ci-dessous.

    Vous définissez un groupe de dossiers dynamiques qui utilisera le processeur de minimisation personnalisé **CoffeeMinify** qui s’appliquera uniquement aux fichiers avec l’extension &quot; **. Coffee**&quot; (fichiers CoffeeScript). Notez que vous pouvez utiliser un modèle de recherche pour sélectionner les fichiers à regrouper dans un dossier, par exemple «\*. Coffee ».

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Ouvrez la console du gestionnaire de package NuGet. Pour ce faire, utilisez la **vue** de menu | autre **console du gestionnaire de package** **Windows** | .
4. Dans la **console du gestionnaire de package,** tapez **Install-Package CoffeeSharp** , puis appuyez sur **entrée**.
5. Cliquez sur le bouton **Afficher tous les fichiers** dans la fenêtre **Explorateur de solutions**

    ![Montrer tous les fichiers](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Montrer tous les fichiers")

    *Montrer tous les fichiers*
6. Cliquez avec le bouton droit sur le fichier **CoffeeMinify.cs** dans le **Explorateur de solutions** et sélectionnez **inclure dans le projet**

    ![Inclure le fichier CoffeeMinify.cs dans le projet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Inclure le fichier CoffeeMinify.cs dans le projet")

    *Inclure le fichier CoffeeMinify.cs dans le projet*
7. Ouvrez le fichier **CoffeeMinify.cs** .

    Cette classe hérite de JsMinify pour réduire la sortie JavaScript résultant de la compilation de code CoffeeScript. Elle appelle le compilateur CoffeeScript pour générer le code JavaScript en premier, puis l’envoie à la méthode JsMinify. Process pour réduire le code résultant.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Ouvrez les fichiers **Script1. Coffee** et **script2. Coffee** à partir du dossier **scripts/Bundle** .

    Ces fichiers incluent le code CoffeScript à compiler lors de l’exécution du regroupement avec la classe CoffeeMinify.

    Pour des raisons de simplicité, les fichiers CoffeeScript fournis incluent uniquement l’exemple de code CoffeeScript. Les commentaires sont exclus par le processus JsMinify.

    ![Fichiers CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "Fichiers CoffeeScript")

    *Fichiers CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) fournit une syntaxe plus simple pour l’écriture de code JavaScript, l’amélioration de la concision et de la lisibilité de JavaScript, ainsi que l’ajout d’autres fonctionnalités telles que la compréhension du tableau et la mise en correspondance des modèles.
9. Ouvrez le fichier **Optimization. aspx** et recherchez les liens du bundle.

    Notez que le lien vers le **Bundle dynamique js** fait référence au dossier **scripts/Bundle** à l’aide du suffixe **/Coffee** que vous avez configuré pour le bundle de dossiers dynamiques.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Appuyez sur **F5** pour exécuter l’application, puis accédez à la page **optimisation** .
11. Cliquez sur le lien groupe de fichiers **js dynamiques** pour ouvrir le fichier généré.

    Notez que le contenu inclus dans ce bundle contient uniquement des fichiers **. Coffee** . Vous pouvez également voir que le code CoffeeScript a été compilé en JavaScript et que les lignes commentées ont été supprimées.

    ![Bundle de fichiers JS dynamiques](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Bundle de fichiers JS dynamiques")

    *Bundle de fichiers JS dynamiques*

> [!NOTE]
> En outre, vous pouvez déployer cette application sur les sites Web Windows Azure à l' [annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixB).

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Ce laboratoire vous aide à comprendre ce qui est nouveau dans ASP.NET et le développement Web dans Visual Studio 2012 et comment tirer parti des améliorations apportées à Visual Studio 2012.

En suivant ce laboratoire pratique, vous avez appris à utiliser les nouvelles fonctionnalités et améliorations des éditeurs Visual Studio 2012 pour CSS, JavaScript et HTML. En outre, vous avez appris comment Visual Studio 2012 implémente le regroupement intégré et la minimisation.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe A : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Vignette VS Express pour le Web*

---

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe B : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

Cette annexe vous indique comment créer un nouveau site Web à partir de Windows Azure Portail de gestion et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tâche 1 : création d’un nouveau site Web à partir du portail Windows Azure

1. Accédez au [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Windows Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente. Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).

    ![Ouvrir une session sur Windows Portail Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Ouvrir une session sur Windows Portail Azure")

    *Connectez-vous à Windows Azure Portail de gestion*
2. Cliquez sur **nouveau** dans la barre de commandes.

    ![Création d’un nouveau site Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Création d’un nouveau site Web")

    *Création d’un nouveau site Web*
3. Cliquez sur **compute** | **site Web**. Sélectionnez ensuite l’option **création rapide** . Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.

    > [!NOTE]
    > Un site Web Windows Azure est l’hôte d’une application Web s’exécutant dans le Cloud que vous pouvez contrôler et gérer. L’option création rapide vous permet de déployer une application Web terminée sur le site Web Windows Azure depuis l’extérieur du portail. Elle n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un nouveau site Web à l’aide de la création rapide](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Création d’un nouveau site Web à l’aide de la création rapide")

    *Création d’un nouveau site Web à l’aide de la création rapide*
4. Patientez jusqu’à la création du nouveau **site Web** .
5. Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** . Vérifiez que le nouveau site Web fonctionne.

    ![Navigation vers le nouveau site Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Navigation vers le nouveau site Web")

    *Navigation vers le nouveau site Web*

    ![Site Web en cours d’exécution](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.

    ![Ouverture des pages de gestion de site Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Ouverture des pages de gestion de site Web")

    *Ouverture des pages de gestion de site Web*
7. Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .

    > [!NOTE]
    > Le *profil de publication* contient toutes les informations requises pour publier une application Web sur un site Web Windows Azure pour chaque méthode de publication activée. Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur les sites Web Windows Azure.

    ![Téléchargement du profil de publication du site Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Téléchargement du profil de publication du site Web")

    *Téléchargement du profil de publication du site Web*
8. Téléchargez le fichier de profil de publication dans un emplacement connu. Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur un site Web Windows Azure à partir de Visual Studio.

    ![Enregistrement du fichier de profil de publication](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Enregistrement du profil de publication")

    *Enregistrement du fichier de profil de publication*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database. Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.

1. Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application. Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Windows Azure, à partir des **bases de données SQL** | **serveurs** | **tableau de bord du serveur**. Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes. Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.

    ![Tableau de bord du serveur SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Tableau de bord du serveur SQL Database")

    *Tableau de bord du serveur SQL Database*
2. Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** . Entrez un nom pour la règle, puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png).

    ![Ajout de l’adresse IP du client](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Ajout de l’adresse IP du client*
3. Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Confirmer les modifications*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.

    ![Publication de l’application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publication de l'application")

    *Publication du site Web*
2. Importez le profil de publication que vous avez enregistré dans la première tâche.

    ![Importation du profil de publication](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la validation terminée, cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.

    ![Validation de la connexion](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validation de la connexion")

    *Validation de la connexion*
4. Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).

    ![Configuration de Web Deploy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Configuration de Web Deploy")

    *Configuration de Web Deploy*
5. Configurez la connexion de base de données comme suit :

   - Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .
   - Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.
   - Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.
   - Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.

     ![Configuration de la chaîne de connexion de destination](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuration de la chaîne de connexion de destination")

     *Configuration de la chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Création de la chaîne de base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Chaîne de connexion pointant vers SQL Database")

    *Chaîne de connexion pointant vers SQL Database*
8. Dans la page **Aperçu** , cliquez sur **publier**.

    ![Publication de l’application Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publication de l’application Web")

    *Publication de l’application Web*
9. Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.

    ![Application publiée sur Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application publiée sur Windows Azure")

    *Application publiée sur Windows Azure*
