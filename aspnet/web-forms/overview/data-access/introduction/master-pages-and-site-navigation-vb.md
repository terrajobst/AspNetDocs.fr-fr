---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: Pages maîtres et navigation dans un site (VB) | Microsoft Docs
author: rick-anderson
description: Une caractéristique courante des sites Web conviviaux est qu’ils disposent d’une disposition de page et d’un schéma de navigation cohérents à l’ensemble du site. Ce didacticiel explique comment y...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4a2b5ba8c1781f1194f951a44661a8f7dd095f41
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74578887"
---
# <a name="master-pages-and-site-navigation-vb"></a>Pages maîtres et navigation dans un site (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe) ou [Télécharger le PDF](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> Une caractéristique courante des sites Web conviviaux est qu’ils disposent d’une disposition de page et d’un schéma de navigation cohérents à l’ensemble du site. Ce didacticiel explique comment vous pouvez créer une apparence cohérente sur toutes les pages qui peuvent être facilement mises à jour.

## <a name="introduction"></a>Introduction

Une caractéristique courante des sites Web conviviaux est qu’ils disposent d’une disposition de page et d’un schéma de navigation cohérents à l’ensemble du site. ASP.NET 2,0 introduit deux nouvelles fonctionnalités qui simplifient la mise en œuvre d’une disposition de page et d’un schéma de navigation à l’ensemble du site : les pages maîtres et la navigation dans les sites. Les pages maîtres permettent aux développeurs de créer un modèle à l’ensemble du site avec des régions modifiables désignées. Ce modèle peut ensuite être appliqué à des pages ASP.NET dans le site. Ces pages ASP.NET doivent uniquement fournir du contenu pour les régions modifiables spécifiées de la page maître. toutes les autres balises de la page maître sont identiques sur toutes les pages ASP.NET qui utilisent la page maître. Ce modèle permet aux développeurs de définir et de centraliser une disposition de page à l’échelle du site, ce qui facilite la création d’une apparence cohérente sur toutes les pages qui peuvent être facilement mises à jour.

Le [système de navigation de site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) fournit un mécanisme permettant aux développeurs de pages de définir un plan de site et une API pour que ce plan de site soit interrogé par programmation. Le nouveau Web de navigation contrôle le menu, TreeView et SiteMapPath, ce qui facilite le rendu de tout ou partie du plan de site dans un élément d’interface utilisateur de navigation commun. Nous allons utiliser le fournisseur de navigation de site par défaut, ce qui signifie que notre plan de site sera défini dans un fichier au format XML.

Pour illustrer ces concepts et rendre notre site Web didacticiels plus utilisable, passons à cette leçon de définition d’une mise en page à l’ensemble du site, de l’implémentation d’un plan de site et de l’ajout de l’interface utilisateur de navigation. À la fin de ce didacticiel, nous disposons d’une conception de site Web soignée pour la création de nos pages Web de didacticiel.

[![le résultat final de ce didacticiel](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**Figure 1**: résultat final de ce didacticiel ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image3.png))

## <a name="step-1-creating-the-master-page"></a>Étape 1 : création de la page maître

La première étape consiste à créer la page maître pour le site. Pour le moment, notre site Web se compose uniquement du DataSet typé (`Northwind.xsd`, dans le dossier `App_Code`), des classes BLL (`ProductsBLL.vb`, `CategoriesBLL.vb`, etc., toutes dans le dossier `App_Code`), de la base de données (`NORTHWND.MDF`, du dossier `App_Data`), du fichier de configuration (`Web.config`) et d’un fichier de feuille de style CSS (`Styles.css`). J’ai nettoyé ces pages et fichiers illustrant l’utilisation de la couche DAL et de la couche BLL des deux premiers didacticiels, car nous allons réexaminer ces exemples plus en détail dans les prochains didacticiels.

![Les fichiers de notre projet](master-pages-and-site-navigation-vb/_static/image4.png)

**Figure 2**: fichiers de notre projet

Pour créer une page maître, cliquez avec le bouton droit sur le nom du projet dans la Explorateur de solutions et choisissez Ajouter un nouvel élément. Sélectionnez ensuite le type de page maître dans la liste des modèles, puis nommez-le `Site.master`.

[![ajouter une nouvelle page maître au site Web](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**Figure 3**: ajouter une nouvelle page maître au site Web ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image7.png))

Définissez la disposition de page à l’ensemble du site dans la page maître. Vous pouvez utiliser la Mode Création et ajouter la disposition ou les contrôles Web dont vous avez besoin, ou vous pouvez ajouter manuellement la balise manuellement en mode Source. Dans ma page maître, j’utilise des [feuilles de style en cascade](http://www.w3schools.com/css/default.asp) pour le positionnement et les styles avec les paramètres CSS définis dans le `Style.css`de fichier externe. Bien que vous ne soyez pas en mesure de déterminer à partir du balisage illustré ci-dessous, les règles CSS sont définies de sorte que le contenu de la `<div>`de navigation est positionné de façon absolue afin qu’il apparaisse à gauche et ait une largeur fixe de 200 pixels.

Site. Master

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

Une page maître définit à la fois la mise en page statique et les régions qui peuvent être modifiées par les pages ASP.NET qui utilisent la page maître. Ces régions modifiables de contenu sont indiquées par le contrôle ContentPlaceHolder, qui peut être affiché dans le `<div>`de contenu. Notre page maître comporte un seul ContentPlaceHolder (`MainContent`), mais la page maître peut avoir plusieurs ContentPlaceHolders.

Avec le balisage entré ci-dessus, le fait de passer au Mode Création affiche la disposition de la page maître. Toutes les pages ASP.NET qui utilisent cette page maître auront cette mise en page uniforme, avec la possibilité de spécifier le balisage pour la région `MainContent`.

[![la page maître, quand elle est consultée en mode conception](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**Figure 4**: page maître, affichée en mode création ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image10.png))

## <a name="step-2-adding-a-homepage-to-the-website"></a>Étape 2 : ajout d’une page d’accueil au site Web

Une fois la page maître définie, nous sommes prêts à ajouter les pages ASP.NET pour le site Web. Commençons par ajouter `Default.aspx`, notre page d’accueil du site Web. Cliquez avec le bouton droit sur le nom du projet dans la Explorateur de solutions et choisissez Ajouter un nouvel élément. Sélectionnez l’option Web Form dans la liste des modèles et nommez le fichier `Default.aspx`. Activez également la case à cocher « sélectionner une page maître ».

[![ajouter un nouveau formulaire Web, activez la case à cocher sélectionner une page maître](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**Figure 5**: ajouter un nouveau formulaire Web, activer la case à cocher sélectionner la page maître ([Cliquer pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image13.png))

Après avoir cliqué sur le bouton OK, nous vous recommandons de choisir la page maître que cette nouvelle page ASP.NET doit utiliser. Bien que vous puissiez avoir plusieurs pages maîtres dans votre projet, nous n’en avons qu’une.

[![choisir la page maître que cette page ASP.NET doit utiliser](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**Figure 6**: choisir la page maître que cette page ASP.net doit utiliser ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image16.png))

Après avoir choisi la page maître, les nouvelles pages ASP.NET contiendront le balisage suivant :

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

Dans la `@Page` directive, il y a une référence au fichier de page maître utilisé (`MasterPageFile="~/Site.master"`), et le balisage de la page ASP.NET contient un contrôle de contenu pour chacun des contrôles ContentPlaceHolder définis dans la page maître, avec le `ContentPlaceHolderID` du contrôle mappant le contrôle de contenu à un ContentPlaceHolder spécifique. Le contrôle de contenu est l’emplacement où vous placez le balisage que vous souhaitez voir apparaître dans le ContentPlaceHolder correspondant. Affectez à l’attribut `Title` de la directive `@Page` la valeur Accueil et ajoutez du contenu d’accueil au contrôle de contenu :

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

L’attribut `Title` de la directive `@Page` nous permet de définir le titre de la page à partir de la page ASP.NET, même si l’élément `<title>` est défini dans la page maître. Nous pouvons également définir le titre par programmation, à l’aide de `Page.Title`. Notez également que les références de la page maître aux feuilles de style (par exemple, `Style.css`) sont automatiquement mises à jour afin qu’elles fonctionnent dans n’importe quelle page ASP.NET, quel que soit le répertoire dans lequel la page ASP.NET est relative à la page maître.

En basculant sur le Mode Création nous voyons comment la page s’affichera dans un navigateur. Notez que, dans le Mode Création de la page ASP.NET, seules les régions modifiables de contenu sont modifiables, le balisage non-ContentPlaceHolder défini dans la page maître est grisé.

[![le mode Design de la page ASP.NET affiche les régions modifiables et non modifiables](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**Figure 7**: la vue de conception de la page ASP.NET affiche les régions modifiables et non modifiables ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image19.png))

Lorsque la page de `Default.aspx` est visitée par un navigateur, le moteur ASP.NET fusionne automatiquement le contenu de la page maître de la page et l’ASP. Contenu du réseau, et restitue le contenu fusionné dans le code HTML final qui est envoyé au navigateur demandeur. Lorsque le contenu de la page maître est mis à jour, le contenu de toutes les pages ASP.NET qui utilisent cette page maître sera refusionné avec le nouveau contenu de la page maître la prochaine fois qu’ils sont demandés. En résumé, le modèle de page maître permet de définir un modèle de mise en page unique (la page maître) dont les modifications sont immédiatement reflétées sur l’ensemble du site.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Ajout de pages ASP.NET supplémentaires au site Web

Prenons un moment pour ajouter des stubs de page ASP.NET supplémentaires sur le site qui contiendra finalement les différentes démonstrations de création de rapports. Il y aura plus de 35 démonstrations au total. par conséquent, au lieu de créer toutes les pages stub, nous allons simplement créer les premiers. Étant donné qu’il y aura également de nombreuses catégories de démonstrations, pour mieux gérer les démonstrations, ajoutez un dossier pour les catégories. Ajoutez les trois dossiers suivants pour le moment :

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Enfin, ajoutez de nouveaux fichiers comme indiqué dans le Explorateur de solutions de la figure 8. Lorsque vous ajoutez chaque fichier, n’oubliez pas de cocher la case « sélectionner une page maître ».

![Ajoutez les fichiers suivants](master-pages-and-site-navigation-vb/_static/image20.png)

**Figure 8**: ajouter les fichiers suivants

## <a name="step-2-creating-a-site-map"></a>Étape 2 : création d’un plan de site

L’un des défis de la gestion d’un site Web composé de plus de quelques pages est de fournir une méthode simple permettant aux visiteurs de naviguer dans le site. Pour commencer, la structure de navigation du site doit être définie. Ensuite, cette structure doit être traduite en éléments d’interface utilisateur navigables, tels que les menus ou les plans de navigation. Enfin, l’ensemble de ce processus doit être maintenu et mis à jour à mesure que de nouvelles pages sont ajoutées au site et supprimées. Avant ASP.NET 2,0, les développeurs étaient autonomes pour la création de la structure de navigation du site, sa gestion et sa traduction en éléments d’interface utilisateur navigables. Avec ASP.NET 2,0, toutefois, les développeurs peuvent utiliser le système de navigation de site le plus flexible.

Le système de navigation de site ASP.NET 2,0 permet à un développeur de définir un plan de site, puis d’accéder à ces informations par le biais d’une API de programmation. ASP.NET est fourni avec un fournisseur de plan de site qui s’attend à ce que les données de plan de site soient stockées dans un fichier XML mis en forme de façon particulière. Toutefois, étant donné que le système de navigation du site repose sur le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , il peut être étendu pour prendre en charge d’autres méthodes de sérialisation des informations de plan de site. L’article de Jeff Prosise, [le fournisseur de plan de site SQL que vous attendiez](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) , montre comment créer un fournisseur de plan de site qui stocke le plan de site dans une base de données SQL Server ; une autre option consiste à créer [un fournisseur de plan de site basé sur la structure du système de fichiers](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Pour ce didacticiel, toutefois, nous allons utiliser le fournisseur de plan de site par défaut fourni avec ASP.NET 2,0. Pour créer le plan de site, cliquez simplement avec le bouton droit sur le nom du projet dans la Explorateur de solutions, choisissez Ajouter un nouvel élément, puis choisissez l’option plan du site. Laissez le nom `Web.sitemap`, puis cliquez sur le bouton Ajouter.

[![ajouter un plan de site à votre projet](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**Figure 9**: ajouter un plan de site à votre projet ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image23.png))

Le fichier de plan de site est un fichier XML. Notez que Visual Studio fournit IntelliSense pour la structure de plan de site. Le fichier de plan de site doit avoir le nœud `<siteMap>` comme nœud racine, qui doit contenir exactement un élément enfant `<siteMapNode>`. Ce premier élément `<siteMapNode>` peut ensuite contenir un nombre arbitraire d’éléments de `<siteMapNode>` descendants.

Définissez le plan du site pour imiter la structure du système de fichiers. Autrement dit, ajoutez un élément `<siteMapNode>` pour chacun des trois dossiers, et les éléments `<siteMapNode>` enfants pour chacune des pages ASP.NET dans ces dossiers, comme suit :

Web. sitemap

[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

Le plan de site définit la structure de navigation du site Web, qui est une hiérarchie qui décrit les différentes sections du site. Chaque élément `<siteMapNode>` dans `Web.sitemap` représente une section dans la structure de navigation du site.

[![le plan de site représente une structure de navigation hiérarchique](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**Figure 10**: le plan de site représente une structure de navigation hiérarchique ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image26.png))

ASP.NET expose la structure du plan de site par le biais de la [classe sitemap](https://msdn.microsoft.com/library/system.web.sitemap.aspx)du .NET Framework. Cette classe a une propriété `CurrentNode`, qui retourne des informations sur la section visitée par l’utilisateur ; la propriété `RootNode` retourne la racine du plan de site (page d’origine, dans notre plan de site). Les propriétés `CurrentNode` et `RootNode` retournent des instances [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) , qui ont des propriétés telles que `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`, etc., qui permettent à la hiérarchie de plan de site d’être parcourue.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Étape 3 : affichage d’un menu basé sur le plan du site

L’accès aux données dans ASP.NET 2,0 peut être effectué par programme, comme dans ASP.NET 1. x, ou de façon déclarative, via les nouveaux [contrôles de source de données](https://msdn.microsoft.com/library/ms227679.aspx). Il existe plusieurs contrôles de source de données intégrés, tels que le contrôle SqlDataSource, pour accéder aux données relationnelles de la base de données, au contrôle ObjectDataSource, à l’accès aux données des classes, etc. Vous pouvez même créer vos propres [contrôles de source de données personnalisés](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Les contrôles de source de données servent de proxy entre votre page ASP.NET et les données sous-jacentes. Pour afficher les données récupérées d’un contrôle de source de données, nous allons généralement ajouter un autre contrôle Web à la page et le lier au contrôle de source de données. Pour lier un contrôle Web à un contrôle de source de données, il vous suffit de définir la propriété `DataSourceID` du contrôle Web sur la valeur de la propriété `ID` du contrôle de source de données.

Pour faciliter l’utilisation des données du plan de site, ASP.NET comprend le contrôle SiteMapDataSource, qui nous permet de lier un contrôle Web au plan du site de notre site Web. Deux contrôles Web l’arborescence et le menu sont couramment utilisés pour fournir une interface utilisateur de navigation. Pour lier les données de plan de site à l’un de ces deux contrôles, ajoutez simplement un SiteMapDataSource à la page avec un contrôle TreeView ou menu dont la propriété `DataSourceID` est définie en conséquence. Par exemple, nous pourrions ajouter un contrôle Menu à la page maître à l’aide du balisage suivant :

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

Pour un niveau de contrôle plus fin du code HTML émis, nous pouvons lier le contrôle SiteMapDataSource au contrôle Repeater, comme suit :

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

Le contrôle SiteMapDataSource retourne la hiérarchie de plan de site un niveau à la fois, en commençant par le nœud de plan de site racine (dans notre plan de site), puis le niveau suivant (création de rapports de base, filtrage des rapports et mise en forme personnalisée), et ainsi de suite. Lors de la liaison de SiteMapDataSource à un Repeater, il énumère le premier niveau retourné et instancie le `ItemTemplate` pour chaque `SiteMapNode` instance de ce premier niveau. Pour accéder à une propriété particulière du `SiteMapNode`, nous pouvons utiliser `Eval(propertyName)`, qui est la façon dont nous obtenons les propriétés `Url` et `Title` de chaque `SiteMapNode`pour le contrôle de lien hypertexte.

L’exemple Repeater ci-dessus affiche le balisage suivant :

[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

Ces nœuds de plan de site (création de rapports de base, filtrage de rapports et mise en forme personnalisée) comportent le *deuxième* niveau du plan de site rendu, et non le premier. Cela est dû au fait que la propriété `ShowStartingNode` de SiteMapDataSource est définie sur false, ce qui amène la SiteMapDataSource à ignorer le nœud de plan de site racine et à commencer par retourner le deuxième niveau dans la hiérarchie de plan de site.

Pour afficher les enfants pour la création de rapports de base, le filtrage des rapports et la mise en forme personnalisée `SiteMapNode` s, nous pouvons ajouter un autre répéteur au `ItemTemplate`du répétiteur initial. Ce deuxième répéteur est lié à la propriété `ChildNodes` de l’instance `SiteMapNode`, comme suit :

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

Ces deux répéteurs entraînent le balisage suivant (certaines balises ont été supprimées par souci de concision) :

[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

À l’aide des styles CSS choisis dans [Rachel Andrew](http://www.rachelandrew.co.uk/), [le document css Anthology : 101, trucs, astuces, &amp; hackers](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), les éléments `<ul>` et `<li>` sont mis en forme de manière à ce que le balisage produise la sortie visuelle suivante :

![Menu composé de deux répéteurs et de certains CSS](master-pages-and-site-navigation-vb/_static/image27.png)

**Figure 11**: un menu composé de deux répéteurs et de certains CSS

Ce menu se trouve dans la page maître et est lié au plan de site défini dans `Web.sitemap`, ce qui signifie que toute modification apportée au plan de site est immédiatement reflétée sur toutes les pages qui utilisent la page maître `Site.master`.

## <a name="disabling-viewstate"></a>Désactivation de ViewState

Tous les contrôles ASP.NET peuvent éventuellement conserver leur état à l' [État d’affichage](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), qui est sérialisé en tant que champ de formulaire masqué dans le HTML rendu. L’état d’affichage est utilisé par les contrôles pour se souvenir de leur état modifié par programme sur les publications (postback), telles que les données liées à un contrôle Web de données. Tandis que l’état d’affichage permet de mémoriser les informations sur les publications, il augmente la taille du balisage qui doit être envoyé au client et peut entraîner une augmentation importante de la page si elle n’est pas surveillée avec précision. Les contrôles Web de données, en particulier le GridView, sont particulièrement connus pour ajouter des dizaines de kilo-octets supplémentaires de balises à une page. Bien qu’une telle augmentation puisse être négligeable pour les utilisateurs distants ou intranet, l’état d’affichage peut ajouter plusieurs secondes à l’aller-retour pour les utilisateurs d’accès à distance.

Pour voir l’impact de l’état d’affichage, visitez une page dans un navigateur, puis affichez la source envoyée par la page Web (dans Internet Explorer, accédez au menu Affichage et choisissez l’option source). Vous pouvez également activer le [suivi de page](https://msdn.microsoft.com/library/sfbfw58f.aspx) pour afficher l’allocation d’état d’affichage utilisée par chacun des contrôles de la page. Les informations d’état d’affichage sont sérialisées dans un champ de formulaire masqué nommé `__VIEWSTATE`, situé dans un élément `<div>` immédiatement après la balise d’ouverture `<form>`. L’état d’affichage est conservé uniquement quand un formulaire Web est utilisé ; Si votre page ASP.NET n’inclut pas de `<form runat="server">` dans sa syntaxe déclarative, aucun champ de formulaire `__VIEWSTATE` masqué n’est présent dans le balisage rendu.

Le champ de formulaire `__VIEWSTATE` généré par la page maître ajoute approximativement 1 800 octets au balisage généré de la page. Cette augmentation supplémentaire est due principalement au contrôle Repeater, car le contenu du contrôle SiteMapDataSource est conservé dans l’état d’affichage. Bien qu’il ne soit pas possible de s’intéresser à un nombre de 1 800 octets supplémentaire, lorsque vous utilisez un GridView avec de nombreux champs et enregistrements, l’état d’affichage peut facilement gonfler d’un facteur de 10 ou plus.

L’état d’affichage peut être désactivé au niveau de la page ou du contrôle en affectant à la propriété `EnableViewState` la valeur `False`, réduisant ainsi la taille du balisage rendu. Étant donné que l’état d’affichage d’un contrôle Web de données rend persistantes les données liées au contrôle Web de données sur les publications (postback), lors de la désactivation de l’état d’affichage pour un contrôle Web de données, les données doivent être liées à chaque publication (postback). Dans ASP.NET version 1. x, cette responsabilité est tombée sur les épaules du développeur de pages. Toutefois, avec ASP.NET 2,0, les contrôles Web de données sont de nouveau liés à leur contrôle de source de données sur chaque publication, si nécessaire.

Pour réduire l’état d’affichage de la page, nous allons définir la propriété `EnableViewState` du contrôle Repeater sur `False`. Pour ce faire, vous pouvez utiliser l’Fenêtre Propriétés dans le concepteur ou de façon déclarative dans la vue source. Une fois cette modification apportée, le balisage déclaratif de Repeater doit ressembler à ce qui suit :

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

Après cette modification, la taille de l’état d’affichage rendu de la page a été réduite à un simple 52 octets, soit une économie de 97% dans la taille de l’état d’affichage ! Dans les didacticiels de cette série, nous allons désactiver par défaut l’état d’affichage des contrôles Web de données afin de réduire la taille du balisage rendu. Dans la majorité des exemples, la propriété `EnableViewState` est définie sur `False` et effectuée sans mentionner. Le seul État d’affichage de l’heure sera abordé dans les scénarios où il doit être activé pour que le contrôle Web de données fournisse ses fonctionnalités attendues.

## <a name="step-4-adding-breadcrumb-navigation"></a>Étape 4 : ajout de navigation dans le chemin d’accès

Pour terminer la page maître, nous allons ajouter un élément d’interface utilisateur de navigation de navigation à chaque page. Le Breadcrumb affiche rapidement les utilisateurs à leur position actuelle dans la hiérarchie de site. L’ajout d’un Breadcrumb dans ASP.NET 2,0 est simple : il suffit d’ajouter un contrôle SiteMapPath à la page ; aucun code n’est nécessaire.

Pour notre site, ajoutez ce contrôle à l’en-tête `<div>`:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

Le Breadcrumb affiche la page actuelle que l’utilisateur visite dans la hiérarchie des plans de site, ainsi que les « ancêtres » du nœud de plan de site, jusqu’à la racine (à l’origine, dans notre plan de site).

![Le Breadcrumb affiche la page actuelle et ses ancêtres dans la hiérarchie des plans de site](master-pages-and-site-navigation-vb/_static/image28.png)

**Figure 12**: le Breadcrumb affiche la page actuelle et ses ancêtres dans la hiérarchie des plans de site

## <a name="step-5-adding-the-default-page-for-each-section"></a>Étape 5 : ajout de la page par défaut pour chaque section

Les didacticiels de notre site sont répartis en différentes catégories : rapports de base, filtrage, mise en forme personnalisée, etc. avec un dossier pour chaque catégorie et les didacticiels correspondants sous forme de pages ASP.NET dans ce dossier. En outre, chaque dossier contient une page `Default.aspx`. Pour cette page par défaut, nous allons afficher tous les didacticiels de la section actuelle. Autrement dit, pour les `Default.aspx` dans le dossier `BasicReporting`, nous avons des liens vers `SimpleDisplay.aspx`, `DeclarativeParams.aspx`et `ProgrammaticParams.aspx`. Là encore, nous pouvons à nouveau utiliser la classe `SiteMap` et un contrôle Web de données pour afficher ces informations en fonction du plan de site défini dans `Web.sitemap`.

Nous allons afficher une liste non triée à l’aide d’un répéteur, mais cette fois, nous affichons le titre et la description des didacticiels. Étant donné que le balisage et le code permettant d’y parvenir doivent être répétés pour chaque page de `Default.aspx`, nous pouvons encapsuler cette logique d’interface utilisateur dans un [contrôle utilisateur](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Créez un dossier dans le site Web appelé `UserControls` et ajoutez-le à un nouvel élément de type contrôle utilisateur Web nommé `SectionLevelTutorialListing.ascx`, puis ajoutez le balisage suivant :

[![ajouter un nouveau contrôle utilisateur Web au dossier UserControls](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**Figure 13**: ajouter un nouveau contrôle utilisateur Web au dossier `UserControls` ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image31.png))

SectionLevelTutorialListing. ascx

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

SectionLevelTutorialListing. ascx. vb

[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

Dans l’exemple précédent Repeater, nous avons lié les données `SiteMap` au répéteur de façon déclarative ; Toutefois, le contrôle utilisateur `SectionLevelTutorialListing`, le fait par programmation. Dans le gestionnaire d’événements `Page_Load`, une vérification est effectuée pour s’assurer que l’URL de cette page correspond à un nœud dans le plan du site. Si ce contrôle utilisateur est utilisé dans une page qui n’a pas d’entrée de `<siteMapNode>` correspondante, `SiteMap.CurrentNode` retourne `Nothing` et aucune donnée n’est liée au répéteur. En supposant que nous avons une `CurrentNode`, nous lions sa collection `ChildNodes` au Repeater. Étant donné que notre plan de site est configuré de telle sorte que la page de `Default.aspx` dans chaque section est le nœud parent de tous les didacticiels de cette section, ce code affiche des liens vers et des descriptions de tous les didacticiels de la section, comme indiqué dans la capture d’écran ci-dessous.

Une fois ce répéteur créé, ouvrez les pages de `Default.aspx` dans chacun des dossiers, accédez au Mode Création et faites simplement glisser le contrôle utilisateur du Explorateur de solutions sur l’aire de conception où vous souhaitez que la liste des didacticiels apparaisse.

[![le contrôle utilisateur a été ajouté à default. aspx](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**Figure 14**: le contrôle utilisateur a été ajouté à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image34.png))

[![les didacticiels de base des rapports sont répertoriés](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**Figure 15**: les didacticiels de base des rapports sont répertoriés ([cliquez pour afficher l’image en taille réelle](master-pages-and-site-navigation-vb/_static/image37.png))

## <a name="summary"></a>Récapitulatif

Une fois le plan de site défini et la page maître terminée, nous disposons désormais d’une mise en page et d’un schéma de navigation cohérents pour nos didacticiels liés aux données. Quel que soit le nombre de pages que nous ajoutons à notre site, la mise à jour des informations relatives à la mise en page à l’ensemble du site ou à la navigation dans le site est un processus simple et rapide en raison de la centralisation de ces informations. Plus précisément, les informations de mise en page sont définies dans la page maître `Site.master` et le plan de site dans `Web.sitemap`. Nous n’avons pas besoin *d’écrire de* code pour atteindre ce mécanisme de mise en page et de navigation à l’ensemble du site, et nous conservons la prise en charge complète du concepteur WYSIWYG dans Visual Studio.

Après avoir terminé la couche d’accès aux données et la couche de logique métier et avoir défini une disposition de page et une navigation de site cohérentes, nous sommes prêts à commencer à explorer les modèles de rapport courants. Dans les trois didacticiels suivants, nous allons examiner les tâches de base de création de rapports qui affichent les données extraites de la couche BLL dans les contrôles GridView, DetailsView et FormView.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Vue d’ensemble des pages maîtres ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Pages maîtres dans ASP.NET 2,0](http://odetocode.com/Articles/419.aspx)
- [Modèles de conception ASP.NET 2,0](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Vue d’ensemble de la navigation de site ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Examen de la navigation de site de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Fonctionnalités de navigation de site ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Fonctionnement de l’état d’affichage de ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Comment : activer le traçage pour une page ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Contrôles utilisateur ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Liz Shulok, Denis Patterson et Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-a-business-logic-layer-vb.md)
