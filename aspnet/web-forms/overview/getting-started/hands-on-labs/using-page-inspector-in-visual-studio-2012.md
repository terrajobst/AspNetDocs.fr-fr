---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Utilisation de Inspecteur de page dans Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: 'Dans ce laboratoire pratique, vous découvrirez un nouvel outil pour rechercher et résoudre les problèmes de pages Web dans Visual Studio : l’Inspecteur de page. Inspecteur de page est un nouvel outil que b...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f42b1be2697ba7d1145b3e334fe8f4ebf019cd12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586253"
---
# <a name="using-page-inspector-in-visual-studio-2012"></a>Utilisation de l’Inspecteur de page dans Visual Studio 2012

par l' [équipe Web camps](https://twitter.com/webcamps)

> Dans ce laboratoire pratique, vous découvrirez un nouvel outil pour rechercher et résoudre les problèmes de pages Web dans Visual Studio : l’Inspecteur de page.
> 
> Inspecteur de page est un nouvel outil qui offre des outils de diagnostic de navigateur à Visual Studio et fournit une expérience intégrée dans le navigateur, le ASP.NET et le code source. Il restitue une page Web (HTML, Web Forms, ASP.NET MVC ou Web pages) directement dans l’IDE de Visual Studio et vous permet d’examiner à la fois le code source et le résultat obtenu. Inspecteur de page vous permet de décomposer facilement un site Web, de créer rapidement des pages à partir de zéro et de diagnostiquer rapidement les problèmes.
> 
> À l’heure actuelle, nous disposons d’un certain nombre de frameworks web qui créent des sites Web flexibles et évolutifs en temps utile, tels que ASP.NET MVC et Web Forms. D’un autre côté, il devient plus difficile de trouver des problèmes sur les pages, car l’IDE ne prend pas en charge la vue de concepteur dans les pages basées sur des modèles et le contenu dynamique. Par conséquent, ces sites Web doivent être ouverts dans un navigateur pour voir comment ils apparaissent à l’utilisateur.
> 
> Les développeurs Web utilisent des outils externes pour détecter les problèmes qui s’exécutent régulièrement dans le navigateur. Ensuite, ils retournent à l’IDE et commencent à résoudre. Cette activité entre l’IDE, le navigateur et les outils de profilage peut être inefficace et nécessite parfois un nouveau déploiement et un nettoyage du cache chaque fois que vous souhaitez reproduire un problème.
> 
> Inspecteur de page permet de combler un fossé dans le développement Web entre le client (outils de navigation) et le serveur (ASP.NET et le code source) en regroupant le meilleur des deux mondes à l’aide d’un ensemble combiné de fonctionnalités.
> 
> À l’aide de Inspecteur de page, vous pouvez voir les éléments des fichiers sources (y compris le code côté serveur) qui ont produit le balisage HTML à afficher dans le navigateur. Inspecteur de page vous permet également de modifier les propriétés CSS et les attributs d’élément DOM pour voir les modifications reflétées immédiatement dans le navigateur.
> 
> Ce laboratoire pratique vous guide tout au long des fonctionnalités de Inspecteur de page et vous montre comment vous pouvez les utiliser pour résoudre les problèmes des applications Web. **Ce laboratoire contient deux exercices utilisant des flux similaires, mais ciblant des technologies différentes. Si vous êtes un développeur MVC ASP.NET, suivez l’exercice 1. Si vous êtes un développeur WebForms, suivez deux exercices**.
> 
> Ce laboratoire vous guide tout au long des améliorations et des nouvelles fonctionnalités décrites précédemment en appliquant des modifications mineures à un exemple d’application Web fournie dans le dossier source.
> 
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Décomposer un site Web à l’aide de Inspecteur de page
- Inspecter et afficher un aperçu des modifications apportées aux styles CSS avec Inspecteur de page
- Détectez et corrigez les problèmes dans vos pages Web à l’aide de Inspecteur de page

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe A](#AppendixA) pour obtenir des instructions sur son installation).
- Internet Explorer 9 ou version ultérieure

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique comprend les exercices suivants :

1. [Exercice 1 : utilisation de Inspecteur de page dans des projets MVC ASP.NET](#Exercise1)
2. [Exercice 2 : utilisation de Inspecteur de page dans des projets WebForms](#Exercise2)

> [!NOTE]
> Chaque exercice est accompagné d’une solution de démarrage, située dans le dossier Begin de l’exercice, qui vous permet de suivre chaque exercice indépendamment des autres. À l’intérieur du code source d’un exercice, vous trouverez également un dossier de fin contenant une solution Visual Studio avec le code qui résulte de l’exécution des étapes de l’exercice correspondant. Vous pouvez utiliser ces solutions comme conseils si vous avez besoin d’aide supplémentaire au fur et à mesure que vous travaillez dans ce laboratoire pratique.

Durée estimée pour effectuer ce laboratoire : **30 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Exercice 1 : utilisation de Inspecteur de page dans des projets MVC ASP.NET

Dans cet exercice, vous allez apprendre à afficher un aperçu et à déboguer une solution **ASP.NET MVC 4** à l’aide de **inspecteur de page**. Tout d’abord, vous allez effectuer un bref tour d’horizon sur l’outil pour apprendre les fonctionnalités qui facilitent le processus de débogage Web. Ensuite, vous allez travailler dans une page Web qui contient des problèmes de style. Vous allez apprendre à utiliser Inspecteur de page pour rechercher le code source qui génère le problème et le corriger.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tâche 1 : exploration de Inspecteur de page

Dans cette tâche, vous allez apprendre à utiliser la Inspecteur de page dans le contexte d’un projet MVC 4 ASP.NET qui affiche une galerie de photos.

1. Ouvrez le **début** de la solution situé à l’emplacement **source/EX1-MVC4/Begin/** Folder.

   1. Vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Dans la Explorateur de solutions, localisez la vue **index. cshtml** dans le dossier du projet **/Views/Home** , cliquez dessus avec le bouton droit et sélectionnez **afficher dans inspecteur de page**.

    ![Sélection d’un fichier à prévisualiser dans Inspecteur de page](using-page-inspector-in-visual-studio-2012/_static/image1.png "Sélection d’un fichier à prévisualiser dans Inspecteur de page")

    *Sélection d’un fichier à prévisualiser dans Inspecteur de page*
3. La fenêtre Inspecteur de page affiche l’URL */home/index* mappée sur la vue source que vous avez sélectionnée.

    ![Premier contact avec PageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Premier contact avec Inspecteur de page*

    L’outil Inspecteur de page est intégré à votre environnement Visual Studio. L’inspecteur contient un navigateur incorporé, ainsi qu’un profileur HTML puissant. Notez que vous n’avez pas besoin d’exécuter la solution pour voir l’aspect de vos pages.

    > [!NOTE]
    > Lorsque la largeur de Inspecteur de page navigateur est inférieure à la largeur de la page ouverte, la page ne s’affiche pas correctement. Si cela se produit, ajustez la largeur du Inspecteur de page.
4. Dans Inspecteur de page, cliquez sur l’onglet **fichiers** .

    Vous verrez tous les fichiers sources qui composent la page d’index. Cette fonctionnalité permet d’identifier tous les éléments d’un coup d’œil, en particulier lorsque vous travaillez avec des vues et des modèles partiels. Notez que vous pouvez également ouvrir chacun des fichiers si vous cliquez sur les liens.

    ![Onglet-Files](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Onglet fichiers*
5. Cliquez sur le bouton **bascule mode d’inspection** , situé à gauche des onglets.

    Cet outil vous permet de sélectionner n’importe quel élément de la page et d’afficher son code HTML et Razor.

    ![Bouton bascule-inspection-mode](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Bouton Activer/désactiver Mode d’inspection*
6. Dans le navigateur Inspecteur de page, déplacez le pointeur de la souris sur les éléments de page. Lorsque vous déplacez le pointeur de la souris sur une partie de la page rendue, le type d’élément est affiché et le balisage ou le code source correspondant est mis en surbrillance dans l’éditeur Visual Studio.

    ![Mode d’inspection en action](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Mode d’inspection en action*

    > [!NOTE]
    > N’agrandissez pas la fenêtre de Inspecteur de page ou vous ne pourrez pas voir l’onglet d’aperçu qui affiche le code source. Si vous cliquez sur l’élément dans Inspecteur de page lorsqu’il est agrandi, le code source de la sélection s’affiche, mais il masque la fenêtre de Inspecteur de page.

    Si vous faites attention au fichier **index. cshtml** , vous remarquerez que la partie du code source qui génère l’élément sélectionné est mise en surbrillance. Cette fonctionnalité facilite la modification des fichiers sources longs, en fournissant un moyen direct et rapide d’accéder au code.

    ![Inspecter des éléments](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Inspecter des éléments*
7. Cliquez sur le bouton **basculer mode d’inspection** (![Sélectionnez l’onglet HTML pour afficher le code HTML rendu dans le navigateur inspecteur de page.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Sélectionnez l’onglet HTML pour afficher le code HTML rendu dans le navigateur Inspecteur de page.") ) pour désactiver le curseur.
8. Sélectionnez l’onglet **HTML** pour afficher le code HTML rendu dans le navigateur inspecteur de page.
9. Dans le balisage HTML, localisez l’élément de liste avec le lien Koala et sélectionnez-le.

    Notez que lorsque vous sélectionnez le code, la sortie correspondante est automatiquement mise en surbrillance dans le navigateur. Cette fonctionnalité est utile pour voir comment un bloc HTML est rendu sur la page.

    ![Sélection d’un élément HTML dans la page](using-page-inspector-in-visual-studio-2012/_static/image8.png "Sélection d’un élément HTML dans la page")

    *Sélection d’un élément HTML dans la page*
10. Cliquez sur le bouton **bascule mode d’inspection** pour activer *mode d’inspection* et cliquez sur la barre de navigation. À droite du code HTML, dans le volet styles, vous verrez une liste avec les styles CSS appliqués à l’élément sélectionné.

    > [!NOTE]
    > Étant donné que l’en-tête fait partie de la disposition du site, Inspecteur de page ouvre également \_fichier Layout. cshtml et met en surbrillance le segment de code affecté.

    ![Découverte de styles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Découverte des styles et des fichiers sources d’un élément sélectionné*
11. Avec l’option basculer le pointeur d’inspection activée, déplacez le pointeur de la souris sous la barre de couleur bleue et cliquez sur le demi-cercle.

    ![Sélection d’un élément](using-page-inspector-in-visual-studio-2012/_static/image10.png "Sélection d’un élément")

    *Sélection d’un élément*
12. Dans le volet styles, localisez l’élément d' **image d’arrière-plan** sous le groupe **. main-content** . **Décochez** l' **image d’arrière-plan** et observez ce qui se passe. Vous remarquerez que le navigateur reflète les modifications immédiatement et que le cercle est masqué.

    > [!NOTE]
    > Les modifications que vous appliquez dans l’onglet Inspecteur de page styles n’affectent pas la feuille de style d’origine. Vous pouvez désactiver les styles ou modifier leurs valeurs autant de fois que vous le souhaitez, mais ils seront restaurés après l’actualisation de la page.

    ![Activation et désactivation de styles CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "Activation et désactivation de styles CSS")

    *Activation et désactivation de styles CSS*
13. Maintenant, cliquez sur le texte «**votre logo ici**» dans l’en-tête à l’aide du mode d’inspection.
14. Dans l’onglet **styles** , localisez l’attribut CSS **de taille de police** sous le groupe **. site-titre** . Double-cliquez sur la valeur de l’attribut et remplacez la valeur em 2,3 par **3 EM**, puis appuyez sur **entrée**. Notez que le titre est plus grand.

    ![Modification des valeurs CSS dans le Inspecteur de page](using-page-inspector-in-visual-studio-2012/_static/image12.png "Modification des valeurs CSS dans le Inspecteur de page")

    *Modification des valeurs CSS dans le Inspecteur de page*
15. Cliquez sur l’onglet **suivi des styles** , situé dans le volet droit de inspecteur de page. Il s’agit d’une autre façon de voir tous les styles appliqués à la sélection, classés par nom d’attribut.

    ![Traçage de styles CSS](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Style CSS traçage de l’élément sélectionné*
16. Une autre fonctionnalité de Inspecteur de page est le volet de disposition. À l’aide du mode d’inspection, sélectionnez la barre de navigation, puis cliquez sur l’onglet **disposition** dans le volet droit. Vous verrez la taille exacte de l’élément sélectionné, ainsi que son décalage, sa marge, son remplissage et sa taille de bordure. Notez que vous pouvez également modifier les valeurs de cette vue.

    ![Disposition des éléments dans Inspecteur de page](using-page-inspector-in-visual-studio-2012/_static/image14.png "Disposition des éléments dans Inspecteur de page")

    *Disposition des éléments dans Inspecteur de page*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tâche 2 : recherche et résolution des problèmes de style dans la Galerie de photos

Comment diagnostiquer les problèmes de pages Web avec les versions antérieures de Visual Studio ? Vous êtes probablement familiarisé avec les outils de débogage Web qui s’exécutent en dehors de l’IDE de Visual Studio, comme Internet Explorer Outils de développement ou Firebug. Les navigateurs comprennent uniquement le HTML, les scripts et les styles, tandis qu’un Framework sous-jacent génère le code HTML qui sera rendu. Pour cette raison, vous devez souvent déployer l’ensemble du site pour voir comment les pages Web ressemblent.

Vous avez probablement suivi ces étapes Lorsque vous souhaitiez détecter et résoudre un problème sur votre site Web :

1. Exécutez la solution à partir de Visual Studio ou déployez la page sur le serveur Web.
2. Dans le navigateur, ouvrez les outils de développement que vous utilisez ou ouvrez simplement le code source et les styles et essayez de faire correspondre le problème. Pour rechercher les fichiers concernés, vous auriez utilisé la recherche &quot;&quot; ou &quot;Rechercher dans les fichiers&quot; fonctionnalités avec le nom des classes de style.
3. Une fois l’erreur détectée, arrêtez le navigateur Web et le serveur.
4. Effacez le cache du navigateur.
5. Revenez à Visual Studio pour appliquer un correctif. Répétez toutes les étapes à tester.

Comme il n’existe pas de véritable WYSIWYG dans ASP.NET MVC 4, la plupart des problèmes de style sont détectés à un moment ultérieur, après l’exécution ou le déploiement de l’application Web. Désormais, avec Inspecteur de page, il est possible d’afficher un aperçu de n’importe quelle page sans exécuter la solution.

Dans cette tâche, vous allez utiliser l’inspecteur de page et corriger certains problèmes de l’application de la Galerie de photos.

1. À l’aide de Inspecteur de page, localisez le **Registre** et les liens **de connexion** sur le côté gauche de l’en-tête.

    Notez que les liens ne s’affichent pas à l’endroit prévu à droite, et qu’ils apparaissent comme une liste à puces. Vous allez maintenant aligner les liens vers la droite et les Restyler en conséquence.

    ![Recherche des liens du Registre et de la connexion](using-page-inspector-in-visual-studio-2012/_static/image15.png "Recherche des liens du Registre et de la connexion")

    *Recherche des liens du Registre et de la connexion*
2. Avec l’option Basculer Mode d’inspection sélectionnée, cliquez sur Fermer sur, mais pas sur, le lien enregistrer pour ouvrir son code.

    Notez que le code source des liens se trouve dans le fichier **\_LoginPartial. cshtml** , et non dans le fichier index. cshtml ni dans le \_Layout. cshtml, qui sont les emplacements que vous pouvez examiner en premier lieu. Vous avez été placé directement dans le fichier source correct.
3. Dans l’onglet **styles** , recherchez et cliquez sur la **section\<> #login** élément, qui est le conteneur html pour ces liens.

    Notez que le style de **#login** est automatiquement situé dans **site. CSS** une fois que vous avez cliqué sur. En outre, le code est maintenant mis en surbrillance.

    ![Sélection des styles CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "Sélection des styles CSS")

    *Sélection des styles CSS*
4. Supprimez les marques de commentaire de l’attribut **text-align** dans le code en surbrillance en supprimant les caractères d’ouverture et de fermeture, puis enregistrez le fichier **site. CSS** .

    Inspecteur de page est conscient de tous les fichiers qui composent la page actuelle, et il peut détecter la modification de l’un de ces fichiers. Elle vous avertit chaque fois que la page active dans le navigateur n’est pas synchronisée avec les fichiers sources.
5. Dans le navigateur Inspecteur de page, cliquez sur la barre située sous la barre d’adresses pour recharger la page.

    ![Rechargement de la page](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Rechargement de la page*

    Les liens sont désormais à droite, mais ils ressemblent toujours à une liste à puces. À présent, vous allez supprimer les puces et aligner les liens horizontalement.

    ![Page mise à jour](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Page mise à jour*
6. À l’aide du mode d’inspection, sélectionnez l’un des éléments **&lt;li&gt;** qui contiennent les &quot;Registre&quot; et &quot;Connectez-vous&quot; liens. Ensuite, cliquez sur la **section&lt;&gt; #login** élément pour accéder au code **styles. CSS** .

    ![Recherche du style](using-page-inspector-in-visual-studio-2012/_static/image19.png "Recherche du style")

    *Recherche du style*
7. Dans **style. CSS**, supprimez les marques de commentaire du code pour **#login éléments Li** . Le style que vous ajoutez masquera la puce et affichera les éléments horizontalement.

    ![Redéfinition du style des liens de connexion](using-page-inspector-in-visual-studio-2012/_static/image20.png "Redéfinition du style des liens de connexion")

    *Redéfinition du style des liens de connexion*
8. Enregistrez le fichier **style. CSS** et cliquez une fois sur la barre située sous l’adresse pour recharger la page. Notez que les liens s’affichent correctement.

    ![Liens alignés sur le côté droit](using-page-inspector-in-visual-studio-2012/_static/image21.png "Liens alignés sur le côté droit")

    *Liens alignés sur le côté droit*
9. Enfin, vous allez modifier le titre de l’en-tête. Utilisez le mode inspection pour cliquer sur **votre logo** et accéder au code source qui le génère.
10. Vous êtes maintenant dans **\_Layout. cshtml**, remplacez «**Your logo here**» par «**Photo Gallery**». Enregistrez et mettez à jour le navigateur Inspecteur de page.

    ![Attribution d’un nouveau titre](using-page-inspector-in-visual-studio-2012/_static/image22.png "Attribution d’un nouveau titre")

    *Attribution d’un nouveau titre*

    ![Page Galerie de photos](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Page de la Galerie de photos mise à jour*
11. Enfin, sélectionnez le projet **Photogallery** et appuyez sur **F5** pour exécuter l’application. Vérifiez que toutes les modifications fonctionnent comme prévu.

---

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Exercice 2 : utilisation de Inspecteur de page dans des projets WebForms

Dans cet exercice, vous allez apprendre à afficher un aperçu et déboguer une solution WebForms à l’aide de Inspecteur de page. Vous allez d’abord effectuer une brève présentation autour de l’outil pour apprendre les fonctionnalités Inspecteur de page qui facilitent le processus de débogage Web. Ensuite, vous allez travailler dans une page Web qui contient des problèmes de style. Vous allez apprendre à utiliser Inspecteur de page pour rechercher le code source qui génère le problème et le corriger.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tâche 1 : exploration de Inspecteur de page

Dans cette tâche, vous allez apprendre à utiliser les fonctionnalités de Inspecteur de page dans le contexte d’un projet WebForms qui affiche une galerie de photos.

1. Ouvrez la solution **Begin** à l’emplacement **source/EX2-WebForms/Begin/** dossier.

   1. Vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Dans la Explorateur de solutions, localisez la page **default. aspx** , cliquez dessus avec le bouton droit et sélectionnez **afficher dans inspecteur de page**.

    ![Ouverture de default. aspx avec Inspecteur de page](using-page-inspector-in-visual-studio-2012/_static/image24.png "Ouverture de default. aspx avec Inspecteur de page")

    *Ouverture de default. aspx avec Inspecteur de page*
3. La fenêtre Inspecteur de page affiche default. aspx.

    ![Affichage du default. aspx dans Inspecteur de page](using-page-inspector-in-visual-studio-2012/_static/image25.png "Affichage du default. aspx dans Inspecteur de page")

    *Affichage du default. aspx dans Inspecteur de page*

    L’outil Inspecteur de page est intégré à votre environnement Visual Studio. L’inspecteur contient un navigateur incorporé, ainsi qu’un profil HTML puissant qui affiche le code sélectionné. Notez que vous n’avez pas besoin d’exécuter la solution pour voir l’aspect de vos pages.

    > [!NOTE]
    > Lorsque la largeur de Inspecteur de page navigateur est inférieure à la largeur de la page ouverte, la page ne s’affiche pas correctement. Si cela se produit, ajustez la largeur du Inspecteur de page.
4. Dans Inspecteur de page, cliquez sur l’onglet **fichiers** .

    Vous verrez tous les fichiers sources qui composent la page par défaut rendue. Il s’agit d’une fonctionnalité utile pour identifier tous les éléments d’un coup d’œil, en particulier lorsque vous travaillez avec des contrôles utilisateur et des pages maîtres. Notez que vous pouvez également accéder à chacun des fichiers.

    ![Onglet fichiers](using-page-inspector-in-visual-studio-2012/_static/image26.png "Onglet fichiers")

    *Onglet fichiers*
5. Cliquez sur le bouton **bascule mode d’inspection** , situé à gauche des onglets.

    Cet outil vous permet de sélectionner n’importe quel élément de la page et d’afficher son code HTML et sa source. aspx.

    ![Bouton Activer/désactiver Mode d’inspection](using-page-inspector-in-visual-studio-2012/_static/image27.png "Bouton Activer/désactiver Mode d’inspection")

    *Bouton Activer/désactiver Mode d’inspection*
6. Dans le navigateur Inspecteur de page, déplacez le curseur de la souris sur les éléments de page. Lorsque vous déplacez le pointeur de la souris sur une partie de la page rendue, le type d’élément est affiché et le balisage ou le code source correspondant est mis en surbrillance dans l’éditeur Visual Studio.

    ![Mode d’inspection en action](using-page-inspector-in-visual-studio-2012/_static/image28.png "Mode d’inspection en action")

    *Mode d’inspection en action*

    > [!NOTE]
    > N’agrandissez pas la fenêtre de Inspecteur de page ou vous ne pourrez pas voir l’onglet d’aperçu qui affiche le code source. Si vous cliquez sur l’élément dans Inspecteur de page lorsqu’il est agrandi, le code source de la sélection s’affiche, mais il masque la fenêtre de Inspecteur de page.

    Si vous faites attention au fichier **default. aspx** , vous remarquerez que la partie du code source qui génère l’élément sélectionné est mise en surbrillance. Cette fonctionnalité facilite l’édition de longs fichiers sources, en fournissant un moyen direct et rapide d’accéder au code.

    ![Inspecter des éléments](using-page-inspector-in-visual-studio-2012/_static/image29.png "Inspecter des éléments")

    *Inspecter des éléments*
7. Cliquez sur le bouton **basculer mode d’inspection** (![sélectionnez-le-HTML-tab-to-Display-the-HTML-code-rendered-in-the-page-Inspector-Browser).](using-page-inspector-in-visual-studio-2012/_static/image30.png "Sélectionnez-le-HTML-tab-to-Display-the-HTML-code-rendered-in-the-page-Inspector-Browser.") ), situé dans Inspecteur de page onglets, pour désactiver le curseur.
8. Sélectionnez l’onglet **HTML** pour afficher le code HTML rendu dans le navigateur inspecteur de page.
9. Dans le code HTML, localisez l’élément de liste avec le lien Koala et sélectionnez-le.

    Notez que lorsque vous sélectionnez le code, la sortie correspondante est automatiquement mise en surbrillance dans le navigateur. Cette fonctionnalité est utile pour voir comment un bloc HTML est rendu sur la page.

    ![Sélection d’un élément HTML dans la page](using-page-inspector-in-visual-studio-2012/_static/image31.png "Sélection d’un élément HTML dans la page")

    *Sélection d’un élément HTML dans la page*
10. Cliquez sur le bouton **bascule mode d’inspection** pour activer *mode d’inspection* et cliquez sur la barre de navigation. À droite du code HTML, dans le volet styles, vous verrez une liste avec les styles CSS appliqués à l’élément sélectionné.

    > [!NOTE]
    > étant donné que l’en-tête fait partie de la disposition du site, Inspecteur de page ouvre également le fichier site. Master et met en surbrillance le segment de code affecté.

    ![Découverte de styles WebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Découverte des styles et des fichiers sources d’un élément sélectionné")

    *Découverte des styles et des fichiers sources d’un élément sélectionné*
11. Avec l’option basculer le pointeur d’inspection activée, déplacez le pointeur de la souris sous la barre de menus et cliquez sur le demi-cercle vide.

    ![Sélection d’un élément](using-page-inspector-in-visual-studio-2012/_static/image33.png "Sélection d’un élément")

    *Sélection d’un élément*
12. Dans le volet styles, localisez l’élément d' **image d’arrière-plan** sous le groupe **. main-content** . **Décochez** l' **image d’arrière-plan** et observez ce qui se passe. Vous remarquerez que le navigateur reflète les modifications immédiatement et que le cercle est masqué.

    > [!NOTE]
    > Les modifications que vous appliquez dans l’onglet Inspecteur de page styles n’affectent pas la feuille de style d’origine. Vous pouvez désactiver les styles ou modifier leurs valeurs autant de fois que vous le souhaitez, mais ils seront restaurés après l’actualisation de la page.

    ![Activation et désactivation de styles2 CSS](using-page-inspector-in-visual-studio-2012/_static/image34.png "Activation et désactivation de styles CSS")

    *Activation et désactivation de styles CSS*
13. Maintenant, cliquez sur le texte «**votre** **logo ici »** dans l’en-tête à l’aide du mode d’inspection.
14. Dans l’onglet **styles** , localisez l’attribut CSS **de taille de police** sous le groupe **. site-titre** . Double-cliquez sur l’attribut pour modifier sa valeur. Remplacez la valeur em 2.3 par **3em**, puis appuyez sur entrée. Notez que le titre est plus grand.

    ![Modification des valeurs CSS dans la page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Modification des valeurs CSS dans le Inspecteur de page")

    *Modification des valeurs CSS dans le Inspecteur de page*
15. Cliquez sur l’onglet **suivi des styles** , situé dans le volet droit de inspecteur de page. Il s’agit d’une autre façon de voir tous les styles appliqués à la sélection, classés par nom d’attribut.

    ![Style CSS traçage de l’élément sélectionné](using-page-inspector-in-visual-studio-2012/_static/image36.png "Style CSS traçage de l’élément sélectionné")

    *Style CSS traçage de l’élément sélectionné*
16. Une autre fonctionnalité de Inspecteur de page est le volet de disposition. À l’aide du mode d’inspection, sélectionnez la barre de navigation, puis cliquez sur l’onglet **disposition** dans le volet droit. Vous verrez la taille exacte de l’élément sélectionné, ainsi que son décalage, sa marge, son remplissage et sa taille de bordure. Notez que vous pouvez également modifier les valeurs de cette vue.

    ![Disposition des éléments dans Inspecteur de page](using-page-inspector-in-visual-studio-2012/_static/image37.png "Disposition des éléments dans Inspecteur de page")

    *Disposition des éléments dans Inspecteur de page*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tâche 2 : recherche et résolution des problèmes de style dans la Galerie de photos

Comment diagnostiquer les problèmes de pages Web avec les versions antérieures de Visual Studio ? Vous êtes probablement familiarisé avec les outils de débogage Web qui s’exécutent en dehors de l’IDE de Visual Studio, comme Internet Explorer Outils de développement ou Firebug. Les navigateurs comprennent uniquement le HTML, les scripts et les styles, tandis qu’un Framework sous-jacent génère le code HTML qui sera rendu. Pour cette raison, vous devez souvent déployer l’ensemble du site pour voir comment les pages Web ressemblent.

Vous avez probablement suivi ces étapes Lorsque vous souhaitiez détecter et résoudre un problème sur votre site Web :

1. Exécutez la solution à partir de Visual Studio ou déployez la page sur le serveur Web.
2. Dans le navigateur, ouvrez les outils de développement que vous utilisez ou ouvrez simplement le code source et les styles et essayez de faire correspondre le problème. Pour rechercher les fichiers impliqués, vous avez utilisé le &quot;de recherche&quot; ou &quot;Rechercher dans les fichiers&quot; fonctionnalités avec le nom des classes de style.
3. Une fois l’erreur détectée, arrêtez le navigateur Web et le serveur.
4. Effacez le cache du navigateur.
5. Revenez à Visual Studio pour appliquer un correctif. Répétez toutes les étapes à tester.

Comme il n’existe pas de véritable WYSIWYG dans ASP.NET WebForms, certains problèmes de style sont détectés à un moment ultérieur, après l’exécution ou le déploiement. Désormais, avec Inspecteur de page, il est possible d’afficher un aperçu de n’importe quelle page sans exécuter la solution.

Dans cette tâche, vous allez utiliser l’inspecteur de page pour résoudre certains problèmes de l’application de la Galerie de photos. Dans les étapes suivantes, vous allez détecter et corriger rapidement un problème de style simple dans l’en-tête.

1. À l’aide de l’inspection de page, localisez les liens **Registre** et **connexion dans** le côté gauche de l’en-tête.

    Notez que le lien ne s’affiche pas à l’endroit prévu à droite. Vous allez maintenant aligner le lien vers la droite et le Restyler en conséquence.

    ![Lien de connexion positionné à gauche](using-page-inspector-in-visual-studio-2012/_static/image38.png "Lien de connexion positionné à gauche")

    *Lien de connexion positionné à gauche*
2. Avec l’option Basculer Mode d’inspection sélectionnée, sélectionnez le lien connexion pour ouvrir son code.

    Notez que le code source du lien se trouve dans le fichier **site. Master** , et non dans la page default. aspx, qui est l’emplacement que vous pouvez examiner en premier lieu. vous avez été placé directement dans le fichier source correct.
3. Dans l’onglet **styles** , recherchez et cliquez sur la **section&lt;&gt; #login** élément, qui est le conteneur html pour ces liens.

    Notez que le style de **#login** est automatiquement situé dans **site. CSS** une fois que vous avez cliqué sur. En outre, le code est maintenant mis en surbrillance.

    ![Sélection des styles CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "Sélection des styles CSS")

    *Sélection des styles CSS*
4. Supprimez les marques de commentaire de l’attribut **text-align** dans le code en surbrillance en supprimant les caractères d’ouverture et de fermeture, puis enregistrez le fichier **site. CSS** .

    Inspecteur de page est conscient de tous les fichiers qui composent la page actuelle, et il peut détecter la modification de l’un de ces fichiers. Elle vous avertit chaque fois que la page active dans le navigateur n’est pas synchronisée avec les fichiers sources.
5. Dans le navigateur Inspecteur de page, cliquez sur la barre située sous la barre d’adresses pour enregistrer les modifications et recharger la page.

    ![Rechargement de la page](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Rechargement de la page*

    Les liens sont désormais à droite, mais ils ressemblent toujours à une liste à puces. À présent, vous allez supprimer les puces et aligner les liens horizontalement.

    ![Page mise à jour](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Page mise à jour*
6. À l’aide du mode d’inspection, sélectionnez l’un des éléments **&lt;li&gt;** qui contiennent les &quot;Registre&quot; et &quot;Connectez-vous&quot; liens. Ensuite, cliquez sur la **section&lt;&gt; #login** élément pour accéder au code **styles. CSS** .

    ![Recherche du style](using-page-inspector-in-visual-studio-2012/_static/image42.png "Recherche du style")

    *Recherche du style*
7. Dans **style. CSS**, supprimez les marques de commentaire du code pour **#login éléments Li** . Le style que vous ajoutez masquera la puce et affichera les éléments horizontalement.

    ![Redéfinition du style des liens de connexion](using-page-inspector-in-visual-studio-2012/_static/image43.png "Redéfinition du style des liens de connexion")

    *Redéfinition du style des liens de connexion*
8. Enregistrez le fichier **style. CSS** et cliquez une fois sur la barre située sous l’adresse pour recharger la page. Notez que les liens s’affichent correctement.

    ![Liens alignés sur le côté droit](using-page-inspector-in-visual-studio-2012/_static/image44.png "Liens alignés sur le côté droit")

    *Liens alignés sur le côté droit*
9. Enfin, vous allez modifier le titre de l’en-tête. Au lieu de rechercher le texte «**votre logo ici »** dans tous les fichiers, utilisez le mode inspection pour cliquer sur le texte et accéder au code source qui le génère.

    ![Recherche du titre du site](using-page-inspector-in-visual-studio-2012/_static/image45.png "Recherche du titre du site")

    *Recherche du titre du site*
10. Vous êtes maintenant dans **site. Master**, remplacez le texte «**Your logo here**» par «**Photo Gallery**». Enregistrez et mettez à jour le navigateur Inspecteur de page.

    ![Page de la Galerie de photos mise à jour](using-page-inspector-in-visual-studio-2012/_static/image46.png "Page de la Galerie de photos mise à jour")

    *Page de la Galerie de photos mise à jour*
11. Enfin, appuyez sur **F5** pour exécuter l’application. Vérifiez que toutes les modifications fonctionnent comme prévu.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En suivant ce laboratoire pratique, vous avez appris à utiliser Inspecteur de page pour afficher un aperçu de votre application Web sans avoir à régénérer et exécuter le site Web dans un navigateur. En outre, vous avez appris à rechercher et à corriger rapidement les bogues en accédant directement à partir de la sortie rendue dans le code source.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe A : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;<em>Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure</em>&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Vignette VS Express pour le Web*
