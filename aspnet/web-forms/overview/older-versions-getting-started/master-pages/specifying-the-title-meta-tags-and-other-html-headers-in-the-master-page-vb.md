---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Spécification du titre, des balises meta et d’autres en-têtes HTML dans la page maître (VB) | Microsoft Docs
author: rick-anderson
description: Examine les différentes techniques permettant de définir les éléments assortis &lt;Head&gt; dans la page maître à partir de la page de contenu.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 160af664cdf27f9ede1273aaf915da749a39ad48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78643072"
---
# <a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Spécification du titre, des balises META et d’autres en-têtes HTML dans la page maître (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Examine les différentes techniques permettant de définir les éléments assortis &lt;Head&gt; dans la page maître à partir de la page de contenu.

## <a name="introduction"></a>Introduction

Les nouvelles pages maîtres créées dans Visual Studio 2008 ont, par défaut, deux contrôles ContentPlaceHolder : l’un nommé `head`et situé dans l’élément `<head>` ; et l’autre nommé `ContentPlaceHolder1`, placé dans le formulaire Web. L’objectif de `ContentPlaceHolder1` est de définir une région dans le formulaire Web qui peut être personnalisée page par page. L' `head` ContentPlaceHolder permet aux pages d’ajouter du contenu personnalisé à la section `<head>`. (Bien sûr, ces deux ContentPlaceHolders peuvent être modifiées ou supprimées, et des ContentPlaceHolder supplémentaires peuvent être ajoutés à la page maître. Notre page maître, `Site.master`, possède actuellement quatre contrôles ContentPlaceHolder.)

L’élément `<head>` HTML sert de référentiel pour les informations relatives au document de la page Web qui ne fait pas partie du document lui-même. Cela comprend des informations telles que le titre de la page Web, les méta-informations utilisées par les moteurs de recherche ou les robots internes, ainsi que des liens vers des ressources externes, telles que des flux RSS, JavaScript et des fichiers CSS. Certaines de ces informations peuvent s’avérer utiles pour toutes les pages du site Web. Par exemple, vous souhaiterez peut-être importer globalement les mêmes règles CSS et fichiers JavaScript pour chaque page ASP.NET. Toutefois, il existe des parties de l’élément `<head>` qui sont spécifiques à la page. Le titre de la page est un exemple premier.

Dans ce didacticiel, nous allons examiner comment définir des balises de section de `<head>` globales et spécifiques à une page dans la page maître et dans ses pages de contenu.

## <a name="examining-the-master-pagesheadsection"></a>Examen de la section`<head>`de la page maître

Le fichier de page maître par défaut créé par Visual Studio 2008 contient le balisage suivant dans sa section `<head>` :

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Notez que l’élément `<head>` contient un attribut `runat="server"`, qui indique qu’il s’agit d’un contrôle serveur (plutôt que du code HTML statique). Toutes les pages ASP.NET dérivent de la [classe`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx), qui se trouve dans l’espace de noms `System.Web.UI`. Cette classe contient une [propriété`Header`](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) qui fournit l’accès à la région `<head>` de la page. À l’aide de la propriété `Header`, nous pouvons définir le titre d’une page ASP.NET ou ajouter des balises supplémentaires à la section `<head>` rendue. Il est possible, ensuite, de personnaliser l’élément `<head>` d’une page de contenu en écrivant un peu de code dans le gestionnaire d’événements `Page_Load` de la page. Nous examinons comment définir par programmation le titre de la page à l’étape 1.

Le balisage affiché dans l’élément `<head>` ci-dessus comprend également un contrôle ContentPlaceHolder nommé `head`. Ce contrôle ContentPlaceHolder n’est pas nécessaire, car les pages de contenu peuvent ajouter du contenu personnalisé à l’élément `<head>` par programmation. Toutefois, il est utile dans les situations où une page de contenu doit ajouter le balisage statique à l’élément `<head>`, car le balisage statique peut être ajouté de façon déclarative au contrôle de contenu correspondant plutôt que par programmation.

En plus de l’élément `<title>` et `head` ContentPlaceHolder, l’élément `<head>` de la page maître doit contenir tout balisage de niveau `<head>`qui est commun à toutes les pages. Dans notre site Web, toutes les pages utilisent les règles CSS définies dans le fichier `Styles.css`. Par conséquent, nous avons mis à jour l’élément `<head>` dans le didacticiel [*création d’une disposition à l’ensemble du site avec des pages maîtres*](creating-a-site-wide-layout-using-master-pages-vb.md) pour inclure un élément `<link>` correspondant. Le balisage de `<head>` actuel de notre page maître `Site.master` est illustré ci-dessous.

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Étape 1 : définition du titre d’une page de contenu

Le titre de la page Web est spécifié via l’élément `<title>`. Il est important de définir le titre de chaque page sur une valeur appropriée. Lorsque vous visitez une page, son titre s’affiche dans la barre de titre du navigateur. En outre, lors de la création d’un signet sur une page, les navigateurs utilisent le titre de la page comme nom suggéré pour le signet. En outre, de nombreux moteurs de recherche affichent le titre de la page lors de l’affichage des résultats de la recherche.

> [!NOTE]
> Par défaut, Visual Studio affecte à l’élément `<title>` de la page maître la valeur « page sans titre ». De même, les `<title>` nouvelles pages de ASP.NET ont également la valeur « page sans titre ». Étant donné qu’il peut être facile d’oublier de définir le titre de la page sur une valeur appropriée, il existe de nombreuses pages sur Internet avec le titre « page sans titre ». La recherche de Google pour les pages Web avec ce titre retourne environ 2 460 000 résultats. Même Microsoft est susceptible de publier des pages Web avec le titre « page sans titre ». Au moment de la rédaction de cet article, une recherche Google a rapporté 236 pages Web dans le domaine Microsoft.com.

Une page ASP.NET peut spécifier son titre de l’une des manières suivantes :

- En plaçant la valeur directement dans l’élément `<title>`
- Utilisation de l’attribut `Title` dans la directive `<%@ Page %>`
- Définition par programmation de la propriété `Title` de la page à l’aide d’un code comme `Page.Title="title"` ou `Page.Header.Title="title"`.

Les pages de contenu n’ont pas d’élément `<title>`, tel qu’il est défini dans la page maître. Par conséquent, pour définir le titre d’une page de contenu, vous pouvez utiliser l’attribut `Title` de la directive `<%@ Page %>` ou le définir par programmation.

### <a name="setting-the-pages-title-declaratively"></a>Définition déclarative du titre de la page

Le titre d’une page de contenu peut être défini de façon déclarative via l’attribut `Title` de la [directive`<%@ Page %>`](https://msdn.microsoft.com/library/ydy4x04a.aspx). Cette propriété peut être définie en modifiant directement la directive `<%@ Page %>` ou le Fenêtre Propriétés. Examinons les deux approches.

À partir de la vue source, localisez la directive `<%@ Page %>`, qui se trouve en haut de la balise déclarative de la page. La directive `<%@ Page %>` pour `Default.aspx` suit :

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

La directive `<%@ Page %>` spécifie des attributs spécifiques à la page utilisés par le moteur ASP.NET lors de l’analyse et de la compilation de la page. Cela comprend le fichier de la page maître, l’emplacement de son fichier de code et son titre, entre autres informations.

Par défaut, lors de la création d’une page de contenu, Visual Studio affecte à l’attribut `Title` la valeur « page sans titre ». Modifiez l’attribut `Title` de `Default.aspx`de « page sans titre » en « didacticiels de page maître », puis affichez la page via un navigateur. La figure 1 montre la barre de titre du navigateur, qui reflète le nouveau titre de la page.

![La barre de titre du navigateur affiche maintenant &quot;didacticiels sur les pages maîtres&quot; au lieu de &quot;page sans titre&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Figure 01**: la barre de titre du navigateur affiche maintenant « didacticiels de page maître » au lieu de « page sans titre »

Le titre de la page peut également être défini à partir de la Fenêtre Propriétés. Dans la Fenêtre Propriétés, sélectionnez DOCUMENT dans la liste déroulante pour charger les propriétés au niveau de la page, ce qui comprend la propriété `Title`. La figure 2 illustre la Fenêtre Propriétés une fois que `Title` a été défini sur « didacticiels de page maître ».

![Vous pouvez également configurer le titre à partir de la fenêtre Propriétés.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Figure 02**: vous pouvez configurer le titre à partir de la fenêtre Propriétés.

### <a name="setting-the-pages-title-programmatically"></a>Définition du titre de la page par programmation

Le balisage `<head runat="server">` de la page maître est traduit en une instance de [classe`HtmlHead`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) lorsque la page est rendue par le moteur ASP.net. La classe `HtmlHead` a une [propriété`Title`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) dont la valeur est reflétée dans l’élément `<title>` rendu. Cette propriété est accessible à partir de la classe code-behind d’une page ASP.NET via `Page.Header.Title`; cette même propriété est également accessible via `Page.Title`.

Pour vous entraîner à définir par programmation le titre de la page, accédez à la classe code-behind de la page `About.aspx` et créez un gestionnaire d’événements pour l’événement `Load` de la page. Ensuite, définissez le titre de la page sur « didacticiels de page maître :: about :: *Date*», où *Date* est la date actuelle. Après avoir ajouté ce code, votre gestionnaire d’événements `Page_Load` doit se présenter comme suit :

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

La figure 3 illustre la barre de titre du navigateur lors de la visite de la page `About.aspx`.

![Le titre de la page est défini par programmation et comprend la date actuelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Figure 03**: le titre de la page est défini par programmation et comprend la date actuelle

## <a name="step-2-automatically-assigning-a-page-title"></a>Étape 2 : attribution automatique d’un titre de page

Comme nous l’avons vu à l’étape 1, le titre d’une page peut être défini de façon déclarative ou par programme. Toutefois, si vous oubliez de remplacer explicitement le titre par un nom plus descriptif, votre page aura le titre par défaut « page sans titre ». Dans l’idéal, le titre de la page est défini automatiquement pour nous en cas de non-spécification explicite de sa valeur. Par exemple, si au moment de l’exécution le titre de la page est « page sans titre », nous pouvons souhaiter que le titre soit automatiquement mis à jour pour être le même que le nom de fichier de la page ASP.NET. La bonne nouvelle, c’est qu’avec un peu de travail initial, il est possible d’attribuer automatiquement le titre.

Toutes les pages Web ASP.NET dérivent de la classe `Page` de l’espace de noms System. Web. UI. La classe `Page` définit les fonctionnalités minimales requises par une page ASP.NET et expose des propriétés essentielles comme `IsPostBack`, `IsValid`, `Request`et `Response`, parmi de nombreuses autres. Souvent, chaque page d’une application Web requiert des fonctionnalités ou des fonctionnalités supplémentaires. Pour ce faire, il est courant de créer une classe de page de base personnalisée. Une classe de page de base personnalisée est une classe que vous créez qui dérive de la classe `Page` et qui comprend des fonctionnalités supplémentaires. Une fois cette classe de base créée, vous pouvez faire en sorte que vos pages ASP.NET dérivent de celle-ci (plutôt que la classe `Page`), offrant ainsi les fonctionnalités étendues à vos pages ASP.NET.

Dans cette étape, nous créons une page de base qui définit automatiquement le titre de la page sur le nom de fichier de la page ASP.NET, si le titre n’a pas été explicitement défini. L’étape 3 examine la définition du titre de la page en fonction du plan du site.

> [!NOTE]
> La création et l’utilisation de classes de page de base personnalisées n’entrent pas dans le cadre de cette série de didacticiels. Pour plus d’informations, consultez [utilisation d’une classe de base personnalisée pour les classes code-behind de vos Pages ASP.net](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).

### <a name="creating-the-base-page-class"></a>Création de la classe de base page

Notre première tâche consiste à créer une classe de page de base, qui est une classe qui étend la classe `Page`. Commencez par ajouter un dossier `App_Code` à votre projet en cliquant avec le bouton droit sur le nom du projet dans la Explorateur de solutions, en choisissant Ajouter un dossier ASP.NET, puis en sélectionnant `App_Code`. Ensuite, cliquez avec le bouton droit sur le dossier `App_Code` et ajoutez une nouvelle classe nommée `BasePage.vb`. La figure 4 montre les Explorateur de solutions après l’ajout du dossier `App_Code` et de la classe `BasePage.vb`.

![Ajoutez un dossier App_Code et une classe nommée BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Figure 04**: ajouter un dossier `App_Code` et une classe nommée `BasePage`

> [!NOTE]
> Visual Studio prend en charge deux modes de gestion de projet : les projets de site Web et les projets d’application Web. Le dossier `App_Code` est conçu pour être utilisé avec le modèle de projet de site Web. Si vous utilisez le modèle de projet d’application Web, placez la classe `BasePage.vb` dans un dossier nommé autre chose que `App_Code`, tel que `Classes`. Pour plus d’informations sur cette rubrique, consultez [migration d’un projet de site Web vers un projet d’application Web](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).

Étant donné que la page de base personnalisée sert de classe de base pour les classes code-behind des pages ASP.NET, elle doit étendre la classe `Page`.

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Chaque fois qu’une page ASP.NET est demandée, elle passe par une série d’étapes, aboutissant à la restitution de la page demandée en HTML. Nous pouvons appuyer sur une étape en remplaçant la méthode `OnEvent` de la classe `Page`. Pour notre page de base, nous allons définir automatiquement le titre s’il n’a pas été explicitement spécifié par l’étape de `LoadComplete` (ce qui, comme vous l’avez peut-être deviné, se produit après l’étape de `Load`).

Pour ce faire, remplacez la méthode `OnLoadComplete` et entrez le code suivant :

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

La méthode `OnLoadComplete` commence par déterminer si la propriété `Title` n’a pas encore été explicitement définie. Si la propriété `Title` est `Nothing`, une chaîne vide ou a la valeur « page sans titre », elle est assignée au nom de fichier de la page ASP.NET demandée. Le chemin d’accès physique à la page ASP.NET demandée-`C:\MySites\Tutorial03\Login.aspx`, par exemple, est accessible via la propriété `Request.PhysicalPath`. La méthode `Path.GetFileNameWithoutExtension` est utilisée pour extraire uniquement la partie de nom de fichier, et ce nom de fichier est ensuite assigné à la propriété `Page.Title`.

> [!NOTE]
> Je vous invite à améliorer cette logique pour améliorer le format du titre. Par exemple, si le nom de fichier de la page est `Company-Products.aspx`, le code ci-dessus produit le titre « Company-Products », mais dans l’idéal, le tiret est remplacé par un espace, comme dans « Company Products ». Envisagez également d’ajouter un espace à chaque fois qu’un changement de casse est nécessaire. Autrement dit, pensez à ajouter du code qui transforme le nom de fichier `OurBusinessHours.aspx` en titre « notre heure d’ouverture ».

### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Utilisation des pages de contenu pour hériter de la classe de base page

Nous devons maintenant mettre à jour les pages ASP.NET de notre site pour qu’elles dérivent de la page de base personnalisée (`BasePage`) au lieu de la classe `Page`. Pour ce faire, accédez à chaque classe code-behind et modifiez la déclaration de classe à partir de :

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

À :

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

À l’issue de cette procédure, visitez le site via un navigateur. Si vous visitez une page dont le titre est défini explicitement, par exemple `Default.aspx` ou `About.aspx`, le titre explicitement spécifié est utilisé. Toutefois, si vous visitez une page dont le titre n’a pas été modifié par rapport à la valeur par défaut (« page sans titre »), la classe de base page définit le titre sur le nom de fichier de la page.

La figure 5 illustre la page de `MultipleContentPlaceHolders.aspx` lorsque vous l’affichez dans un navigateur. Notez que le titre est précisément le nom de fichier de la page (moins l’extension), « MultipleContentPlaceHolders ».

[![si un titre n’est pas explicitement spécifié, le nom de fichier de la page est automatiquement utilisé](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Figure 05**: si un titre n’est pas explicitement spécifié, le nom de fichier de la page est automatiquement utilisé ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))

## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Étape 3 : baser le titre de la page sur le plan du site

ASP.NET offre une infrastructure de plan de site robuste qui permet aux développeurs de pages de définir un plan de site hiérarchique dans une ressource externe (par exemple, un fichier XML ou une table de base de données), ainsi que des contrôles Web permettant d’afficher des informations sur le plan de site (par exemple, SiteMapPath, Les contrôles menu et TreeView).

La structure de plan de site est également accessible par programme à partir de la classe code-behind d’une page ASP.NET. De cette façon, nous pouvons définir automatiquement le titre d’une page sur le titre de son nœud correspondant dans le plan du site. Nous allons améliorer la classe `BasePage` créée à l’étape 2 afin qu’elle offre cette fonctionnalité. Nous devons tout d’abord créer un plan de site pour notre site.

> [!NOTE]
> Ce didacticiel part du principe que le lecteur est déjà familiarisé avec ASP. Fonctionnalités du plan de site du réseau. Pour plus d’informations sur l’utilisation du plan de site, consultez la série d’articles en plusieurs parties, [examen d’ASP. Navigation sur le site NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).

### <a name="creating-the-site-map"></a>Création du plan de site

Le système de plan de site est créé au-dessus du [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), qui découple l’API de plan de site de la logique qui sérialise les informations de plan de site entre la mémoire et un magasin persistant. Le .NET Framework est fourni avec la [classe`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), qui est le fournisseur de plan de site par défaut. Comme son nom l’indique, `XmlSiteMapProvider` utilise un fichier XML comme magasin de plan de site. Utilisons ce fournisseur pour définir notre plan de site.

Commencez par créer un fichier de plan de site dans le dossier racine du site Web nommé `Web.sitemap`. Pour ce faire, cliquez avec le bouton droit sur le nom du site Web dans Explorateur de solutions, choisissez Ajouter un nouvel élément, puis sélectionnez le modèle plan du site. Assurez-vous que le fichier est nommé `Web.sitemap`, puis cliquez sur Ajouter.

[![ajouter un fichier nommé Web. sitemap au dossier racine du site Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Figure 06**: ajouter un fichier nommé `Web.sitemap` au dossier racine du site Web ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))

Ajoutez le code XML suivant au fichier `Web.sitemap` :

[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Ce code XML définit la structure de plan de site hiérarchique présentée à la figure 7.

![Le plan de site est actuellement composé de trois nœuds de plan de site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Figure 07**: le plan de site est actuellement composé de trois nœuds de plan de site

Nous allons mettre à jour la structure de plan de site dans les prochains didacticiels à mesure que nous ajouterons de nouveaux exemples.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Mise à jour de la page maître pour inclure des contrôles Web de navigation

Maintenant que nous disposons d’un plan de site défini, nous allons mettre à jour la page maître pour inclure des contrôles Web de navigation. En particulier, nous allons ajouter un contrôle ListView à la colonne de gauche de la section leçons qui restitue une liste non triée avec un élément de liste pour chaque nœud défini dans le plan de site.

> [!NOTE]
> Le contrôle ListView est une nouveauté de ASP.NET version 3,5. Si vous utilisez une version antérieure de ASP.NET, utilisez le contrôle Repeater à la place. Pour plus d’informations sur le contrôle ListView, consultez [utilisation des contrôles ListView et DataPager de ASP.net 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Commencez par supprimer le balisage de liste non triée existant de la section leçons. Ensuite, faites glisser un contrôle ListView de la boîte à outils et déposez-le sous l’en-tête leçons. Le ListView se trouve dans la section données de la boîte à outils, parallèlement aux autres contrôles d’affichage : GridView, DetailsView et FormView. Définissez la propriété `ID` de ListView sur `LessonsList`.

À partir de l’Assistant Configuration de source de données, choisissez de lier le ListView à un nouveau contrôle SiteMapDataSource nommé `LessonsDataSource`. Le contrôle SiteMapDataSource retourne la structure hiérarchique du système de plan de site.

[![lier un contrôle SiteMapDataSource au contrôle ListView LessonsList](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Figure 08**: lier un contrôle SiteMapDataSource au contrôle ListView LessonsList ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))

Après avoir créé le contrôle SiteMapDataSource, nous devons définir les modèles de ListView afin qu’il restitue une liste non triée avec un élément de liste pour chaque nœud retourné par le contrôle SiteMapDataSource. Pour ce faire, vous pouvez utiliser le balisage de modèle suivant :

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

Le `LayoutTemplate` génère le balisage pour une liste non triée (`<ul>...</ul>`) tandis que le `ItemTemplate` restitue chaque élément retourné par SiteMapDataSource sous la forme d’un élément de liste (`<li>`) qui contient un lien vers la leçon en question.

Une fois les modèles de ListView configurés, visitez le site Web. Comme le montre la figure 9, la section leçons contient un seul élément à puces, page d’hébergement. Où se trouvent les leçons sur et à l’aide de plusieurs contrôles ContentPlaceHolder ? SiteMapDataSource est conçu pour retourner un ensemble hiérarchique de données, mais le contrôle ListView ne peut afficher qu’un seul niveau de la hiérarchie. Par conséquent, seul le premier niveau des nœuds de plan de site retournés par SiteMapDataSource est affiché.

[![la section leçons contient un seul élément de liste](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Figure 09**: la section leçons contient un seul élément de liste ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))

Pour afficher plusieurs niveaux, nous pourrions imbriquer plusieurs ListViews au sein de la `ItemTemplate`. Cette technique a été examinée dans le [didacticiel *pages maîtres et navigation* ](../../data-access/introduction/master-pages-and-site-navigation-vb.md) dans le site consacré [à la série de didacticiels](../../data-access/index.md)sur l’utilisation des données. Toutefois, pour cette série de didacticiels, notre plan de site contient seulement deux niveaux : la page d’hébergement (le plus haut niveau); et chaque leçon en tant qu’enfant de la famille. Au lieu d’élaborer un ListView imbriqué, nous pouvons demander à SiteMapDataSource de ne pas retourner le nœud de départ en définissant sa [propriété`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) sur `False`. L’effet net est que la SiteMapDataSource commence par retourner le deuxième niveau de nœuds de plan de site.

Avec cette modification, ListView affiche des puces pour les leçons sur et à l’aide de plusieurs contrôles ContentPlaceHolder, mais omet un élément de puce pour la page d’hébergement. Pour y remédier, nous pouvons ajouter explicitement un élément à puce pour la page d’hébergement dans le `LayoutTemplate`:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Si vous configurez SiteMapDataSource pour omettre le nœud de départ et ajouter explicitement un élément à puce personnelle, la section leçons affiche maintenant la sortie prévue.

[![la section leçons contient un élément à puce pour la page d’hébergement et chaque nœud enfant](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Figure 10**: la section leçons contient un élément à puce pour la page d’hébergement et chaque nœud enfant ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))

### <a name="setting-the-title-based-on-the-site-map"></a>Définition du titre en fonction du plan du site

Une fois le plan de site en place, nous pouvons mettre à jour notre classe de `BasePage` pour utiliser le titre spécifié dans le plan du site. Comme nous l’avons fait à l’étape 2, nous souhaitons uniquement utiliser le titre du nœud de plan de site si le titre de la page n’a pas été explicitement défini par le développeur de pages. Si la page demandée n’a pas de titre de page explicitement défini et qu’elle est introuvable dans le plan de site, nous revenons à l’utilisation du nom de fichier de la page demandée (moins l’extension), comme nous l’avons fait à l’étape 2. La figure 11 illustre ce processus de décision.

![En l’absence d’un titre de page défini explicitement, le titre du nœud de plan de site correspondant est utilisé](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Figure 11**: en l’absence d’un titre de page défini explicitement, le titre du nœud de plan de site correspondant est utilisé

Mettez à jour la méthode `OnLoadComplete` de la classe `BasePage` pour inclure le code suivant :

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Comme précédemment, la méthode `OnLoadComplete` commence par déterminer si le titre de la page a été défini explicitement. Si `Page.Title` est `Nothing`, une chaîne vide ou si la valeur « page sans titre » lui est assignée, le code assigne automatiquement une valeur à `Page.Title`.

Pour déterminer le titre à utiliser, le code commence par référencer la [propriété`CurrentNode`](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx)de la [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx). `CurrentNode` retourne l’instance de [`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) dans le plan de site qui correspond à la page actuellement demandée. En supposant que la page actuellement demandée se trouve dans le plan du site, la propriété `Title` de l' `SiteMapNode`est assignée au titre de la page. Si la page actuellement demandée ne se trouve pas dans le plan du site, `CurrentNode` retourne `Nothing` et le nom de fichier de la page demandée est utilisé comme titre (comme cela a été fait à l’étape 2).

La figure 12 illustre la page de `MultipleContentPlaceHolders.aspx` lorsque vous l’affichez dans un navigateur. Étant donné que le titre de cette page n’est pas défini explicitement, le titre du nœud de plan de site correspondant est utilisé à la place.

![Le titre de la page MultipleContentPlaceHolders. aspx est extrait du plan du site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Figure 12**: le titre de la page MultipleContentPlaceHolders. aspx est extrait du plan du site

## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Étape 4 : ajout d’un autre balisage spécifique à la page à la section`<head>`

Les étapes 1, 2 et 3 ont examiné la personnalisation de l’élément `<title>` page par page. En plus de `<title>`, la section `<head>` peut contenir des éléments `<meta>` et des éléments de `<link>`. Comme indiqué précédemment dans ce didacticiel, la section `<head>` de `Site.master`comprend un élément `<link>` à `Styles.css`. Étant donné que cet élément `<link>` est défini dans la page maître, il est inclus dans la section `<head>` pour toutes les pages de contenu. Mais comment faire pour ajouter des `<meta>` et `<link>` des éléments page par page ?

Le moyen le plus simple d’ajouter du contenu spécifique à la page à la section `<head>` consiste à créer un contrôle ContentPlaceHolder dans la page maître. Nous avons déjà un tel ContentPlaceHolder (nommé `head`). Par conséquent, pour ajouter des balises `<head>` personnalisées, créez un contrôle de contenu correspondant dans la page et placez-y le balisage.

Pour illustrer l’ajout de balises `<head>` personnalisées à une page, nous allons inclure un élément de description `<meta>` à notre ensemble de pages de contenu actuel. L’élément Description de `<meta>` fournit une brève description de la page Web. la plupart des moteurs de recherche incorporent ces informations sous une forme quelconque lors de l’affichage des résultats de la recherche.

Un élément de description `<meta>` se présente sous la forme suivante :

[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Pour ajouter cette balise à une page de contenu, ajoutez le texte ci-dessus au contrôle de contenu mappé au `head` ContentPlaceHolder de la page maître. Par exemple, pour définir un élément de description `<meta>` pour `Default.aspx`, ajoutez le balisage suivant :

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Étant donné que le `head` ContentPlaceHolder ne se trouve pas dans le corps de la page HTML, le balisage ajouté au contrôle de contenu n’est pas affiché dans le Mode Création. Pour afficher l’élément de description `<meta>`, accédez `Default.aspx` via un navigateur. Une fois la page chargée, affichez la source et notez que la section `<head>` inclut le balisage spécifié dans le contrôle de contenu.

Prenez un moment pour ajouter des éléments de description `<meta>` à `About.aspx`, `MultipleContentPlaceHolders.aspx`et `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Ajout par programmation du balisage à la région`<head>`

Le `head` ContentPlaceHolder nous permet d’ajouter de façon déclarative un balisage personnalisé à la région `<head>` de la page maître. Un balisage personnalisé peut également être ajouté par programme. N’oubliez pas que la propriété `Header` de la classe `Page` retourne l’instance `HtmlHead` définie dans la page maître (le `<head runat="server">`).

La possibilité d’ajouter du contenu à la zone de `<head>` par programmation est utile lorsque le contenu à ajouter est dynamique. Il est peut-être basé sur l’utilisateur visitant la page. elle est peut-être extraite d’une base de données. Quelle que soit la raison, vous pouvez ajouter du contenu à la `HtmlHead` en ajoutant des contrôles à sa collection `Controls` comme suit :

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Le code ci-dessus ajoute l’élément `<meta>` Keywords à la région `<head>`, qui fournit une liste délimitée par des virgules de mots clés qui décrivent la page. Notez que pour ajouter une balise `<meta>` vous créez une [`HtmlMeta`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) instance, définissez ses propriétés `Name` et `Content`, puis ajoutez-la à la collection `Header`de `Controls`. De même, pour ajouter par programmation un élément `<link>`, créez un objet [`HtmlLink`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) , définissez ses propriétés, puis ajoutez-le à la collection `Controls` de `Header`.

> [!NOTE]
> Pour ajouter des balises arbitraires, créez une instance [`LiteralControl`](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) , définissez sa propriété `Text`, puis ajoutez-la à la collection `Controls` de `Header`.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons étudié plusieurs façons d’ajouter `<head>` balise de région page par page. Une page maître doit inclure une instance de `HtmlHead` (`<head runat="server">`) avec ContentPlaceHolder. L’instance `HtmlHead` permet aux pages de contenu d’accéder par programme à la région `<head>` et de définir de façon déclarative et par programmation le titre de la page ; le contrôle ContentPlaceHolder permet d’ajouter le balisage personnalisé à la section `<head>` de façon déclarative via un contrôle de contenu.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Définition dynamique du titre de la page dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Examen d’ASP. Navigation sur le site NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Comment utiliser des balises méta HTML](http://searchenginewatch.com/showPage.html?page=2167931)
- [Pages maîtres dans ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Utilisation des contrôles ListView et DataPager de ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Utilisation d’une classe de base personnalisée pour les classes code-behind de vos pages ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Zack Jones et Banerjee. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](multiple-contentplaceholders-and-default-content-vb.md)
> [Suivant](urls-in-master-pages-vb.md)
