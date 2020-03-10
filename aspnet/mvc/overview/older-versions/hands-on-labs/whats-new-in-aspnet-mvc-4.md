---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Nouveautés de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 est une infrastructure permettant de créer des applications Web évolutives et basées sur des normes à l’aide de modèles de conception bien établis et de la puissance des ASP.NET et...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 4235f4fe666cdeb7d0821127a2b349f2ff30cd6e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539437"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Nouveautés d’ASP.NET MVC 4

par l' [équipe Web camps](https://twitter.com/webcamps)

[Télécharger le kit de formation Web camps](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 est une infrastructure permettant de créer des applications Web évolutives et basées sur des normes à l’aide de modèles de conception bien établis et de la puissance du ASP.NET et du .NET Framework. Cette nouvelle version de l’infrastructure est axée sur la simplification du développement d’applications Web mobiles.

Pour commencer, lorsque vous créez un projet ASP.NET MVC 4, il existe désormais un modèle de projet d’application mobile que vous pouvez utiliser pour générer une application autonome spécifiquement pour les appareils mobiles. En outre, ASP.NET MVC 4 s’intègre à jQuery mobile via un package NuGet jQuery. mobile. MVC. jQuery mobile est une infrastructure basée sur HTML5 pour le développement d’applications Web qui sont compatibles avec toutes les plateformes d’appareils mobiles populaires, y compris Windows Phone, iPhone, Android, etc. Toutefois, si vous avez besoin d’une spécialisation, ASP.NET MVC 4 vous permet également de servir des vues différentes pour différents appareils et de fournir des optimisations spécifiques à l’appareil.

Dans ce laboratoire pratique, vous allez commencer par le modèle de projet ASP.NET MVC 4 &quot;Internet application&quot; pour créer une application de la galerie photo. Vous allez améliorer progressivement l’application à l’aide des nouvelles fonctionnalités de jQuery mobile et ASP.NET MVC 4 pour la rendre compatible avec différents appareils mobiles et navigateurs Web de bureau. Vous en apprendrez également plus sur les nouvelles recettes de code pour la génération de code et sur la manière dont ASP.NET MVC 4 facilite l’écriture de méthodes d’action asynchrones en prenant en charge les types de retour de tâche&lt;ActionResult&gt;.

> [!NOTE]
> Tous les exemples de code et les extraits de code sont inclus dans le kit de formation Web camps, disponible dans les [versions Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Le projet propre à ce laboratoire est disponible dans [Nouveautés de Web Forms dans ASP.NET 4,5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans ce laboratoire pratique, vous allez apprendre à :

- Tirez parti des améliorations apportées aux modèles de projet ASP.NET MVC, y compris le nouveau modèle de projet d’application mobile
- Utiliser l’attribut de fenêtre d’affichage HTML5 et les requêtes de média CSS pour améliorer l’affichage sur les appareils mobiles
- Utilisez jQuery mobile pour les améliorations progressives et la création d’une interface utilisateur Web optimisée pour la technologie tactile
- Créer des affichages spécifiques aux appareils mobiles
- Utiliser le composant d’affichage-sélecteur pour basculer entre les affichages mobiles et de bureau dans l’application
- Créer des contrôleurs asynchrones à l’aide du support des tâches

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Conditions préalables requises

Pour effectuer ce laboratoire, vous devez disposer des éléments suivants :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieur (lisez [l’annexe B](#AppendixB) pour obtenir des instructions sur la façon de l’installer).
- [ASP.NET MVC 4](../../../mvc4.md) (inclus dans l’installation Microsoft Visual Studio 2012)
- Windows Phone Emulator (inclus dans le [Kit de développement logiciel (SDK) Windows Phone 7.1.1](https://www.microsoft.com/download/details.aspx?id=29233))
- Facultatif- [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) avec l’extension de l’appareil pour le **simulateur de la prune iPhone** (uniquement pour l’exercice 3 utilisé pour parcourir l’application Web avec un simulateur iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Installation

Tout au long du document Lab, vous serez invité à insérer des blocs de code. Pour plus de commodité, la majeure partie de ce code est fournie sous la forme d’extraits de Visual Studio Code, que vous pouvez utiliser dans Visual Studio pour éviter d’avoir à l’ajouter manuellement.

Pour installer les extraits de code :

1. Ouvrez une fenêtre de l’Explorateur Windows et accédez au dossier **Source\Setup** du laboratoire.
2. Double-cliquez sur le fichier **Setup. cmd** dans ce dossier pour installer les extraits de code Visual Studio.

Si vous n’êtes pas familiarisé avec les extraits de Visual Studio Code et que vous souhaitez apprendre à les utiliser, vous pouvez vous référer à l’annexe de ce document &quot;[annexe A : utilisation d’extraits de Code](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique comprend les exercices suivants :

1. [Nouveaux modèles de projet ASP.NET MVC 4](#Exercise1)
2. [Création de l’application Web de la Galerie de photos](#Exercise2)
3. [Ajout de la prise en charge des appareils mobiles](#Exercise3)
4. [Utilisation de contrôleurs asynchrones](#Exercise4)

> [!NOTE]
> Chaque exercice est accompagné d’un dossier de **fin** contenant la solution obtenue à obtenir après avoir effectué les exercices. Vous pouvez utiliser cette solution comme guide si vous avez besoin d’aide supplémentaire pour suivre les exercices.

Durée estimée pour effectuer ce laboratoire : **60 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Exercice 1 : nouveaux modèles de projet ASP.NET MVC 4

Dans cet exercice, vous allez découvrir les améliorations apportées aux modèles de projet ASP.NET MVC 4. En plus du modèle d’application Internet, déjà présent dans MVC 3, cette version comprend désormais un modèle distinct pour les applications mobiles. Tout d’abord, vous allez examiner les fonctionnalités pertinentes de chacun des modèles. Ensuite, vous allez travailler sur le rendu de votre page correctement sur les différentes plateformes à l’aide de l’approche appropriée.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Tâche 1 : exploration du modèle d’application Internet

1. Ouvrez **Visual Studio**.
2. Sélectionnez le **fichier | Nouveau |** Commande de menu projet. Dans la boîte de dialogue **nouveau projet** , sélectionnez le **visuel C# | Modèle Web** dans l’arborescence du volet gauche, puis choisissez **Application Web ASP.NET MVC 4.** Nommez la **Galerie**de projets, sélectionnez un emplacement (ou laissez la valeur par défaut), puis cliquez sur **OK**.

    > [!NOTE]
    > Vous allez personnaliser ultérieurement la solution Photogallery ASP.NET MVC 4 que vous créez maintenant.

    ![Création d’un nouveau projet](whats-new-in-aspnet-mvc-4/_static/image1.png "Création d’un projet")

    *Création d’un nouveau projet*
3. Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle de projet **application Internet** , puis cliquez sur **OK**. Assurez-vous que vous avez sélectionné Razor comme moteur de vue.

    ![Création d’une nouvelle application Internet ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "Création d’une nouvelle application Internet ASP.NET MVC 4")

    *Création d’une nouvelle application Internet ASP.NET MVC 4*

    > [!NOTE]
    > Syntaxe Razor a été introduite dans ASP.NET MVC 3. Son objectif est de réduire le nombre de caractères et les séquences de touches nécessaires dans un fichier, ce qui permet un flux de travail de codage rapide et fluide. Razor exploite les C# compétences linguistiques existantes/vb (ou autres) et fournit une syntaxe de balisage de modèle qui permet un flux de travail de construction html impressionnant.
4. Appuyez sur **F5** pour exécuter la solution et voir les modèles renouvelés. Vous pouvez consulter les fonctionnalités suivantes :

    **Modèles modernes**

    Les modèles ont été renouvelés, ce qui fournit des styles plus modernes.

    ![Modèles ASP.NET MVC 4 restylisés](whats-new-in-aspnet-mvc-4/_static/image3.png "Modèles MVC 4 restylisés")

    *Modèles ASP.NET MVC 4 restylisés*

    ![Page nouveau contact](whats-new-in-aspnet-mvc-4/_static/image4.png "Page nouveau contact")

    *Page nouveau contact*

    **Rendu adaptatif**

    Examinez le redimensionnement de la fenêtre du navigateur et notez la façon dont la mise en page s’adapte dynamiquement à la nouvelle taille de fenêtre. Ces modèles utilisent la technique de rendu adaptatif pour un rendu correct sur les plateformes de bureau et mobiles sans aucune personnalisation.

    ![Modèle de projet ASP.NET MVC 4 dans différentes tailles de navigateur](whats-new-in-aspnet-mvc-4/_static/image5.png "Modèle de projet ASP.NET MVC 4 dans différentes tailles de navigateur")

    *Modèle de projet ASP.NET MVC 4 dans différentes tailles de navigateur*

    **Interface utilisateur plus riche avec JavaScript**

    Une autre amélioration des modèles de projet par défaut est l’utilisation de JavaScript pour fournir un code JavaScript plus interactif. Les liens de connexion et de Registre utilisés dans le modèle illustrent l’utilisation des validations jQuery pour valider les champs d’entrée côté client.

    ![Validation jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *Validation jQuery*

    > [!NOTE]
    > Notez les deux sections de connexion, dans la première section, vous pouvez vous connecter à l’aide d’un compte inscrit à partir du site. dans la deuxième section, vous pouvez également vous connecter à l’aide d’un autre service d’authentification tel que Google (désactivé par défaut).
5. Fermez le navigateur pour arrêter le débogueur et revenir à Visual Studio.
6. Ouvrez le fichier **authconfig.cs** situé sous le dossier de démarrage de l' **application\_** .
7. Supprimez le commentaire de la dernière ligne pour inscrire le client Google pour l’authentification *OAuth* .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Notez que vous pouvez facilement activer l’authentification à l’aide de n’importe quel service OpenID ou OAuth comme Facebook, Twitter, Microsoft, etc.
8. Appuyez sur **F5** pour exécuter la solution et accéder à la page de connexion.
9. Sélectionnez service **Google** pour vous connecter.

    ![Sélection du service de connexion](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Sélection du service de connexion*
10. Connectez-vous à l’aide de votre compte Google.
11. Autoriser le site (localhost) à récupérer des informations à partir d’un compte Google.
12. Enfin, vous devez vous inscrire sur le site pour associer le compte Google.

   ![Associer votre compte Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Association de votre compte Google*
13. Fermez le navigateur pour arrêter le débogueur et revenir à Visual Studio.
14. Explorez maintenant la solution pour découvrir d’autres nouvelles fonctionnalités introduites par ASP.NET MVC 4 dans le modèle de projet.

   ![Modèle de projet d’application Internet ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image9.png "Modèle de projet d’application Internet ASP.NET MVC 4")

   *Modèle de projet d’application Internet ASP.NET MVC 4*

    - **Balisage HTML 5**

       Parcourez les vues du modèle pour trouver le nouveau balisage du thème.

       ![Nouveau modèle, à l’aide du balisage Razor et HTML5 sur. cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "Nouveau modèle, à l’aide du balisage Razor et HTML5 sur. cshtml.")

       *Nouveau modèle, à l’aide du balisage Razor et HTML5 (about. cshtml).*
    - **Bibliothèques JavaScript mises à jour**

       Le modèle par défaut ASP.NET MVC 4 comprend maintenant KnockoutJS, une infrastructure MVVM JavaScript qui vous permet de créer des applications Web riches et très réactives à l’aide de JavaScript et HTML. Comme dans MVC3, les bibliothèques d’interface utilisateur jQuery et jQuery sont également incluses dans ASP.NET MVC 4.

     > [!NOTE]
     > Vous pouvez obtenir plus d’informations sur la bibliothèque KnockOutJS dans ce lien : [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/). En outre, vous pouvez en savoir plus sur jQuery et l’interface utilisateur jQuery dans [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Tâche 2 : exploration du modèle d’application mobile

ASP.NET MVC 4 facilite le développement de sites Web pour les navigateurs mobiles et de tablette. Ce modèle a la même structure d’application que le modèle d’application Internet (Notez que le code du contrôleur est pratiquement identique), mais son style a été modifié pour être rendu correctement dans les appareils mobiles tactiles.

1. Sélectionnez le **fichier | Nouveau |** Commande de menu projet. Dans la boîte de dialogue **nouveau projet** , sélectionnez le **visuel C# | Modèle Web** dans l’arborescence du volet gauche, puis choisissez l' **Application Web ASP.NET MVC 4.** Nommez le projet **Photogallery. mobile**, sélectionnez un emplacement (ou laissez la valeur par défaut), sélectionnez &quot;ajouter à la solution&quot; et cliquez sur **OK**.
2. Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle de projet **application mobile** , puis cliquez sur **OK**. Assurez-vous que vous avez sélectionné Razor comme moteur de vue.

    ![Création d’une nouvelle application mobile MVC 4 ASP.NET](whats-new-in-aspnet-mvc-4/_static/image11.png "Création d’une nouvelle application mobile MVC 4 ASP.NET")

    *Création d’une nouvelle application mobile MVC 4 ASP.NET*
3. Vous pouvez à présent explorer la solution et découvrir les nouvelles fonctionnalités introduites par le modèle de solution ASP.NET MVC 4 pour mobile :

    - **Bibliothèque mobile jQuery**

        Le modèle de projet application mobile comprend la bibliothèque mobile jQuery, qui est une bibliothèque open source pour la compatibilité avec les navigateurs mobiles. jQuery Mobile applique une amélioration progressive aux navigateurs mobiles qui prennent en charge CSS et JavaScript. L’amélioration progressive permet à tous les navigateurs d’afficher le contenu de base d’une page Web, alors qu’il permet uniquement aux navigateurs les plus puissants d’afficher le contenu enrichi. Les fichiers JavaScript et CSS, inclus dans le style jQuery mobile, aident les navigateurs mobiles à adapter le contenu à l’écran sans apporter de modification au balisage de la page.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *bibliothèque mobile jQuery incluse dans le modèle*
    - **Balisage HTML5**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Modèle d’application mobile utilisant le balisage HTML5, (login. cshtml et index. cshtml)*
4. Appuyez sur **F5** pour exécuter la solution.
5. Ouvrez l' **émulateur Windows Phone 7**.
6. Dans l’écran de démarrage du téléphone, ouvrez Internet Explorer. Consultez l’URL de démarrage de l’application de bureau et accédez à cette URL à partir du téléphone (par exemple, `http://localhost:[PortNumber]/`).
7. Vous pouvez maintenant accéder à la page de connexion ou consulter la page à propos de. Notez que le style du site Web est basé sur la nouvelle application Metro pour mobile. Le modèle de projet ASP.NET MVC 4 s’affiche correctement sur les périphériques mobiles, ce qui garantit que tous les éléments de la page sont visibles et activés. Notez que les liens sur l’en-tête sont suffisamment grands pour être cliqués ou frappés.

    ![Pages de modèle de projet dans un appareil mobile](whats-new-in-aspnet-mvc-4/_static/image14.png "Pages de modèle de projet dans un appareil mobile")

    *Pages de modèle de projet dans un appareil mobile*
8. Le nouveau modèle utilise également la **balise méta de Viewport**. La plupart des navigateurs mobiles définissent une largeur pour une fenêtre de navigateur virtuelle ou &quot;fenêtre d’affichage&quot;, ce qui est plus grand que la largeur réelle du périphérique mobile. Cela permet aux navigateurs mobiles d’afficher l’intégralité de la page Web dans l’affichage virtuel. La **balise Meta Viewport** permet aux développeurs Web de définir la largeur, la hauteur et l’échelle de la zone de navigateur sur les périphériques mobiles **.** Le modèle ASP.NET MVC 4 pour les applications mobiles définit la fenêtre d’affichage sur la largeur de l’appareil (&quot;Width = Device-Width&quot;) dans le modèle de disposition (*Views\Shared\_Layout. cshtml*), de sorte que la fenêtre d’affichage de toutes les pages est définie sur la largeur de l’écran de l’appareil. Notez que la balise méta de la fenêtre d’affichage ne modifie pas l’affichage du navigateur par défaut.
9. Ouvrez **\_Layout. cshtml**, situé dans les **vues | Dossier partagé** et commentez la balise Meta Viewport. Exécutez l’application, si elle n’est pas déjà ouverte, et consultez les différences.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Le site après avoir commenté la balise Meta Viewport](whats-new-in-aspnet-mvc-4/_static/image15.png "Le site après avoir commenté la balise Meta Viewport")

*Le site après avoir commenté la balise Meta Viewport*
10. Dans Visual Studio, appuyez sur **maj** + **F5** pour arrêter le débogage de l’application.
11. Supprimez les marques de commentaire de la balise Meta Viewport.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Tâche 3 : utilisation du rendu adaptatif

Au cours de cette tâche, vous allez apprendre une autre méthode permettant de restituer une page Web correctement sur les appareils mobiles et les navigateurs Web en même temps sans aucune personnalisation. Vous avez déjà utilisé la balise méta de Viewport avec un objectif similaire. Vous allez maintenant rencontrer une autre méthode puissante : le *rendu adaptatif*.

Le rendu adaptatif est une technique qui utilise des **requêtes de média CSS3** pour personnaliser le style appliqué à une page. Les requêtes de média définissent des conditions dans une feuille de style, en regroupant les styles CSS sous une certaine condition. Si la condition a la valeur true, le style est appliqué aux objets déclarés.

La flexibilité offerte par la technique de rendu adaptatif permet de personnaliser l’affichage du site sur différents appareils. Vous pouvez définir autant de styles que vous le souhaitez sur une feuille de style unique sans écrire de code logique pour choisir le style. Par conséquent, il s’agit d’un moyen très clair d’adapter les styles de page, car il réduit la quantité de code et de logique dupliqués à des fins de rendu. En revanche, la consommation de bande passante augmente, car la taille de vos fichiers CSS peut croître de manière marginale.

En utilisant la technique de rendu adaptatif, votre site s' **affiche correctement, quel que soit le navigateur.** Toutefois, vous devez envisager si la charge supplémentaire de la bande passante est un problème.

> [!NOTE]
> Le format de base d’une requête de média est : @media \[portée : tout | ordinateur de poche | imprimer | projection |\] d’écran ([propriété : valeur] et... [propriété : valeur])

Exemples de requêtes de média : &gt; **@media All et (max-width : 1000px) et (min-width : 700px) {}:** pour toutes les résolutions entre 700px et 1000px.

> **@media écran et (min-width : 400px) et (max-width : 700px) {...} :** Uniquement pour les écrans. La résolution doit être comprise entre 400 et 700px.
> 
> **@media Handheld et (min-width : 20em), Screen et (min-width : 20em) {...} :** Pour les appareils de poche et les écrans. La largeur minimale doit être supérieure à 20em.
> 
> Vous trouverez plus d’informations à ce sujet sur le [site W3C](http://www.w3.org/TR/css3-mediaqueries/).

Vous allez maintenant découvrir le fonctionnement du rendu adaptatif, en améliorant la lisibilité du modèle de site Web par défaut ASP.NET MVC 4.

1. Ouvrez la solution **Photogallery. sln** que vous avez créée à la tâche 1, puis sélectionnez le projet **Photogallery** . Appuyez sur **F5** pour exécuter la solution.
2. Redimensionnez la largeur du navigateur, en définissant les fenêtres sur la moitié ou sur une valeur inférieure à celle du quart de sa taille d’origine. Notez ce qui se passe avec les éléments dans l’en-tête : certains éléments n’apparaîtront pas dans la zone visible de l’en-tête.
3. Ouvrez le fichier **site. CSS** à partir de l’Explorateur de solutions de Visual Studio, situé dans le dossier du projet de **contenu** . Appuyez sur **Ctrl + F** pour ouvrir la recherche intégrée Visual Studio, puis écrivez `@media` pour localiser la **requête de média CSS**.

    La condition de requête de média définie dans ce modèle fonctionne de cette façon : lorsque la taille de la fenêtre du navigateur est inférieure à **850 PX**, les règles CSS appliquées sont celles définies à l’intérieur de ce bloc multimédia.

    ![Recherche de la requête de média](whats-new-in-aspnet-mvc-4/_static/image16.png "Recherche de la requête de média")

    *Recherche de la requête de média*
4. Remplacez la valeur d’attribut max-width définie dans 850 PX par **10px**, afin de désactiver le rendu adaptatif, puis appuyez sur **CTRL + S** pour enregistrer les modifications. Revenez au navigateur et appuyez sur **CTRL + F5** pour actualiser la page avec les modifications que vous avez apportées. Notez les différences dans les deux pages lorsque vous ajustez la largeur de la fenêtre.

    ![À gauche, la page applique le style de @media, dans la partie droite, le style est omis](whats-new-in-aspnet-mvc-4/_static/image17.png "À gauche, la page applique le style de @media, dans la partie droite, le style est omis")

    *À gauche, la page applique le style de @media, dans la partie droite, le style est omis*

    À présent, examinons ce qui se passe sur les appareils mobiles :

    ![À gauche, la page applique le style de @media, dans la partie droite, le style est omis](whats-new-in-aspnet-mvc-4/_static/image18.png "À gauche, la page applique le style de @media, dans la partie droite, le style est omis")

    *À gauche, la page applique le style de @media, dans la partie droite, le style est omis*

    Même si vous remarquerez que les modifications apportées lors du rendu de la page dans un navigateur Web ne sont pas très importantes, lorsque vous utilisez un appareil mobile, les différences deviennent plus évidentes. Sur le côté gauche de l’image, nous pouvons voir que le style personnalisé a amélioré la lisibilité.

    Le rendu adaptatif peut être utilisé dans de nombreux autres scénarios, ce qui facilite l’application d’un style conditionnel à un site Web et la résolution des problèmes de style courants avec une approche claire.

    La balise Meta Viewport et les requêtes de média CSS ne sont pas spécifiques à ASP.NET MVC 4, ce qui vous permet de tirer parti de ces fonctionnalités dans n’importe quelle application Web.
5. Dans Visual Studio, appuyez sur **maj** + **F5** pour arrêter le débogage de l’application.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Exercice 2 : création de l’application Web de la Galerie de photos

Dans cet exercice, vous allez travailler sur une application de la galerie photo pour afficher des photos. Vous allez commencer par le modèle de projet ASP.NET MVC 4, puis vous ajouterez une fonctionnalité pour récupérer des photos d’un service et les afficher dans la page d’accueil.

Dans l’exercice suivant, vous allez mettre à jour cette solution pour améliorer son affichage sur les appareils mobiles.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Tâche 1 : création d’un service de photos factices

Dans cette tâche, vous allez créer un simulacre du service photo pour récupérer le contenu qui s’affichera dans la Galerie. Pour ce faire, vous allez ajouter un nouveau contrôleur qui renverra simplement un fichier JSON avec les données de chaque photo.

1. Ouvrez **Visual Studio** s’il n’est pas déjà ouvert.
2. Sélectionnez le **fichier | Nouveau |** Commande de menu projet. Dans la boîte de dialogue **nouveau projet** , sélectionnez le **visuel C# | Modèle Web** dans l’arborescence du volet gauche, puis choisissez **Application Web ASP.NET MVC 4.** Nommez la **Galerie**de projets, sélectionnez un emplacement (ou laissez la valeur par défaut), puis cliquez sur **OK**. Vous pouvez également continuer à travailler à partir de votre solution d' **application Internet** ASP.NET MVC 4 existante de l' **exercice 1** et ignorer l’étape suivante.
3. Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez le modèle de projet **application Internet** , puis cliquez sur **OK**. Assurez-vous que l’option Razor est sélectionnée comme moteur d’affichage.
4. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier de données de l' **application\_** de votre projet, puis sélectionnez **Ajouter | Élément existant**. Accédez au dossier **Source\Assets\App\_Data** de ce laboratoire et ajoutez le fichier **photos. JSON** .
5. Créez un nouveau contrôleur avec le nom **photocontroller**. Pour ce faire, cliquez avec le bouton droit sur le dossier **Controllers** , accédez à **Ajouter** et sélectionnez **contrôleur.** Renseignez le nom du contrôleur, laissez le modèle de **contrôleur MVC vide** , puis cliquez sur **Ajouter**.

    ![Ajout du photocontrôleur](whats-new-in-aspnet-mvc-4/_static/image19.png "Ajout du photocontrôleur")

    *Ajout du photocontrôleur*
6. Remplacez la méthode d' **index** par l’action de la **Galerie** suivante et retournez le contenu du fichier JSON que vous avez récemment ajouté au projet.

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex02-Gallery action*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Appuyez sur **F5** pour exécuter la solution, puis accédez à l’URL suivante afin de tester le service de photo factice : `http://localhost:[port]/photo/gallery` (la valeur [port] dépend du port actuel dans lequel l’application a été lancée). La demande adressée à cette URL doit récupérer le contenu du fichier **photos. JSON** .

    ![Test du service de photos factice](whats-new-in-aspnet-mvc-4/_static/image20.png "Test du service de photos factice")

    *Test du service de photos factice*

Dans une implémentation réelle, vous pouvez utiliser [API Web ASP.net](../../../../web-api/index.md) pour implémenter le service de Galerie de photos. ASP.NET Web API est une infrastructure qui facilite le développement de services HTTP disponibles sur un large éventail de clients, tels que des navigateurs et des appareils mobiles. L'API Web ASP.NET est une plate-forme idéale pour générer des applications RESTful sur le .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Tâche 2 : affichage de la Galerie de photos

Dans cette tâche, vous allez mettre à jour la page d’hébergement pour afficher la Galerie de photos à l’aide du service simulé que vous avez créé lors de la première tâche de cet exercice. Vous allez ajouter des fichiers de modèle et mettre à jour les vues de la Galerie.

1. Dans Visual Studio, appuyez sur **maj** + **F5** pour arrêter le débogage de l’application.
2. Créez la classe **photo** dans le dossier **Models** . Pour ce faire, cliquez avec le bouton droit sur le dossier **modèles** , sélectionnez **Ajouter** , puis cliquez sur **classe**. Ensuite, définissez le nom sur **photo.cs** et cliquez sur **Ajouter**.
3. Ajoutez les membres suivants à la classe **photo** .

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex02-photo Model*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Ouvrez le fichier **HomeController.cs** dans le dossier **Controllers**.
5. Ajoutez les instructions using suivantes.

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex02-HomeController using*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Mettez à jour l’action d' **index** pour utiliser **httpclient** pour récupérer les données de la Galerie, puis utilisez **JavaScriptSerializer** pour la désérialiser dans le modèle de vue.

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex02-index*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Ouvrez le fichier **index. cshtml** situé dans le dossier **Views\Home** et remplacez tout le contenu par le code suivant.

    Ce code parcourt toutes les photos récupérées à partir du service et les affiche dans une liste non triée.

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex02-photo List*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier de **contenu** de votre projet, puis sélectionnez **Ajouter | Élément existant**. Accédez au dossier **Source\Assets\Content** de ce Lab et ajoutez le fichier **site. CSS** . Vous devrez confirmer son remplacement. Si le fichier **site. CSS** est ouvert, vous devez confirmer le rechargement du fichier.
9. Ouvrez l’Explorateur de fichiers et copiez le dossier **photos** entier situé dans le dossier **Source\Assets** de ce Lab dans le dossier racine de votre projet dans Explorateur de solutions.
10. Exécutez l'application. Vous devez maintenant voir la page d’hébergement affichant les photos dans la Galerie.

    ![Galerie de photos](whats-new-in-aspnet-mvc-4/_static/image21.png "Galerie de photos")

    *Galerie de photos*
11. Dans Visual Studio, appuyez sur **maj** + **F5** pour arrêter le débogage de l’application.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Exercice 3 : ajout de la prise en charge des appareils mobiles

L’une des principales mises à jour dans ASP.NET MVC 4 est la prise en charge du développement mobile. Dans cet exercice, vous allez explorer ASP.NET MVC 4 nouvelles fonctionnalités pour les applications mobiles en étendant la solution Photogallery que vous avez créée dans l’exercice précédent.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Tâche 1 : installation de jQuery mobile dans une application ASP.NET MVC 4

1. Ouvrez le **début** de la solution situé dans **source/EX3-MobileSupport/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la **console du gestionnaire de package** en cliquant sur **Outils** > **Gestionnaire de package NuGet** > option de menu **console du gestionnaire de package** .

    ![Ouverture de la console du gestionnaire de package NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "Ouverture de la console du gestionnaire de package NuGet")

    *Ouverture de la console du gestionnaire de package NuGet*
3. Dans la console du gestionnaire de package, exécutez la commande suivante pour installer le package **jQuery. mobile. Mvc** .

    jQuery mobile est une bibliothèque open source pour la création d’une interface utilisateur Web optimisée pour les fonctions tactiles. Le package NuGet jQuery. mobile. MVC offre des assistances pour utiliser jQuery mobile avec une application ASP.NET MVC 4.

    > [!NOTE]
    > En exécutant la commande suivante, vous allez télécharger la bibliothèque jQuery. mobile. MVC à partir de NuGet.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Cette commande installe jQuery mobile et certains fichiers d’assistance, y compris les éléments suivants :

    - **Views/Shared/\_Layout. mobile. cshtml**: est une disposition basée sur mobile jQuery optimisée pour un écran plus petit. Lorsque le site Web reçoit une demande d’un navigateur mobile, il remplace la disposition d’origine (\_Layout. cshtml) par celle-ci.
    - Composant View-Switcher : est constitué de la vue partielle **Views/Shared/\_ViewSwitcher. cshtml** et du contrôleur **ViewSwitcherController.cs** . Ce composant affiche un lien sur les navigateurs mobiles pour permettre aux utilisateurs de basculer vers la version de bureau de la page.  
        ![Projet de Galerie de photos avec prise en charge mobile](whats-new-in-aspnet-mvc-4/_static/image23.png "Projet de Galerie de photos avec prise en charge mobile")

        *Projet de Galerie de photos avec prise en charge mobile*
4. Inscrivez les offres groupées mobiles. Pour ce faire, ouvrez le fichier **global.asax.cs** et ajoutez la ligne suivante.

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex03-inscrire les offres groupées mobiles*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Exécutez l’application à l’aide d’un navigateur Web de bureau.
6. Ouvrez l' **émulateur Windows Phone 7,** situé dans le **menu Démarrer | Tous les programmes | Kit de développement logiciel (SDK) Windows Phone 7,1 | Windows Phone émulateur.**
7. Dans l’écran de démarrage du téléphone, ouvrez Internet Explorer. Consultez l’URL de démarrage de l’application et accédez à cette URL avec le navigateur téléphonique (par exemple, `http://localhost:[PortNumber]/`).

    Vous remarquerez que votre application sera différente dans l’émulateur Windows Phone, car jQuery. mobile. MVC a créé de nouvelles ressources dans votre projet qui affichent des vues optimisées pour les appareils mobiles.

    Notez le message en haut du téléphone, en indiquant le lien qui bascule vers la vue du bureau. En outre, la disposition **\_. mobile. cshtml** qui a été créée par le package que vous avez installé comprend une disposition différente dans l’application.

    > [!NOTE]
    > Jusqu’à présent, il n’existe aucun lien pour revenir à l’affichage mobile. Il sera inclus dans les versions ultérieures.

    ![Affichage mobile de la page d’hébergement de la Galerie de photos](whats-new-in-aspnet-mvc-4/_static/image24.png "Affichage mobile de la page d’hébergement de la Galerie de photos")

    *Affichage mobile de la page d’hébergement de la Galerie de photos*
8. Dans Visual Studio, appuyez sur **maj** + **F5** pour arrêter le débogage de l’application.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Tâche 2 : création de vues mobiles

Dans cette tâche, vous allez créer une version mobile de la vue d’index avec un contenu adapté pour une meilleure apparence des appareils mobiles.

1. Copiez la vue **Views\Home\Index.cshtml** et collez-la pour créer une copie, renommez le nouveau fichier en **index. mobile. cshtml**.
2. Ouvrez la vue New created **index. mobile. cshtml** et remplacez la balise &lt;UL&gt; existante par ce code. En procédant ainsi, vous allez mettre à jour la balise &lt;UL&gt; avec des annotations de données mobiles jQuery pour utiliser les thèmes mobiles de jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Notez que :
    > 
    > - L’attribut de **rôle de données** défini sur **ListView** affiche la liste à l’aide des styles ListView.
    > 
    > - L’attribut d' **incrustation des données** défini sur true affiche la liste avec une bordure et une marge arrondies.
    > 
    > - L’attribut de **filtre de données** défini sur **true** génère une zone de recherche.
    > 
    > Pour plus d’informations sur les conventions pour les appareils mobiles jQuery, consultez la documentation du projet : [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Appuyez sur **CTRL + S** pour enregistrer les modifications.
4. Basculez vers l' **émulateur de Windows Phone** et actualisez le site. Notez la nouvelle apparence de la liste de la Galerie, ainsi que la nouvelle zone de recherche située en haut. Ensuite, tapez un mot dans la zone de recherche (par exemple, **tulipes**) pour lancer une recherche dans la Galerie de photos.

    ![Galerie utilisant le style ListView avec filtrage](whats-new-in-aspnet-mvc-4/_static/image25.png "Galerie utilisant le style ListView avec filtrage")

    *Galerie utilisant le style ListView avec filtrage*

    Pour résumer, vous avez utilisé la recette afficher la recette pour créer une copie de la vue d’index avec le suffixe de&quot; mobile &quot;. Ce suffixe indique à ASP.NET MVC 4 que chaque demande générée à partir d’un périphérique mobile utilise cette copie de l’index. En outre, vous avez mis à jour la version mobile de la vue index pour utiliser jQuery mobile pour améliorer l’apparence du site dans les appareils mobiles.
5. Revenez à Visual Studio et ouvrez **site. mobile. CSS** situé sous le dossier **content** .
6. Corrigez la position du titre de la photo pour qu’il apparaisse sur le côté droit de l’image. Pour ce faire, ajoutez le code suivant au fichier **site. mobile. CSS** .

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Appuyez sur **CTRL + S** pour enregistrer les modifications.
8. Revenez à l' **émulateur Windows Phone** et actualisez le site. Notez que le titre de la photo est correctement positionné.

    ![Titre positionné sur le côté droit de l’image](whats-new-in-aspnet-mvc-4/_static/image26.png "Titre positionné sur le côté droit de l’image")

    *Titre positionné sur le côté droit de l’image*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Tâche 3-thèmes mobiles jQuery

Chaque disposition et chaque widget de jQuery mobile est conçu autour d’un nouveau Framework CSS orienté objet qui permet d’appliquer un thème de conception visuel unifié complet aux sites et aux applications.

le thème par défaut de jQuery mobile comprend 5 nuances qui sont des lettres (a, b, c, d, e) à des fins de référence rapide.

Dans cette tâche, vous allez mettre à jour la disposition mobile pour utiliser un autre thème que celui par défaut.

1. Revenez à Visual Studio.
2. Ouvrez le fichier **\_Layout. mobile. cshtml** situé dans **Views\Shared**.
3. Recherchez l’élément div avec le rôle de données défini sur &quot;&quot; de page et mettez à jour le **thème de données** pour &quot;**e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Appuyez sur **CTRL + S** pour enregistrer les modifications.
5. Actualisez le site dans l' **émulateur de Windows Phone** et notez le nouveau schéma de couleurs.

    ![Disposition mobile avec un autre modèle de couleurs](whats-new-in-aspnet-mvc-4/_static/image27.png "Disposition mobile avec un autre modèle de couleurs")

    *Disposition mobile avec un autre modèle de couleurs*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Tâche 4 : utilisation du composant d’affichage-sélecteur et des fonctionnalités de remplacement du navigateur

Une convention pour les pages Web optimisées pour les appareils mobiles consiste à ajouter un lien dont le texte est une vue du bureau ou un mode de site complet qui permet aux utilisateurs de basculer vers une version de bureau de la page. Le package jQuery. mobile. MVC comprend un exemple de composant d' **affichage-sélecteur** à cet effet utilisé dans **\_la vue Layout. mobile. cshtml** .

![Lien pour basculer vers la vue du Bureau](whats-new-in-aspnet-mvc-4/_static/image28.png "Lien pour basculer vers la vue du Bureau")

*Lien pour basculer vers la vue du Bureau*

Le sélecteur de vue utilise une nouvelle fonctionnalité appelée **remplacement du navigateur**. Cette fonctionnalité permet à votre application de traiter les requêtes comme si elles provenaient d’un autre navigateur (agent utilisateur) que celui à partir duquel elles proviennent.

Dans cette tâche, vous allez explorer l’exemple d’implémentation d’un commutateur de vue ajouté par jQuery. mobile. MVC et les nouvelles fonctionnalités de remplacement de navigateur dans ASP.NET MVC 4.

1. Revenez à Visual Studio.
2. Ouvrez\_la vue **Layout. mobile. cshtml** qui se trouve sous le dossier **Views\Shared** et notez le composant View-Switcher référencé comme vue partielle.

    ![Disposition mobile à l’aide du composant View-Switcher](whats-new-in-aspnet-mvc-4/_static/image29.png "Disposition mobile à l’aide du composant View-Switcher")

    *Disposition mobile à l’aide du composant View-Switcher*
3. Ouvrez la vue partielle **ViewSwitcher. cshtml\_** .

    La vue partielle utilise la nouvelle méthode **ViewContext. HttpContext. GetOverriddenBrowser ()** pour déterminer l’origine de la demande Web et afficher le lien correspondant pour basculer vers le bureau ou les affichages mobiles.

    La méthode **GetOverriddenBrowser** retourne une instance **HttpBrowserCapabilitiesBase** qui correspond à l’agent utilisateur actuellement défini pour la demande (réelle ou substituée). Vous pouvez utiliser cette valeur pour récupérer des propriétés telles que **IsMobileDevice**.

    ![Vue partielle ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "Vue partielle ViewSwitcher")

    *Vue partielle ViewSwitcher*
4. Ouvrez la classe **ViewSwitcherController.cs** située dans le dossier **Controllers** . Vérifiez que l’action SwitchView est appelée par le lien dans le composant ViewSwitcher et remarquez les nouvelles méthodes HttpContext.

    - La méthode **HttpContext. ClearOverriddenBrowser ()** supprime tout agent utilisateur substitué pour la demande actuelle.
    - La méthode **HttpContext. SetOverriddenBrowser ()** remplace la valeur réelle de l’agent utilisateur de la demande à l’aide de l’agent utilisateur spécifié.  
        ![Contrôleur ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "Contrôleur ViewSwitcher")  
*Contrôleur ViewSwitcher*

        La substitution du navigateur est une fonctionnalité principale de ASP.NET MVC 4, qui est également disponible même si vous n’installez pas le package jQuery. mobile. MVC. Toutefois, cette fonctionnalité affecte uniquement l’affichage, la disposition et la vue partielle, et n’affecte pas les fonctionnalités qui dépendent de l’objet Request. Browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Tâche 5 : ajout du sélecteur d’affichage dans la vue du Bureau

Dans cette tâche, vous allez mettre à jour la disposition du Bureau pour inclure le sélecteur d’affichage. Cela permettra aux utilisateurs mobiles de revenir à l’affichage mobile pour naviguer dans la vue du bureau.

1. Actualisez le site dans l' **émulateur de Windows Phone**.
2. Cliquez sur le lien **affichage du Bureau** en haut de la Galerie. Notez qu’il n’existe aucun sélecteur d’affichage dans la vue du Bureau pour vous permettre de revenir à l’affichage mobile.
3. Revenez à Visual Studio et ouvrez la vue **Layout. cshtml\_** .
4. Recherchez la section de connexion et insérez un appel pour afficher le\_vue partielle **ViewSwitcher** sous la vue partielle **\_LogOnPartial** . Ensuite, appuyez sur **CTRL + S** pour enregistrer les modifications.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Appuyez sur **CTRL + S** pour enregistrer les modifications.
6. Actualisez la page dans l’émulateur de Windows Phone et double-cliquez sur l’écran pour effectuer un zoom avant. Notez que la page d’hébergement affiche maintenant le lien **affichage mobile** qui passe de la vue mobile à la vue bureau.

    ![Sélecteur de vue rendu dans la vue du Bureau](whats-new-in-aspnet-mvc-4/_static/image32.png "Sélecteur de vue rendu dans la vue du Bureau")

    *Sélecteur de vue rendu dans la vue du Bureau*
7. Revenez à la vue mobile et accédez à la page à **propos** de (http://localhost[port]/Home/about). Notez que, même si vous n’avez pas créé de vue about. mobile. cshtml, la page about est affichée à l’aide de la disposition mobile (\_Layout. mobile. cshtml).

    ![Page à propos de](whats-new-in-aspnet-mvc-4/_static/image33.png "Page About")

    *Page à propos de*
8. Enfin, ouvrez le site dans un navigateur Web de bureau. Notez qu’aucune des mises à jour précédentes n’a affecté la vue du bureau.

    ![Vue de bureau de Photogallery](whats-new-in-aspnet-mvc-4/_static/image34.png "Vue de bureau de Photogallery")

    *Vue de bureau de Photogallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Tâche 6 : création de nouveaux modes d’affichage

La nouvelle fonctionnalité modes d’affichage permet à une application de sélectionner des vues en fonction du navigateur qui génère la demande. Par exemple, si un navigateur de bureau demande la page d’hébergement, l’application renverra le modèle **Views\Home\Index.cshtml** . Ensuite, si un navigateur mobile demande la page d’hébergement, l’application renverra le modèle **Views\Home\Index.mobile.cshtml** .

Dans cette tâche, vous allez créer une disposition personnalisée pour les appareils iPhone, et vous devrez simuler les demandes des appareils iPhone. Pour ce faire, vous pouvez utiliser un émulateur ou un simulateur iPhone (comme [Electric mobile Simulator](http://www.electricplum.com/)) ou un navigateur avec des modules complémentaires qui modifient l’agent utilisateur. Pour obtenir des instructions sur la façon de définir la chaîne de l’agent utilisateur dans un navigateur Safari pour émuler un iPhone, consultez Comment faire en [sorte que Safari le](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) fasse savoir IE sur le blog de David Alison.

**Notez que cette tâche est facultative et que vous pouvez continuer tout au long du laboratoire sans l’exécuter.**

1. Dans Visual Studio, appuyez sur **maj** + **F5** pour arrêter le débogage de l’application.
2. Ouvrez **global.asax.cs** et ajoutez l’instruction using suivante.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Ajoutez le code en surbrillance suivant à la méthode Start de l’application\_.

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex03-iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Vous avez inscrit un nouveau **DefaultDisplayMode nommé &quot;iPhone&quot;** , dans la liste statique **DisplayModeProvider. instance. modes** statique, qui sera comparé à chaque demande entrante. Si la demande entrante contient la chaîne &quot;iPhone&quot;, ASP.NET MVC trouvera les vues dont le nom contient le suffixe de&quot; iPhone &quot;. Le paramètre 0 indique la manière dont un spécifique est le nouveau mode ; par exemple, cette vue est plus spécifique que la règle générale &quot;. mobile&quot; qui correspond aux demandes des appareils mobiles.

Après l’exécution de ce code, quand un navigateur iPhone génère une demande, votre application utilise la disposition **Views\Shared\\_Layout. iPhone. cshtml** que vous allez créer au cours des étapes suivantes.

> [!NOTE]
> Cette méthode de test de la demande pour iPhone a été simplifiée à des fins de démonstration et peut ne pas fonctionner comme prévu pour chaque chaîne d’agent utilisateur iPhone (par exemple, le test respecte la casse).

4. Créez une copie du fichier **\_Layout. mobile. cshtml** dans le dossier **Views\Shared** et renommez la copie en &quot; **\_Layout. iPhone. cshtml**&quot;.
5. Ouvrez **\_Layout. iPhone. cshtml** que vous avez créé à l’étape précédente.
6. Recherchez l’élément div avec l’attribut de rôle de données défini sur **page** et remplacez l’attribut **Data-theme** par &quot;**un**&quot;.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Vous avez maintenant 3 dispositions dans votre application ASP.NET MVC 4 :

1. **\_Layout. cshtml**: disposition par défaut utilisée pour les navigateurs de bureau.
2. **\_Layout. mobile. cshtml**: disposition par défaut utilisée pour les appareils mobiles.
3. **\_Layout. iPhone. cshtml**: disposition spécifique pour les appareils iPhone, à l’aide d’un jeu de couleurs différent à distinguer de \_Layout. mobile. cshtml.
7. Appuyez sur **F5** pour exécuter l’application et parcourir le site dans l' **émulateur de Windows Phone**.
8. Ouvrez un **simulateur iPhone** (voir l' [annexe C](#AppendixC) pour obtenir des instructions sur l’installation et la configuration d’un simulateur iPhone), puis accédez au site. Notez que chaque téléphone utilise le modèle spécifique.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Utilisation de différents affichages pour chaque appareil mobile*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Exercice 4 : utilisation de contrôleurs asynchrones

Microsoft .NET Framework 4,5 introduit de nouvelles fonctionnalités de C# langage dans et Visual Basic pour fournir une nouvelle base pour l’asynchronie dans la programmation .net. Cette nouvelle Fondation rend la programmation asynchrone similaire à-et à propos de la programmation synchrone aussi simple que synchrone. Vous êtes maintenant en mesure d’écrire des méthodes d’action asynchrones dans ASP.NET MVC 4 à l’aide de la classe **AsyncController** . Vous pouvez utiliser des méthodes d'action asynchrones pour des requêtes à durée d'exécution longue et n'utilisant pas le processeur de manière intensive. Cela permet de ne pas bloquer le serveur Web pendant le traitement de la requête. La classe AsyncController est généralement utilisée pour les appels de service Web de longue durée.

Cet exercice explique les principes fondamentaux de l’opération asynchrone dans ASP.NET MVC 4. Si vous souhaitez approfondir votre expérience, vous pouvez consulter l’article suivant : [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Tâche 1 : implémentation d’un contrôleur asynchrone

1. Ouvrez la solution **Begin** à l’emplacement **source/EX4-Async/Begin/** Folder. Dans le cas contraire, vous pouvez continuer à utiliser la solution **finale** obtenue en effectuant l’exercice précédent.

   1. Si vous avez ouvert la solution **Begin** fournie, vous devrez télécharger des packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le menu **projet** et sélectionnez **gérer les packages NuGet**.
   2. Dans la boîte de dialogue **gérer les packages NuGet** , cliquez sur **restaurer** pour télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **générer** | **générer la solution**.

      > [!NOTE]
      > L’un des avantages de l’utilisation de NuGet est que vous n’avez pas besoin d’expédier toutes les bibliothèques de votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions du package dans le fichier Packages. config, vous pourrez télécharger toutes les bibliothèques requises la première fois que vous exécuterez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez la classe **HomeController.cs** à partir du dossier **Controllers** .
3. Ajoutez l’instruction using suivante.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Mettez à jour la classe **HomeController** pour hériter de **AsyncController**. Les contrôleurs qui dérivent de AsyncController permettent à ASP.NET de traiter les requêtes asynchrones, et ils peuvent toujours traiter les méthodes d’action synchrones.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Ajoutez le mot clé **Async** à la méthode **index** et faites en sorte qu’il retourne la tâche de type **&lt;ActionResult&gt;** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > Le mot clé **Async** est l’un des nouveaux mots clés que le .NET Framework 4,5 fournit ; elle indique au compilateur que cette méthode contient du code asynchrone. Un objet de **tâche** représente une opération asynchrone qui peut se terminer à un moment donné dans le futur.
6. Remplacez le **client. Appel de GetAsync ()** avec la version Async complète à l’aide du mot clé await, comme indiqué ci-dessous.

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex04-GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > Dans la version précédente, vous utilisiez la propriété **result** de l’objet **Task** pour bloquer le thread jusqu’à ce que le résultat soit retourné (version Sync).
    > 
    > L’ajout du mot clé **await** indique au compilateur d’attendre de façon asynchrone la tâche retournée par l’appel de méthode. Cela signifie que le reste du code est exécuté comme un rappel uniquement après la fin de la méthode attendue. Une autre chose à noter est que vous n’avez pas besoin de modifier votre bloc try-catch pour effectuer ce travail : les exceptions qui se produisent en arrière-plan ou au premier plan seront toujours interceptées sans travail supplémentaire à l’aide d’un gestionnaire fourni par le Framework.
7. Modifiez le code pour poursuivre l’implémentation asynchrone en remplaçant les lignes par le nouveau code comme indiqué ci-dessous.

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex04-ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Exécutez l'application. Vous ne remarquerez aucune modification majeure, mais votre code ne bloquera pas un thread du pool de threads, ce qui améliore l’utilisation des ressources du serveur et améliore les performances.

    > [!NOTE]
    > Vous pouvez en savoir plus sur les nouvelles fonctionnalités de programmation asynchrone dans le laboratoire &quot;la **programmation asynchrone dans C# .net 4,5 avec et Visual Basic**&quot; inclus dans le kit de formation Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Tâche 2 : gestion des délais d’attente avec des jetons d’annulation

Les méthodes d’action asynchrones qui retournent des instances de tâche peuvent également prendre en charge les délais d’attente. Dans cette tâche, vous allez mettre à jour le code de la méthode d’index pour gérer un scénario de délai d’attente à l’aide d’un jeton d’annulation.

1. Revenez à Visual Studio et appuyez sur **Maj + F5** pour arrêter le débogage.
2. Ajoutez l’instruction using suivante au fichier **HomeController.cs** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Mettez à jour l’action d’index pour recevoir un argument **CancellationToken** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Mettez à jour l’appel **GetAsync** pour passer le jeton d’annulation.

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex04-SendAsync avec CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Décorez la méthode d' *index* avec un attribut **AsyncTimeout** défini sur 500 millisecondes et un attribut **HandleError** configuré pour gérer **TaskCanceledException** en redirigeant vers une vue **TimedOut** .

    (Extrait de code- *ASP.NET MVC 4 Lab-Ex04-Attributes*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Ouvrez la classe du **photocontrôleur** et mettez à jour la méthode de la **Galerie** pour retarder l’exécution 1000 millisecondes (1 seconde) pour simuler une tâche longue.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Ouvrez le fichier **Web. config** et activez les erreurs personnalisées en ajoutant l’élément suivant.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Créez un nouvel affichage dans **Views\Shared** nommé **TimedOut** et utilisez la disposition par défaut. Dans le Explorateur de solutions, cliquez avec le bouton droit sur le dossier **Views\Shared** et sélectionnez **Ajouter | Affichage**.

    ![Utilisation de différents affichages pour chaque appareil mobile](whats-new-in-aspnet-mvc-4/_static/image36.png "Utilisation de différents affichages pour chaque appareil mobile")

    *Utilisation de différents affichages pour chaque appareil mobile*
9. Mettez à jour le contenu de la vue **TimedOut** comme indiqué ci-dessous.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Exécutez l’application et accédez à l’URL racine. À mesure que vous avez ajouté un **thread. veille** de 1000 millisecondes, vous obtiendrez une erreur de délai d’attente, générée par l’attribut **AsyncTimeout** et interceptée par l’attribut **HandleError** .

    ![Exception de délai d’expiration gérée](whats-new-in-aspnet-mvc-4/_static/image37.png "Exception de délai d’expiration gérée")

    *Exception de délai d’expiration gérée*

> [!NOTE]
> En outre, vous pouvez déployer cette application sur les sites Web Windows Azure à l' [annexe D : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy](#AppendixD).

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Dans ce laboratoire pratique, vous avez observé certaines des nouvelles fonctionnalités qui résident dans ASP.NET MVC 4. Les concepts suivants ont été abordés :

- Tirez parti des améliorations apportées aux modèles de projet ASP.NET MVC, y compris le nouveau modèle de projet d’application mobile
- Utiliser l’attribut de fenêtre d’affichage HTML5 et les requêtes de média CSS pour améliorer l’affichage sur les appareils mobiles
- Utilisez jQuery mobile pour les améliorations progressives et la création d’une interface utilisateur Web optimisée pour la technologie tactile
- Créer des affichages spécifiques aux appareils mobiles
- Utiliser le composant d’affichage-sélecteur pour basculer entre les affichages mobiles et de bureau dans l’application
- Créer des contrôleurs asynchrones à l’aide du support des tâches

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Annexe A : utilisation d’extraits de code

Avec les extraits de code, vous avez tout le code dont vous avez besoin à portée de main. Le document Lab vous indique exactement quand vous pouvez les utiliser, comme illustré dans la figure suivante.

![Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet](whats-new-in-aspnet-mvc-4/_static/image38.png "Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet")

*Utilisation d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aideC# du clavier (uniquement)***

1. Placez le curseur à l’endroit où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ni traits d’Union).
3. Regarder comme IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code approprié (ou continuez à taper jusqu’à ce que le nom de l’extrait entier soit sélectionné).
5. Appuyez deux fois sur la touche Tab pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](whats-new-in-aspnet-mvc-4/_static/image39.png "Commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait en surbrillance](whats-new-in-aspnet-mvc-4/_static/image40.png "Appuyez sur Tab pour sélectionner l’extrait en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait en surbrillance*

![Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé](whats-new-in-aspnet-mvc-4/_static/image41.png "Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé")

*Appuyez de nouveau sur la touche Tab et l’extrait de code sera développé*

***Pour ajouter un extrait de code à l’aideC#de la souris (, Visual Basic et XML)***

1. Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code.
2. Sélectionnez **Insérer un extrait** suivi de **mes extraits de code**.
3. Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.

![Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait](whats-new-in-aspnet-mvc-4/_static/image42.png "Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait")

*Cliquez avec le bouton droit sur l’emplacement où vous souhaitez insérer l’extrait de code, puis sélectionnez Insérer un extrait*

![Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.](whats-new-in-aspnet-mvc-4/_static/image43.png "Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.")

*Sélectionnez l’extrait de code approprié dans la liste en cliquant dessus.*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Annexe B : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour le Web** ou une autre version &quot;Express&quot; à l’aide de la **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Les instructions suivantes vous guident tout au long des étapes requises pour installer *Visual Studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;*Visual Studio Express 2012 pour le Web avec le kit de développement logiciel (SDK) Windows Azure*&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l'installation](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à l’écran d' **Accueil** et commencez à écrire &quot;**vs Express**&quot;, puis cliquez sur la vignette **vs Express pour le Web** .

    ![Vignette VS Express pour le Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Vignette VS Express pour le Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Annexe C : installation de WebMatrix 2 et du simulateur iPhone

Pour exécuter votre site sur un appareil iPhone simulé, vous pouvez utiliser l’extension WebMatrix &quot;le simulateur mobile électrique pour la&quot;iPhone. En outre, vous pouvez configurer la même extension pour exécuter le simulateur à partir de Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Tâche 1 : installation de WebMatrix 2

1. Accédez à [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également l’ouvrir et rechercher le produit &quot;*WebMatrix 2*&quot;.
2. Cliquez sur **Installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous serez redirigé pour le télécharger et l’installer d’abord.
3. Une fois **Web Platform Installer** ouverte, cliquez sur **installer** pour démarrer l’installation.

    ![Installer WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Installer WebMatrix 2")

    *Installer WebMatrix 2*
4. Lisez tous les termes et licences des produits, puis cliquez sur **J’accepte** pour continuer.

    ![Acceptation des termes du contrat de licence](whats-new-in-aspnet-mvc-4/_static/image50.png "Acceptation des termes du contrat de licence")

    *Acceptation des termes du contrat de licence*
5. Attendez que le processus de téléchargement et d’installation se termine.

    ![Progression de l’installation](whats-new-in-aspnet-mvc-4/_static/image51.png "Progression de l'installation")

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation terminée](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation terminée")

    *Installation terminée*
7. Cliquez sur **quitter** pour fermer Web Platform Installer.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Tâche 2 : installation de l’extension du simulateur iPhone

1. Exécutez **WebMatrix** et ouvrez un site Web existant, ou créez-en un nouveau.
2. Cliquez sur le bouton **exécuter** dans le ruban d' **hébergement** et sélectionnez **Ajouter nouveau**.

    ![Ajout d’une nouvelle extension WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "Ajout d’une nouvelle extension WebMatrix")

    *Ajout d’une nouvelle extension WebMatrix*
3. Sélectionnez **iPhone Simulator** , puis cliquez sur **installer**.

    ![Exploration des extensions WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "Exploration des extensions WebMatrix")

    *Exploration des extensions WebMatrix*
4. Dans les détails du package, cliquez sur **installer** pour poursuivre l’installation de l’extension.

    ![extension du simulateur iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "extension du simulateur iPhone")

    *extension du simulateur iPhone*
5. Lisez et acceptez le CLUF de l’extension.

    ![CLUF de l’extension WebMatrix](whats-new-in-aspnet-mvc-4/_static/image56.png "CLUF de l’extension WebMatrix")

    *CLUF de l’extension WebMatrix*
6. Vous pouvez maintenant exécuter votre site Web à partir de WebMatrix à l’aide de l’option simulateur iPhone.

    ![Exécuter à l’aide de iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Exécuter à l’aide de iPhone")

    *Exécuter à l’aide de iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Tâche 3 : configuration de Visual Studio 2012 pour exécuter iPhone Simulator

1. Ouvrez **Visual Studio 2012** et ouvrez un site Web ou créez un nouveau projet.
2. Cliquez sur la flèche vers le bas à partir du bouton Exécuter et sélectionnez **Parcourir avec**.

    ![Naviguer avec](whats-new-in-aspnet-mvc-4/_static/image58.png "Naviguer avec")

    *Naviguer avec*
3. Dans la boîte de dialogue &quot;naviguer avec&quot;, cliquez sur **Ajouter**.
4. Dans la boîte de dialogue &quot;ajouter un programme&quot;, utilisez les valeurs suivantes :

   - **Programme**: C:\Users\*{CurrentUser} * \AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(mettre à jour le chemin en conséquence)*
   - **Arguments**: &quot;1&quot;
   - **Nom convivial**: simulateur iPhone

     ![Ajouter un programme](whats-new-in-aspnet-mvc-4/_static/image59.png "Ajouter un programme")

     *Ajouter un programme à explorer*
5. Cliquez sur **OK** et fermez les boîtes de dialogue.
6. Vous pouvez maintenant exécuter vos applications Web dans le simulateur iPhone à partir de Visual Studio 2012.

    ![Naviguer avec iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Naviguer avec iPhone Simulator")

    *Naviguer avec iPhone Simulator*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe D : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

Cette annexe vous indique comment créer un nouveau site Web à partir de Windows Azure Portail de gestion et publier l’application que vous avez obtenue en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tâche 1 : création d’un nouveau site Web à partir du portail Windows Azure

1. Accédez au [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous à l’aide des informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Windows Azure, vous pouvez héberger gratuitement 10 sites Web ASP.NET, puis mettre à l’échelle à mesure que votre trafic augmente. Vous pouvez vous inscrire [ici](https://aka.ms/aspnet-hol-azure).

    ![Ouvrir une session sur Windows Portail Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Ouvrir une session sur Windows Portail Azure")

    *Connectez-vous à Windows Azure Portail de gestion*
2. Cliquez sur **nouveau** dans la barre de commandes.

    ![Création d’un nouveau site Web](whats-new-in-aspnet-mvc-4/_static/image62.png "Création d’un nouveau site Web")

    *Création d’un nouveau site Web*
3. Cliquez sur **compute** | **site Web**. Sélectionnez ensuite l’option **création rapide** . Fournissez une URL disponible pour le nouveau site Web, puis cliquez sur **créer un site Web**.

    > [!NOTE]
    > Un site Web Windows Azure est l’hôte d’une application Web s’exécutant dans le Cloud que vous pouvez contrôler et gérer. L’option création rapide vous permet de déployer une application Web terminée sur le site Web Windows Azure depuis l’extérieur du portail. Elle n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un nouveau site Web à l’aide de la création rapide](whats-new-in-aspnet-mvc-4/_static/image63.png "Création d’un nouveau site Web à l’aide de la création rapide")

    *Création d’un nouveau site Web à l’aide de la création rapide*
4. Patientez jusqu’à la création du nouveau **site Web** .
5. Une fois le site Web créé, cliquez sur le lien sous la colonne **URL** . Vérifiez que le nouveau site Web fonctionne.

    ![Navigation vers le nouveau site Web](whats-new-in-aspnet-mvc-4/_static/image64.png "Navigation vers le nouveau site Web")

    *Navigation vers le nouveau site Web*

    ![Site Web en cours d’exécution](whats-new-in-aspnet-mvc-4/_static/image65.png "Site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site Web dans la colonne **nom** pour afficher les pages de gestion.

    ![Ouverture des pages de gestion de site Web](whats-new-in-aspnet-mvc-4/_static/image66.png "Ouverture des pages de gestion de site Web")

    *Ouverture des pages de gestion de site Web*
7. Dans la page **tableau de bord** , sous la section **Aperçu rapide** , cliquez sur le lien **Télécharger le profil de publication** .

    > [!NOTE]
    > Le *profil de publication* contient toutes les informations requises pour publier une application Web sur un site Web Windows Azure pour chaque méthode de publication activée. Le profil de publication contient les URL, les informations d'identification de l'utilisateur et les chaînes de base de données nécessaires pour la connexion et l'authentification auprès de chacun des points de terminaison pour lesquels une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour le web** et **Microsoft Visual Studio 2012** prennent en charge la lecture des profils de publication pour automatiser la configuration de ces programmes pour la publication d’applications Web sur les sites Web Windows Azure.

    ![Téléchargement du profil de publication du site Web](whats-new-in-aspnet-mvc-4/_static/image67.png "Téléchargement du profil de publication du site Web")

    *Téléchargement du profil de publication du site Web*
8. Téléchargez le fichier de profil de publication dans un emplacement connu. Dans cet exercice, vous allez apprendre à utiliser ce fichier pour publier une application Web sur un site Web Windows Azure à partir de Visual Studio.

    ![Enregistrement du fichier de profil de publication](whats-new-in-aspnet-mvc-4/_static/image68.png "Enregistrement du profil de publication")

    *Enregistrement du fichier de profil de publication*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application utilise des bases de données SQL Server, vous devez créer un serveur SQL Database. Si vous souhaitez déployer une application simple qui n’utilise pas SQL Server vous pouvez ignorer cette tâche.

1. Vous aurez besoin d’un serveur de SQL Database pour stocker la base de données d’application. Vous pouvez afficher les serveurs SQL Database à partir de votre abonnement dans le portail de gestion Windows Azure, à partir des **bases de données SQL** | **serveurs** | **tableau de bord du serveur**. Si vous n’avez pas de serveur créé, vous pouvez en créer un à l’aide du bouton **Ajouter** de la barre de commandes. Notez le **nom et l’URL du serveur,** ainsi que le nom et le mot de passe de connexion de l’administrateur, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore la base de données, car elle sera créée dans une étape ultérieure.

    ![Tableau de bord du serveur SQL Database](whats-new-in-aspnet-mvc-4/_static/image69.png "Tableau de bord du serveur SQL Database")

    *Tableau de bord du serveur SQL Database*
2. Dans la tâche suivante, vous allez tester la connexion à la base de données à partir de Visual Studio. pour cette raison, vous devez inclure votre adresse IP locale dans la liste des **adresses IP autorisées**du serveur. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de l' **adresse IP actuelle du client** et collez-la dans les zones de texte **adresse IP de début** et **adresse IP de fin** , puis cliquez sur le bouton ![ajouter-client-IP-adresse-OK-button](whats-new-in-aspnet-mvc-4/_static/image70.png).

    ![Ajout de l’adresse IP du client](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Ajout de l’adresse IP du client*
3. Une fois l' **adresse IP du client** ajoutée à la liste des adresses IP autorisées, cliquez sur **Enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Confirmer les modifications*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une application ASP.NET MVC 4 à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet de site Web et sélectionnez **publier**.

    ![Publication de l’application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publication de l'application")

    *Publication du site Web*
2. Importez le profil de publication que vous avez enregistré dans la première tâche.

    ![Importation du profil de publication](whats-new-in-aspnet-mvc-4/_static/image74.png "Importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la validation terminée, cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte s’affiche en regard du bouton valider la connexion.

    ![Validation de la connexion](whats-new-in-aspnet-mvc-4/_static/image75.png "Validation de la connexion")

    *Validation de la connexion*
4. Dans la page **paramètres** , sous la section **bases de données** , cliquez sur le bouton en regard de la zone de texte de votre connexion à la base de données (par exemple, **DefaultConnection**).

    ![Configuration de Web Deploy](whats-new-in-aspnet-mvc-4/_static/image76.png "Configuration de Web Deploy")

    *Configuration de Web Deploy*
5. Configurez la connexion de base de données comme suit :

   - Dans **nom du serveur** , tapez l’URL de votre serveur SQL Database à l’aide du préfixe *TCP :* .
   - Dans **nom d’utilisateur** , tapez le nom de connexion de l’administrateur du serveur.
   - Dans **mot de passe** , tapez le mot de passe de connexion de l’administrateur du serveur.
   - Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.

     ![Configuration de la chaîne de connexion de destination](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuration de la chaîne de connexion de destination")

     *Configuration de la chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](whats-new-in-aspnet-mvc-4/_static/image78.png "Création de la chaîne de base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous allez utiliser pour vous connecter à SQL Database dans Windows Azure est affichée dans la zone de texte connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Chaîne de connexion pointant vers SQL Database")

    *Chaîne de connexion pointant vers SQL Database*
8. Dans la page **Aperçu** , cliquez sur **publier**.

    ![Publication de l’application Web](whats-new-in-aspnet-mvc-4/_static/image80.png "Publication de l’application Web")

    *Publication de l’application Web*
9. Une fois le processus de publication terminé, votre navigateur par défaut ouvre le site Web publié.

    ![Application publiée sur Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application publiée sur Windows Azure")

    *Application publiée sur Windows Azure*
