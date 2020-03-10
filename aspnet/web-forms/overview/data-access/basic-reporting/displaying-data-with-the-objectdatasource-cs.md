---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: Affichage des données avec ObjectDataSource (C#) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel examine le contrôle ObjectDataSource à l’aide de ce contrôle. vous pouvez lier les données extraites de la couche BLL créée dans le didacticiel précédent sans Havi...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 9419504ace15b39c35a034dda22f2700ee720157
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577475"
---
# <a name="displaying-data-with-the-objectdatasource-c"></a>Affichage de données avec ObjectDataSource (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe) ou [Télécharger le PDF](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> Ce didacticiel examine le contrôle ObjectDataSource à l’aide de ce contrôle. vous pouvez lier des données récupérées à partir de la couche BLL créée dans le didacticiel précédent sans avoir à écrire une ligne de code.

## <a name="introduction"></a>Introduction

Avec l’architecture de l’application et la mise en page de site Web terminées, nous sommes prêts à commencer à découvrir comment effectuer diverses tâches courantes liées aux données et aux rapports. Dans les didacticiels précédents, nous avons vu comment lier par programmation les données de la couche DAL et de la couche BLL à un contrôle Web de données dans une page ASP.NET. Cette syntaxe assignant la propriété `DataSource` du contrôle Web de données aux données à afficher, puis à appeler la méthode `DataBind()` du contrôle était le modèle utilisé dans les applications ASP.NET 1. x, et peut continuer à être utilisé dans vos applications 2,0. Toutefois, les nouveaux contrôles de source de données de ASP.NET 2.0 offrent un moyen déclaratif de travailler avec des données. À l’aide de ces contrôles, vous pouvez lier des données récupérées à partir de la couche BLL créée dans le [didacticiel précédent](../introduction/creating-a-business-logic-layer-cs.md) sans avoir à écrire une ligne de code.

ASP.NET 2,0 est fourni avec cinq contrôles de source de données intégrés [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx)et [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx) , bien que vous puissiez créer vos propres contrôles de [source de données personnalisés](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), si nécessaire. Étant donné que nous avons développé une architecture pour notre application de didacticiel, nous utiliserons l’ObjectDataSource sur nos classes BLL.

![ASP.NET 2,0 comprend cinq contrôles de source de données intégrés.](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**Figure 1**: ASP.NET 2,0 comprend cinq contrôles de source de données intégrés

ObjectDataSource sert de proxy pour travailler avec un autre objet. Pour configurer l’ObjectDataSource, nous spécifions cet objet sous-jacent et la façon dont ses méthodes mappent aux méthodes `Select`, `Insert`, `Update`et `Delete` de l’ObjectDataSource. Une fois cet objet sous-jacent spécifié et ses méthodes mappées à l’ObjectDataSource, nous pouvons lier le ObjectDataSource à un contrôle Web de données. ASP.NET est fourni avec de nombreux contrôles Web de données, notamment GridView, DetailsView, RadioButtonList et DropDownList, entre autres. Pendant le cycle de vie de la page, le contrôle Web des données peut avoir besoin d’accéder aux données auxquelles il est lié, ce qu’il effectuera en appelant la méthode de `Select` de l’ObjectDataSource. Si le contrôle Web de données prend en charge l’insertion, la mise à jour ou la suppression, des appels peuvent être effectués sur les méthodes `Insert`, `Update`ou `Delete` de l’ObjectDataSource. Ces appels sont ensuite routés par ObjectDataSource vers les méthodes appropriées de l’objet sous-jacent, comme le montre le diagramme suivant.

[![l’ObjectDataSource sert de proxy](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**Figure 2**: ObjectDataSource sert de proxy ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image4.png))

Si ObjectDataSource peut être utilisé pour appeler des méthodes d’insertion, de mise à jour ou de suppression de données, nous allons simplement nous concentrer sur le retour de données. les prochains didacticiels explorent l’utilisation de ObjectDataSource et des contrôles Web de données qui modifient les données.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Étape 1 : ajout et configuration du contrôle ObjectDataSource

Commencez par ouvrir la page `SimpleDisplay.aspx` dans le dossier `BasicReporting`, basculez vers Mode Création, puis faites glisser un contrôle ObjectDataSource de la boîte à outils vers l’aire de conception de la page. L’ObjectDataSource apparaît sous la forme d’une zone grise sur l’aire de conception, car elle ne produit pas de balisage ; il accède simplement aux données en appelant une méthode à partir d’un objet spécifié. Les données retournées par un ObjectDataSource peuvent être affichées par un contrôle Web de données, tel que GridView, DetailsView, FormView, et ainsi de suite.

> [!NOTE]
> Vous pouvez également ajouter le contrôle Web de données à la page, puis, à partir de sa balise active, choisir l’option &lt;nouvelle&gt; de source de données dans la liste déroulante.

Pour spécifier l’objet sous-jacent de ObjectDataSource et comment les méthodes de cet objet sont mappées à l’ObjectDataSource, cliquez sur le lien configurer la source de données à partir de la balise active de l’ObjectDataSource.

[![cliquez sur le lien configurer la source de données à partir de la balise active](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**Figure 3**: cliquer sur le lien configurer la source de données à partir de la balise active ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image7.png))

L’Assistant Configuration de la source de données s’affiche. Tout d’abord, nous devons spécifier l’objet avec lequel ObjectDataSource doit travailler. Si la case à cocher « Afficher uniquement les composants de données » est activée, la liste déroulante de cet écran répertorie uniquement les objets qui ont été décorés avec l’attribut `DataObject`. Actuellement, notre liste comprend les TableAdapters dans le DataSet typé et les classes BLL que nous avons créées dans le didacticiel précédent. Si vous avez oublié d’ajouter l’attribut `DataObject` aux classes de la couche de logique métier, vous ne les verrez pas dans cette liste. Dans ce cas, décochez la case « Afficher uniquement les composants de données » pour afficher tous les objets, qui doivent inclure les classes BLL (ainsi que les autres classes du DataSet typé, DataTables, DataRow, etc.).

Dans ce premier écran, choisissez la classe `ProductsBLL` dans la liste déroulante, puis cliquez sur suivant.

[![spécifier l’objet à utiliser avec le contrôle ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**Figure 4**: spécifier l’objet à utiliser avec le contrôle ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image10.png))

L’écran suivant de l’Assistant vous invite à sélectionner la méthode que l’ObjectDataSource doit appeler. La liste déroulante répertorie les méthodes qui retournent des données dans l’objet sélectionné à partir de l’écran précédent. Nous voyons ici `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`et `GetProductsBySupplierID`. Sélectionnez la méthode `GetProducts` dans la liste déroulante et cliquez sur Terminer (si vous avez ajouté la `DataObjectMethodAttribute` aux méthodes de l' `ProductBLL`comme indiqué dans le didacticiel précédent, cette option sera sélectionnée par défaut).

[![choisir la méthode de renvoi des données à partir de l’onglet sélectionner](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**Figure 5**: choisir la méthode de renvoi des données à partir de l’onglet sélectionner ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image13.png))

## <a name="configure-the-objectdatasource-manually"></a>Configurer l’ObjectDataSource manuellement

L’Assistant Configuration de la source de données de ObjectDataSource offre un moyen rapide de spécifier l’objet qu’il utilise et d’associer les méthodes de l’objet qui sont appelées. Toutefois, vous pouvez configurer ObjectDataSource par le biais de ses propriétés, soit par le Fenêtre Propriétés soit directement dans le balisage déclaratif. Il vous suffit de définir la propriété `TypeName` sur le type de l’objet sous-jacent à utiliser, et la `SelectMethod` à la méthode à appeler lors de la récupération de données.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

Même si vous préférez l’Assistant Configuration de la source de données, il peut arriver que vous deviez configurer manuellement l’ObjectDataSource, car l’Assistant ne répertorie que les classes créées par le développeur. Si vous souhaitez lier l’ObjectDataSource à une classe de la .NET Framework telle que la [classe d’appartenance](https://msdn.microsoft.com/library/system.web.security.membership.aspx), pour accéder aux informations de compte d’utilisateur ou à la [classe de répertoire](https://msdn.microsoft.com/library/system.io.directory.aspx) pour travailler avec les informations du système de fichiers, vous devez définir manuellement les propriétés de l’ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Étape 2 : ajout d’un contrôle Web de données et liaison de celui-ci à l’ObjectDataSource

Une fois que ObjectDataSource a été ajouté à la page et configuré, nous sommes prêts à ajouter des contrôles Web de données à la page pour afficher les données retournées par la méthode `Select` de l’ObjectDataSource. Tout contrôle Web de données peut être lié à un ObjectDataSource ; Examinons l’affichage des données de l’ObjectDataSource dans un GridView, DetailsView et FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Liaison d’un GridView à ObjectDataSource

Ajoutez un contrôle GridView de la boîte à outils à l’aire de conception de `SimpleDisplay.aspx`. À partir de la balise active de GridView, choisissez le contrôle ObjectDataSource que nous avons ajouté à l’étape 1. Cela créera automatiquement un BoundField dans le GridView pour chaque propriété retournée par les données à partir de la méthode de `Select` de l’ObjectDataSource (c’est-à-dire, les propriétés définies par le DataTable Products).

[![un GridView a été ajouté à la page et lié au ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**Figure 6**: un GridView a été ajouté à la page et lié à ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image16.png))

Vous pouvez ensuite personnaliser, réorganiser ou supprimer le BoundFields du contrôle GridView en cliquant sur l’option modifier les colonnes de la balise active.

[![gérer le BoundFields du contrôle GridView via la boîte de dialogue Modifier les colonnes](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**Figure 7**: gérer les BoundFields du contrôle GridView via la boîte de dialogue Modifier les colonnes ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image19.png))

Prenez un moment pour modifier le BoundFields du contrôle GridView, en supprimant le `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`et `ReorderLevel` BoundFields. Sélectionnez simplement le BoundField dans la liste en bas à gauche, puis cliquez sur le bouton supprimer (X rouge) pour les supprimer. Ensuite, réorganisez le BoundFields afin que les `CategoryName` et `SupplierName` BoundFields précèdent le `UnitPrice` BoundField en sélectionnant ces BoundFields et en cliquant sur la flèche vers le haut. Définissez les propriétés de `HeaderText` de la BoundFields restante sur `Products`, `Category`, `Supplier`et `Price`, respectivement. Ensuite, utilisez le `Price` BoundField au format monétaire en affectant à la propriété `HtmlEncode` de BoundField la valeur false et à sa propriété `DataFormatString` la valeur `{0:c}`. Enfin, alignez horizontalement le `Price` à droite et la case `Discontinued` dans le centre via la propriété `ItemStyle`/`HorizontalAlign`.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

[![les BoundFields du GridView ont été personnalisés](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**Figure 8**: les BoundFields du GridView ont été personnalisées ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image22.png))

## <a name="using-themes-for-a-consistent-look"></a>Utilisation des thèmes pour une présentation cohérente

Ces didacticiels visent à supprimer tous les paramètres de style au niveau du contrôle, plutôt que d’utiliser des feuilles de style en cascade définies dans un fichier externe chaque fois que cela est possible. Le fichier `Styles.css` contient des classes CSS `DataWebControlStyle`, `HeaderStyle`, `RowStyle`et `AlternatingRowStyle` qui doivent être utilisées pour dicter l’apparence des contrôles Web de données utilisés dans ces didacticiels. Pour ce faire, nous pourrions définir la propriété de `CssClass` de GridView sur `DataWebControlStyle`, et ses propriétés `HeaderStyle`, `RowStyle`et `AlternatingRowStyle` « `CssClass` » en conséquence.

Si nous définissons ces propriétés de `CssClass` au niveau du contrôle Web, nous devons penser à définir explicitement ces valeurs de propriétés pour chaque contrôle Web de données ajouté à nos didacticiels. Une approche plus gérable consiste à définir les propriétés CSS par défaut pour les contrôles GridView, DetailsView et FormView à l’aide d’un thème. Un thème est une collection de paramètres de propriété de niveau contrôle, d’images et de classes CSS qui peuvent être appliquées à des pages sur un site pour appliquer une apparence commune.

Notre thème n’inclura pas d’images ou de fichiers CSS (nous laissons la feuille de style `Styles.css` telle quelle, définie dans le dossier racine de l’application Web), mais inclurez deux apparences. Une apparence est un fichier qui définit les propriétés par défaut d’un contrôle Web. Plus précisément, nous disposons d’un fichier d’apparence pour les contrôles GridView et DetailsView, indiquant les propriétés de `CssClass`par défaut.

Commencez par ajouter un nouveau fichier d’apparence à votre projet nommé `GridView.skin` en cliquant avec le bouton droit sur le nom du projet dans le Explorateur de solutions puis en sélectionnant Ajouter un nouvel élément.

[![ajouter un fichier d’apparence nommé GridView. Skin](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**Figure 9**: ajouter un fichier d’apparence nommé `GridView.skin` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image25.png))

Les fichiers d’apparence doivent être placés dans un thème, qui se trouve dans le dossier `App_Themes`. Étant donné que nous n’avons pas encore de dossier, Visual Studio proposera d’en créer un pour nous lors de l’ajout de notre première peau. Cliquez sur Oui pour créer le dossier `App_Theme` et y placer le nouveau fichier de `GridView.skin`.

[![laisser Visual Studio créer le dossier App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**Figure 10**: laisser Visual Studio créer le dossier `App_Theme` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image28.png))

Cette opération crée un nouveau thème dans le dossier `App_Themes` nommé GridView avec le fichier d’apparence `GridView.skin`.

![Le thème GridView a été ajouté au dossier App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**Figure 11**: le thème GridView a été ajouté au dossier `App_Theme`

Renommez le thème GridView en DataWebControls (cliquez avec le bouton droit sur le dossier GridView dans le dossier `App_Theme` et choisissez Renommer). Ensuite, entrez le balisage suivant dans le fichier `GridView.skin` :

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

Cela définit les propriétés par défaut des propriétés relatives au `CssClass`pour n’importe quel GridView dans une page qui utilise le thème DataWebControls. Nous allons ajouter une autre apparence pour le contrôle DetailsView, un contrôle Web de données que nous utiliserons prochainement. Ajoutez une nouvelle apparence au thème DataWebControls nommé `DetailsView.skin` et ajoutez le balisage suivant :

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

Une fois notre thème défini, la dernière étape consiste à appliquer le thème à notre page ASP.NET. Un thème peut être appliqué page par page ou pour toutes les pages d’un site Web. Utilisons ce thème pour toutes les pages du site Web. Pour ce faire, ajoutez le balisage suivant à la section `<system.web>` de `Web.config`:

[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

C’est aussi simple que cela ! Le paramètre `styleSheetTheme` indique que les propriétés spécifiées dans le thème ne doivent *pas* substituer les propriétés spécifiées au niveau du contrôle. Pour spécifier que les paramètres de thème doivent utiliser des paramètres de contrôle, utilisez l’attribut `theme` à la place de `styleSheetTheme`; Malheureusement, les paramètres de thème spécifiés via l’attribut `theme` n’apparaissent pas dans le Mode Création Visual Studio. Pour plus d’informations sur les thèmes et les apparences, consultez [vue d’ensemble des thèmes et des apparences ASP.net](https://msdn.microsoft.com/library/ykzx33wh.aspx) et [styles côté serveur à l’aide de thèmes](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) . Pour plus d’informations sur la configuration d’une page pour utiliser un thème, consultez [Comment : appliquer des thèmes ASP.net](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx) .

[![le contrôle GridView affiche le nom, la catégorie, le fournisseur, le prix et les informations abandonnées du produit](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**Figure 12**: le contrôle GridView affiche le nom, la catégorie, le fournisseur, le prix et les informations abandonnées du produit ([cliquez pour afficher l’image en plein écran](displaying-data-with-the-objectdatasource-cs/_static/image32.png))

## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Affichage d’un enregistrement à la fois dans le DetailsView

Le contrôle GridView affiche une ligne pour chaque enregistrement retourné par le contrôle de source de données auquel il est lié. Toutefois, il peut arriver que vous souhaitiez afficher un seul enregistrement ou un seul enregistrement à la fois. Le [contrôle DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) offre cette fonctionnalité, rendu sous forme de `<table>` HTML avec deux colonnes et une ligne pour chaque colonne ou propriété liée au contrôle. Vous pouvez considérer le DetailsView comme un GridView avec un enregistrement unique pivoté de 90 degrés.

Commencez par ajouter un contrôle DetailsView *au-dessus* du GridView dans `SimpleDisplay.aspx`. Ensuite, liez-le au même contrôle ObjectDataSource que le contrôle GridView. Comme avec le GridView, un BoundField est ajouté au DetailsView pour chaque propriété de l’objet retourné par la méthode de `Select` de l’ObjectDataSource. La seule différence est que les BoundFields du contrôle DetailsView sont disposés horizontalement plutôt que verticalement.

[![ajoutez un DetailsView à la page et liez-le à l’ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**Figure 13**: ajouter un contrôle DetailsView à la page et le lier à l’ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image35.png))

À l’instar de GridView, le BoundFields du contrôle DetailsView peut être modifié pour fournir un affichage plus personnalisé des données retournées par l’ObjectDataSource. La figure 14 illustre le DetailsView après que ses propriétés BoundFields et `CssClass` ont été configurées pour donner à son apparence un aspect similaire à l’exemple GridView.

[![le contrôle DetailsView affiche un enregistrement unique](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**Figure 14**: le contrôle DetailsView affiche un enregistrement unique ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image38.png))

Notez que le contrôle DetailsView affiche uniquement le premier enregistrement retourné par sa source de données. Pour permettre à l’utilisateur d’effectuer un pas à pas détaillé de tous les enregistrements, l’un après l’autre, nous devons activer la pagination pour le contrôle DetailsView. Pour ce faire, revenez à Visual Studio et cochez la case Activer la pagination dans la balise active du contrôle DetailsView.

[![activer la pagination dans le contrôle DetailsView](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**Figure 15**: activer la pagination dans le contrôle DetailsView ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image41.png))

[![avec la pagination activée, le contrôle DetailsView permet à l’utilisateur d’afficher l’un des produits](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**Figure 16**: avec la pagination activée, le contrôle DetailsView permet à l’utilisateur d’afficher n’importe quel produit ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image44.png))

Nous aborderons la pagination dans les prochains didacticiels.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Disposition plus flexible pour l’affichage d’un enregistrement à la fois

Le contrôle DetailsView est assez rigide dans la manière dont il affiche chaque enregistrement retourné à partir de l’ObjectDataSource. Nous pouvons être amenés à obtenir une vue plus flexible des données. Par exemple, au lieu d’afficher le nom du produit, la catégorie, le fournisseur, le prix et les informations supprimées chacun sur une ligne distincte, nous pouvons afficher le nom et le prix du produit dans un titre de `<h4>`, avec les informations de catégorie et de fournisseur apparaissant sous le nom et le prix dans une plus petite taille de police. Et nous ne pouvons pas afficher les noms de propriété (produit, catégorie, etc.) en regard des valeurs.

Le [contrôle FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) fournit ce niveau de personnalisation. Plutôt que d’utiliser des champs (comme GridView et DetailsView), le contrôle FormView utilise des modèles, qui permettent de combiner des contrôles Web, du code HTML statique et de la [syntaxe DataBinding](http://www.15seconds.com/issue/040630.htm). Si vous êtes familiarisé avec le contrôle Repeater de ASP.NET 1. x, vous pouvez considérer le FormView comme le répéteur pour l’indication d’un seul enregistrement.

Ajoutez un contrôle FormView à l’aire de conception de la page `SimpleDisplay.aspx`. Au départ, le contrôle FormView s’affiche sous la forme d’un bloc gris, en nous informant que nous devons fournir au minimum le `ItemTemplate`du contrôle.

[![le FormView doit inclure un ItemTemplate](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**Figure 17**: le FormView doit inclure une `ItemTemplate` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image47.png))

Vous pouvez lier le contrôle FormView directement à un contrôle de source de données par le biais de la balise active du FormView, ce qui crée automatiquement un `ItemTemplate` par défaut (avec un `EditItemTemplate` et un `InsertItemTemplate`, si les propriétés `InsertMethod` et `UpdateMethod` du contrôle ObjectDataSource sont définies). Toutefois, pour cet exemple, nous allons lier les données au FormView et spécifier leur `ItemTemplate` manuellement. Commencez par définir la propriété `DataSourceID` du FormView sur la `ID` du contrôle ObjectDataSource, `ObjectDataSource1`. Ensuite, créez le `ItemTemplate` afin qu’il affiche le nom et le prix du produit dans un élément `<h4>` et les noms de catégorie et d’expéditeur sous celui-ci dans une taille de police plus petite.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]

[![le premier produit (Chai) est affiché dans un format personnalisé](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**Figure 18**: le premier produit (Chai) est affiché dans un format personnalisé ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image50.png))

La `<%# Eval(propertyName) %>` est la syntaxe DataBinding. La méthode `Eval` retourne la valeur de la propriété spécifiée pour l’objet actuel qui est lié au contrôle FormView. Pour plus d’informations sur les coulisses de la liaison de données, consultez l’article relatif à la [2,0 syntaxe de liaison de données simplifiée et étendue](http://www.15seconds.com/issue/040630.htm) de l’article d’Alex.

À l’instar du contrôle DetailsView, le contrôle FormView affiche uniquement le premier enregistrement retourné à partir de l’ObjectDataSource. Vous pouvez activer la pagination dans le FormView pour permettre aux visiteurs de parcourir les produits un par un.

## <a name="summary"></a>Récapitulatif

L’accès et l’affichage des données à partir d’une couche de logique métier peuvent être réalisés sans écrire une ligne de code grâce au contrôle ObjectDataSource de ASP.NET 2.0. ObjectDataSource appelle une méthode spécifiée d’une classe et retourne les résultats. Ces résultats peuvent être affichés dans un contrôle Web de données lié à l’ObjectDataSource. Dans ce didacticiel, nous avons examiné la liaison des contrôles GridView, DetailsView et FormView à ObjectDataSource.

Jusqu’à présent, nous avons vu comment utiliser ObjectDataSource pour appeler une méthode sans paramètre, mais que se passe-t-il si nous voulons appeler une méthode qui attend des paramètres d’entrée, tels que la `GetProductsByCategoryID(categoryID)`de la classe `ProductBLL` ? Afin d’appeler une méthode qui attend un ou plusieurs paramètres, nous devons configurer l’ObjectDataSource pour spécifier les valeurs de ces paramètres. Nous verrons comment effectuer cette procédure dans le [prochain didacticiel](declarative-parameters-cs.md).

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Créer vos propres contrôles de source de données](https://msdn.microsoft.com/library/ms364049.aspx)
- [Exemples GridView pour ASP.NET 2,0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Syntaxe de liaison de données simplifiée et étendue dans ASP.NET 2,0](http://www.15seconds.com/issue/040630.htm)
- [Thèmes dans ASP.NET 2,0](http://www.odetocode.com/Articles/423.aspx)
- [Styles côté serveur utilisant des thèmes](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Comment : appliquer des thèmes ASP.NET par programmation](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](declarative-parameters-cs.md)
