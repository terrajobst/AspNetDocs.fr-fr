---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: Affichage de données binaires dans les contrôles WebC#de données () | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous examinons les options permettant de présenter des données binaires sur une page Web, y compris l’affichage d’un fichier image et la fourniture d’un lien’Télécharger'...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: f38de7adcd77b3dc2622759646168cf533b8308f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74642564"
---
# <a name="displaying-binary-data-in-the-data-web-controls-c"></a>Affichage de données binaires dans les contrôles web de données (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) ou [Télécharger le PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> Dans ce didacticiel, nous examinons les options permettant de présenter des données binaires sur une page Web, notamment l’affichage d’un fichier image et la fourniture d’un lien « Télécharger » pour un fichier PDF.

## <a name="introduction"></a>Introduction

Dans le didacticiel précédent, nous avons exploré les deux techniques permettant d’associer des données binaires à un modèle de données sous-jacent des applications et utilisé le contrôle FileUpload pour charger des fichiers à partir d’un navigateur vers le système de fichiers du serveur Web s. Nous avons encore vu comment associer les données binaires téléchargées au modèle de données. Autrement dit, une fois qu’un fichier a été téléchargé et enregistré dans le système de fichiers, un chemin d’accès au fichier doit être stocké dans l’enregistrement de base de données approprié. Si les données sont stockées directement dans la base de données, les données binaires téléchargées ne doivent pas être enregistrées dans le système de fichiers, mais doivent être injectées dans la base de données.

Avant d’envisager d’associer les données au modèle de données, voyons d’abord comment fournir les données binaires à l’utilisateur final. La présentation des données texte est assez simple, mais comment les données binaires doivent-elles être présentées ? Il dépend, bien sûr, du type de données binaires. Pour les images, nous souhaitons probablement afficher l’image. pour les fichiers PDF, les documents Microsoft Word, les fichiers ZIP et d’autres types de données binaires, la fourniture d’un lien de téléchargement est probablement plus appropriée.

Dans ce didacticiel, nous allons examiner comment présenter les données binaires en même temps que les données texte associées à l’aide de contrôles Web de données tels que GridView et DetailsView. Dans le didacticiel suivant, nous allons attirer l’attention sur l’Association d’un fichier chargé à la base de données.

## <a name="step-1-providingbrochurepathvalues"></a>Étape 1 : fournir des valeurs de`BrochurePath`

La `Picture` colonne de la table `Categories` contient déjà des données binaires pour les différentes images de catégorie. Plus précisément, la `Picture` colonne pour chaque enregistrement contient le contenu binaire d’une image bitmap de couleur de grain, de faible qualité et de plus de 16 couleurs. Chaque image de catégorie a une largeur de 172 pixels et une hauteur de 120 pixels et consomme environ 11 Ko. De plus, le contenu binaire de la colonne `Picture` comprend un en-tête [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) de 78 octets qui doit être supprimé avant d’afficher l’image. Ces informations d’en-tête sont présentes, car la base de données Northwind a ses racines dans Microsoft Access. Dans Access, les données binaires sont stockées à l’aide du type de données objet OLE, qui s’oriente sur cet en-tête. Pour le moment, nous verrons comment supprimer les en-têtes de ces images de qualité inférieure afin d’afficher l’image. Dans un prochain didacticiel, nous allons créer une interface pour mettre à jour une catégorie s `Picture` colonne et remplacer ces images bitmap qui utilisent des en-têtes OLE avec des images JPG équivalentes sans en-têtes OLE inutiles.

Dans le didacticiel précédent, nous avons vu comment utiliser le contrôle FileUpload. Par conséquent, vous pouvez continuer et ajouter des fichiers de brochure au système de fichiers du serveur Web s. Toutefois, cela ne met pas à jour la colonne `BrochurePath` dans la table `Categories`. Dans le didacticiel suivant, nous allons voir comment y parvenir, mais pour le moment, nous devons fournir manuellement les valeurs de cette colonne.

Dans ce didacticiel, vous allez télécharger sept fichiers de brochure PDF dans le dossier `~/Brochures`, un pour chacune des catégories, à l’exception des fruits de mer. J’ai volontairement omis d’ajouter une brochure de produits de la mer pour illustrer comment gérer les scénarios où tous les enregistrements n’ont pas de données binaires associées. Pour mettre à jour la table `Categories` avec ces valeurs, cliquez avec le bouton droit sur le nœud `Categories` à partir de Explorateur de serveurs, puis choisissez afficher les données de la table. Ensuite, entrez les chemins d’accès virtuels aux fichiers de brochure pour chaque catégorie qui a une brochure, comme le montre la figure 1. Étant donné qu’il n’y a aucune brochure pour la catégorie de produits de la mer, conservez la valeur de `BrochurePath` colonne s comme `NULL`.

[![entrer manuellement les valeurs de la colonne BrochurePath de la table des catégories](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Figure 1**: entrer manuellement les valeurs de la colonne `Categories` de la Table s `BrochurePath` ([cliquez pour afficher l’image en taille réelle](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))

## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Étape 2 : fournir un lien de téléchargement pour les brochures dans un GridView

Avec les valeurs `BrochurePath` fournies pour la table `Categories`, nous sommes prêts à créer un GridView qui répertorie chaque catégorie avec un lien pour télécharger la brochure catégorie s. À l’étape 4, nous allons étendre ce GridView pour afficher également l’image de la catégorie s.

Commencez par faire glisser un contrôle GridView de la boîte à outils vers le concepteur de la page `DisplayOrDownloadData.aspx` dans le dossier `BinaryData`. Définissez le `ID` GridView s sur `Categories` et par le biais de la balise active GridView s, puis choisissez de le lier à une nouvelle source de données. Plus précisément, liez-le à un ObjectDataSource nommé `CategoriesDataSource` qui récupère les données à l’aide de la méthode `GetCategories()` `CategoriesBLL` objet.

[![créer un ObjectDataSource nommé CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Figure 2**: créer un ObjectDataSource nommé `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))

[![configurer ObjectDataSource pour utiliser la classe CategoriesBLL](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Figure 3**: configurer ObjectDataSource pour utiliser la classe `CategoriesBLL` ([cliquez pour afficher l’image en taille réelle](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))

[![récupérer la liste des catégories à l’aide de la méthode GetCategories ()](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Figure 4**: récupérer la liste des catégories à l’aide de la méthode `GetCategories()` ([cliquez pour afficher l’image en taille réelle](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, Visual Studio ajoute automatiquement un BoundField au `Categories` GridView pour les `NumberOfProducts`s `CategoryID`, `CategoryName`, `Description`, `BrochurePath` et `DataColumn`. Continuez et supprimez le `NumberOfProducts` BoundField puisque la requête de la méthode `GetCategories()` ne récupère pas ces informations. Supprimez également le `CategoryID` BoundField et renommez les propriétés `CategoryName` et `BrochurePath` BoundFields `HeaderText` en Category et brochure, respectivement. Une fois ces modifications effectuées, vos balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Affichez cette page dans un navigateur (voir figure 5). Chacune des huit catégories est indiquée. Les sept catégories avec `BrochurePath` valeurs ont la valeur `BrochurePath` affichée dans le BoundField respectif. Les fruits de mer, qui ont une valeur de `NULL` pour ses `BrochurePath`, affichent une cellule vide.

[![chaque nom, description et valeur BrochurePath de chaque catégorie est indiqué](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Figure 5**: chaque nom de catégorie, Description et `BrochurePath` valeur est listé ([cliquez pour afficher l’image en taille réelle](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))

Au lieu d’afficher le texte de la colonne `BrochurePath`, nous voulons créer un lien vers la brochure. Pour ce faire, supprimez le `BrochurePath` BoundField et remplacez-le par un HyperLinkField. Affectez à la nouvelle propriété HyperLinkField s `HeaderText` la valeur brochure, à sa propriété `Text` la valeur afficher la brochure et à la propriété `DataNavigateUrlFields` la valeur `BrochurePath`.

![Ajouter un HyperLinkField pour BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Figure 6**: ajouter un HyperLinkField pour `BrochurePath`

Cette opération ajoute une colonne de liens au GridView, comme le montre la figure 7. Le fait de cliquer sur un lien Afficher la brochure permet d’afficher le fichier PDF directement dans le navigateur ou de demander à l’utilisateur de télécharger le fichier, selon qu’un lecteur PDF est installé et les paramètres du navigateur.

[![une brochure de catégorie s peut être affichée en cliquant sur le lien Afficher la brochure](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Figure 7**: une brochure catégorie s peut être affichée en cliquant sur le lien Afficher la brochure ([cliquez pour afficher l’image en taille réelle](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))

[![le fichier PDF de la brochure catégorie s s’affiche](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Figure 8**: affichage du fichier PDF de la brochure catégorie s ([cliquez pour afficher l’image en taille réelle](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))

## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Masquage du texte afficher la brochure pour les catégories sans brochure

Comme illustré à la figure 7, le `BrochurePath` HyperLinkField affiche sa valeur de propriété `Text` (afficher la brochure) pour tous les enregistrements, qu’il y ait une valeur non`NULL` pour `BrochurePath`ou non. Bien entendu, si `BrochurePath` est `NULL`, le lien est affiché sous forme de texte uniquement, comme c’est le cas avec la catégorie Food (reportez-vous à la figure 7). Au lieu d’afficher la brochure de l’affichage de texte, il peut être intéressant de faire en sorte que ces catégories n’affichent pas `BrochurePath` de texte de remplacement, comme aucune brochure disponible.

Pour fournir ce comportement, nous devons utiliser un TemplateField dont le contenu est généré via un appel à une méthode de page qui émet la sortie appropriée en fonction de la valeur de `BrochurePath`. Nous avons d’abord abordé cette technique de mise en forme dans le didacticiel [utilisation de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) .

Transformez le HyperLinkField en TemplateField en sélectionnant le `BrochurePath` HyperLinkField, puis en cliquant sur le lien convertir ce champ en TemplateField dans la boîte de dialogue Modifier les colonnes.

![Convertir le HyperLinkField en TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Figure 9**: convertir le HyperLinkField en TemplateField

Cela créera un TemplateField avec un `ItemTemplate` qui contient un contrôle Web HYPERLINK dont la propriété `NavigateUrl` est liée à la valeur `BrochurePath`. Remplacez ce balisage par un appel à la méthode `GenerateBrochureLink`, en passant la valeur de `BrochurePath`:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

Ensuite, créez une méthode `protected` dans la classe code-behind ASP.NET page s nommée `GenerateBrochureLink` qui retourne un `string` et qui accepte un `object` comme paramètre d’entrée.

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Cette méthode détermine si la valeur `object` passée est une base de données `NULL` et, le cas échéant, retourne un message indiquant que la catégorie n’a pas de brochure. Sinon, s’il existe une valeur `BrochurePath`, elle s’affiche dans un lien hypertexte. Notez que si la valeur `BrochurePath` est présente, elle est transmise dans la [méthode`ResolveUrl(url)`](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Cette méthode résout l' *URL*passée, en remplaçant le caractère `~` par le chemin d’accès virtuel approprié. Par exemple, si l’application est enracinée à `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` retourne `/Tutorial55/Brochures/Meat.pdf`.

La figure 10 montre la page après que ces modifications ont été appliquées. Notez que le champ `BrochurePath` des catégories de produits de la mer contient maintenant le texte aucune brochure disponible.

[![le texte aucune brochure disponible ne s’affiche pour ces catégories sans brochure](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Figure 10**: le texte aucune brochure disponible s’affiche pour ces catégories sans brochure ([cliquez pour afficher l’image en plein écran](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))

## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Étape 3 : ajout d’une page Web pour afficher une image de catégorie

Lorsqu’un utilisateur visite une page ASP.NET, il reçoit le code HTML de la page ASP.NET. Le code HTML reçu est simplement du texte et ne contient pas de données binaires. Toutes les données binaires supplémentaires, telles que les images, les fichiers audio, les applications Macromedia Flash, les vidéos Windows Media Player incorporées, etc., existent en tant que ressources distinctes sur le serveur Web. Le code HTML contient des références à ces fichiers, mais il n’inclut pas le contenu réel des fichiers.

Par exemple, en HTML, l’élément `<img>` est utilisé pour référencer une image, avec l’attribut `src` qui pointe vers le fichier image comme suit :

[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Quand un navigateur reçoit ce code HTML, il envoie une autre demande au serveur Web pour récupérer le contenu binaire du fichier image, qu’il affiche ensuite dans le navigateur. Le même concept s’applique à toutes les données binaires. À l’étape 2, la brochure n’a pas été envoyée au navigateur dans le cadre du balisage HTML de la page. Au lieu de cela, le HTML rendu fournissait des liens hypertexte qui, lorsque vous cliquez dessus, ont fait en sorte que le navigateur demande le document PDF directement.

Pour afficher ou autoriser les utilisateurs à télécharger des données binaires qui résident dans la base de données, nous devons créer une page Web distincte qui retourne les données. Pour notre application, il n’y a qu’un seul champ de données binaires stocké directement dans la base de données de l’image de la catégorie. Par conséquent, nous avons besoin d’une page qui, lorsqu’elle est appelée, retourne les données d’image pour une catégorie particulière.

Ajoutez une nouvelle page ASP.NET au dossier `BinaryData` nommé `DisplayCategoryPicture.aspx`. Quand vous procédez ainsi, n’activez pas la case à cocher sélectionner une page maître. Cette page attend une valeur `CategoryID` dans QueryString et retourne les données binaires de cette catégorie `Picture` colonne. Étant donné que cette page retourne des données binaires et rien d’autre, elle n’a besoin d’aucun balisage dans la section HTML. Par conséquent, cliquez sur l’onglet source dans le coin inférieur gauche et supprimez tout le balisage de la page, à l’exception de la directive `<%@ Page %>`. Autrement dit, `DisplayCategoryPicture.aspx` le balisage déclaratif doit se composer d’une seule ligne :

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Si vous voyez l’attribut `MasterPageFile` dans la directive `<%@ Page %>`, supprimez-le.

Dans la classe code-behind de la page, ajoutez le code suivant au gestionnaire d’événements `Page_Load` :

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Ce code commence par lire la valeur QueryString `CategoryID` dans une variable nommée `categoryID`. Ensuite, les données de l’image sont récupérées via un appel à la méthode `CategoriesBLL` classe s `GetCategoryWithBinaryDataByCategoryID(categoryID)`. Ces données sont retournées au client à l’aide de la méthode `Response.BinaryWrite(data)`, mais avant que cette méthode soit appelée, l’en-tête OLE de la valeur de colonne `Picture` s doit être supprimé. Pour ce faire, vous devez créer un tableau `byte` nommé `strippedImageData` qui contiendra précisément 78 caractères inférieurs à ceux de la colonne `Picture`. La [méthode`Array.Copy`](https://msdn.microsoft.com/library/z50k9bft.aspx) est utilisée pour copier les données à partir de `category.Picture` en commençant à la position 78 sur `strippedImageData`.

La propriété `Response.ContentType` spécifie le [type MIME](http://en.wikipedia.org/wiki/MIME) du contenu renvoyé afin que le navigateur sache comment le restituer. Étant donné que la colonne `Categories` table s `Picture` est une image bitmap, le type MIME bitmap est utilisé ici (image/BMP). Si vous omettez le type MIME, la plupart des navigateurs affichent toujours l’image correctement, car ils peuvent déduire le type en fonction du contenu des données binaires du fichier image. Toutefois, il est prudent d’inclure le type MIME lorsque cela est possible. Pour obtenir la liste complète des [types de média MIME](http://www.iana.org/assignments/media-types/), consultez le [site Web de l’autorité de certification Internet Assigned Numbers](http://www.iana.org/) .

Une fois cette page créée, une image particulière des catégories peut être affichée en visitant `DisplayCategoryPicture.aspx?CategoryID=categoryID`. La figure 11 montre l’image des catégories de boissons, que vous pouvez afficher à partir de `DisplayCategoryPicture.aspx?CategoryID=1`.

[![l’image de la catégorie boissons est affichée](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Figure 11**: l’image de la catégorie boissons s s’affiche ([cliquez pour afficher l’image en taille réelle](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))

Si, lors de la visite `DisplayCategoryPicture.aspx?CategoryID=categoryID`, vous obtenez une exception qui empêche le cast d’un objet de type’System. DBNull’en type’System. Byte [] ', il y a deux choses qui peuvent être à l’origine de ce problème. Tout d’abord, la colonne `Categories` table s `Picture` autorise les valeurs `NULL`. La page `DisplayCategoryPicture.aspx`, cependant, suppose qu’il y a une valeur non`NULL` présente. La propriété `Picture` du `CategoriesDataTable` ne peut pas être directement accessible si elle a une valeur de `NULL`. Si vous ne souhaitez pas autoriser `NULL` valeurs pour la colonne `Picture`, vous devez inclure la condition suivante :

[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

Le code ci-dessus suppose que certains fichiers image nommés `NoPictureAvailable.gif` dans le dossier `Images` que vous souhaitez afficher pour ces catégories sans image.

Cette exception peut également se produire si l’instruction `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` méthode s `SELECT` est revenue à la liste des colonnes de la requête principale, ce qui peut se produire si vous utilisez des instructions SQL ad hoc et que vous exécutez à nouveau l’Assistant pour la requête principale du TableAdapter. Vérifiez que `GetCategoryWithBinaryDataByCategoryID` méthode s `SELECT` contient toujours la colonne `Picture`.

> [!NOTE]
> Chaque fois que la `DisplayCategoryPicture.aspx` est visitée, la base de données est accédée et les données d’image des catégories spécifiées sont retournées. Toutefois, si l’image de la catégorie n’a pas changé depuis la dernière consultation de l’utilisateur, l’effort est perdu. Heureusement, le protocole HTTP permet d' *obtenir des inconditionnels*. Avec un accès conditionnel, le client qui effectue la requête HTTP envoie le long d’un [en-tête http`If-Modified-Since`](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) qui fournit la date et l’heure auxquelles le client a récupéré la dernière fois cette ressource à partir du serveur Web. Si le contenu n’a pas été modifié depuis la date spécifiée, le serveur Web peut répondre avec un [code d’État non modifié (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) et renoncer à renvoyer le contenu des ressources demandées. En bref, cette technique évite au serveur Web d’avoir à renvoyer le contenu d’une ressource s’il n’a pas été modifié depuis le dernier accès au client.

Toutefois, pour implémenter ce comportement, vous devez ajouter une colonne `PictureLastModified` à la table `Categories` pour capturer la dernière mise à jour de la colonne `Picture`, ainsi que le code permettant de vérifier l’en-tête `If-Modified-Since`. Pour plus d’informations sur l’en-tête `If-Modified-Since` et le flux de travail d’extraction conditionnel, consultez la page obtenir une requête HTTP [conditionnelle pour les pirates RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) et [un examen plus approfondi sur l’exécution de requêtes http dans une page ASP.net](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Étape 4 : affichage des images de catégorie dans un GridView

Maintenant que nous disposons d’une page Web pour afficher une image particulière des catégories, nous pouvons l’afficher à l’aide du [contrôle image Web](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) ou d’un élément `<img>` html pointant vers `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Les images dont l’URL est déterminée par les données de la base de données peuvent être affichées dans le contrôle GridView ou DetailsView à l’aide de ImageField. ImageField contient des propriétés `DataImageUrlField` et `DataImageUrlFormatString` qui fonctionnent comme les propriétés HyperLinkField s `DataNavigateUrlFields` et `DataNavigateUrlFormatString`.

Nous allons ajouter le `Categories` GridView dans `DisplayOrDownloadData.aspx` en ajoutant un ImageField pour afficher chaque image de catégorie. Ajoutez simplement le ImageField et définissez ses propriétés `DataImageUrlField` et `DataImageUrlFormatString` sur `CategoryID` et `DisplayCategoryPicture.aspx?CategoryID={0}`, respectivement. Cela permet de créer une colonne GridView qui restitue un élément `<img>` dont l’attribut `src` fait référence à `DisplayCategoryPicture.aspx?CategoryID={0}`, où {0} est remplacé par la valeur `CategoryID` de la ligne GridView.

![Ajouter un ImageField au GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Figure 12**: ajouter un ImageField au GridView

Après avoir ajouté le ImageField, la syntaxe déclarative de GridView s doit ressembler à la suivante :

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Prenez un moment pour afficher cette page par le biais d’un navigateur. Notez comment chaque enregistrement comprend désormais une image pour la catégorie.

[![l’image des catégories s’affiche pour chaque ligne](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Figure 13**: l’image des catégories s’affiche pour chaque ligne ([cliquez pour afficher l’image en taille réelle](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons examiné comment présenter des données binaires. Le mode de présentation des données dépend du type de données. Pour les fichiers de la brochure PDF, nous avons proposé à l’utilisateur un lien Afficher la brochure qui, lorsque vous cliquez dessus, a pris l’utilisateur directement dans le fichier PDF. Pour l’image des catégories, nous avons d’abord créé une page pour récupérer et retourner les données binaires de la base de données, puis utilisé cette page pour afficher chaque image de catégorie dans un GridView.

Maintenant que nous avons vu comment afficher des données binaires, nous sommes prêts à examiner comment effectuer des insertions, des mises à jour et des suppressions sur la base de données avec les données binaires. Dans le didacticiel suivant, nous allons examiner comment associer un fichier chargé à son enregistrement de base de données correspondant. Dans ce didacticiel, nous verrons comment mettre à jour les données binaires existantes et comment supprimer les données binaires lorsque l’enregistrement qui lui est associé est supprimé.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Teresa Murphy et Dave Gardner. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](uploading-files-cs.md)
> [Suivant](including-a-file-upload-option-when-adding-a-new-record-cs.md)
