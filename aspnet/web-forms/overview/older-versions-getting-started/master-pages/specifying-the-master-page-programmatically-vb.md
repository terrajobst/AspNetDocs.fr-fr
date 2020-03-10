---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: Spécification de la page maître par programmation (VB) | Microsoft Docs
author: rick-anderson
description: Examine la définition par programmation de la page maître de la page de contenu via le gestionnaire d’événements PreInit.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b039b22bef38ae6ebf80be070820dc1638f87f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567010"
---
# <a name="specifying-the-master-page-programmatically-vb"></a>Spécification de la page maître par programmation (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Examine la définition par programmation de la page maître de la page de contenu via le gestionnaire d’événements PreInit.

## <a name="introduction"></a>Introduction

Depuis l’exemple de [*création d’une disposition à l’ensemble du site à l’aide de pages maîtres*](creating-a-site-wide-layout-using-master-pages-vb.md), toutes les pages de contenu ont référencé leur page maître de manière déclarative via l’attribut `MasterPageFile` de la directive `@Page`. Par exemple, la directive `@Page` suivante lie la page de contenu à la page maître `Site.master`:

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

La [classe`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx) de l’espace de noms `System.Web.UI` comprend une [propriété`MasterPageFile`](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) qui retourne le chemin d’accès à la page maître de la page de contenu. C’est cette propriété qui est définie par la directive `@Page`. Cette propriété peut également être utilisée pour spécifier par programmation la page maître de la page de contenu. Cette approche est utile si vous souhaitez affecter dynamiquement la page maître en fonction de facteurs externes, tels que l’utilisateur visitant la page.

Dans ce didacticiel, nous allons ajouter une deuxième page maître à notre site Web et déterminer de manière dynamique la page maître à utiliser au moment de l’exécution.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Étape 1 : examiner le cycle de vie de la page

Chaque fois qu’une requête arrive sur le serveur Web pour une page ASP.NET qui est une page de contenu, le moteur ASP.NET doit fusible les contrôles de contenu de la page dans les contrôles ContentPlaceHolder correspondants de la page maître. Cette fusion crée une hiérarchie de contrôle unique qui peut ensuite passer par le cycle de vie de la page typique.

La figure 1 illustre cette fusion. L’étape 1 de la figure 1 montre les hiérarchies de contrôle de contenu et de page maître initiales. À la fin de l’étape de préinit, les contrôles de contenu de la page sont ajoutés au ContentPlaceHolders correspondant dans la page maître (étape 2). Après cette fusion, la page maître sert de racine de la hiérarchie des contrôles fusionnés. Cette hiérarchie de contrôle fusionnée est ensuite ajoutée à la page pour produire la hiérarchie des contrôles finalisés (étape 3). Le résultat net est que la hiérarchie des contrôles de la page comprend la hiérarchie des contrôles fusionnés.

[![la page maître et les hiérarchies de contrôle de la page de contenu sont fusionnées au cours de l’étape de préinitialisation](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Figure 01**: les hiérarchies de contrôle de page maître et de page de contenu sont fusionnées au cours de l’étape de préinitialisation ([cliquez pour afficher l’image en taille réelle](specifying-the-master-page-programmatically-vb/_static/image3.png))

## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Étape 2 : définition de la propriété`MasterPageFile`à partir du code

La page maître partakes dans cette fusion dépend de la valeur de la propriété `MasterPageFile` de l’objet `Page`. La définition de l’attribut `MasterPageFile` dans la directive `@Page` a l’effet net de l’assignation de la propriété `MasterPageFile` de `Page`pendant l’étape d’initialisation, qui est la première étape du cycle de vie de la page. Vous pouvez également définir cette propriété par programmation. Toutefois, il est impératif que cette propriété soit définie avant que la fusion de la figure 1 n’ait lieu.

Au début de l’étape de préinitialisation, l’objet `Page` déclenche son [événement`PreInit`](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) et appelle sa [méthode`OnPreInit`](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Pour définir la page maître par programmation, nous pouvons soit créer un gestionnaire d’événements pour l’événement `PreInit`, soit substituer la méthode `OnPreInit`. Examinons les deux approches.

Commencez par ouvrir `Default.aspx.vb`, le fichier de classe code-behind pour la page d’accueil de notre site. Ajoutez un gestionnaire d’événements pour l’événement `PreInit` de la page en tapant le code suivant :

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

À partir de là, nous pouvons définir la propriété `MasterPageFile`. Mettez à jour le code afin qu’il affecte la valeur « ~/site.Master » à la propriété `MasterPageFile`.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Si vous définissez un point d’arrêt et que vous démarrez avec le débogage, vous verrez que chaque fois que la page `Default.aspx` est visitée ou qu’il y a une publication (postback) sur cette page, le gestionnaire d’événements `Page_PreInit` s’exécute et la propriété `MasterPageFile` est assignée à « ~/site.Master ».

Vous pouvez également remplacer la méthode `OnPreInit` de la classe `Page` et y définir la propriété `MasterPageFile`. Pour cet exemple, nous allons ne pas définir la page maître dans une page particulière, mais plutôt `BasePage`. Rappelez-vous que nous avons créé une classe de page de base personnalisée (`BasePage`) dans la [*section Spécification du titre, des balises meta et d’autres en-têtes HTML dans le didacticiel de la page maître*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) . Actuellement `BasePage` remplace la méthode de `OnLoadComplete` de la classe `Page`, où elle définit la propriété de `Title` de la page en fonction des données de plan de site. Mettons à jour `BasePage` pour remplacer également la méthode `OnPreInit` pour spécifier par programmation la page maître.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Étant donné que toutes les pages de contenu dérivent de `BasePage`, elles sont désormais affectées par programmation à leur page maître. À ce stade, le gestionnaire d’événements `PreInit` dans `Default.aspx.vb` est superflu ; n’hésitez pas à la supprimer.

### <a name="what-about-thepagedirective"></a>Qu’en est-il de la directive de`@Page`?

Ce qui peut être un peu confus, c’est que les propriétés de `MasterPageFile` de pages de contenu sont maintenant spécifiées à deux emplacements : par programmation dans la méthode `OnPreInit` de la classe `BasePage`, ainsi que via l’attribut `MasterPageFile` dans la directive `@Page` de chaque page de contenu.

La première étape du cycle de vie de la page est l’étape d’initialisation. Pendant cette étape, la propriété `MasterPageFile` de l’objet `Page` est assignée à la valeur de l’attribut `MasterPageFile` dans la directive `@Page` (si elle est fournie). L’étape PreInit suit l’étape d’initialisation, et c’est ici que nous définissons par programmation la propriété `MasterPageFile` de l’objet `Page`, remplaçant ainsi la valeur assignée à partir de la directive `@Page`. Étant donné que nous définissons la propriété `MasterPageFile` de l’objet `Page` par programmation, nous pourrions supprimer l’attribut `MasterPageFile` de la directive `@Page` sans affecter l’expérience de l’utilisateur final. Pour vous en convaincre, supprimez l’attribut `MasterPageFile` de la directive `@Page` dans `Default.aspx` puis accédez à la page via un navigateur. Comme vous pouvez vous y attendre, la sortie est la même qu’avant la suppression de l’attribut.

Le fait que la propriété `MasterPageFile` soit définie via la directive `@Page` ou par programmation n’est pas une incidence sur l’expérience de l’utilisateur final. Toutefois, l’attribut `MasterPageFile` de la directive `@Page` est utilisé par Visual Studio au moment du design pour produire la vue WYSIWYG dans le concepteur. Si vous revenez à `Default.aspx` dans Visual Studio et que vous accédez au concepteur, vous verrez le message « erreur de la page maître : la page contient des contrôles qui nécessitent une référence de page maître, mais aucun n’est spécifié » (voir figure 2).

En bref, vous devez conserver l’attribut `MasterPageFile` dans la directive `@Page` pour bénéficier d’une expérience de conception riche dans Visual Studio.

[![Visual Studio utilise l’attribut MasterPageFile de la directive @Page pour restituer le mode Design](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Figure 02**: Visual Studio utilise l’attribut `MasterPageFile` de la directive `@Page` pour restituer le mode Design ([Cliquer pour afficher l’image en taille réelle](specifying-the-master-page-programmatically-vb/_static/image6.png))

## <a name="step-3-creating-an-alternative-master-page"></a>Étape 3 : création d’une autre page maître

Étant donné que la page maître d’une page de contenu peut être définie par programmation au moment de l’exécution, il est possible de charger dynamiquement une page maître particulière en fonction de certains critères externes. Cette fonctionnalité peut être utile dans les situations où la disposition du site doit varier en fonction de l’utilisateur. Par exemple, une application Web du moteur de blog peut permettre à ses utilisateurs de choisir une disposition pour leur blog, où chaque disposition est associée à une autre page maître. Lors de l’exécution, lorsqu’un visiteur visualise le blog d’un utilisateur, l’application Web doit déterminer la disposition du blog et associer de manière dynamique la page maître correspondante à la page de contenu.

Examinons comment charger dynamiquement une page maître au moment de l’exécution en fonction de certains critères externes. Notre site Web contient actuellement une seule page maître (`Site.master`). Nous avons besoin d’une autre page maître pour illustrer le choix d’une page maître au moment de l’exécution. Cette étape porte sur la création et la configuration de la nouvelle page maître. L’étape 4 permet de déterminer la page maître à utiliser au moment de l’exécution.

Créez une nouvelle page maître dans le dossier racine nommé `Alternate.master`. Ajoutez également une nouvelle feuille de style au site Web nommé `AlternateStyles.css`.

[![ajouter une autre page maître et un autre fichier CSS au site Web](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Figure 03**: ajouter une autre page maître et un autre fichier CSS au site Web ([cliquez pour afficher l’image en taille réelle](specifying-the-master-page-programmatically-vb/_static/image9.png))

J’ai conçu la page maître `Alternate.master` pour afficher le titre en haut de la page, centré et sur un arrière-plan bleu marine. J’ai distribué la colonne de gauche et déplacé ce contenu sous le contrôle `MainContent` ContentPlaceHolder, qui s’étend maintenant à la largeur totale de la page. En outre, j’nixed la liste des leçons non triées et je l’ai remplacée par une liste horizontale au-dessus `MainContent`. J’ai également mis à jour les polices et les couleurs utilisées par la page maître (et, par extension, ses pages de contenu). La figure 4 montre `Default.aspx` lors de l’utilisation de la page maître `Alternate.master`.

> [!NOTE]
> ASP.NET offre la possibilité de définir des *thèmes*. Un thème est une collection d’images, de fichiers CSS et de paramètres de propriété de contrôle Web liés aux styles qui peuvent être appliqués à une page au moment de l’exécution. Les thèmes sont la méthode à suivre si les dispositions de votre site diffèrent uniquement dans les images affichées et par leurs règles CSS. Si les dispositions diffèrent de façon plus importante, par exemple en utilisant différents contrôles Web ou une disposition radicalement différente, vous devrez utiliser des pages maîtres distinctes. Pour plus d’informations sur les thèmes, consultez la section de lecture supplémentaire à la fin de ce didacticiel.

[![nos pages de contenu peuvent désormais utiliser une nouvelle apparence](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Figure 04**: nos pages de contenu peuvent désormais utiliser une nouvelle apparence ([Cliquer pour afficher l’image en taille réelle](specifying-the-master-page-programmatically-vb/_static/image12.png))

Lorsque le balisage maître et les pages de contenu sont fusionnés, la classe `MasterPage` vérifie que chaque contrôle de contenu de la page de contenu fait référence à un ContentPlaceHolder dans la page maître. Une exception est levée si un contrôle de contenu qui référence un ContentPlaceHolder inexistant est trouvé. En d’autres termes, il est impératif que la page maître qui est assignée à la page de contenu ait un ContentPlaceHolder pour chaque contrôle de contenu dans la page de contenu.

La page maître `Site.master` comprend quatre contrôles ContentPlaceHolder :

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Certaines pages de contenu de notre site Web incluent simplement un ou deux contrôles de contenu. d’autres incluent un contrôle de contenu pour chaque ContentPlaceHolders disponible. Si la nouvelle page maître (`Alternate.master`) peut être assignée aux pages de contenu qui ont des contrôles de contenu pour tous les ContentPlaceHolders dans `Site.master`, il est essentiel que `Alternate.master` inclue également les mêmes contrôles ContentPlaceHolder que `Site.master`.

Pour faire en sorte que votre page maître `Alternate.master` ressemble à mine (voir figure 4), commencez par définir les styles de la page maître dans la feuille de style `AlternateStyles.css`. Ajoutez les règles suivantes dans `AlternateStyles.css`:

[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

Ensuite, ajoutez le balisage déclaratif suivant à `Alternate.master`. Comme vous pouvez le voir, `Alternate.master` contient quatre contrôles ContentPlaceHolder avec les mêmes `ID` valeurs que les contrôles ContentPlaceHolder dans `Site.master`. En outre, il comprend un contrôle ScriptManager, qui est nécessaire pour les pages de notre site Web qui utilisent l’infrastructure AJAX ASP.NET.

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Test de la nouvelle page maître

Pour tester cette nouvelle page maître, mettez à jour la méthode `OnPreInit` de la classe `BasePage` afin que la valeur `"~/Alternate.maser"` soit affectée à la propriété `MasterPageFile`, puis accédez au site Web. Chaque page doit fonctionner sans erreur, à l’exception de deux : `~/Admin/AddProduct.aspx` et `~/Admin/Products.aspx`. L’ajout d’un produit au contrôle DetailsView dans `~/Admin/AddProduct.aspx` produit une `NullReferenceException` à partir de la ligne de code qui tente de définir la propriété `GridMessageText` de la page maître. Lorsque vous vous rendez `~/Admin/Products.aspx` une `InvalidCastException` est levée lors du chargement de la page avec le message suivant : « impossible d’effectuer un cast d’un objet de type’ASP. Alternate\_Master’en type’ASP. site\_Master'. »

Ces erreurs se produisent parce que la classe code-behind `Site.master` comprend des événements publics, des propriétés et des méthodes qui ne sont pas définis dans `Alternate.master`. La partie du balisage de ces deux pages a une directive `@MasterType` qui fait référence à la page maître `Site.master`.

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

En outre, le gestionnaire d’événements `ItemInserted` du contrôle DetailsView dans `~/Admin/AddProduct.aspx` comprend du code qui convertit la propriété faiblement typée `Page.Master` en un objet de type `Site`. La directive `@MasterType` (utilisée de cette façon) et le cast dans le gestionnaire d’événements `ItemInserted` associent étroitement les pages `~/Admin/AddProduct.aspx` et `~/Admin/Products.aspx` à la page maître `Site.master`.

Pour rompre ce couplage étroit, nous pouvons `Site.master` et `Alternate.master` dérivent d’une classe de base commune qui contient des définitions pour les membres publics. Après cela, nous pouvons mettre à jour la directive `@MasterType` pour référencer ce type de base commun.

### <a name="creating-a-custom-base-master-page-class"></a>Création d’une classe de page maître personnalisée

Ajoutez un nouveau fichier de classe au dossier `App_Code` nommé `BaseMasterPage.vb` et dérivez-le de `System.Web.UI.MasterPage`. Nous devons définir la méthode `RefreshRecentProductsGrid` et la propriété `GridMessageText` dans `BaseMasterPage`, mais nous ne pouvons pas simplement les déplacer à partir de `Site.master`, car ces membres utilisent des contrôles Web spécifiques à la page maître `Site.master` (le `RecentProducts` GridView et l’étiquette `GridMessage`).

Nous devons configurer `BaseMasterPage` de manière à ce que ces membres soient définis ici, mais ils sont implémentés par les classes dérivées de `BaseMasterPage`(`Site.master` et `Alternate.master`). Ce type d’héritage est possible en marquant la classe en tant que `MustInherit` et ses membres comme `MustOverride`. En résumé, l’ajout de ces mots clés à la classe et à ses deux membres annonce que `BaseMasterPage` n’a pas implémenté `RefreshRecentProductsGrid` et `GridMessageText`, mais que ses classes dérivées le feront.

Nous devons également définir l’événement `PricesDoubled` dans `BaseMasterPage` et fournir un moyen aux classes dérivées de déclencher l’événement. Le modèle utilisé dans le .NET Framework pour faciliter ce comportement consiste à créer un événement public dans la classe de base et à ajouter une méthode protégée et substituable nommée `OnEventName`. Les classes dérivées peuvent ensuite appeler cette méthode pour déclencher l’événement ou la substituer pour exécuter du code immédiatement avant ou après le déclenchement de l’événement.

Mettez à jour votre classe `BaseMasterPage` de manière à ce qu’elle contienne le code suivant :

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

Ensuite, accédez à la classe code-behind `Site.master` et dérivez-la de `BaseMasterPage`. Étant donné que `BaseMasterPage` contient des membres marqués `MustOverride` nous devons remplacer ces membres ici dans `Site.master`. Ajoutez le mot clé `Overrides` aux définitions de méthode et de propriété. Mettez également à jour le code qui déclenche l’événement `PricesDoubled` dans le gestionnaire d’événements `Click` du bouton `DoublePrice` à l’aide d’un appel à la méthode `OnPricesDoubled` de la classe de base.

Après ces modifications, la `Site.master` classe code-behind doit contenir le code suivant :

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

Nous devons également mettre à jour la classe code-behind de `Alternate.master`pour dériver de `BaseMasterPage` et remplacer les deux membres `MustOverride`. Toutefois, étant donné que `Alternate.master` ne contient pas de GridView qui répertorie les produits les plus récents ou une étiquette qui affiche un message après l’ajout d’un nouveau produit à la base de données, ces méthodes n’ont rien à faire.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>Référencement de la classe de page maître de base

Maintenant que nous avons terminé la classe `BaseMasterPage` et que nos deux pages maîtres l’étendent, notre dernière étape consiste à mettre à jour les pages `~/Admin/AddProduct.aspx` et `~/Admin/Products.aspx` pour faire référence à ce type commun. Commencez par modifier la directive `@MasterType` dans les deux pages à partir de :

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

À :

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

Au lieu de référencer un chemin d’accès de fichier, la propriété `@MasterType` fait désormais référence au type de base (`BaseMasterPage`). Par conséquent, la propriété fortement typée `Master` utilisée dans les deux classes code-behind des pages est désormais de type `BaseMasterPage` (au lieu de type `Site`). Une fois cette modification effectuée, visitez `~/Admin/Products.aspx`. Auparavant, cela provoquait une erreur de conversion, car la page est configurée pour utiliser la page maître `Alternate.master`, mais la directive `@MasterType` a référencé le fichier `Site.master`. Mais maintenant, la page s’affiche sans erreur. Cela est dû au fait que la `Alternate.master` page maître peut être convertie en un objet de type `BaseMasterPage` (puisqu’elle l’étend).

Une petite modification doit être apportée dans `~/Admin/AddProduct.aspx`. Le gestionnaire d’événements `ItemInserted` du contrôle DetailsView utilise à la fois la propriété de `Master` fortement typée et la propriété de `Page.Master` faiblement typée. Nous avons corrigé la référence fortement typée lorsque nous avons mis à jour la directive `@MasterType`, mais nous devons toujours mettre à jour la référence faiblement typée. Remplacez la ligne de code suivante :

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Avec le code suivant, qui effectue un cast `Page.Master` vers le type de base :

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Étape 4 : détermination de la page maître à lier aux pages de contenu

Notre classe `BasePage` définit actuellement toutes les propriétés de `MasterPageFile` de contenu sur une valeur codée en dur au cours de l’étape de préinitialisation du cycle de vie de la page. Nous pouvons mettre à jour ce code pour baser la page maître sur un facteur externe. Par exemple, la page maître à charger dépend des préférences de l’utilisateur actuellement connecté. Dans ce cas, nous aurions besoin d’écrire du code dans la méthode `OnPreInit` dans `BasePage` qui recherche les préférences de la page maître de l’utilisateur en cours de consultation.

Nous allons créer une page Web qui permet à l’utilisateur de choisir la page maître à utiliser : `Site.master` ou `Alternate.master` et enregistrer ce choix dans une variable de session. Commencez par créer une nouvelle page Web dans le répertoire racine nommé `ChooseMasterPage.aspx`. Lors de la création de cette page (ou de toute autre page de contenu), vous n’avez pas besoin de la lier à une page maître, car la page maître est définie par programme dans `BasePage`. Toutefois, si vous ne liez pas la nouvelle page à une page maître, le balisage déclaratif par défaut de la nouvelle page contient un formulaire Web et tout autre contenu fourni par la page maître. Vous devez remplacer manuellement ce balisage par les contrôles de contenu appropriés. Pour cette raison, je trouve qu’il est plus facile de lier la nouvelle page ASP.NET à une page maître.

> [!NOTE]
> Étant donné que `Site.master` et `Alternate.master` ont le même jeu de contrôles ContentPlaceHolder, peu importe la page maître que vous choisissez lors de la création de la page de contenu. Pour des fins de cohérence, je vous suggère d’utiliser `Site.master`.

[![ajouter une nouvelle page de contenu au site Web](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Figure 05**: ajouter une nouvelle page de contenu au site Web ([cliquez pour afficher l’image en taille réelle](specifying-the-master-page-programmatically-vb/_static/image15.png))

Mettez à jour le fichier `Web.sitemap` pour inclure une entrée pour cette leçon. Ajoutez le balisage suivant sous le `<siteMapNode>` pour les pages maîtres et la leçon ASP.NET AJAX :

[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Avant d’ajouter du contenu à la page `ChooseMasterPage.aspx`, prenez un moment pour mettre à jour la classe code-behind de la page afin qu’elle dérive de `BasePage` (plutôt que `System.Web.UI.Page`). Ensuite, ajoutez un contrôle DropDownList à la page, affectez à sa propriété `ID` la valeur `MasterPageChoice`et ajoutez deux ListItems avec les valeurs `Text` de « ~/site.Master » et de « ~/Alternate.Master ».

Ajoutez un contrôle Web Button à la page et définissez ses propriétés `ID` et `Text` sur `SaveLayout` et « enregistrer le choix de disposition », respectivement. À ce stade, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Lorsque la page est visitée pour la première fois, vous devez afficher le choix de la page maître actuellement sélectionnée de l’utilisateur. Créez un gestionnaire d’événements `Page_Load` et ajoutez le code suivant :

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

Le code ci-dessus s’exécute uniquement sur la première page visitée (et non sur les publications (postbacks) suivantes). Il vérifie d’abord si la variable de session `MyMasterPage` existe. Si c’est le cas, il tente de trouver le ListItem correspondant dans le `MasterPageChoice` DropDownList. Si une valeur ListItem correspondante est trouvée, sa propriété `Selected` est définie sur `True`.

Nous avons également besoin d’un code qui enregistre le choix de l’utilisateur dans la variable de session `MyMasterPage`. Créez un gestionnaire d’événements pour l’événement `Click` du bouton `SaveLayout` et ajoutez le code suivant :

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> Au moment où le gestionnaire d’événements `Click` s’exécute lors de la publication (postback), la page maître a déjà été sélectionnée. Par conséquent, la sélection de la liste déroulante de l’utilisateur ne sera pas appliquée jusqu’à la prochaine visite de la page. Le `Response.Redirect` oblige le navigateur à redemander `ChooseMasterPage.aspx`.

Une fois la page `ChooseMasterPage.aspx` terminée, la dernière tâche consiste à avoir `BasePage` affecter la propriété `MasterPageFile` en fonction de la valeur de la variable de session `MyMasterPage`. Si la variable de session n’est pas définie, `BasePage` valeur par défaut est `Site.master`.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> J’ai déplacé le code qui affecte la propriété `MasterPageFile` de l’objet `Page` à partir du gestionnaire d’événements `OnPreInit` et à deux méthodes distinctes. Cette première méthode, `SetMasterPageFile`, assigne la propriété `MasterPageFile` à la valeur retournée par la deuxième méthode, `GetMasterPageFileFromSession`. J’ai marqué la méthode `SetMasterPageFile` `Overridable` afin que les futures classes qui étendent `BasePage` puissent éventuellement la substituer pour implémenter une logique personnalisée, si nécessaire. Nous verrons un exemple de remplacement de la propriété `SetMasterPageFile` de `BasePage`dans le didacticiel suivant.

Une fois ce code en place, accédez à la page `ChooseMasterPage.aspx`. Au départ, la page maître `Site.master` est sélectionnée (voir figure 6), mais l’utilisateur peut choisir une autre page maître dans la liste déroulante.

[![pages de contenu s’affichent à l’aide de la page maître site. Master](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Figure 06**: les pages de contenu s’affichent à l’aide de la `Site.master` page maître ([cliquez pour afficher l’image en taille réelle](specifying-the-master-page-programmatically-vb/_static/image18.png))

[![pages de contenu sont maintenant affichées à l’aide de la page maître autre. Master.](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Figure 07**: les pages de contenu s’affichent désormais à l’aide de la `Alternate.master` page maître ([cliquez pour afficher l’image en taille réelle](specifying-the-master-page-programmatically-vb/_static/image21.png))

## <a name="summary"></a>Récapitulatif

Quand une page de contenu est visitée, ses contrôles de contenu sont fusionnés avec les contrôles ContentPlaceHolder de sa page maître. La page maître de la page de contenu est indiquée par la propriété `MasterPageFile` de la classe `Page`, qui est assignée à l’attribut `MasterPageFile` de la directive `@Page` au cours de l’étape d’initialisation. Comme ce didacticiel l’a montré, nous pouvons affecter une valeur à la propriété `MasterPageFile` tant que nous le faisons avant la fin de l’étape de préinitialisation. La possibilité de spécifier par programmation la page maître ouvre la porte à des scénarios plus avancés, tels que la liaison dynamique d’une page de contenu à une page maître en fonction de facteurs externes.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Diagramme du cycle de vie de la page ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Présentation du cycle de vie de la page ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Vue d’ensemble des thèmes et des apparences ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Pages maîtres : conseils, astuces et interruptions](http://www.odetocode.com/articles/450.aspx)
- [Thèmes dans ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Teli Banerjee. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-pages-and-asp-net-ajax-vb.md)
> [Suivant](nested-master-pages-vb.md)
