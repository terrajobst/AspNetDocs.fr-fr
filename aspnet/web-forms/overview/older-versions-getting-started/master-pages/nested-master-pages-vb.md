---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Pages maîtres imbriquées (VB) | Microsoft Docs
author: rick-anderson
description: Montre comment imbriquer une page maître dans une autre.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9bb39712855c37f5cbcbb447f7691e9451b8dc92
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642317"
---
# <a name="nested-master-pages-vb"></a>Pages maîtres imbriquées (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Montre comment imbriquer une page maître dans une autre.

## <a name="introduction"></a>Introduction

Au cours des neuf derniers didacticiels, nous avons vu comment implémenter une disposition à l’ensemble du site avec des pages maîtres. En résumé, les pages maîtres nous permettent, le développeur de pages, de définir le balisage courant dans la page maître, ainsi que des régions spécifiques qui peuvent être personnalisées sur une base de page de contenu. Les contrôles ContentPlaceHolder dans une page maître indiquent les régions personnalisables ; le balisage personnalisé pour les contrôles ContentPlaceHolder est défini dans la page de contenu à l’aide de contrôles de contenu.

Les techniques de la page maître que nous avons explorées jusqu’à présent sont idéales si vous avez une seule disposition utilisée sur l’ensemble du site. Toutefois, de nombreux sites Web de grande taille disposent d’une disposition de site personnalisée dans différentes sections. Par exemple, considérez une application de soins de santé utilisée par le personnel hospitalier pour gérer les informations, les activités et la facturation des patients. Cette application peut comporter trois types de pages Web :

- Pages spécifiques aux membres du personnel dans lesquelles les membres du personnel peuvent mettre à jour la disponibilité, afficher les planifications ou demander des congés.
- Pages spécifiques aux patients, où les membres du personnel affichent ou modifient les informations relatives à un patient spécifique.
- Pages spécifiques à la facturation dans lesquelles les comptables examinent les États de la revendication actuelle et les rapports financiers.

Chaque page peut partager une disposition commune, telle qu’un menu en haut et une série de liens fréquemment utilisés en bas. Mais les pages du personnel, des patients et de la facturation peuvent avoir besoin de personnaliser cette disposition générique. Par exemple, toutes les pages spécifiques au personnel doivent peut-être inclure un calendrier et une liste de tâches présentant la disponibilité et la planification quotidienne de l’utilisateur actuellement connecté. Peut-être que toutes les pages spécifiques aux patients doivent afficher le nom, l’adresse et les informations d’assurance du patient dont les informations sont modifiées.

Il est possible de créer des dispositions personnalisées à l’aide de *pages maîtres imbriquées*. Pour implémenter le scénario ci-dessus, nous commençons par créer une page maître qui définissait la disposition à l’ensemble du site, le contenu du menu et du pied de page, avec des ContentPlaceHolders définissant les régions personnalisables. Nous créons ensuite trois pages maîtres imbriquées, une pour chaque type de page Web. Chaque page maître imbriquée définit le contenu parmi le type de pages de contenu qui utilisent la page maître. En d’autres termes, la page maître imbriquée pour les pages de contenu spécifiques aux patients inclut le balisage et la logique de programmation permettant d’afficher des informations sur le patient en cours de modification. Lors de la création d’une page spécifique au patient, nous allons la lier à cette page maître imbriquée.

Ce didacticiel commence par mettre en évidence les avantages des pages maîtres imbriquées. Il montre ensuite comment créer et utiliser des pages maîtres imbriquées.

> [!NOTE]
> Les pages maîtres imbriquées sont possibles depuis la version 2,0 du .NET Framework. Toutefois, Visual Studio 2005 n’incluait pas la prise en charge au moment du design des pages maîtres imbriquées. La bonne nouvelle, c’est que Visual Studio 2008 offre une expérience de conception riche pour les pages maîtres imbriquées. Si vous êtes intéressé par l’utilisation de pages maîtres imbriquées, mais que vous utilisez toujours Visual Studio 2005, consultez l’entrée de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [conseils pour les pages maîtres imbriquées dans le temps de conception de vs 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).

## <a name="the-benefits-of-nested-master-pages"></a>Avantages des pages maîtres imbriquées

De nombreux sites Web ont une conception de site qui se déplace, ainsi que des conceptions plus personnalisées spécifiques à certains types de pages. Par exemple, dans notre application Web de démonstration, nous avons créé une section d’administration rudimentaire (les pages du dossier `~/Admin`). Actuellement, les pages Web du dossier `~/Admin` utilisent la même page maître que les pages qui ne se trouvent pas dans la section Administration (à savoir, `Site.master` ou `Alternate.master`, selon la sélection de l’utilisateur).

> [!NOTE]
> Pour le moment, supposons que notre site ne possède qu’une seule page maître, `Site.master`. Nous traiterons de l’utilisation de pages maîtres imbriquées avec deux pages maîtres (ou plus) à partir de « utilisation d’une page maître imbriquée pour la section administration » plus loin dans ce didacticiel.

Imaginez que nous avons été invités à personnaliser la disposition des pages d’administration pour inclure des informations supplémentaires ou des liens qui, autrement, ne seraient pas présents dans d’autres pages du site. Il existe quatre techniques pour implémenter cette exigence :

1. Ajoutez manuellement les informations spécifiques à l’administration et les liens vers chaque page de contenu dans le dossier `~/Admin`.
2. Mettez à jour la page maître `Site.master` pour inclure les informations et les liens spécifiques à la section Administration, puis ajoutez du code à la page maître pour afficher ou masquer ces sections selon que l’une des pages d’administration est visitée ou non.
3. Créez une nouvelle page maître spécifiquement pour la section Administration, copiez la balise à partir de `Site.master`, ajoutez les informations et les liens spécifiques à la section Administration, puis mettez à jour les pages de contenu dans le dossier `~/Admin` pour utiliser cette nouvelle page maître.
4. Créez une page maître imbriquée qui crée une liaison avec `Site.master` et les pages de contenu dans le dossier `~/Admin` utilisent cette nouvelle page maître imbriquée. Cette page maître imbriquée inclut uniquement les informations supplémentaires et les liens spécifiques aux pages d’administration, et n’a pas besoin de répéter le balisage déjà défini dans `Site.master`.

La première option est la moins agréable. L’objectif principal de l’utilisation de pages maîtres est de ne pas avoir à copier et coller manuellement le balisage commun dans les nouvelles pages ASP.NET. La deuxième option est acceptable, mais rend l’application plus facile à gérer, car elle fait en sorte que les pages maîtres contiennent des balises qui ne s’affichent qu’occasionnellement et qui oblige les développeurs à modifier la page maître pour contourner ce balisage et à se souvenir quand, exactement, certaines balises sont affichées par rapport à quand elles sont masquées. Cette approche est moins tenable que les personnalisations de plusieurs types de pages Web qui devaient être prises en charge par cette page maître unique.

La troisième option élimine les problèmes d’encombrement et de complexité rencontrés avec la deuxième option. Toutefois, l’inconvénient principal de cette option est qu’elle nécessite de copier et de coller la disposition commune de `Site.master` vers la nouvelle page maître propre à la section Administration. Si, par la suite, nous décidons de modifier la disposition à l’ensemble du site, nous devons vous souvenir de la modifier à deux emplacements.

La quatrième option, les pages maîtres imbriquées, nous donnent le meilleur des deuxième et troisième options. Les informations de disposition à l’échelle du site sont conservées dans un fichier : la page maître de niveau supérieur, tandis que le contenu spécifique à des régions particulières est séparé dans différents fichiers.

Ce didacticiel commence par une présentation de la création et de l’utilisation d’une page maître imbriquée simple. Nous créons une nouvelle page maître de niveau supérieur, deux pages maîtres imbriquées et deux pages de contenu. À compter de « utilisation d’une page maître imbriquée pour la section Administration », nous examinons la mise à jour de notre architecture de page maître existante pour inclure l’utilisation de pages maîtres imbriquées. Plus précisément, nous créons une page maître imbriquée et l’utilisons pour inclure du contenu personnalisé supplémentaire pour les pages de contenu dans le dossier `~/Admin`.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Étape 1 : création d’une page maître simple de niveau supérieur

La création d’une base de données Master imbriquée basée sur l’une des pages maîtres existantes, puis la mise à jour d’une page de contenu existante pour utiliser cette nouvelle page maître imbriquée au lieu de la page maître de niveau supérieur implique une certaine complexité, car les pages de contenu existantes attendent déjà certains Contrôles ContentPlaceHolder définis dans la page maître de niveau supérieur. Par conséquent, la page maître imbriquée doit également inclure les mêmes contrôles ContentPlaceHolder portant le même nom. En outre, notre application de démonstration particulière comporte deux pages maîtres (`Site.master` et `Alternate.master`) qui sont affectées dynamiquement à une page de contenu en fonction des préférences de l’utilisateur, ce qui accroît encore cette complexité. Nous allons examiner la mise à jour de l’application existante pour utiliser des pages maîtres imbriquées plus loin dans ce didacticiel, mais nous allons tout d’abord nous concentrer sur un exemple simple de pages maîtres imbriquées.

Créez un nouveau dossier nommé `NestedMasterPages` puis ajoutez un nouveau fichier de page maître à ce dossier nommé `Simple.master`. (Voir la figure 1 pour obtenir une capture d’écran du Explorateur de solutions après l’ajout de ce dossier et de ce fichier.) Faites glisser le fichier de feuille de style `AlternateStyles.css` du Explorateur de solutions sur le concepteur. Cela ajoute un élément `<link>` au fichier de feuille de style dans l’élément `<head>`, après quoi le balisage de l’élément de `<head>` de la page maître doit ressembler à ceci :

[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Ensuite, ajoutez le balisage suivant dans le formulaire Web de `Simple.master`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Ce balisage affiche un lien intitulé « pages maîtres imbriquées (simples) » en haut de la page, dans une grande police blanche sur un arrière-plan bleu. En dessous, il s’agit de l' `MainContent` ContentPlaceHolder. La figure 1 montre la page maître de `Simple.master` lors du chargement dans le concepteur Visual Studio.

[![la page maître imbriquée définit le contenu spécifique aux pages de la section Administration](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Figure 01**: la page maître imbriquée définit le contenu spécifique aux pages de la section Administration ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image3.png))

## <a name="step-2-creating-a-simple-nested-master-page"></a>Étape 2 : création d’une page maître imbriquée simple

`Simple.master` contient deux contrôles ContentPlaceHolder : l' `MainContent` ContentPlaceHolder que nous avons ajouté dans le formulaire Web avec le `head` ContentPlaceHolder dans l’élément `<head>`. Si nous devions créer une page de contenu et la lier à `Simple.master` la page de contenu aurait deux contrôles de contenu référençant les deux ContentPlaceHolders. De même, si nous créons une page maître imbriquée et que vous la liez à `Simple.master`, la page maître imbriquée aura deux contrôles de contenu.

Nous allons ajouter une nouvelle page maître imbriquée au dossier `NestedMasterPages` nommé `SimpleNested.master`. Cliquez avec le bouton droit sur le dossier `NestedMasterPages` et choisissez Ajouter un nouvel élément. La boîte de dialogue Ajouter un nouvel élément s’affiche dans la figure 2. Sélectionnez le type de modèle page maître et tapez le nom de la nouvelle page maître. Pour indiquer que la nouvelle page maître doit être une page maître imbriquée, cochez la case « sélectionner une page maître ».

Ensuite, cliquez sur le bouton Ajouter. La boîte de dialogue Sélectionner une page maître s’affiche lorsque vous liez une page de contenu à une page maître (voir figure 3). Choisissez la page maître `Simple.master` dans le dossier `NestedMasterPages`, puis cliquez sur OK.

> [!NOTE]
> Si vous avez créé votre site Web ASP.NET à l’aide du modèle de projet d’application Web au lieu du modèle de projet de site Web, vous ne verrez pas la case à cocher « sélectionner une page maître » dans la boîte de dialogue Ajouter un nouvel élément, comme illustré à la figure 2. Pour créer une page maître imbriquée lors de l’utilisation du modèle de projet d’application Web, vous devez choisir le modèle de page maître imbriqué (au lieu du modèle de page maître). Après avoir sélectionné le modèle de page maître imbriquée et cliqué sur Ajouter, la boîte de dialogue Sélectionner une page maître comme illustré à la figure 3 s’affiche.

[![activez la case à cocher &quot;sélectionner la page maître&quot; pour ajouter une page maître imbriquée](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Figure 02**: cochez la case « sélectionner la page maître » pour ajouter une page maître imbriquée ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image6.png))

[![lier la page maître imbriquée à la page maître. Master simple](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Figure 03**: lier la page maître imbriquée à la page maître `Simple.master` ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image9.png))

Le balisage déclaratif de la page maître imbriquée, comme indiqué ci-dessous, contient deux contrôles de contenu qui référencent les deux contrôles ContentPlaceHolder de la page maître de niveau supérieur.

[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

À l’exception de la directive `<%@ Master %>`, le balisage déclaratif initial de la page maître imbriquée est identique au balisage initialement généré lors de la liaison d’une page de contenu à la même page maître de niveau supérieur. À l’instar de la directive `<%@ Page %>` d’une page de contenu, la directive `<%@ Master %>` contient ici un attribut `MasterPageFile` qui spécifie la page maître parente de la page maître imbriquée. La principale différence entre la page maître imbriquée et une page de contenu liée à la même page maître de niveau supérieur est que la page maître imbriquée peut inclure des contrôles ContentPlaceHolder. Les contrôles ContentPlaceHolder de la page maître imbriquée définissent les régions dans lesquelles les pages de contenu peuvent personnaliser le balisage.

Mettez à jour cette page maître imbriquée afin qu’elle affiche le texte « Hello, from SimpleNested ! » dans le contrôle de contenu qui correspond au contrôle `MainContent` ContentPlaceHolder.

[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Après avoir apporté cet ajout, enregistrez la page maître imbriquée, puis ajoutez une nouvelle page de contenu au dossier `NestedMasterPages` nommé `Default.aspx`et liez-la à la page maître `SimpleNested.master`. Lors de l’ajout de cette page, vous pouvez être surpris de voir qu’il ne contient pas de contrôles de contenu (voir la figure 4) ! Une page de contenu ne peut accéder qu’à l’ContentPlaceHolders de son page maître *parente* . `SimpleNested.master` ne contient pas de contrôles ContentPlaceHolder ; par conséquent, toute page de contenu liée à cette page maître ne peut pas contenir de contrôles de contenu.

[![la page nouveau contenu ne contient aucun contrôle de contenu](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Figure 04**: la page nouveau contenu ne contient aucun contrôle de contenu ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image12.png))

Nous devons mettre à jour la page maître imbriquée (`SimpleNested.master`) pour inclure les contrôles ContentPlaceHolder. En général, vous souhaiterez que vos pages maîtres imbriquées incluent un ContentPlaceHolder pour chaque ContentPlaceHolder défini par sa page maître parente, ce qui permet à sa page maître enfant ou page de contenu de fonctionner avec l’un des ContentPlaceHolder de la page maître de niveau supérieur. commandes.

Mettez à jour la page maître `SimpleNested.master` pour inclure un ContentPlaceHolder dans ses deux contrôles de contenu. Donnez aux contrôles ContentPlaceHolder le même nom que le contrôle ContentPlaceHolder auquel leur contrôle de contenu fait référence. Autrement dit, ajoutez un contrôle ContentPlaceHolder nommé `MainContent` au contrôle de contenu dans `SimpleNested.master` qui référence le `MainContent` ContentPlaceHolder dans `Simple.master`. Faites la même chose dans le contrôle de contenu qui référence le `head` ContentPlaceHolder.

> [!NOTE]
> Je vous recommande de nommer les contrôles ContentPlaceHolder dans la page maître imbriquée de la même façon que le ContentPlaceHolders dans la page maître de niveau supérieur, cette symétrie de nommage n’est pas nécessaire. Vous pouvez attribuer à tous les contrôles ContentPlaceHolder de votre page maître imbriquée le nom de votre choix. Toutefois, je trouve qu’il est plus facile de se souvenir des ContentPlaceHolders qui correspondent aux régions de la page si ma page maître de niveau supérieur et les pages maîtres imbriquées utilisent le même nom.

Après avoir apporté ces ajouts, le balisage déclaratif de votre page maître `SimpleNested.master` doit ressembler à ce qui suit :

[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Supprimer la page de contenu `Default.aspx` que nous venons de créer, puis l’ajouter à nouveau, en la liant à la page maître `SimpleNested.master`. Cette fois, Visual Studio ajoute deux contrôles de contenu au `Default.aspx`, en référençant le ContentPlaceHolders maintenant défini dans `SimpleNested.master` (voir figure 6). Ajoutez le texte « Hello, from default. aspx ! » dans le contrôle de contenu qui a référencé `MainContent`.

La figure 5 illustre les trois entités impliquées ici : `Simple.master`, `SimpleNested.master`et `Default.aspx`-et comment elles sont liées les unes aux autres. Comme le montre le diagramme, la page maître imbriquée implémente des contrôles de contenu pour le ContentPlaceHolder de son parent. Si ces régions doivent être accessibles à la page de contenu, la page maître imbriquée doit ajouter son propre ContentPlaceHolders aux contrôles de contenu.

[![les pages maîtres imbriquées et de niveau supérieur déterminent la disposition de la page de contenu](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Figure 05**: les pages maîtres imbriquées et de niveau supérieur dictent la disposition de la page de contenu ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image15.png))

Ce comportement illustre comment une page de contenu ou une page maître est uniquement Cognizant de sa page maître parente. Ce comportement est également indiqué par le concepteur Visual Studio. La figure 6 illustre le concepteur de `Default.aspx`. Tandis que le concepteur montre clairement quelles sont les régions modifiables à partir de la page de contenu et quelles parties ne le sont pas, elle ne lève pas d’ambiguïté sur les régions non modifiables de la page maître imbriquée et sur les régions de la page maître de niveau supérieur.

[![la page de contenu comprend désormais des contrôles de contenu pour le ContentPlaceHolders de la page maître imbriquée](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Figure 06**: la page de contenu contient maintenant des contrôles de contenu pour le ContentPlaceHolders de la page maître imbriquée ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image18.png))

## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Étape 3 : ajout d’une deuxième page maître imbriquée simple

L’avantage des pages maîtres imbriquées est plus évident lorsqu’il y a plusieurs pages maîtres imbriquées. Pour illustrer cet avantage, créez une autre page maître imbriquée dans le dossier `NestedMasterPages` ; Nommez cette nouvelle page maître imbriquée `SimpleNestedAlternate.master` et liez-la à la page maître `Simple.master`. Ajoutez des contrôles ContentPlaceHolder dans les deux contrôles de contenu de la page maître imbriquée, comme nous l’avons fait à l’étape 2. Ajoutez également le texte « Hello, from SimpleNestedAlternate ! » dans le contrôle de contenu qui correspond à la `MainContent` ContentPlaceHolder de la page maître de niveau supérieur. Après avoir apporté ces modifications, le balisage déclaratif de votre nouvelle page maître imbriquée doit ressembler à ce qui suit :

[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Créez une page de contenu nommée `Alternate.aspx` dans le dossier `NestedMasterPages` et liez-la à la page maître imbriquée `SimpleNestedAlternate.master`. Ajoutez le texte « Hello, from alternatif ! » dans le contrôle de contenu qui correspond à `MainContent`. La figure 7 illustre `Alternate.aspx` affichées via le concepteur Visual Studio.

[![autre. aspx est lié à la page maître SimpleNestedAlternate. Master](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Figure 07**: `Alternate.aspx` est lié à la Page maître `SimpleNestedAlternate.master` ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image21.png))

Comparez le concepteur de la figure 7 au concepteur de la figure 6. Les deux pages de contenu partagent la même disposition définie dans la page maître de niveau supérieur (`Simple.master`), à savoir le titre « didacticiel sur les pages maîtres imbriquées (simple) ». Toutefois, le contenu distinct est encore défini dans les pages maîtres parentes : le texte « Hello, from SimpleNested ! » à la figure 6 et « Bonjour, de SimpleNestedAlternate ! » à la figure 7. Ces différences sont très simples, mais vous pouvez étendre cet exemple pour inclure des différences plus significatives. Par exemple, la page `SimpleNested.master` peut inclure un menu avec des options spécifiques à ses pages de contenu, alors que `SimpleNestedAlternate.master` peut avoir des informations pertinentes pour les pages de contenu qui y sont liées.

À présent, imaginez que nous avions besoin d’apporter une modification à la disposition du site en trop. Par exemple, imaginons que nous voulions ajouter une liste de liens communs à toutes les pages de contenu. Pour ce faire, nous mettons à jour la page maître de niveau supérieur, `Simple.master`. Toutes les modifications apportées sont immédiatement reflétées dans les pages maîtres imbriquées et, par extension, leurs pages de contenu.

Pour illustrer la facilité avec laquelle nous pouvons modifier la disposition du site principal, ouvrez la page maître `Simple.master` et ajoutez le balisage suivant entre les éléments `topContent` et `mainContent` `<div>` :

[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Cela ajoute deux liens en haut de chaque page qui sont liés à `Simple.master`, `SimpleNested.master`ou `SimpleNestedAlternate.master`; Ces modifications s’appliquent immédiatement à toutes les pages maîtres imbriquées et à leurs pages de contenu. La figure 8 illustre `Alternate.aspx` affichées dans un navigateur. Notez l’ajout des liens en haut de la page (par rapport à la figure 7).

[![modifié en page maître de niveau supérieur sont immédiatement reflétés dans ses pages maîtres imbriquées et leurs pages de contenu](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Figure 08**: modification apportée à la page maître de niveau supérieur est immédiatement reflétée dans ses pages maîtres imbriquées et leurs pages de contenu ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image24.png))

## <a name="using-a-nested-master-page-for-the-administration-section"></a>Utilisation d’une page maître imbriquée pour la section Administration

À ce stade, nous avons examiné les avantages des pages maîtres imbriquées et vu comment les créer et les utiliser dans une application ASP.NET. Toutefois, les exemples des étapes 1, 2 et 3 impliquaient la création d’une nouvelle page maître de niveau supérieur, de nouvelles pages maîtres imbriquées et de nouvelles pages de contenu. Qu’en est-il de l’ajout d’une nouvelle page maître imbriquée à un site Web avec une page maître et des pages de contenu de niveau supérieur existantes ?

L’intégration d’une page maître imbriquée dans un site Web existant et son association à des pages de contenu existantes nécessite un peu plus de travail que le démarrage à partir de zéro. Les étapes 4, 5, 6 et 7 explorent ces défis au fur et à mesure que nous enrichissons notre application de démonstration pour inclure une nouvelle page maître imbriquée nommée `AdminNested.master` qui contient des instructions pour l’administrateur et qui est utilisée par les pages ASP.NET dans le dossier `~/Admin`.

L’intégration d’une page maître imbriquée dans notre application de démonstration présente les obstacles suivants :

- Les pages de contenu existantes dans le dossier `~/Admin` ont certaines attentes de leur page maître. Pour commencer, ils s’attendent à ce que certains contrôles ContentPlaceHolder soient présents. En outre, les pages `~/Admin/AddProduct.aspx` et `~/Admin/Products.aspx` appellent la méthode `RefreshRecentProductsGrid` publique de la page maître, définissent sa propriété `GridMessageText` ou ont un gestionnaire d’événements pour son événement `PricesDoubled`. Par conséquent, notre page maître imbriquée doit fournir les mêmes membres ContentPlaceHolders et public.
- Dans le didacticiel précédent, nous avons amélioré la classe `BasePage` pour définir dynamiquement la propriété `MasterPageFile` de l’objet `Page` en fonction d’une variable de session. Comment prendre en charge les pages maîtres dynamiques lors de l’utilisation de pages maîtres imbriquées ?

Ces deux défis sont exposés lorsque nous créons la page maître imbriquée et l’utilisons à partir de nos pages de contenu existantes. Nous allons examiner et surmonter ces problèmes au fur et à mesure de leur apparition.

## <a name="step-4-creating-the-nested-master-page"></a>Étape 4 : création de la page maître imbriquée

Notre première tâche consiste à créer la page maître imbriquée qui sera utilisée par les pages de la section Administration. Comme nous l’avons vu à l’étape 2, lors de l’ajout d’une nouvelle page maître imbriquée, nous devons spécifier la page maître parente de la page maître imbriquée. Toutefois, nous avons deux pages maîtres de niveau supérieur : `Site.master` et `Alternate.master`. Rappelez-vous que nous avons créé `Alternate.master` dans le didacticiel précédent et écrit du code dans la classe `BasePage` qui définissait la propriété `MasterPageFile` de l’objet `Page` au moment de l’exécution sur `Site.master` ou `Alternate.master` selon la valeur de la variable de session `MyMasterPage`.

Comment configurer notre page maître imbriquée afin qu’elle utilise la page maître de niveau supérieur appropriée ? Deux options s’offrent à vous :

- Créez deux pages maîtres imbriquées, `AdminNestedSite.master` et `AdminNestedAlternate.master`, et liez-les aux pages maîtres de niveau supérieur `Site.master` et `Alternate.master`, respectivement. Dans `BasePage`, nous définissons le `MasterPageFile` de l’objet `Page` sur la page maître imbriquée appropriée.
- Créer une page maître imbriquée unique et faire en sorte que les pages de contenu utilisent cette page maître particulière. Ensuite, lors de l’exécution, nous aurions besoin de définir la propriété `MasterPageFile` de la page maître imbriquée sur la page maître de niveau supérieur appropriée au moment de l’exécution. (Comme vous l’avez peut-être déjà dû, les pages maîtres ont également une propriété `MasterPageFile`.)

Utilisons la deuxième option. Créez un fichier de page maître imbriqué unique dans le dossier `~/Admin` nommé `AdminNested.master`. Étant donné que les `Site.master` et `Alternate.master` ont le même ensemble de contrôles ContentPlaceHolder, peu importe la page maître à laquelle vous liez, bien que je vous encourage à les lier à `Site.master` pour des raisons de cohérence.

[![ajouter une page maître imbriquée au dossier ~/Admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Figure 09**: ajouter une page maître imbriquée au dossier `~/Admin`. ([Cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image27.png))

Étant donné que la page maître imbriquée est liée à une page maître avec quatre contrôles ContentPlaceHolder, Visual Studio ajoute quatre contrôles de contenu au balisage initial du nouveau fichier de page maître imbriqué. Comme nous l’avons fait aux étapes 2 et 3, ajoutez un contrôle ContentPlaceHolder dans chaque contrôle de contenu, en lui donnant le même nom que le contrôle ContentPlaceHolder de la page maître de niveau supérieur. Ajoutez également le balisage suivant au contrôle de contenu qui correspond au `MainContent` ContentPlaceHolder :

[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Ensuite, définissez la classe CSS `instructions` dans les fichiers CSS `Styles.css` et `AlternateStyles.css`. Les règles CSS suivantes provoquent l’affichage des éléments HTML dont le style est avec la classe `instructions` avec une couleur d’arrière-plan jaune clair et une bordure noire unie :

[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Étant donné que ce balisage a été ajouté à la page maître imbriquée, il n’apparaît que dans les pages qui utilisent cette page maître imbriquée (à savoir, les pages de la section Administration).

Après avoir apporté ces ajouts à votre page maître imbriquée, son balisage déclaratif doit ressembler à ce qui suit :

[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Notez que chaque contrôle de contenu dispose d’un contrôle ContentPlaceHolder et que les mêmes valeurs sont assignées aux propriétés des contrôles ContentPlaceHolder' `ID` les mêmes valeurs que les contrôles ContentPlaceHolder correspondants dans la page maître de niveau supérieur. En outre, le balisage spécifique à la section d’administration apparaît dans le `MainContent` ContentPlaceHolder.

La figure 10 montre l' `AdminNested.master` page maître imbriquée quand vous l’affichez dans le concepteur de Visual Studio. Vous pouvez voir les instructions dans la zone jaune en haut du contrôle de contenu `MainContent`.

[![la page maître imbriquée étend la page maître de niveau supérieur pour inclure des instructions pour l’administrateur.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Figure 10**: la page maître imbriquée étend la page maître de niveau supérieur pour inclure des instructions pour l’administrateur. ([Cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image30.png))

## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Étape 5 : mise à jour des pages de contenu existantes pour utiliser la nouvelle page maître imbriquée

Chaque fois que nous ajoutons une nouvelle page de contenu à la section Administration, nous devons la lier à la page maître de `AdminNested.master` que nous venons de créer. Mais qu’en est-il des pages de contenu existantes ? Actuellement, toutes les pages de contenu du site dérivent de la classe `BasePage`, qui définit par programmation la page maître de la page de contenu au moment de l’exécution. Il ne s’agit pas du comportement que nous souhaitons pour les pages de contenu de la section Administration. Au lieu de cela, nous voulons que ces pages de contenu utilisent toujours la page `AdminNested.master`. La page maître imbriquée est chargée de choisir la page de contenu de niveau supérieur appropriée au moment de l’exécution.

La meilleure façon d’atteindre ce comportement souhaité consiste à créer une classe de page de base personnalisée nommée `AdminBasePage` qui étend la classe `BasePage`. `AdminBasePage` pouvez alors remplacer le `SetMasterPageFile` et définir le `MasterPageFile` de l’objet `Page` sur la valeur codée en dur « ~/Admin/AdminNested.master ». De cette façon, toute page qui dérive de `AdminBasePage` utilise `AdminNested.master`, tandis que toute page qui dérive de `BasePage` aura sa propriété `MasterPageFile` définie de manière dynamique sur « ~/site.Master » ou « ~/Alternate.Master » en fonction de la valeur de la variable de session `MyMasterPage`.

Commencez par ajouter un nouveau fichier de classe au dossier `App_Code` nommé `AdminBasePage.vb`. N’avez `AdminBasePage` étendre `BasePage` puis substituez la méthode `SetMasterPageFile`. Dans cette méthode, assignez la `MasterPageFile` la valeur « ~/Admin/AdminNested.master ». Une fois ces modifications apportées, votre fichier de classe doit ressembler à ce qui suit :

[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Nous devons maintenant faire en sorte que les pages de contenu existantes de la section Administration dérivent de `AdminBasePage` au lieu de `BasePage`. Accédez au fichier de classe code-behind pour chaque page de contenu dans le dossier `~/Admin` et apportez cette modification. Par exemple, dans `~/Admin/Default.aspx` vous modifiez la déclaration de classe code-behind à partir de :

[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

À :

[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

La figure 11 décrit comment la page maître de niveau supérieur (`Site.master` ou `Alternate.master`), la page maître imbriquée (`AdminNested.master`) et les pages de contenu de la section d’administration sont liées les unes aux autres.

[![la page maître imbriquée définit le contenu spécifique aux pages de la section Administration](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Figure 11**: la page maître imbriquée définit le contenu spécifique aux pages de la section Administration ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image33.png))

## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Étape 6 : mettre en miroir les méthodes et propriétés publiques de la page maître

Rappelez-vous que les pages `~/Admin/AddProduct.aspx` et `~/Admin/Products.aspx` interagissent par programmation avec la page maître : `~/Admin/AddProduct.aspx` appelle la méthode `RefreshRecentProductsGrid` publique de la page maître et définit sa propriété `GridMessageText` ; `~/Admin/Products.aspx` a un gestionnaire d’événements pour l’événement `PricesDoubled`. Dans le didacticiel précédent, nous avons créé une classe `MustInherit` `BaseMasterPage` qui définissait ces membres publics.

Les pages `~/Admin/AddProduct.aspx` et `~/Admin/Products.aspx` supposent que leur page maître dérive de la classe `BaseMasterPage`. Toutefois, la page `AdminNested.master` étend actuellement la classe `System.Web.UI.MasterPage`. Par conséquent, lorsque vous vous rendez `~/Admin/Products.aspx` une `InvalidCastException` est levée avec le message suivant : « impossible d’effectuer un cast d’un objet de type’ASP. admin\_adminnested\_Master’en type’BaseMasterPage' ».

Pour résoudre ce problème, nous devons disposer de la classe code-behind `AdminNested.master` étendre `BaseMasterPage`. Mettez à jour la déclaration de classe code-behind de la page maître imbriquée à partir de :

[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

À :

[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Nous n’avons pas encore terminé. Nous devons remplacer les membres marqués comme `MustOverride`, à savoir `RefreshRecentProductsGrid` et `GridMessageText`. Ces membres sont utilisés par les pages maîtres de niveau supérieur pour mettre à jour leurs interfaces utilisateur. (En fait, seule la page maître `Site.master` utilise ces méthodes, bien que les pages maîtres de niveau supérieur implémentent ces méthodes, puisque les deux étendent `BaseMasterPage`.)

Bien que nous ayons besoin d’implémenter ces membres dans `AdminNested.master`, toutes ces implémentations doivent simplement appeler le même membre dans la page maître de niveau supérieur utilisée par la page maître imbriquée. Par exemple, lorsqu’une page de contenu de la section Administration appelle la méthode `RefreshRecentProductsGrid` de la page maître imbriquée, toute la page maître imbriquée doit, à son tour, appeler `Site.master` méthode de `RefreshRecentProductsGrid` ou de `Alternate.master`.

Pour ce faire, commencez par ajouter la directive `@MasterType` suivante au début de `AdminNested.master`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Rappelez-vous que la directive `@MasterType` ajoute une propriété fortement typée à la classe code-behind nommée `Master`. Substituez ensuite les membres `RefreshRecentProductsGrid` et `GridMessageText` et déléguez simplement l’appel à la méthode correspondante de l' `Master`:

[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Une fois ce code en place, vous devez être en mesure de consulter et d’utiliser les pages de contenu de la section Administration. La figure 12 illustre la page de `~/Admin/Products.aspx` lorsque vous l’affichez dans un navigateur. Comme vous pouvez le voir, la page contient la zone instructions d’administration, qui est définie dans la page maître imbriquée.

[![les pages de contenu de la section Administration incluent des instructions en haut de chaque page.](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Figure 12**: les pages de contenu de la section Administration incluent des instructions en haut de chaque page ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image36.png))

## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Étape 7 : utilisation de la page maître de niveau supérieur appropriée au moment de l’exécution

Alors que toutes les pages de contenu de la section Administration sont entièrement fonctionnelles, elles utilisent toutes la même page maître de niveau supérieur et ignorent la page maître sélectionnée par l’utilisateur au `ChooseMasterPage.aspx`. Ce comportement est dû au fait que la propriété `MasterPageFile` de la page maître imbriquée est statiquement définie sur `Site.master` dans sa directive `<%@ Master %>`.

Pour utiliser la page maître de niveau supérieur sélectionnée par l’utilisateur final, nous devons affecter à la propriété `MasterPageFile` de l' `AdminNested.master`la valeur dans la variable de session `MyMasterPage`. Étant donné que nous avons défini les propriétés de `MasterPageFile` de pages de contenu dans `BasePage`, vous pouvez penser que nous définissons la propriété `MasterPageFile` de la page maître imbriquée dans `BaseMasterPage` ou dans la classe code-behind de `AdminNested.master`. Toutefois, cela ne fonctionnera pas, car nous devons définir la propriété `MasterPageFile` à la fin de l’étape PreInit. La première fois que nous pouvons utiliser par programmation le cycle de vie d’une page à partir d’une page maître est l’étape init (qui se produit après l’étape de préinitialisation).

Par conséquent, nous devons définir la propriété `MasterPageFile` de la page maître imbriquée à partir des pages de contenu. Les seules pages de contenu qui utilisent la page maître `AdminNested.master` dérivent de `AdminBasePage`. Par conséquent, nous pouvons y placer cette logique. À l’étape 5, nous a remplacé la méthode `SetMasterPageFile`, en affectant à la propriété `MasterPageFile` de l’objet page la valeur « ~/Admin/AdminNested.master ». Mettez à jour `SetMasterPageFile` pour définir également la propriété `MasterPageFile` de la page maître sur le résultat stocké dans la session :

[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

La méthode `GetMasterPageFileFromSession`, que nous avons ajoutée à la classe `BasePage` dans le didacticiel précédent, retourne le chemin d’accès au fichier de la page maître approprié en fonction de la valeur de la variable de session.

Une fois cette modification en place, la sélection de la page maître de l’utilisateur est transmise à la section Administration. La figure 13 illustre la même page que la figure 12, mais une fois que l’utilisateur a modifié la sélection de sa page maître en `Alternate.master`.

[![la page d’administration imbriquée utilise la page maître de niveau supérieur sélectionnée par l’utilisateur](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Figure 13**: la page d’administration imbriquée utilise la page maître de niveau supérieur sélectionnée par l’utilisateur ([cliquez pour afficher l’image en taille réelle](nested-master-pages-vb/_static/image39.png))

## <a name="summary"></a>Récapitulatif

À l’instar de la façon dont les pages de contenu peuvent être liées à une page maître, il est possible de créer des pages maîtres imbriquées en faisant en sorte qu’une page maître enfant soit liée à une page maître parente. La page maître enfant peut définir des contrôles de contenu pour chacun des ContentPlaceHolders de son parent ; Il peut ensuite ajouter ses propres contrôles ContentPlaceHolder (ainsi que d’autres balises) à ces contrôles de contenu. Les pages maîtres imbriquées sont très utiles dans les applications Web volumineuses où toutes les pages partagent une apparence et une convivialité différentes, mais certaines sections du site nécessitent des personnalisations uniques.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Pages maîtres ASP.NET imbriquées](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Conseils pour les pages maîtres imbriquées et le temps de conception VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Prise en charge de la page maître imbriquée VS 2008](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](specifying-the-master-page-programmatically-vb.md)
