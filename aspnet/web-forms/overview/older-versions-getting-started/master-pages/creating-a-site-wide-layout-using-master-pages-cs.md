---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
title: Création d’une disposition à l’ensemble du site àC#l’aide de pages maîtres () | Microsoft Docs
author: rick-anderson
description: Ce didacticiel présente les concepts de base de la page maître. À savoir, que sont les pages maîtres, comment créer une page maître, qu’est-ce que les espaces réservés de contenu, comment fait un CR...
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 78f8d194-03b9-44a5-8255-90e7cd1c2ee1
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a5e85c443a2a3642ec185ab1897c43cdb2ab1f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625390"
---
# <a name="creating-a-site-wide-layout-using-master-pages-c"></a>Création d’une disposition à l’échelle d’un site avec des pages maîtres (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_CS.pdf)

> Ce didacticiel présente les concepts de base de la page maître. À savoir, que sont les pages maîtres, comment créer une page maître, qu’est-ce qu’un détenteur de contenu, comment créer une page ASP.NET qui utilise une page maître, comment la modification de la page maître est automatiquement reflétée dans les pages de contenu associées, et ainsi de suite.

## <a name="introduction"></a>Introduction

L’un des attributs d’un site Web bien conçu est une disposition de page cohérente à l’ensemble du site. Prenez le site Web www.asp.net, par exemple. Au moment de la rédaction de cet article, chaque page a le même contenu en haut et en bas de la page. Comme le montre la figure 1, la partie supérieure de chaque page affiche une barre grise avec une liste de communautés Microsoft. En dessous, il s’agit du logo de site, de la liste des langues dans lesquelles le site a été traduit et des principales sections : Accueil, prise en main, apprentissage, téléchargements, etc. De même, le bas de la page contient des informations sur la publicité sur www.asp.net, une déclaration de copyright et un lien vers la déclaration de confidentialité.

[![le site Web www.asp.net utilise une apparence cohérente sur toutes les pages](creating-a-site-wide-layout-using-master-pages-cs/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image1.png)

<strong>Figure 01</strong>: le site Web www.asp.NET utilise une apparence cohérente sur toutes les pages ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image3.png))

Un autre attribut d’un site bien conçu est la facilité avec laquelle l’apparence du site peut être modifiée. La figure 1 montre la page d’accueil www.asp.net à partir du 2008 mars, mais entre maintenant et la publication de ce didacticiel, l’apparence a peut-être changé. Par exemple, les éléments de menu situés en haut seront développés pour inclure une nouvelle section pour l’infrastructure MVC. Ou peut-être une conception radicalement nouvelle avec des couleurs, des polices et une mise en page différentes seront dévoilées. L’application de ces modifications à l’ensemble du site doit être un processus simple et rapide qui ne nécessite pas la modification des milliers de pages Web qui composent le site.

La création d’un modèle de page à l’ensemble du site dans ASP.NET est possible grâce à l’utilisation de *pages maîtres*. En résumé, une page maître est un type spécial de page ASP.NET qui définit le balisage qui est commun à toutes les *pages de contenu* , ainsi que les régions qui peuvent être personnalisées en fonction de la page de contenu page par contenu. (Une page de contenu est une page ASP.NET liée à la page maître.) Chaque fois que la mise en page ou la mise en forme d’une page maître est modifiée, toutes ses sorties de pages de contenu sont également immédiatement mises à jour, ce qui permet d’appliquer des modifications d’apparence au niveau du site aussi facilement que de mettre à jour et de déployer un seul fichier (c’est-à-dire la page maître).

Il s’agit du premier didacticiel d’une série de didacticiels qui explorent l’utilisation des pages maîtres. Au cours de cette série de didacticiels, nous allons :

- Examiner la création de pages maîtres et leurs pages de contenu associées,
- Discutez de divers trucs, astuces et pièges,
- Identifiez les pièges courants de page maître et explorez les solutions de contournement,
- Découvrez comment accéder à la page maître à partir d’une page de contenu et vice-versa,
- Découvrez comment spécifier la page maître d’une page de contenu au moment de l’exécution, et
- Autres rubriques de la page maître avancée.

Ces didacticiels sont destinés à être concis et fournissent des instructions pas à pas avec de nombreuses captures d’écran pour vous guider tout au long du processus. Chaque Didacticiel est disponible dans C# et Visual Basic versions et comprend un téléchargement du code complet utilisé.

Ce didacticiel plus inaugural commence par un examen des principes fondamentaux de la page maître. Nous expliquons comment les pages maîtres fonctionnent, nous allons créer une page maître et les pages de contenu associées à l’aide de Visual Web Developer et voir comment les modifications apportées à une page maître sont immédiatement reflétées dans ses pages de contenu. C’est parti !

## <a name="understanding-how-master-pages-work"></a>Comprendre le fonctionnement des pages maîtres

La création d’un site Web avec une disposition de page cohérente sur l’ensemble du site requiert que chaque page Web émette un balisage de mise en forme commun en plus de son contenu personnalisé. Par exemple, bien que chaque didacticiel ou publication de Forum sur www.asp.net ait son propre contenu unique, chacune de ces pages affiche également une série d’éléments de `<div>` communs qui affichent la section de niveau supérieur liens : Accueil, prise en main, apprentissage, etc.

Il existe diverses techniques pour créer des pages Web avec une apparence et une convivialité cohérentes. Une approche naïve consiste simplement à copier et à coller le balisage de disposition commun dans toutes les pages Web, mais cette approche présente un certain nombre d’inconvénients. Pour commencer, chaque fois qu’une nouvelle page est créée, vous devez vous souvenir de copier et coller le contenu partagé dans la page. De telles opérations de copie et de collage sont des erreurs, car vous risquez de ne copier accidentellement qu’un sous-ensemble du balisage partagé dans une nouvelle page. En premier lieu, cette approche permet de remplacer l’apparence existante au niveau du site par une nouvelle, car chaque page du site doit être modifiée pour pouvoir utiliser l’apparence et la convivialité.

Avant ASP.NET version 2,0, les développeurs de pages présentaient souvent un balisage commun dans les [contrôles utilisateur](https://msdn.microsoft.com/library/y6wb1a0e.aspx) , puis ajoutaient ces contrôles utilisateur à chaque page et à chaque page. Cette approche nécessitait que le développeur de pages n’ait pas à ajouter manuellement les contrôles utilisateur à chaque nouvelle page, mais qu’il était possible d’apporter des modifications plus simples au niveau du site, car lors de la mise à jour du balisage commun, seuls les contrôles utilisateur devaient être modifiés. Malheureusement, Visual Studio .NET 2002 et 2003-les versions de Visual Studio utilisées pour créer des applications ASP.NET 1. x affichent les contrôles utilisateur dans le Mode Création sous forme de zones grises. Par conséquent, les développeurs de pages qui utilisent cette approche ne bénéficient pas d’un environnement WYSIWYG au moment de la conception.

Les lacunes de l’utilisation des contrôles utilisateur ont été traitées dans ASP.NET version 2,0 et Visual Studio 2005 avec l’introduction des *pages maîtres*. Une page maître est un type spécial de page ASP.NET qui définit à la fois le balisage à l’ensemble du site et les *régions* où les *pages de contenu* associées définissent leur balisage personnalisé. Comme nous le verrons à l’étape 1, ces régions sont définies par les contrôles ContentPlaceHolder. Le contrôle ContentPlaceHolder désigne simplement une position dans la hiérarchie des contrôles de la page maître, où le contenu personnalisé peut être injecté par une page de contenu.

> [!NOTE]
> Les concepts de base et les fonctionnalités des pages maîtres n’ont pas changé depuis la version 2,0 de ASP.NET. Toutefois, Visual Studio 2008 offre une prise en charge au moment du design des pages maîtres imbriquées, une fonctionnalité qui manque dans Visual Studio 2005. Nous examinerons l’utilisation des pages maîtres imbriquées dans un prochain didacticiel.

La figure 2 montre à quoi peut ressembler la page maître de www.asp.net. Notez que la page maître définit la disposition commune à l’ensemble du site, le balisage en haut, en bas et à droite de chaque page, ainsi qu’un ContentPlaceHolder dans le milieu de gauche, où se trouve le contenu unique de chaque page Web individuelle.

![Une page maître définit la disposition à l’échelle du site et les régions modifiables sur une base de page de contenu page par contenu.](creating-a-site-wide-layout-using-master-pages-cs/_static/image4.png)

**Figure 02**: une page maître définit la disposition à l’échelle du site et les régions modifiables sur une base de page de contenu page par contenu

Une fois qu’une page maître a été définie, elle peut être liée à de nouvelles pages ASP.NET via le cycle d’une case à cocher. Ces pages ASP.NET, appelées pages de contenu, incluent un contrôle de contenu pour chacun des contrôles ContentPlaceHolder de la page maître. Lorsque la page de contenu est visitée via un navigateur, le moteur ASP.NET crée la hiérarchie des contrôles de la page maître et injecte la hiérarchie des contrôles de la page de contenu aux emplacements appropriés. Cette hiérarchie de contrôle combinée est rendue et le code HTML qui en résulte est retourné au navigateur de l’utilisateur final. Par conséquent, la page de contenu émet à la fois le balisage commun défini dans sa page maître en dehors des contrôles ContentPlaceHolder et le balisage spécifique à la page défini dans ses propres contrôles de contenu. La figure 3 illustre ce concept.

[![le balisage de la page demandée est fusible dans la page maître](creating-a-site-wide-layout-using-master-pages-cs/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image5.png)

**Figure 03**: le balisage de la page demandée est fusible dans la page maître ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image7.png))

Maintenant que nous avons abordé le fonctionnement des pages maîtres, jetons un coup d’œil à la création d’une page maître et de pages de contenu associées à l’aide de Visual Web Developer.

> [!NOTE]
> Pour atteindre le public le plus vaste possible, le site Web ASP.NET que nous créons dans cette série de didacticiels sera créé à l’aide de ASP.NET 3,5 avec la version gratuite de Microsoft Visual Studio 2008, [Visual Web developer 2008](https://www.microsoft.com/express/vwd/). Si vous n’avez pas encore effectué la mise à niveau vers ASP.NET 3,5, ne vous inquiétez pas : les concepts abordés dans ces didacticiels fonctionnent aussi bien avec ASP.NET 2,0 et Visual Studio 2005. Toutefois, certaines applications de démonstration peuvent utiliser des fonctionnalités nouvelles à la version de .NET Framework 3,5 ; Lorsque vous utilisez des fonctionnalités spécifiques à 3,5, j’ai inclus une note qui explique comment implémenter des fonctionnalités similaires dans la version 2,0. N’oubliez pas que les applications de démonstration disponibles en téléchargement à partir de chaque didacticiel ciblent la version 3,5 de .NET Framework, ce qui se traduit par un fichier `Web.config` qui comprend des éléments de configuration spécifiques à 3,5 et des références à des espaces de noms spécifiques à 3,5 dans les instructions `using` des classes code-behind des pages ASP.NET. Long Story Short, si vous n’avez pas encore installé .NET 3,5 sur votre ordinateur, l’application Web téléchargeable ne fonctionnera pas sans supprimer au préalable le balisage spécifique à 3,5 de `Web.config`. Pour plus d’informations sur cette rubrique, consultez [discroisement du fichier de `Web.config` de ASP.NET version 3.5](http://www.4guysfromrolla.com/articles/121207-1.aspx) . Vous devrez également supprimer les instructions `using` qui font référence à des espaces de noms spécifiques à 3,5.

## <a name="step-1-creating-a-master-page"></a>Étape 1 : création d’une page maître

Avant de pouvoir explorer la création et l’utilisation de pages maîtres et de contenu, nous avons d’abord besoin d’un site Web ASP.NET. Commencez par créer un site Web ASP.NET basé sur un système de fichiers. Pour ce faire, lancez Visual Web Developer, puis accédez au menu fichier et sélectionnez Nouveau site Web, en affichant la boîte de dialogue Nouveau site Web (voir figure 4). Choisissez le modèle de site Web ASP.NET, définissez la liste déroulante emplacement sur système de fichiers, choisissez un dossier pour placer le site Web et définissez la langue C#sur. Cela permet de créer un nouveau site Web avec une `Default.aspx` page ASP.NET, un dossier `App_Data` et un fichier `Web.config`.

> [!NOTE]
> Visual Studio prend en charge deux modes de gestion de projet : les projets de site Web et les projets d’application Web. Les projets de site Web n’ont pas de fichier projet, tandis que les projets d’application Web imitent l’architecture du projet dans Visual Studio .NET 2002/2003. ils incluent un fichier projet et compilent le code source du projet dans un assembly unique, qui est placé dans le dossier `/bin`. Visual Studio 2005 n’a initialement pris en charge que les projets de site Web, même si le [modèle de projet d’application Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) a été réintroduit avec Service Pack 1. Visual Studio 2008 offre les deux modèles de projet. Toutefois, les éditions Visual Web Developer 2005 et 2008 ne prennent en charge que les projets de site Web. J’utilise le modèle de projet de site Web pour mes démonstrations de cette série de didacticiels. Si vous utilisez une édition non Express et que vous souhaitez utiliser le modèle de projet d’application Web à la place, n’hésitez pas à le faire, mais sachez qu’il peut y avoir des différences entre ce que vous voyez sur votre écran et les étapes que vous devez prendre par rapport aux captures d’écran indiquées et instructio NS fourni dans ces didacticiels.

[![créer un site Web basé sur un système de fichiers](creating-a-site-wide-layout-using-master-pages-cs/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image8.png)

**Figure 04**: créer un site Web basé sur un système de fichiers ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image10.png))

Ensuite, ajoutez une page maître au site dans le répertoire racine en cliquant avec le bouton droit sur le nom du projet, en choisissant Ajouter un nouvel élément, puis en sélectionnant le modèle page maître. Notez que les pages maîtres se terminent par l’extension `.master`. Nommez cette nouvelle page maître `Site.master`, puis cliquez sur Ajouter.

[![ajouter une page maître nommée site. Master au site Web](creating-a-site-wide-layout-using-master-pages-cs/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image11.png)

**Figure 05**: ajouter une page maître nommée `Site.master` au site Web ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image13.png))

L’ajout d’un nouveau fichier de page maître via Visual Web Developer crée une page maître avec le balisage déclaratif suivant :

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample1.aspx)]

La première ligne du balisage déclaratif est la [directive`@Master`](https://msdn.microsoft.com/library/ms228176.aspx). La directive `@Master` est semblable à la [directive`@Page`](https://msdn.microsoft.com/library/ydy4x04a.aspx) qui apparaît dans les pages ASP.net. Il définit la langue côté serveur (C#) et les informations sur l’emplacement et l’héritage de la classe code-behind de la page maître.

Le `DOCTYPE` et le balisage déclaratif de la page s’affichent sous la directive `@Master`. La page contient du code HTML statique et quatre contrôles côté serveur :

- **Un formulaire Web (le `<form runat="server">`)** -parce que toutes les pages ASP.net ont généralement un formulaire Web, et parce que la page maître peut inclure des contrôles Web qui doivent apparaître dans un formulaire Web, veillez à ajouter le formulaire Web à votre page maître (au lieu d’ajouter un formulaire Web à chaque page de contenu).
- **Un contrôle ContentPlaceHolder nommé `ContentPlaceHolder1`** -ce contrôle ContentPlaceHolder apparaît dans le formulaire Web et sert de région pour l’interface utilisateur de la page de contenu.
- **Élément `<head>` côté serveur** -l’élément `<head>` a l’attribut `runat="server"`, ce qui le rend accessible via du code côté serveur. L’élément `<head>` est implémenté de cette manière afin que le titre de la page et d’autres balises liées à la `<head>`puissent être ajoutés ou ajustés par programme. Par exemple, la définition de la propriété `Title` d’une page ASP.NET modifie l’élément `<title>` restitué par le contrôle serveur `<head>`.
- **Un contrôle ContentPlaceHolder nommé `head`** -ce contrôle ContentPlaceHolder apparaît dans le contrôle serveur `<head>` et peut être utilisé pour ajouter de façon déclarative du contenu à l’élément `<head>`.

Ce balisage déclaratif de page maître par défaut sert de point de départ pour la conception de vos propres pages maîtres. N’hésitez pas à modifier le code HTML ou à ajouter des contrôles Web ou des ContentPlaceHolders à la page maître.

> [!NOTE]
> Lors de la conception d’une page maître, assurez-vous que la page maître contient un formulaire Web et qu’au moins un contrôle ContentPlaceHolder apparaît dans ce formulaire Web.

### <a name="creating-a-simple-site-layout"></a>Création d’une disposition de site simple

Développons le balisage déclaratif par défaut de `Site.master`pour créer une disposition de site dans laquelle toutes les pages sont partagées : un en-tête commun ; une colonne de gauche avec navigation, Actualités et tout autre contenu à l’ensemble du site ; et un pied de page qui affiche l’icône « alimenté par Microsoft ASP.NET ». La figure 6 montre le résultat final de la page maître lorsque l’une de ses pages de contenu est affichée par le biais d’un navigateur. La région rouge cerclée de la figure 6 est spécifique à la page visitée (`Default.aspx`); l’autre contenu est défini dans la page maître et est donc cohérent dans toutes les pages de contenu.

[![la page maître définit le balisage des parties supérieure, gauche et inférieure](creating-a-site-wide-layout-using-master-pages-cs/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image14.png)

**Figure 06**: la page maître définit le balisage des parties supérieure, gauche et inférieure ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image16.png))

Pour obtenir la disposition du site illustrée à la figure 6, commencez par mettre à jour la `Site.master` page maître pour qu’elle contienne le balisage déclaratif suivant :

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample2.aspx)]

La disposition de la page maître est définie à l’aide d’une série d’éléments HTML `<div>`. Le `<div>` `topContent` contient le balisage qui apparaît en haut de chaque page, tandis que les `mainContent`, `leftContent`et `footerContent` sont utilisés pour afficher respectivement le contenu de la page, la colonne de gauche et l’icône « alimenté par `<div>` ». En plus d’ajouter ces éléments `<div>`, j’ai également renommé la propriété `ID` du contrôle ContentPlaceHolder principal de `ContentPlaceHolder1` à `MainContent`.

Les règles de mise en forme et de disposition pour ces éléments de `<div>` assortis sont épelées dans le fichier de [feuille de style en cascade (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) `Styles.css`, qui est spécifié via un &lt;élément de&gt; de lien dans l’élément &lt;Head&gt; de la page maître. Ces diverses règles définissent l’apparence de chaque élément `<div>` indiqué ci-dessus. Par exemple, l’élément `topContent` `<div>`, qui affiche le texte et le lien « didacticiels sur les pages maîtres », a ses règles de mise en forme spécifiées dans `Styles.css` comme suit :

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample3.css)]

Si vous suivez la procédure sur votre ordinateur, vous devez télécharger le code associé à ce didacticiel et ajouter le `Styles.css` fichier à votre projet. De même, vous devrez également créer un dossier nommé images et copier l’icône « alimenté par Microsoft ASP.NET » à partir du site Web de démonstration téléchargé dans votre projet.

> [!NOTE]
> Cet article n’aborde pas la mise en forme des feuilles de style CSS et des pages Web. Pour plus d’informations sur CSS, consultez les [didacticiels CSS](http://www.w3schools.com/css/default.asp) sur [w3schools.com](http://www.w3schools.com/). Je vous conseille également de télécharger le code associé de ce didacticiel et de jouer avec les paramètres CSS dans `Styles.css` pour voir les effets de différentes règles de mise en forme.

### <a name="creating-a-master-page-using-an-existing-design-template"></a>Création d’une page maître à l’aide d’un modèle de conception existant

Au fil des années, j’ai créé un certain nombre d’applications Web ASP.NET pour les petites et moyennes entreprises. Certains de mes clients avaient une disposition de site existante qu’ils souhaitaient utiliser. d’autres ont recruté un concepteur graphique compétent. Quelques-uns ont été confiés pour concevoir la disposition de site Web. Comme vous pouvez le constater à la figure 6, la tâche d’un programmeur pour concevoir la mise en page d’un site Web est généralement aussi prudente que le fait de faire en sorte que votre comptable effectue une opération d’Open-coer, tandis que votre médecin effectue vos impôts.

Heureusement, il existe de nombreux sites Web qui offrent des modèles de conception HTML gratuits. Google a renvoyé plus de 6 millions résultats pour le terme de recherche « modèles de sites Web gratuits ». L’un de mes préférés est [OpenDesigns.org](http://opendesigns.org/). Une fois que vous avez trouvé un modèle de site Web de votre choix, ajoutez les fichiers CSS et les images à votre projet de site Web et intégrez le code HTML du modèle dans votre page maître.

> [!NOTE]
> Microsoft propose également un certain nombre de [modèles de kit de démarrage de conception ASP.net gratuits](https://msdn.microsoft.com/asp.net/aa336613.aspx) qui s’intègrent dans la boîte de dialogue Nouveau site Web de Visual Studio.

## <a name="step-2-creating-associated-content-pages"></a>Étape 2 : création de pages de contenu associées

Une fois la page maître créée, nous sommes prêts à commencer à créer des pages ASP.NET liées à la page maître. Ces pages sont appelées *pages de contenu*.

Nous allons ajouter une nouvelle page ASP.NET au projet et la lier à la page maître `Site.master`. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet et choisissez l’option Ajouter un nouvel élément. Sélectionnez le modèle de formulaire Web, entrez le nom `About.aspx`, puis activez la case à cocher « sélectionner la page maître », comme illustré à la figure 7. Cela permet d’afficher la boîte de dialogue Sélectionner une page maître (voir figure 8) à partir de laquelle vous pouvez choisir la page maître à utiliser.

> [!NOTE]
> Si vous avez créé votre site Web ASP.NET à l’aide du modèle de projet d’application Web au lieu du modèle de projet de site Web, vous ne verrez pas la case à cocher « sélectionner une page maître » dans la boîte de dialogue Ajouter un nouvel élément, comme illustré à la figure 7. Pour créer une page de contenu lors de l’utilisation du modèle de projet d’application Web, vous devez choisir le modèle de formulaire de contenu Web au lieu du modèle de formulaire Web. Une fois que vous avez sélectionné le modèle de formulaire de contenu Web et cliqué sur Ajouter, la boîte de dialogue Sélectionner une page maître présentée dans la figure 8 s’affiche.

[![ajouter une nouvelle page de contenu](creating-a-site-wide-layout-using-master-pages-cs/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image17.png)

**Figure 07**: ajouter une nouvelle page de contenu ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image19.png))

[![sélectionnez la page maître site. Master.](creating-a-site-wide-layout-using-master-pages-cs/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image20.png)

**Figure 08**: sélectionner la Page maître `Site.master` ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image22.png))

Comme le montre le balisage déclaratif suivant, une nouvelle page de contenu contient une directive `@Page` qui pointe vers sa page maître et un contrôle de contenu pour chacun des contrôles ContentPlaceHolder de la page maître.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample4.aspx)]

> [!NOTE]
> Dans la section « création d’une disposition de site simple » de l’étape 1, j’ai renommé `ContentPlaceHolder1` en `MainContent`. Si vous n’avez pas renommé ce contrôle ContentPlaceHolder `ID` de la même façon, le balisage déclaratif de votre page de contenu diffère légèrement du balisage indiqué ci-dessus. À savoir, le deuxième `ContentPlaceHolderID` du contrôle de contenu reflète la `ID` du contrôle ContentPlaceHolder correspondant dans votre page maître.

Lors du rendu d’une page de contenu, le moteur ASP.NET doit fusible les contrôles de contenu de la page avec les contrôles ContentPlaceHolder de sa page maître. Le moteur ASP.NET détermine la page maître de la page de contenu à partir de l’attribut `MasterPageFile` de la directive `@Page`. Comme le montre le balisage ci-dessus, cette page de contenu est liée à `~/Site.master`.

Étant donné que la page maître a deux contrôles ContentPlaceHolder-`head` et `MainContent`-Visual Web Developer a généré deux contrôles de contenu. Chaque contrôle de contenu fait référence à un ContentPlaceHolder particulier par le biais de sa propriété `ContentPlaceHolderID`.

La façon dont les pages maîtres s’appuient sur les techniques de modèle à l’ensemble du site précédentes est la prise en charge au moment de la conception. La figure 9 illustre la page de contenu `About.aspx` lorsqu’elle est consultée par le Mode Création de Visual Web Developer. Notez que, si le contenu de la page maître est visible, il est grisé et ne peut pas être modifié. Toutefois, les contrôles de contenu correspondant au ContentPlaceHolders de la page maître sont modifiables. Et comme avec toute autre page ASP.NET, vous pouvez créer l’interface de la page de contenu en ajoutant des contrôles Web via les vues source ou Design.

[![le mode Design de la page de contenu affiche à la fois le contenu spécifique à la page et le contenu de la page maître](creating-a-site-wide-layout-using-master-pages-cs/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image23.png)

**Figure 09**: le mode Design de la page de contenu affiche à la fois le contenu de la page maître et le contenu de la page maître ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image25.png))

### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Ajout de balises et de contrôles Web à la page de contenu

Prenez un moment pour créer du contenu pour la page `About.aspx`. Comme vous pouvez le voir à la figure 10, j’ai entré un titre « à propos de l’auteur » et quelques paragraphes de texte, mais vous pouvez également ajouter des contrôles Web. Une fois cette interface créée, accédez à la page `About.aspx` via un navigateur.

[![visiter la page about. aspx via un navigateur](creating-a-site-wide-layout-using-master-pages-cs/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image26.png)

**Figure 10**: visiter la Page de `About.aspx` via un navigateur ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image28.png))

Il est important de comprendre que la page de contenu demandée et sa page maître associée sont fusionnées et entièrement rendues entièrement sur le serveur Web. Le navigateur de l’utilisateur final reçoit alors le code HTML fusionné qui en résulte. Pour vérifier cela, affichez le code HTML reçu par le navigateur en accédant au menu Affichage et en sélectionnant source. Notez qu’il n’y a pas d’images ou d’autres techniques spécialisées pour afficher deux pages Web différentes dans une même fenêtre.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Liaison d’une page maître à une page ASP.NET existante

Comme nous l’avons vu dans cette étape, l’ajout d’une nouvelle page de contenu à une application Web ASP.NET est aussi simple que l’activation de la case à cocher « sélectionner une page maître » et la sélection de la page maître. Malheureusement, la conversion d’une page ASP.NET existante en page maître n’est pas aussi simple.

Pour lier une page maître à une page ASP.NET existante, vous devez effectuer les étapes suivantes :

1. Ajoutez l’attribut `MasterPageFile` à la directive `@Page` de la page ASP.NET, en la faisant pointer vers la page maître appropriée.
2. Ajoutez des contrôles de contenu pour chaque ContentPlaceHolders de la page maître.
3. Coupez et collez de manière sélective le contenu existant de la page ASP.NET dans les contrôles de contenu appropriés. Je dit « de manière sélective » ici, car la page ASP.NET contient probablement des balises qui sont déjà exprimées par la page maître, telles que le `DOCTYPE`, l’élément `<html>` et le formulaire Web.

Pour obtenir des instructions pas à pas sur ce processus, ainsi que des captures d’écran, consultez le didacticiel sur les [pages maîtres et la navigation](http://webproject.scottgu.com/CSharp/MasterPages/MasterPages.aspx) dans le site de [Scott Guthrie](https://weblogs.asp.net/scottgu/). La section « mettre à jour `Default.aspx` et `DataSample.aspx` pour utiliser la page maître » détaille ces étapes.

Étant donné qu’il est beaucoup plus facile de créer des pages de contenu que de convertir des pages ASP.NET existantes en pages de contenu, je vous recommande de les utiliser chaque fois que vous créez un site Web ASP.NET ajouter une page maître au site. Lier toutes les nouvelles pages ASP.NET à cette page maître. Ne vous inquiétez pas si la page maître initiale est très simple ou simple ; vous pouvez mettre à jour la page maître ultérieurement.

> [!NOTE]
> Lors de la création d’une application ASP.NET, Visual Web Developer ajoute une page `Default.aspx` qui n’est pas liée à une page maître. Si vous voulez vous exercer à convertir une page de ASP.NET existante en page de contenu, faites-le avec `Default.aspx`. Vous pouvez également supprimer `Default.aspx` puis l’ajouter à nouveau, mais cette fois, en cochant la case « sélectionner une page maître ».

## <a name="step-3-updating-the-master-pages-markup"></a>Étape 3 : mise à jour du balisage de la page maître

L’un des principaux avantages des pages maîtres est qu’une seule page maître peut être utilisée pour définir la disposition générale de nombreuses pages sur le site. Par conséquent, la mise à jour de l’apparence du site nécessite la mise à jour d’un seul fichier (la page maître).

Pour illustrer ce comportement, nous allons mettre à jour notre page maître pour inclure la date actuelle dans en haut de la colonne de gauche. Ajoutez une étiquette nommée `DateDisplay` au `<div>``leftContent`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample5.aspx)]

Ensuite, créez un gestionnaire d’événements `Page_Load` pour la page maître et ajoutez le code suivant :

[!code-csharp[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample6.cs)]

Le code ci-dessus définit la propriété de `Text` de l’étiquette sur la date et l’heure actuelles sous la forme du jour de la semaine, du nom du mois et du jour à deux chiffres (voir figure 11). Une fois cette modification apportée, réexaminez l’une de vos pages de contenu. Comme le montre la figure 11, le balisage résultant est immédiatement mis à jour pour inclure la modification apportée à la page maître.

[![les modifications apportées à la page maître sont reflétées lors de l’affichage de la page de contenu](creating-a-site-wide-layout-using-master-pages-cs/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image29.png)

**Figure 11**: les modifications apportées à la page maître sont reflétées lors de l’affichage de la page de contenu ([cliquez pour afficher l’image en taille réelle](creating-a-site-wide-layout-using-master-pages-cs/_static/image31.png))

> [!NOTE]
> Comme l’illustre cet exemple, les pages maîtres peuvent contenir des contrôles Web, du code et des gestionnaires d’événements côté serveur.

## <a name="summary"></a>Récapitulatif

Les pages maîtres permettent aux développeurs ASP.NET de concevoir une disposition cohérente à l’ensemble du site, facile à mettre à jour. La création de pages maîtres et de leurs pages de contenu associées est aussi simple que la création de pages ASP.NET standard, car Visual Web Developer offre une prise en charge enrichie au moment du Design.

L’exemple de page maître que nous avons créé dans ce didacticiel avait deux contrôles ContentPlaceHolder, `head` et `MainContent`. Toutefois, nous avons spécifié le balisage uniquement pour le contrôle `MainContent` ContentPlaceHolder dans notre page de contenu. Dans le didacticiel suivant, nous examinerons l’utilisation de plusieurs contrôles de contenu dans la page de contenu. Nous voyons également comment définir le balisage par défaut pour les contrôles de contenu dans la page maître, ainsi que la manière de basculer entre l’utilisation du balisage par défaut défini dans la page maître et la fourniture d’un balisage personnalisé à partir de la page de contenu.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [ASP.NET pour les concepteurs : modèles de conception gratuits et conseils sur la création de sites Web ASP.NET à l’aide de normes Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Vue d’ensemble des pages maîtres ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Didacticiels de feuilles de style en cascade (CSS)](http://www.w3schools.com/css/default.asp)
- [Définition dynamique du titre de la page](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Pages maîtres dans ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Didacticiels de démarrage rapide de pages maîtres](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Next](multiple-contentplaceholders-and-default-content-cs.md)
