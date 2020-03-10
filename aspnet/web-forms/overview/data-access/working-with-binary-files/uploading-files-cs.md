---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: Téléchargement des fichiers (C#) | Microsoft Docs
author: rick-anderson
description: Découvrez comment permettre aux utilisateurs de charger des fichiers binaires (tels que des documents Word ou PDF) sur votre site Web où ils peuvent être stockés dans le système de fichiers du serveur...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e3e32a829de386a681504c8d5d61dd258b8b2e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548796"
---
# <a name="uploading-files-c"></a>Chargement de fichiers (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe) ou [Télécharger le PDF](uploading-files-cs/_static/datatutorial54cs1.pdf)

> Découvrez comment permettre aux utilisateurs de charger des fichiers binaires (tels que des documents Word ou PDF) sur votre site Web où ils peuvent être stockés dans le système de fichiers du serveur ou dans la base de données.

## <a name="introduction"></a>Introduction

Tous les didacticiels que nous avons examinés jusqu’à présent ont travaillé exclusivement avec des données texte. Toutefois, de nombreuses applications ont des modèles de données qui capturent à la fois des données texte et des données binaires. Un site de mise à jour en ligne peut permettre aux utilisateurs de télécharger une image à associer à leur profil. Un site Web de recrutement peut permettre aux utilisateurs de charger leur CV sous forme de document Microsoft Word ou PDF.

L’utilisation de données binaires ajoute un nouvel ensemble de défis. Nous devons décider de la façon dont les données binaires sont stockées dans l’application. L’interface utilisée pour l’insertion de nouveaux enregistrements doit être mise à jour pour permettre à l’utilisateur de télécharger un fichier à partir de son ordinateur et des étapes supplémentaires doivent être effectuées pour afficher ou fournir un moyen de télécharger les données binaires associées à un enregistrement. Dans ce didacticiel et les trois suivants, nous allons découvrir comment faire face à ces défis. À la fin de ces didacticiels, nous avons créé une application entièrement fonctionnelle qui associe une brochure d’image et de PDF à chaque catégorie. Dans ce didacticiel, nous allons examiner différentes techniques pour stocker des données binaires et découvrir comment permettre aux utilisateurs de télécharger un fichier à partir de leur ordinateur et les avoir enregistrés sur le système de fichiers du serveur Web s.

> [!NOTE]
> Les données binaires qui font partie d’un modèle de données d’application sont parfois appelées [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), acronyme pour Binary Large Object. Dans ces didacticiels, j’ai choisi d’utiliser la terminologie données binaires, bien que le terme objet BLOB soit synonyme.

## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Étape 1 : création de l’utilisation des pages Web de données binaires

Avant de commencer à explorer les défis associés à l’ajout de la prise en charge des données binaires, commencez par prendre un moment pour créer les pages ASP.NET dans notre projet de site Web dont nous aurons besoin pour ce didacticiel et les trois prochaines. Commencez par ajouter un nouveau dossier nommé `BinaryData`. Ajoutez ensuite les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`

![Ajouter les pages ASP.NET pour les didacticiels relatifs aux données binaires](uploading-files-cs/_static/image1.gif)

**Figure 1**: ajouter les pages ASP.net pour les didacticiels relatifs aux données binaires

Comme dans les autres dossiers, `Default.aspx` dans le dossier `BinaryData` répertorie les didacticiels de la section. N’oubliez pas que le contrôle utilisateur `SectionLevelTutorialListing.ascx` fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur la page s Mode Création.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**Figure 2**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image2.png))

Enfin, ajoutez ces pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après l’amélioration du `<siteMapNode>`GridView :

[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels utilisation de données binaires.

![Le plan de site contient maintenant des entrées pour les didacticiels utilisation de données binaires](uploading-files-cs/_static/image3.gif)

**Figure 3**: le plan de site contient maintenant des entrées pour les didacticiels utilisation de données binaires

## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Étape 2 : choix de l’emplacement de stockage des données binaires

Les données binaires associées au modèle de données de l’application peuvent être stockées dans l’un des deux emplacements suivants : sur le système de fichiers du serveur Web, avec une référence au fichier stocké dans la base de données ; ou directement dans la base de données elle-même (voir figure 4). Chaque approche a son propre ensemble de professionnels et de inconvénients et mérite une discussion plus détaillée.

[![données binaires peuvent être stockées dans le système de fichiers ou directement dans la base de données](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**Figure 4**: les données binaires peuvent être stockées dans le système de fichiers ou directement dans la base de données ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image4.png))

Imaginez que nous souhaitions étendre la base de données Northwind pour associer une image à chaque produit. Une option consiste à stocker ces fichiers image sur le système de fichiers du serveur Web et à enregistrer le chemin d’accès dans la table `Products`. Avec cette approche, nous ajoutons une `ImagePath` colonne à la table de `Products` de type `varchar(200)`, peut-être. Lorsqu’un utilisateur a chargé une image pour Chai, cette image peut être stockée sur le système de fichiers du serveur Web s à `~/Images/Tea.jpg`, où `~` représente le chemin d’accès physique de l’application. Autrement dit, si le site Web est enraciné au chemin d’accès physique `C:\Websites\Northwind\`, `~/Images/Tea.jpg` équivaut à `C:\Websites\Northwind\Images\Tea.jpg`. Après avoir chargé le fichier image, nous mettons à jour l’enregistrement Chai dans la table `Products` afin que sa colonne `ImagePath` référencée le chemin d’accès de la nouvelle image. Nous pourrions utiliser `~/Images/Tea.jpg` ou simplement `Tea.jpg` si nous avons décidé que toutes les images de produit seraient placées dans le dossier application s `Images`.

Les principaux avantages du stockage des données binaires sur le système de fichiers sont les suivants :

- La **facilité d’implémentation** , comme nous le verrons bientôt, le stockage et la récupération de données binaires stockées directement dans la base de données impliquent un peu plus de code que lors de l’utilisation de données via le système de fichiers. En outre, pour permettre à un utilisateur d’afficher ou de télécharger des données binaires, une URL vers ces données doit être présentée à l’utilisateur. Si les données résident sur le système de fichiers du serveur Web, l’URL est simple. Toutefois, si les données sont stockées dans la base de données, vous devez créer une page Web qui récupère et retourne les données de la base de données.
- **Accès plus étendu aux données binaires** il se peut que les données binaires doivent être accessibles à d’autres services ou applications, qui ne peuvent pas extraire les données de la base de données. Par exemple, les images associées à chaque produit peuvent également être accessibles aux utilisateurs via [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol). dans ce cas, nous voulons stocker les données binaires sur le système de fichiers.
- **Performances** si les données binaires sont stockées sur le système de fichiers, la demande et la congestion du réseau entre le serveur de base de données et le serveur Web sont inférieures à si les données binaires sont stockées directement dans la base de données.

Le principal inconvénient du stockage de données binaires sur le système de fichiers est qu’il dissocie les données de la base de données. Si un enregistrement est supprimé de la table `Products`, le fichier associé sur le système de fichiers du serveur Web n’est pas supprimé automatiquement. Nous devons écrire du code supplémentaire pour supprimer le fichier, sans quoi le système de fichiers deviendra encombré de fichiers orphelins inutilisés. En outre, lors de la sauvegarde de la base de données, nous devons veiller à effectuer également des sauvegardes des données binaires associées dans le système de fichiers. Le déplacement de la base de données vers un autre site ou serveur pose des problèmes similaires.

En guise d’alternative, les données binaires peuvent être stockées directement dans une base de données Microsoft SQL Server 2005 en créant une colonne de type `varbinary`. Comme avec d’autres types de données de longueur variable, vous pouvez spécifier une longueur maximale des données binaires qui peuvent être conservées dans cette colonne. Par exemple, pour réserver au plus 5 000 octets, utilisez `varbinary(5000)`; `varbinary(MAX)` permet d’obtenir la taille de stockage maximale, environ 2 Go.

Le principal avantage de stocker des données binaires directement dans la base de données est le couplage étroit entre les données binaires et l’enregistrement de base de données. Cela simplifie grandement les tâches d’administration de base de données, telles que les sauvegardes ou le déplacement de la base de données vers un autre site ou serveur. En outre, la suppression d’un enregistrement entraîne la suppression automatique des données binaires correspondantes. Il existe également des avantages plus subtils du stockage des données binaires dans la base de données. Pour plus d’informations, consultez [stockage de fichiers binaires directement dans la base de données à l’aide de ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) .

> [!NOTE]
> Dans Microsoft SQL Server 2000 et versions antérieures, le type de données `varbinary` avait une limite maximale de 8 000 octets. Pour stocker jusqu’à 2 Go de données binaires, le [type de données`image`](https://msdn.microsoft.com/library/ms187993.aspx) doit être utilisé à la place. Toutefois, avec l’ajout de `MAX` dans SQL Server 2005, le type de données `image` est déconseillé. Elle est toujours prise en charge à des fins de compatibilité descendante, mais Microsoft a annoncé que le type de données `image` sera supprimé dans une future version de SQL Server.

Si vous utilisez un modèle de données plus ancien, vous pouvez voir le type de données `image`. La table `Categories` de la base de données Northwind contient une colonne `Picture` qui peut être utilisée pour stocker les données binaires d’un fichier image pour la catégorie. Étant donné que la base de données Northwind a ses racines dans Microsoft Access et les versions antérieures de SQL Server, cette colonne est de type `image`.

Pour ce didacticiel et les trois suivants, nous allons utiliser les deux approches. La table `Categories` a déjà une colonne `Picture` pour stocker le contenu binaire d’une image pour la catégorie. Nous allons ajouter une colonne supplémentaire, `BrochurePath`, pour stocker un chemin d’accès à un fichier PDF sur le système de fichiers du serveur Web, qui peut être utilisé pour fournir une vue d’ensemble de qualité d’impression et impeccable de la catégorie.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Étape 3 : ajout de la colonne`BrochurePath`à la table`Categories`

Actuellement, la table categories ne contient que quatre colonnes : `CategoryID`, `CategoryName`, `Description`et `Picture`. En plus de ces champs, nous devons en ajouter un nouveau qui pointera vers la brochure catégorie s (le cas échéant). Pour ajouter cette colonne, accédez à la Explorateur de serveurs, explorez les tables, cliquez avec le bouton droit sur la table `Categories` et choisissez Ouvrir la définition de table (voir la figure 5). Si vous ne voyez pas la Explorateur de serveurs, affichez-la en sélectionnant l’option Explorateur de serveurs dans le menu Affichage ou appuyez sur Ctrl + Alt + S.

Ajoutez une nouvelle `varchar(200)` colonne à la table `Categories` nommée `BrochurePath` et autorise `NULL` s, puis cliquez sur l’icône d’enregistrement (ou appuyez sur CTRL + S).

[![ajouter une colonne BrochurePath à la table Categories](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**Figure 5**: ajouter une colonne `BrochurePath` à la table `Categories` ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image6.png))

## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Étape 4 : mise à jour de l’architecture pour utiliser les colonnes`Picture`et`BrochurePath`

Le `CategoriesDataTable` de la couche d’accès aux données (DAL) possède actuellement quatre `DataColumn` s définis : `CategoryID`, `CategoryName`, `Description`et `NumberOfProducts`. Lorsque nous avons conçu à l’origine ce DataTable dans le didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md) , le `CategoriesDataTable` avait uniquement les trois premières colonnes ; la colonne `NumberOfProducts` a été ajoutée dans le [maître/détail à l’aide d’une liste à puces d’enregistrements principaux avec un didacticiel des détails DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) .

Comme décrit dans *création d’une couche d’accès aux données*, les DataTables dans le DataSet typé composent les objets métier. Les TableAdapters sont responsables de la communication avec la base de données et du remplissage des objets métier avec les résultats de la requête. Le `CategoriesDataTable` est rempli par le `CategoriesTableAdapter`, qui a trois méthodes de récupération de données :

- `GetCategories()` exécute la requête principale du TableAdapter et retourne les champs `CategoryID`, `CategoryName`et `Description` de tous les enregistrements de la table `Categories`. La requête principale est utilisée par les méthodes de `Insert` et de `Update` générées automatiquement.
- `GetCategoryByCategoryID(categoryID)` retourne les champs `CategoryID`, `CategoryName`et `Description` de la catégorie dont `CategoryID` est égal à *CategoryID*.
- `GetCategoriesAndNumberOfProducts()` : renvoie les champs `CategoryID`, `CategoryName`et `Description` pour tous les enregistrements de la table `Categories`. Utilise également une sous-requête pour retourner le nombre de produits associés à chaque catégorie.

Notez qu’aucune de ces requêtes ne retourne la `Categories` table s `Picture` ou `BrochurePath` colonnes ; les `CategoriesDataTable` ne fournissent pas non plus `DataColumn` s pour ces champs. Pour pouvoir utiliser les propriétés Picture et `BrochurePath`, nous devons d’abord les ajouter au `CategoriesDataTable`, puis mettre à jour la classe `CategoriesTableAdapter` pour retourner ces colonnes.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Ajout des`Picture`et des`BrochurePath``DataColumn` s

Commencez par ajouter ces deux colonnes au `CategoriesDataTable`. Cliquez avec le bouton droit sur l’en-tête `CategoriesDataTable` s, sélectionnez Ajouter dans le menu contextuel, puis choisissez l’option colonne. Cette opération crée un `DataColumn` dans le DataTable nommé `Column1`. Renommez cette colonne pour `Picture`. À partir de la Fenêtre Propriétés, définissez la propriété `DataColumn` s `DataType` sur `System.Byte[]` (il ne s’agit pas d’une option dans la liste déroulante ; vous devez la saisir).

[![créer une image nommée DataColumn dont le type de données est System. Byte []](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**Figure 6**: créer un `DataColumn` nommé `Picture` dont `DataType` est `System.Byte[]` ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image8.png))

Ajoutez un autre `DataColumn` au DataTable, en le nommant `BrochurePath` à l’aide de la valeur de `DataType` par défaut (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Retour des valeurs`Picture`et`BrochurePath`à partir du TableAdapter

Une fois ces deux `DataColumn` ajoutés au `CategoriesDataTable`, nous sommes prêts à mettre à jour le `CategoriesTableAdapter`. Nous aurions pu faire en sorte que ces deux valeurs de colonne soient retournées dans la requête principale du TableAdapter, mais cela réafficherait les données binaires chaque fois que la méthode `GetCategories()` a été appelée. Au lieu de cela, nous allons mettre à jour la requête principale du TableAdapter pour rétablir `BrochurePath` et créer une méthode de récupération de données supplémentaire qui retourne une catégorie particulière de `Picture` colonne.

Pour mettre à jour la requête principale du TableAdapter, cliquez avec le bouton droit sur l’en-tête `CategoriesTableAdapter` s et choisissez l’option configurer dans le menu contextuel. L’Assistant Configuration de l’adaptateur de table, que nous avons vu dans un certain nombre de didacticiels précédents, s’affiche. Mettez à jour la requête pour rétablir le `BrochurePath`, puis cliquez sur Terminer.

[![mettre à jour la liste des colonnes dans l’instruction SELECT pour retourner également BrochurePath](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**Figure 7**: mettre à jour la liste des colonnes dans l’instruction `SELECT` pour retourner également `BrochurePath` ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image10.png))

Lorsque vous utilisez des instructions SQL ad hoc pour le TableAdapter, la mise à jour de la liste des colonnes dans la requête principale met à jour la liste des colonnes de toutes les `SELECT` méthodes de requête dans le TableAdapter. Cela signifie que la méthode `GetCategoryByCategoryID(categoryID)` a été mise à jour pour retourner la colonne `BrochurePath`, ce qui peut être ce que nous avions prévu. Toutefois, elle a également mis à jour la liste des colonnes dans la méthode `GetCategoriesAndNumberOfProducts()`, en supprimant la sous-requête qui retourne le nombre de produits pour chaque catégorie. Par conséquent, nous devons mettre à jour cette méthode `SELECT` requête. Cliquez avec le bouton droit sur la méthode `GetCategoriesAndNumberOfProducts()`, choisissez configurer, puis rétablissez la valeur d’origine de la requête `SELECT`.

[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

Ensuite, créez une nouvelle méthode TableAdapter qui retourne une catégorie particulière s `Picture` valeur de colonne. Cliquez avec le bouton droit sur l’en-tête `CategoriesTableAdapter` s et choisissez l’option Ajouter une requête pour lancer l’Assistant Configuration de requêtes TableAdapter. La première étape de cet Assistant nous demande si vous souhaitez interroger les données à l’aide d’une instruction SQL ad hoc, d’une nouvelle procédure stockée ou d’une procédure existante. Sélectionnez utiliser des instructions SQL, puis cliquez sur suivant. Étant donné que nous allons retourner une ligne, choisissez l’option SELECT qui retourne les lignes de la deuxième étape.

[![sélectionnez l’option utiliser des instructions SQL](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**Figure 8**: sélectionner l’option utiliser des instructions SQL ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image12.png))

[![étant donné que la requête renverra un enregistrement de la table Categories, choisissez SELECT qui retourne des lignes](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**Figure 9**: étant donné que la requête renverra un enregistrement de la table Categories, choisissez SELECT qui retourne des lignes ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image14.png))

À la troisième étape, entrez la requête SQL suivante, puis cliquez sur suivant :

[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

La dernière étape consiste à choisir le nom de la nouvelle méthode. Utilisez `FillCategoryWithBinaryDataByCategoryID` et `GetCategoryWithBinaryDataByCategoryID` pour les modèles remplir un DataTable et retourner un DataTable, respectivement. Cliquez sur Terminer pour terminer l'Assistant.

[![choisir les noms des méthodes du TableAdapter](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**Figure 10**: choisir les noms des méthodes des TableAdapter s ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image16.png))

> [!NOTE]
> Après avoir terminé l’Assistant Configuration des requêtes de l’adaptateur de table, vous pouvez voir une boîte de dialogue vous informant que le nouveau texte de la commande retourne des données avec un schéma différent du schéma de la requête principale. En bref, l’Assistant remarque que la requête principale du TableAdapter s `GetCategories()` retourne un schéma différent de celui que nous venons de créer. Mais c’est ce que nous voulons, vous pouvez donc ignorer ce message.

En outre, gardez à l’esprit que si vous utilisez des instructions SQL ad hoc et que vous utilisez l’Assistant pour modifier la requête principale du TableAdapter à un moment ultérieur, elle modifiera la `GetCategoryWithBinaryDataByCategoryID` méthode s `SELECT` liste des colonnes de l’instruction s pour n’inclure que les colonnes de la requête principale (autrement dit, elle supprimera la `Picture` colonne de la requête). Vous devrez mettre à jour manuellement la liste des colonnes pour retourner la colonne `Picture`, de la même façon que nous l’avons fait avec la méthode `GetCategoriesAndNumberOfProducts()` précédemment dans cette étape.

Après avoir ajouté les deux `DataColumn` s aux `CategoriesDataTable` et la méthode `GetCategoryWithBinaryDataByCategoryID` au `CategoriesTableAdapter`, ces classes dans le concepteur de DataSet typé doivent ressembler à la capture d’écran de la figure 11.

![Le concepteur de DataSet comprend les nouvelles colonnes et la nouvelle méthode](uploading-files-cs/_static/image11.gif)

**Figure 11**: le concepteur de DataSet comprend les nouvelles colonnes et la nouvelle méthode

## <a name="updating-the-business-logic-layer-bll"></a>Mise à jour de la couche de logique métier (BLL)

Avec la couche DAL mise à jour, il ne reste plus qu’à compléter la couche BLL (Business Logic Layer) pour inclure une méthode pour la nouvelle méthode `CategoriesTableAdapter`. Ajoutez la méthode suivante à la classe `CategoriesBLL` :

[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Étape 5 : Téléchargement d’un fichier à partir du client vers le serveur Web

Lorsque vous collectez des données binaires, ces données sont souvent fournies par un utilisateur final. Pour capturer ces informations, l’utilisateur doit être en mesure de charger un fichier à partir de son ordinateur sur le serveur Web. Les données chargées doivent ensuite être intégrées au modèle de données, ce qui peut signifier l’enregistrement du fichier dans le système de fichiers du serveur Web et l’ajout d’un chemin d’accès au fichier dans la base de données, ou l’écriture du contenu binaire directement dans la base de données. Dans cette étape, nous allons examiner comment autoriser un utilisateur à télécharger des fichiers à partir de son ordinateur sur le serveur. Dans le didacticiel suivant, nous allons attirer l’attention sur l’intégration du fichier téléchargé avec le modèle de données.

ASP.NET 2,0 s nouveau [contrôle Web FileUpload](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) fournit un mécanisme permettant aux utilisateurs d’envoyer un fichier de leur ordinateur au serveur Web. Le contrôle FileUpload est rendu sous la forme d’un élément `<input>` dont l’attribut `type` est défini sur file, que les navigateurs affichent sous forme de zone de texte avec un bouton Parcourir. Le fait de cliquer sur le bouton Parcourir permet d’ouvrir une boîte de dialogue dans laquelle l’utilisateur peut sélectionner un fichier. Lorsque le formulaire est publié, le contenu des fichiers sélectionnés est envoyé avec la publication (postback). Côté serveur, les informations sur le fichier téléchargé sont accessibles via les propriétés du contrôle FileUpload s.

Pour illustrer le téléchargement de fichiers, ouvrez la page `FileUpload.aspx` dans le dossier `BinaryData`, faites glisser un contrôle FileUpload de la boîte à outils vers le concepteur et définissez la propriété Control s `ID` sur `UploadTest`. Ensuite, ajoutez un contrôle Web Button en définissant ses propriétés `ID` et `Text` sur `UploadButton` et télécharger le fichier sélectionné, respectivement. Enfin, placez un contrôle Web Label sous le bouton, effacez sa propriété `Text` et affectez à sa propriété `ID` la valeur `UploadDetails`.

[![ajouter un contrôle FileUpload à la page ASP.NET](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**Figure 12**: ajouter un contrôle FileUpload à la page ASP.net ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image18.png))

La figure 13 illustre cette page lorsque vous l’affichez dans un navigateur. Notez que le fait de cliquer sur le bouton Parcourir permet d’ouvrir une boîte de dialogue de sélection de fichier, ce qui permet à l’utilisateur de choisir un fichier sur son ordinateur. Une fois qu’un fichier a été sélectionné, le fait de cliquer sur le bouton charger le fichier sélectionné entraîne une publication qui envoie le contenu binaire des fichiers sélectionnés au serveur Web.

[![l’utilisateur peut sélectionner un fichier à télécharger à partir de son ordinateur sur le serveur](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**Figure 13**: l’utilisateur peut sélectionner un fichier à télécharger à partir de son ordinateur sur le serveur ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image20.png))

Lors de la publication (postback), le fichier téléchargé peut être enregistré dans le système de fichiers ou ses données binaires peuvent être utilisées directement via un flux. Pour cet exemple, vous allez créer un dossier `~/Brochures` et y enregistrer le fichier chargé. Commencez par ajouter le dossier `Brochures` au site en tant que sous-dossier du répertoire racine. Ensuite, créez un gestionnaire d’événements pour l’événement `UploadButton` s `Click` et ajoutez le code suivant :

[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

Le contrôle FileUpload fournit diverses propriétés permettant d’utiliser les données chargées. Par exemple, la [propriété`HasFile`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) indique si un fichier a été téléchargé par l’utilisateur, tandis que la [propriété`FileBytes`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) fournit l’accès aux données binaires téléchargées sous la forme d’un tableau d’octets. Le gestionnaire d’événements `Click` démarre en s’assurant qu’un fichier a été chargé. Si un fichier a été chargé, l’étiquette indique le nom du fichier téléchargé, sa taille en octets et son Content-type.

> [!NOTE]
> Pour vous assurer que l’utilisateur charge un fichier, vous pouvez vérifier la propriété `HasFile` et afficher un avertissement s’il est `false`ou vous pouvez utiliser le [contrôle RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) à la place.

Le `SaveAs(filePath)` FileUpload enregistre le fichier chargé dans le *filePath*spécifié. *filePath* doit être un *chemin d’accès physique* (`C:\Websites\Brochures\SomeFile.pdf`) au lieu *d’un chemin d’accès* *virtuel* (`/Brochures/SomeFile.pdf`). La [méthode`Server.MapPath(virtPath)`](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) prend un chemin d’accès virtuel et retourne son chemin d’accès physique correspondant. Ici, le chemin d’accès virtuel est `~/Brochures/fileName`, où *filename* est le nom du fichier chargé. Pour plus d’informations sur les chemins d’accès virtuel et physique et l’utilisation de `Server.MapPath`, consultez [utilisation de Server. MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) .

Après avoir terminé le gestionnaire d’événements `Click`, prenez un moment pour tester la page dans un navigateur. Cliquez sur le bouton Parcourir et sélectionnez un fichier sur votre disque dur, puis cliquez sur le bouton charger le fichier sélectionné. La publication envoie le contenu du fichier sélectionné au serveur Web, qui affiche ensuite des informations sur le fichier avant de l’enregistrer dans le dossier `~/Brochures`. Après avoir téléchargé le fichier, revenez à Visual Studio et cliquez sur le bouton Actualiser dans la Explorateur de solutions. Vous devez voir le fichier que vous venez de charger dans le dossier ~/brochures !

[![le fichier EvolutionValley. jpg a été chargé sur le serveur Web](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**Figure 14**: le fichier `EvolutionValley.jpg` a été chargé sur le serveur Web ([cliquez pour afficher l’image en taille réelle](uploading-files-cs/_static/image22.png))

![EvolutionValley. jpg a été enregistré dans le dossier ~/brochures](uploading-files-cs/_static/image15.gif)

**Figure 15**: `EvolutionValley.jpg` a été enregistré dans le dossier `~/Brochures`

## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Subtilités de l’enregistrement des fichiers téléchargés dans le système de fichiers

Plusieurs subtilités sont à prendre en compte lors de l’enregistrement du téléchargement des fichiers sur le système de fichiers du serveur Web s. Tout d’abord, il y a un problème de sécurité. Pour enregistrer un fichier dans le système de fichiers, le contexte de sécurité sous lequel la page ASP.NET s’exécute doit avoir des autorisations en écriture. Le serveur Web de développement ASP.NET s’exécute dans le contexte de votre compte d’utilisateur actuel. Si vous utilisez Microsoft s Internet Information Services (IIS) comme serveur Web, le contexte de sécurité dépend de la version d’IIS et de sa configuration.

L’enregistrement des fichiers dans le système de fichiers repose également sur l’attribution d’un nom aux fichiers. Actuellement, notre page enregistre tous les fichiers téléchargés dans le répertoire `~/Brochures` en utilisant le même nom que le fichier sur l’ordinateur client s. Si l’utilisateur A télécharge une brochure portant le nom `Brochure.pdf`, le fichier est enregistré en tant que `~/Brochure/Brochure.pdf`. Mais que se passe-t-il si, plus tard, l’utilisateur B charge un autre fichier de brochure qui a le même nom de fichier (`Brochure.pdf`) ? Avec le code que nous avons maintenant, l’utilisateur A est remplacé par le fichier de téléchargement de l’utilisateur B.

Il existe plusieurs techniques pour résoudre les conflits de noms de fichiers. L’une des options consiste à interdire le chargement d’un fichier s’il en existe déjà un portant le même nom. Avec cette approche, lorsque l’utilisateur B tente de télécharger un fichier nommé `Brochure.pdf`, le système n’enregistre pas son fichier et affiche à la place un message informant l’utilisateur B pour renommer le fichier et réessayer. Une autre approche consiste à enregistrer le fichier à l’aide d’un nom de fichier unique, qui peut être un [identificateur global unique (Guid)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) ou la valeur de la ou des colonnes de clé primaire de l’enregistrement de base de données correspondant (en supposant que le téléchargement est associé à une ligne particulière dans le modèle de données). Dans le didacticiel suivant, nous allons explorer ces options plus en détail.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Défis liés à de très grandes quantités de données binaires

Ces didacticiels partent du principe que la taille des données binaires capturées est modeste. L’utilisation de très grandes quantités de fichiers de données binaires de plusieurs mégaoctets ou plus entraîne de nouveaux défis qui n’entrent pas dans le cadre de ces didacticiels. Par exemple, par défaut, ASP.NET rejette les chargements de plus de 4 Mo, bien que cela puisse être configuré par le biais de l' [élément`<httpRuntime>`](https://msdn.microsoft.com/library/e1f13641.aspx) dans `Web.config`. IIS impose également ses propres limitations de taille de chargement de fichiers. Pour plus d’informations, consultez [taille du fichier de téléchargement IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) . En outre, le temps nécessaire pour charger des fichiers volumineux peut dépasser la valeur par défaut de 110 secondes ASP.NET attend une demande. Il existe également des problèmes de mémoire et de performances qui surviennent lors de l’utilisation de fichiers volumineux.

Le contrôle FileUpload n’est pas pratique pour les chargements de fichiers volumineux. Au fur et à mesure que le contenu des fichiers est publié sur le serveur, l’utilisateur final doit patienter sans confirmer que son chargement progresse. Cela ne pose pas de problème lorsque vous traitez des fichiers plus petits qui peuvent être téléchargés en quelques secondes, mais peut être un problème lorsque vous traitez des fichiers plus volumineux qui peuvent prendre plusieurs minutes à charger. Un grand nombre de contrôles de téléchargement de fichiers tiers sont mieux adaptés à la gestion des chargements volumineux et un grand nombre de ces fournisseurs fournissent des indicateurs de progression et des gestionnaires de téléchargement ActiveX qui présentent une expérience utilisateur beaucoup plus soignée.

Si votre application doit gérer des fichiers volumineux, vous devez examiner attentivement les défis et trouver les solutions appropriées pour vos besoins spécifiques.

## <a name="summary"></a>Récapitulatif

La création d’une application qui doit capturer des données binaires présente un certain nombre de défis. Dans ce didacticiel, nous avons abordé les deux premières : décider où stocker les données binaires et permettre à un utilisateur de charger du contenu binaire via une page Web. Dans les trois didacticiels suivants, nous verrons comment associer les données chargées à un enregistrement dans la base de données et comment afficher les données binaires en même temps que les champs de données de texte.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Utilisation des types de données de valeur élevée](https://msdn.microsoft.com/library/ms178158.aspx)
- [Démarrages rapides du contrôle FileUpload](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Contrôle serveur FileUpload ASP.NET 2,0](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Côté sombre des chargements de fichiers](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Teresa Murphy et Bernadette Leigh. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](displaying-binary-data-in-the-data-web-controls-cs.md)
