---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Atelier pratique : outils Web Visual Studio 2013 | Microsoft Docs'
author: rick-anderson
description: Visual Studio est un excellent environnement de développement pour. Projets Windows et Web basés sur .net. Il comprend un éditeur de texte puissant qui peut facilement être utilisé pour...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622674"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a>Atelier pratique : outils Web Visual Studio 2013

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

> Visual Studio est un excellent environnement de développement pour. Projets Windows et Web basés sur .net. Il comprend un éditeur de texte puissant qui peut facilement être utilisé pour modifier des fichiers autonomes sans projet.
> 
> Visual Studio gère une arborescence d’analyse complète à mesure que vous modifiez chaque fichier. Cela permet à Visual Studio de fournir une saisie semi-automatique inégalée et des actions basées sur les documents tout en rendant l’expérience de développement plus rapide et plus agréable. Ces fonctionnalités sont particulièrement puissantes dans les documents HTML et CSS.
> 
> Toute cette puissance est également disponible pour les extensions, ce qui simplifie l’extension des éditeurs avec de nouvelles fonctionnalités puissantes adaptées à vos besoins. Web Essentials est une collection d' (principalement) améliorations liées au Web dans Visual Studio. Il comprend un grand nombre de nouvelles saisies semi-automatiques IntelliSense (en particulier pour CSS), de nouvelles fonctionnalités de lien de navigateur, des JSHint automatiques pour les fichiers JavaScript, de nouveaux avertissements pour HTML et CSS, et de nombreuses autres fonctionnalités qui sont essentielles au développement Web moderne.
> 
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible sur [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Présentation

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Utilisez les nouvelles fonctionnalités de l’éditeur HTML incluses dans Web Essentials, telles que les extraits de code HTML5 enrichis et le codage Zen
- Utilisez les nouvelles fonctionnalités de l’éditeur CSS incluses dans Web Essentials, telles que le sélecteur de couleurs et l’info-bulle de la matrice du navigateur
- Utilisez les nouvelles fonctionnalités de l’éditeur JavaScript incluses dans Web Essentials, telles que Extract to file et IntelliSense pour tous les éléments HTML
- Échanger des données entre votre navigateur et Visual Studio à l’aide d’un lien de navigateur

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Les éléments suivants sont requis pour effectuer ce laboratoire pratique :

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou version ultérieure
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Installation

Pour exécuter les exercices dans ce laboratoire pratique, vous devez d’abord configurer votre environnement.

1. Ouvrez une fenêtre de l’Explorateur Windows et accédez au dossier **source** du laboratoire.
2. Cliquez avec le bouton droit sur **Setup. cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui va configurer votre environnement et installer les extraits de code Visual Studio pour ce Lab.
3. Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.

> [!NOTE]
> Vérifiez que vous avez vérifié toutes les dépendances de ce Lab avant d’exécuter le programme d’installation.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Utilisation des extraits de code

Tout au long du document Lab, vous serez invité à insérer des blocs de code. Pour plus de commodité, la majeure partie de ce code est fournie sous la forme d’extraits de Visual Studio Code, auxquels vous pouvez accéder à partir de Visual Studio 2013 pour éviter d’avoir à l’ajouter manuellement.

> [!NOTE]
> Chaque exercice est accompagné d’une solution de démarrage située dans le dossier **Begin** de l’exercice, qui vous permet de suivre chaque exercice indépendamment des autres. N’oubliez pas que les extraits de code ajoutés au cours d’un exercice sont absents de ces solutions de démarrage et peuvent ne pas fonctionner tant que vous n’avez pas terminé l’exercice. À l’intérieur du code source d’un exercice, vous trouverez également un dossier de **fin** contenant une solution Visual Studio avec le code qui résulte de l’exécution des étapes de l’exercice correspondant. Vous pouvez utiliser ces solutions comme conseils si vous avez besoin d’aide supplémentaire au fur et à mesure que vous travaillez dans ce laboratoire pratique.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique comprend les exercices suivants :

1. [Utilisation du lien de navigateur et de Web Essentials](#Exercise1)
2. [Tirer parti des extraits de code et IntelliSense](#Exercise2)

> [!NOTE]
> Lorsque vous démarrez Visual Studio pour la première fois, vous devez sélectionner l’une des collections de paramètres prédéfinies. Chaque collection prédéfinie est conçue pour correspondre à un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, les extraits de code IntelliSense et les options de boîte de dialogue. Les procédures de ce laboratoire décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lors de l’utilisation de la collection de **paramètres de développement généraux** . Si vous choisissez une autre collection de paramètres pour votre environnement de développement, il peut y avoir des différences entre les étapes que vous devez prendre en compte.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Exercice 1 : utilisation de la liaison de navigateur et de Web Essentials

**Web Essentials** est une extension Visual Studio qui ajoute une variété de fonctionnalités utiles pour le développement Web moderne, principalement axée sur l’expérience de développement Web beaucoup plus rapide et plus agréable. Vous pouvez installer Web Essentials à partir de la Galerie d’extensions dans Visual Studio.

Le **lien du navigateur** est une nouvelle fonctionnalité incluse dans Visual Studio 2013 qui fournit un canal entre l’IDE de Visual Studio et tout navigateur ouvert pour échanger des données entre votre application Web et Visual Studio. Web Essentials étend le lien du navigateur avec les outils permettant de manipuler le modèle objet DOM et les styles CSS de vos pages Web directement à partir du navigateur.

Dans cet exercice, vous allez explorer certaines des fonctionnalités prises en charge par **Web Essentials** et le **lien du navigateur** pour améliorer une page de quiz simple.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Tâche 1 : exécution du projet dans plusieurs navigateurs

Dans cette tâche, vous allez configurer votre application Web pour qu’elle s’exécute dans plusieurs navigateurs à la fois, ce qui est utile pour les tests entre navigateurs.

1. Ouvrez **Microsoft Visual Studio**.
2. Dans le menu **fichier** , sélectionnez **ouvrir | Projet/solution...** et accédez à **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** dans le dossier **source** du Lab (C:\WebCampsTK\HOL\VSWebTooling\Source). Sélectionnez **Begin. sln** , puis cliquez sur **ouvrir**.
3. Dans la barre d’outils de Visual Studio, développez le menu du navigateur et sélectionnez **Parcourir avec...** .

    ![Browse with (option de menu)](visual-studio-2013-web-tools/_static/image1.png "Naviguer avec... dans le menu du navigateur")

    *Browse with (option de menu)*
4. Dans la boîte de dialogue **naviguer avec** , sélectionnez **Google Chrome** et **Internet Explorer** en maintenant la touche **CTRL** enfoncée et cliquez sur **définir par défaut**.

    ![Boîte de dialogue naviguer avec](visual-studio-2013-web-tools/_static/image2.png "Boîte de dialogue naviguer avec")

    *Sélection de plusieurs navigateurs par défaut*
5. Google Chrome et Internet Explorer doivent maintenant apparaître comme navigateurs par défaut. Cliquez sur **Annuler** pour refermer la boîte de dialogue.

    ![Google Chrome et Internet Explorer en tant que navigateurs par défaut](visual-studio-2013-web-tools/_static/image3.png "Navigateurs par défaut Google Chrome et Internet Explorer")

    *Google Chrome et Internet Explorer en tant que navigateurs par défaut*

    > [!NOTE]
    > Après la configuration des navigateurs par défaut, l’option **plusieurs navigateurs** est sélectionnée dans le menu du navigateur.
    > 
    > ![Plusieurs navigateurs](visual-studio-2013-web-tools/_static/image4.png "Plusieurs navigateurs")
6. Appuyez sur **CTRL** + **F5** pour exécuter l’application sans débogage.
7. Lorsque les deux fenêtres du navigateur sont ouvertes, placez l’une d’elles au-dessus de l’autre pour voir les mises à jour sur les deux navigateurs simultanément. Les navigateurs doivent afficher une page Web avec un rectangle bleu clair.

    ![Placer un navigateur au-dessus de l’autre](visual-studio-2013-web-tools/_static/image5.png "Placer un navigateur au-dessus de l’autre")

    *Placer un navigateur au-dessus de l’autre*
8. Ne fermez pas les navigateurs. Vous les utiliserez dans la tâche suivante.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Tâche 2 : utilisation du codage zen pour créer des éléments HTML

Le **codage Zen** est un plug-in d’éditeur pour le codage et la modification de code HTML, XML, XSL (ou tout autre format de code structuré) à grande vitesse. Le cœur de ce plug-in est un moteur d’abréviation puissant qui vous permet de développer des expressions, similaires aux sélecteurs CSS, dans du code HTML. Le codage Zen est un moyen rapide d’écrire du code HTML à l’aide d’une syntaxe de sélecteur de style CSS.

Dans cet exercice, vous allez utiliser la fonctionnalité de codage Zen fournie par Web Essentials pour générer les boutons HTML qui représentent les options de la question.

1. Revenez à Visual Studio.
2. Ouvrez le fichier **index. cshtml** situé dans les **vues** | dossier de **démarrage** .
3. Remplacez le **&lt;!--TODO : ajoutez des options ici--&gt;** commentaire avec le code suivant, puis appuyez sur la touche **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Le code doit être développé en HTML.

    ![HTML développé](visual-studio-2013-web-tools/_static/image6.png "HTML développé")

    *HTML développé*

    > [!NOTE]
    > Pour en savoir plus sur la syntaxe de codage Zen, consultez l' [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)suivant.
5. Cliquez sur le bouton **Actualiser les navigateurs liés** pour mettre à jour les deux navigateurs.

    ![Actualiser les navigateurs liés](visual-studio-2013-web-tools/_static/image7.png "Actualiser les navigateurs liés")

    *Actualiser les navigateurs liés*

    ![Internet Explorer-page mise à jour avec quatre boutons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-page mise à jour avec quatre boutons")

    *Internet Explorer-page mise à jour avec quatre boutons*

    ![Google Chrome-page mise à jour avec quatre boutons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-page mise à jour avec quatre boutons")

    *Google Chrome-page mise à jour avec quatre boutons*
6. Revenez à Visual Studio.
7. Vous avez ajouté les boutons à la page, mais vous devez toujours ajouter une question de modèle. Pour ce faire, vous allez utiliser une nouvelle fonctionnalité dans Web Essentials, appelée **Lorem ipsum Generator**. Localisez l’élément **div** avec l’attribut de **classe** **front**.
8. Ajoutez le code suivant en tant que premier élément enfant de la **balise div**, puis appuyez sur la touche **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Le code doit être développé en HTML.

    ![Lorem ipsum généré automatiquement](visual-studio-2013-web-tools/_static/image10.png "Lorem ipsum généré automatiquement")

    *Lorem ipsum généré automatiquement*

    > [!NOTE]
    > Dans le cadre du codage Zen, vous pouvez désormais générer du code Lorem ipsum directement dans l’éditeur HTML. Tapez simplement **Lorem** et appuyez sur Tab pour insérer un texte de 30 mots **(** Lorem). Par exemple, *lorem10* insère 10 mots de Lorem ipsum.
10. Vous allez ajouter un logo en haut de la question à l’aide d’une autre nouvelle fonctionnalité de Web Essentials appelée « **Générateur de pixels Lorem**». Ajoutez le code suivant en tant que premier élément enfant de l’élément **div** avec le **conteneur** comme valeur de **classe** , puis appuyez sur la touche **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Le code doit se développer en HTML.

    ![Pixel de Lorem généré automatiquement](visual-studio-2013-web-tools/_static/image11.png "Pixel de Lorem généré automatiquement")

    *Pixel de Lorem généré automatiquement*

    > [!NOTE]
    > Dans le cadre du codage Zen, vous pouvez également générer du code de pixel Lorem directement dans l’éditeur HTML. Pour ce faire, tapez **pix-200x200-animaux** et appuyez sur la **touche Tab** . une balise **img** avec une image 200x200 d’un animal est insérée. Pour plus d’informations, reportez-vous au [Pixel Lorem](http://www.lorempixel.com).
12. Cliquez sur le bouton **Actualiser les navigateurs liés** pour mettre à jour les deux navigateurs.

    ![Internet Explorer-image et texte générés automatiquement](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer-image et texte générés automatiquement")

    *Internet Explorer-image et texte générés automatiquement*

    ![Google Chrome-image et texte générés automatiquement](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-image et texte générés automatiquement")

    *Google Chrome-image et texte générés automatiquement*

    > [!NOTE]
    > Étant donné que l’image est sélectionnée de manière aléatoire lors de l’ajout de l’extrait de code, l’image affichée dans les navigateurs peut être différente.
13. Ne fermez pas les navigateurs. Vous les utiliserez dans la tâche suivante.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Tâche 3 : mise à jour d’une propriété de style

Dans cette tâche, vous allez utiliser la fonctionnalité de **mode d’inspection** du lien de navigateur pour détecter l’emplacement exact où l’élément DOM spécifique est généré, puis mettre à jour la propriété Color de cet élément à l’aide d’un sélecteur de couleurs fourni par Web Essentials.

1. Dans le navigateur Internet Explorer, appuyez sur **CTRL** + **ALT** + **I** pour activer le mode inspection.
2. Placez le pointeur sur la bordure bleue claire, puis cliquez sur.

    ![Déplacement du pointeur sur la bordure bleue claire](visual-studio-2013-web-tools/_static/image14.png "Déplacement du pointeur sur la bordure bleue claire")

    *Déplacement du pointeur sur la bordure bleue claire*
3. Revenez à Visual Studio. Notez comment l’élément HTML que vous avez sélectionné dans le navigateur est également sélectionné dans l’éditeur HTML de Visual Studio.

    ![Élément HTML sélectionné dans l’éditeur HTML de Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Élément HTML sélectionné dans l’éditeur HTML de Visual Studio")

    *Élément HTML sélectionné dans l’éditeur HTML de Visual Studio*
4. Vous allez maintenant mettre à jour la classe CSS **front** pour modifier le style de l’élément sélectionné. Pour ce faire, appuyez sur **CTRL** +  **,** pour ouvrir la zone **de** recherche. Tapez **site. CSS** et appuyez sur **entrée** pour ouvrir le fichier.

    ![Ouverture du fichier site. CSS](visual-studio-2013-web-tools/_static/image16.png "Ouverture du fichier site. CSS")

    *Ouverture du fichier site. CSS*
5. Appuyez sur **CTRL** + **F** et tapez **. Flip-Container. front** pour trouver le sélecteur CSS.
6. Cliquez sur le carré bleu clair dans la propriété border de la classe pour ouvrir le sélecteur de couleurs.

    ![Ouverture du sélecteur de couleurs](visual-studio-2013-web-tools/_static/image17.png "Ouverture du sélecteur de couleurs")

    *Ouverture du sélecteur de couleurs*
7. Développez le sélecteur de couleurs en cliquant sur le bouton de Chevron et en sélectionnant une nouvelle couleur.

    ![Agrandissement du sélecteur de couleurs](visual-studio-2013-web-tools/_static/image18.png "Agrandissement du sélecteur de couleurs")

    *Agrandissement du sélecteur de couleurs*
8. Appuyez sur **CTRL** + **ALT** + **entrée** pour actualiser les navigateurs liés.
9. Basculez vers Internet Explorer et notez la modification de la couleur de la bordure.

    ![Internet Explorer-couleur de bordure mise à jour](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-couleur de bordure mise à jour")

    *Internet Explorer-couleur de bordure mise à jour*
10. Basculez vers Google Chrome et notez la modification de la couleur de la bordure.

    ![Google Chrome-couleur de bordure mise à jour](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-couleur de bordure mise à jour")

    *Google Chrome-couleur de bordure mise à jour*
11. Revenez à Visual Studio.
12. Accédez à la fin du fichier **site. CSS** et appuyez sur **CTRL** + **F** pour rechercher le sélecteur **. BTN** .
13. Notez que la propriété **-WebKit-border-radius** est soulignée en vert.

    ![-WebKit-propriété border-radius du sélecteur BTN](visual-studio-2013-web-tools/_static/image21.png "-WebKit-propriété border-radius du sélecteur BTN")

    *-WebKit-propriété border-radius du sélecteur BTN*
14. Placez le signe insertion dans la propriété **-WebKit-border-radius** . Une ligne bleue doit s’afficher sous la première lettre du premier mot de la propriété. Il s’agit de la **balise active**.
15. Appuyez sur **CTRL** +  **.** pour ouvrir le menu suggestions et cliquez sur **Ajouter la propriété standard manquante (frontière-RADIUS)** .

    ![Ajouter une suggestion de propriété standard manquante](visual-studio-2013-web-tools/_static/image22.png "Ajouter une suggestion de propriété standard manquante")

    *Ajouter une suggestion de propriété standard manquante*
16. La propriété **border-radius** est automatiquement ajoutée à la règle CSS.

    ![Propriété standard ajoutée manquante](visual-studio-2013-web-tools/_static/image23.png "Propriété standard ajoutée manquante")

    *Propriété standard ajoutée manquante*
17. Déplacez le pointeur sur la propriété **border-radius** pour afficher l' **info-bulle**de la matrice du navigateur. L' **info-bulle** de la matrice du navigateur affiche la disponibilité de la propriété dans chaque navigateur.

    ![Info-bulle de la matrice du navigateur](visual-studio-2013-web-tools/_static/image24.png "Info-bulle de la matrice du navigateur")

    *Info-bulle de la matrice du navigateur*
18. Notez que la valeur de la propriété **border-radius** est toujours soulignée. Déplacez le pointeur sur la valeur pour afficher le message d’avertissement.

    ![Border-avertissement de valeur de propriété RADIUS](visual-studio-2013-web-tools/_static/image25.png "Border-avertissement de valeur de propriété RADIUS")

    *Border-avertissement de valeur de propriété RADIUS*
19. Supprimez l’unité de la valeur de propriété **border-radius** comme suggéré par l’info-bulle.
20. Comme **border-radius** est la propriété standard permettant de définir le mode d’affichage des angles de bordure arrondis, vous pouvez supprimer la propriété et la valeur **-WebKit-border-radius** de la règle CSS.
21. Placez le signe insertion dans la propriété **retour automatique** à la ligne et notez que la balise active s’affiche également ci-dessous.
22. Ouvrez le menu et cliquez sur **Ajouter les détails du fournisseur manquant**.

    ![Ajouter une suggestion de spécificités de fournisseur manquantes](visual-studio-2013-web-tools/_static/image26.png "Ajouter une suggestion de spécificités de fournisseur manquantes")

    *Ajouter une suggestion de spécificités de fournisseur manquantes*
23. La propriété **-MS-Word-Wrap** est automatiquement ajoutée à la règle CSS.

    ![Propriété spécifique au fournisseur ajoutée](visual-studio-2013-web-tools/_static/image27.png "Propriété spécifique au fournisseur ajoutée")

    *Propriété spécifique au fournisseur ajoutée*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Tâche 4 : mise à jour du code HTML à partir du navigateur

Dans cette tâche, vous allez utiliser la fonctionnalité de **mode création** du lien du navigateur pour modifier l’objet DOM à partir du navigateur et transférer les modifications vers le fichier source HTML dans Visual Studio.

1. Dans Google Chrome, appuyez sur **CTRL** + **ALT** + **D** pour activer le mode création.
2. Déplacez le pointeur sur l’étiquette **Lorem ipsum dolor sit amet** , puis cliquez sur.

    ![Modification de la question](visual-studio-2013-web-tools/_static/image28.png "Modification de la question")

    *Modification de la question*
3. Un curseur doit s’afficher. Remplacez le texte d’origine par ce qui ressemble à l' *écriture d’une question plus longue*, puis appuyez sur **Échap** pour quitter le mode création.

    ![Question modifiée](visual-studio-2013-web-tools/_static/image29.png "Question modifiée")

    *Question modifiée*
4. Revenez à Visual Studio et ouvrez **index. cshtml**, s’il n’est pas déjà ouvert. Notez que le texte interne de l’élément **&lt;p&gt;** a été mis à jour.

    ![Mise à jour de la question dans la page HTML](visual-studio-2013-web-tools/_static/image30.png "Mise à jour de la question dans la page HTML")

    *Mise à jour de la question dans la page HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Tâche 5 : examen des avertissements liés au SEO

L' **optimisation du moteur de recherche** (SEO) est le processus qui consiste à rendre un site Web plus haut dans la liste des résultats d’un moteur de recherche. Plus le nombre de sites est élevé et plus la liste est cohérente, plus le nombre de visiteurs du moteur de recherche sera important. Web Essentials intègre un outil analytique qui examine le code HTML, signale les problèmes détectés et fournit de l’aide pour les résoudre.

1. Accédez au menu **affichage** , puis cliquez sur **liste d’erreurs** pour ouvrir la fenêtre **liste d’erreurs** .

    ![Liste d’erreurs dans le menu Affichage](visual-studio-2013-web-tools/_static/image31.png "Liste d’erreurs dans le menu Affichage")

    *Liste d’erreurs dans le menu Affichage*
2. Notez qu’il existe un avertissement SEO indiquant qu’il manque une balise de **méta&gt;&lt;** pour la description de la page. Double-cliquez sur l’entrée d’avertissement SEO pour la corriger.

    ![Fenêtre Liste d’erreurs](visual-studio-2013-web-tools/_static/image32.png "Liste d'erreurs (fenêtre)")

    *Fenêtre Liste d’erreurs*
3. Dans la boîte de dialogue **Web Essentials** , cliquez sur **Oui** pour insérer une description &lt;balise Meta&gt;.

    ![Boîte de dialogue Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Boîte de dialogue Web Essentials")

    *Boîte de dialogue Web Essentials*
4. L’éditeur pour **\_Layout. cshtml** s’ouvre et la balise de **méta-&gt;&lt;** est automatiquement ajoutée à la section **Head** du fichier html.

    ![Balise méta ajoutée automatiquement dans _Layout page](visual-studio-2013-web-tools/_static/image34.png "Balise méta ajoutée automatiquement dans _Layout page")

    *Balise méta ajoutée automatiquement à \_page de disposition*
5. Remplacez la valeur de l’attribut **content** par *GeekQuiz* et enregistrez le fichier.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Exercice 2 : tirer parti des extraits de code et d’IntelliSense

Avec Web Essentials, l’éditeur HTML a été étendu avec des fonctionnalités supplémentaires. Dans cet exercice, vous verrez quelques nouvelles fonctionnalités qui sont utiles lors du développement d’applications Web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Tâche 1 : utilisation d’IntelliSense dans les documents HTML

La première nouvelle fonctionnalité que vous verrez dans cette tâche est appelée **Dynamic IntelliSense**. Dynamic IntelliSense lit d’autres balises et attributs pour déduire les ID possibles que vous allez utiliser.

Dans cette tâche, vous allez créer un nouvel élément de formulaire HTML qui contient une étiquette et un champ d’entrée. Ensuite, vous allez ajouter un attribut **for** à l’étiquette pour le lier à l’entrée, et vous verrez des suggestions IntelliSense basées sur les ID des entrées dans la portée.

1. Ouvrez **Visual Studio Express 2013 pour le Web** et la solution **Begin. sln** située dans le dossier **source/EX2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** . Vous pouvez également continuer avec la solution que vous avez obtenue au cours de l’exercice précédent.
2. Dans **Explorateur de solutions**, ouvrez le fichier **index. cshtml** situé dans les **vues** | dossier de **démarrage** .
3. Ajoutez le formulaire suivant à la **section&lt;&gt;** élément.

    (Extrait de code- *VisualStudio2013WebTooling* - *EX2* - *formulaire*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. La balise d’entrée doit être précédée d’une étiquette avec une description du champ. Ajoutez l’étiquette suivante avant la balise d’entrée.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. L’attribut **for** d’un **&lt;étiquette&gt;** spécifie l’élément de formulaire auquel une étiquette est liée. La valeur de l’attribut doit être égale à l’ID de l’élément associé. Ajoutez l’attribut **for** à l' **&lt;étiquette&gt;** élément. Comme indiqué dans l’illustration suivante, la valeur&quot; nom de l' &quot;s’affiche dans la zone IntelliSense, en fonction de l’ID des éléments dans la même portée (le **&lt;de formulaire** englobant&gt;).

    ![Indication de l’ID dans IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Indication de l’ID dans IntelliSense")

    *Indication de l’ID dans IntelliSense*
6. Supprimez le **formulaire&lt;** ajouté récemment&gt;et son contenu.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Tâche 2 : utilisation d’extraits de code HTML

HTML5 a introduit plus de 25 nouvelles balises sémantiques. Visual Studio offrait déjà la prise en charge d’IntelliSense pour ces balises, mais Visual Studio 2013 le rend plus rapide et plus facile à écrire en ajoutant de nouveaux extraits de code. Bien que ces balises ne soient pas compliquées, elles sont accompagnées de quelques subtilités, telles que l’ajout du codec de secours approprié pour la balise *audio* . Dans cette tâche, vous verrez les extraits de code HTML pour la balise audio.

1. Dans le fichier **index. cshtml** , tapez **&lt;AUD** à l’intérieur de la **section&lt;&gt;** élément, comme indiqué dans l’illustration suivante.

    ![Insertion d’un élément audio](visual-studio-2013-web-tools/_static/image36.png "Insertion d’un élément audio")

    *Insertion d’un élément audio*
2. Appuyez deux fois sur la **touche Tab** et notez la façon dont le code suivant est ajouté à la page et le curseur est placé sur l’attribut **src** de la première source.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > En appuyant deux fois sur la touche **Tab** , l’extrait de code est inséré. L’extrait de code audio montre l’utilisation standard de la balise *audio* , avec deux fichiers sources pour une meilleure prise en charge.
3. Supprimez la deuxième ligne et mettez à jour la source de la première ligne à l’aide du lien suivant vers le WebCampsTV Katana Show : [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Le code obtenu est illustré ci-dessous.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Le fichier source *KatanaProject. mp3* est utilisé à titre d’exemple. Vous pouvez utiliser une autre source si vous préférez.
4. Appuyez sur **CTRL** + **S** pour enregistrer le fichier.
5. Appuyez sur **CTRL** + **F5** pour démarrer l’application.
6. Notez qu’un lecteur audio a été ajouté à l’application.

    ![Lecteur audio dans Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Lecteur audio dans Internet Explorer")

    *Lecteur audio dans Internet Explorer*

    ![Lecteur audio dans Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Lecteur audio dans Google Chrome")

    *Lecteur audio dans Google Chrome*
7. Ne fermez pas les navigateurs. Vous les utiliserez dans la tâche suivante.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Tâche 3 : utilisation d’IntelliSense dans les documents JavaScript

Avec Web Essentials 2013, les feuilles de style et les pages HTML produisent une liste des ID et des noms de classe. Dans cette tâche, vous allez apprendre comment ces listes améliorent la prise en charge de JavaScript IntelliSense dans Web Essentials 2013.

1. Dans le fichier **index. cshtml** , ajoutez le code suivant pour définir une balise de **script** pour le code JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Ajoutez le code suivant à l’intérieur de la balise de **script** pour définir la fonction de rappel Ready.

    (Extrait de code- *VisualStudio2013WebTooling* - *EX2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Placez le signe insertion dans la balise **script** et appuyez sur **CTRL** +  **.** pour ouvrir le menu de suggestion.
4. Cliquez sur **Extraire dans un fichier**.

    ![Suggestions JavaScript d’extraction vers un fichier](visual-studio-2013-web-tools/_static/image39.png "Suggestions JavaScript d’extraction vers un fichier")

    *Suggestions JavaScript d’extraction vers un fichier*
5. Dans la fenêtre **Enregistrer sous** , sélectionnez le dossier **scripts** , nommez le fichier **init. js** , puis cliquez sur **Enregistrer**.

    ![Enregistrer en tant que fenêtre](visual-studio-2013-web-tools/_static/image40.png "Enregistrer en tant que fenêtre")

    *Enregistrer en tant que fenêtre*

    > [!NOTE]
    > Le fichier **init. js** est créé et le contenu du script est déplacé vers le fichier.
    > 
    > ![Fichier init. js créé avec le contenu inclus](visual-studio-2013-web-tools/_static/image41.png "Fichier init. js créé avec le contenu inclus")
    > 
    > *Fichier init. js créé avec le contenu inclus*
6. Ouvrez le fichier **index. cshtml** et vérifiez que la balise de script a été remplacée par une référence au fichier **init. js** .

    ![Référence HTML init. js](visual-studio-2013-web-tools/_static/image42.png "Référence HTML init. js")

    *Référence HTML init. js*
7. Accédez à la **Explorateur de solutions** et notez que le fichier **init. js** a été inclus automatiquement dans la solution.

    ![Fichier init. js inclus dans la solution](visual-studio-2013-web-tools/_static/image43.png "Fichier init. js inclus dans la solution")

    *Fichier init. js inclus dans la solution*
8. Revenez au fichier **init. js** pour mettre à jour le rappel de fonction **Ready** .
9. À l’intérieur de la définition de rappel de fonction qui est passée à *Ready*, ajoutez le code suivant pour obtenir tous les éléments par un attribut de classe spécifique.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Appuyez sur **CTRL** + **espace** entre les guillemets à l’intérieur de l’appel de fonction **getElementsByClassName** .

    ![Présentation d’IntelliSense pour la fonction getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Présentation d’IntelliSense pour la fonction getElementsByClassName")

    *Présentation d’IntelliSense pour la fonction getElementsByClassName*

    > [!NOTE]
    > Notez qu’IntelliSense affiche les classes définies dans les feuilles de style de projet.
11. Remplacez la ligne que vous avez créée par le code suivant.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Placez le **curseur après l'** intérieur des guillemets de la fonction **GetElementsByTagName** et appuyez sur **CTRL** + **espace**.

    ![Présentation d’IntelliSense pour la méthode getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Présentation d’IntelliSense pour la méthode getElementByTagName")

    *Présentation d’IntelliSense pour la méthode getElementsByTagName*
13. Sélectionnez **&quot;&quot;audio** dans la liste et appuyez sur **entrée**. Le résultat est affiché dans l’illustration suivante.

    ![Récupération d’éléments audio](visual-studio-2013-web-tools/_static/image46.png "Récupération d’éléments audio")

    *Récupération d’éléments audio*
14. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le fichier **init. js** dans le dossier **scripts** et sélectionnez **réduire le ou les fichiers JavaScript** dans le menu **Web Essentials** .

    ![Réduire le ou les fichiers JavaScript](visual-studio-2013-web-tools/_static/image47.png "Réduire les fichiers JavaScript")

    *Réduire le ou les fichiers JavaScript*
15. Lorsque vous êtes invité à activer la minimisation automatique lorsque le fichier source change, cliquez sur **Oui**.

    ![Activation de l’avertissement de minimisation automatique](visual-studio-2013-web-tools/_static/image48.png "Activation de l’avertissement de minimisation automatique")

    *Activation de l’avertissement de minimisation automatique*

    > [!NOTE]
    > **Init. min. js** est créé et ajouté en tant que dépendance du fichier **init. js** .
    > 
    > ![Fichier init. min. js créé](visual-studio-2013-web-tools/_static/image49.png "Fichier init. min. js créé")
    > 
    > *Fichier init. min. js créé*
16. Ouvrez le fichier **init. min. js** et notez que le fichier est minimisés.

    ![Contenu du fichier init. min. js](visual-studio-2013-web-tools/_static/image50.png "Contenu du fichier init. min. js")

    *Contenu du fichier init. min. js*
17. Dans le fichier **init. js** , ajoutez le code suivant sous l’appel de fonction **GetElementsByTagName** pour lire tous les éléments audio.

    (Extrait de code- *VisualStudio2013WebTooling* - *EX2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Cliquez sur **CTRL** + **S** pour enregistrer le fichier. Étant donné que le fichier minimisés est déjà ouvert, une boîte de dialogue s’affiche indiquant que le fichier a été modifié en dehors de l’éditeur de code source. Cliquez sur **Oui**.

    ![AVERTISSEMENT Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "AVERTISSEMENT Microsoft Visual Studio")

    *AVERTISSEMENT Microsoft Visual Studio*
19. Revenez au fichier **init. min. js** pour vérifier que le fichier a été mis à jour avec le nouveau code.

    ![Fichier init. min. js mis à jour](visual-studio-2013-web-tools/_static/image52.png "Fichier init. min. js mis à jour")

    *Fichier init. min. js mis à jour*
20. Cliquez sur le bouton **Actualiser le lien du navigateur** .
21. Une fois les deux navigateurs actualisés, les lecteurs audio que vous avez vus au cours de la tâche précédente commencent à être lus automatiquement.

    ![Lecteur audio inclus dans l’affichage](visual-studio-2013-web-tools/_static/image53.png "Lecteur audio inclus dans l’affichage")

    *Lecteur audio inclus dans l’affichage*

---

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

En effectuant ce laboratoire pratique, vous avez appris à :

- Utilisez les nouvelles fonctionnalités de l’éditeur HTML incluses dans Web Essentials, telles que les extraits de code HTML5 enrichis et le codage Zen
- Utilisez les nouvelles fonctionnalités de l’éditeur CSS incluses dans Web Essentials, telles que le sélecteur de couleurs et l’info-bulle de la matrice du navigateur
- Utilisez les nouvelles fonctionnalités de l’éditeur JavaScript incluses dans Web Essentials, telles que Extract to file et IntelliSense pour tous les éléments HTML
- Échanger des données entre votre navigateur et Visual Studio à l’aide d’un lien de navigateur
