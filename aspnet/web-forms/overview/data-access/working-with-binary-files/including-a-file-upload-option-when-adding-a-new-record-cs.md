---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: Inclusion d’une option de chargement de fichier lors de l'C#ajout d’un nouvel enregistrement () | Microsoft Docs
author: rick-anderson
description: Ce didacticiel montre comment créer une interface Web qui permet à l’utilisateur d’entrer des données texte et de charger des fichiers binaires. Pour illustrer les options disponibles...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: f1287e180151b3034a7b90ef4b3f1fbe68354a09
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576978"
---
# <a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Inclusion d’une option de chargement de fichier lors de l’ajout d’un nouvel enregistrement (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) ou [Télécharger le PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> Ce didacticiel montre comment créer une interface Web qui permet à l’utilisateur d’entrer des données texte et de charger des fichiers binaires. Pour illustrer les options disponibles pour stocker des données binaires, un fichier est enregistré dans la base de données tandis que l’autre est stocké dans le système de fichiers.

## <a name="introduction"></a>Introduction

Dans les deux didacticiels précédents, nous avons exploré les techniques de stockage des données binaires associées au modèle de données de l’application, nous avons vu comment utiliser le contrôle FileUpload pour envoyer des fichiers du client vers le serveur Web et vu comment présenter ces données binaires dans un Data W contrôle EB. Toutefois, nous avons déjà parlé de la façon d’associer des données chargées au modèle de données.

Dans ce didacticiel, nous allons créer une page Web pour ajouter une nouvelle catégorie. En plus des zones de texte pour le nom et la description de la catégorie, cette page doit inclure deux contrôles FileUpload, un pour la nouvelle image de catégorie et un pour la brochure. L’image chargée est stockée directement dans le nouvel enregistrement s `Picture` colonne, tandis que la brochure sera enregistrée dans le dossier `~/Brochures` avec le chemin d’accès au fichier enregistré dans la colonne nouvel enregistrement s `BrochurePath`.

Avant de créer cette nouvelle page Web, nous devrons mettre à jour l’architecture. La requête principale `CategoriesTableAdapter` s ne récupère pas la colonne `Picture`. Par conséquent, la méthode de `Insert` générée automatiquement n’a que des entrées pour les champs `CategoryName`, `Description`et `BrochurePath`. Par conséquent, nous devons créer une méthode supplémentaire dans le TableAdapter qui invite à entrer les quatre `Categories` champs. La classe `CategoriesBLL` de la couche de logique métier doit également être mise à jour.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Étape 1 : ajout d’une méthode de`InsertWithPicture`à la`CategoriesTableAdapter`

Lorsque nous avons créé le `CategoriesTableAdapter` dans le didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md) , nous l’avons configuré pour générer automatiquement des instructions `INSERT`, `UPDATE`et `DELETE` basées sur la requête principale. En outre, nous avons fait en sorte que le TableAdapter utilise l’approche DB direct, qui a créé les méthodes `Insert`, `Update`et `Delete`. Ces méthodes exécutent les instructions générées automatiquement `INSERT`, `UPDATE`et `DELETE` et, par conséquent, acceptent les paramètres d’entrée en fonction des colonnes retournées par la requête principale. Dans le didacticiel sur le [téléchargement de fichiers](uploading-files-cs.md) , nous avons augmenté la requête principale de `CategoriesTableAdapter` s pour utiliser la colonne `BrochurePath`.

Étant donné que la requête principale `CategoriesTableAdapter` s ne fait pas référence à la colonne `Picture`, nous ne pouvons pas ajouter un nouvel enregistrement ni mettre à jour un enregistrement existant avec une valeur pour la colonne `Picture`. Pour capturer ces informations, nous pouvons créer une nouvelle méthode dans le TableAdapter qui est utilisée spécifiquement pour insérer un enregistrement avec des données binaires ou vous pouvez personnaliser l’instruction de `INSERT` générée automatiquement. Le problème de la personnalisation de l’instruction de `INSERT` générée automatiquement est que nous risquons de remplacer les personnalisations par l’Assistant. Par exemple, imaginez que nous avons personnalisé l’instruction `INSERT` pour inclure l’utilisation de la colonne `Picture`. Cela permet de mettre à jour la méthode TableAdapter s `Insert` pour inclure un paramètre d’entrée supplémentaire pour les données binaires de l’image de la catégorie s. Nous pourrions ensuite créer une méthode dans la couche de logique métier pour utiliser cette méthode DAL et appeler cette méthode BLL via la couche de présentation, et tout fonctionnera très bien. Autrement dit, jusqu’à la prochaine configuration du TableAdapter par le biais de l’Assistant Configuration de TableAdapter. Dès que l’Assistant a terminé, nos personnalisations de l’instruction `INSERT` seraient remplacées, la méthode `Insert` rétablirait son ancien format et notre code ne serait plus compilé !

> [!NOTE]
> Ce problème n’est pas lié à l’utilisation de procédures stockées plutôt qu’à des instructions SQL ad hoc. Un futur didacticiel explore l’utilisation des procédures stockées à la place des instructions SQL ad hoc dans la couche d’accès aux données.

Pour éviter ce problème, au lieu de personnaliser les instructions SQL générées automatiquement, il est préférable de créer une nouvelle méthode pour le TableAdapter. Cette méthode, nommée `InsertWithPicture`, accepte les valeurs des colonnes `CategoryName`, `Description`, `BrochurePath`et `Picture` et exécute une instruction `INSERT` qui stocke les quatre valeurs dans un nouvel enregistrement.

Ouvrez le DataSet typé et, à partir du concepteur, cliquez avec le bouton droit sur l’en-tête `CategoriesTableAdapter` s et choisissez Ajouter une requête dans le menu contextuel. Cela lance l’Assistant Configuration de requêtes TableAdapter, qui commence par nous demander comment la requête TableAdapter doit accéder à la base de données. Choisissez utiliser des instructions SQL, puis cliquez sur suivant. L’étape suivante vous invite à entrer le type de requête à générer. Étant donné que nous avons recréé une requête pour ajouter un nouvel enregistrement à la table `Categories`, choisissez Insérer, puis cliquez sur suivant.

[![sélectionnez l’option d’insertion](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Figure 1**: sélectionner l’option d’insertion ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))

Nous devons maintenant spécifier l’instruction SQL `INSERT`. L’Assistant suggère automatiquement une instruction `INSERT` correspondant à la requête principale du TableAdapter. Dans ce cas, il s’agit d’une instruction `INSERT` qui insère les valeurs `CategoryName`, `Description`et `BrochurePath`. Mettez à jour l’instruction afin que la colonne `Picture` soit incluse avec un paramètre `@Picture`, comme suit :

[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Le dernier écran de l’Assistant nous invite à nommer la nouvelle méthode TableAdapter. Entrez `InsertWithPicture`, puis cliquez sur Terminer.

[![nommez la nouvelle méthode TableAdapter InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Figure 2**: nommer la nouvelle méthode TableAdapter `InsertWithPicture` ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))

## <a name="step-2-updating-the-business-logic-layer"></a>Étape 2 : mise à jour de la couche de logique métier

Étant donné que la couche de présentation doit uniquement interagir avec la couche de logique métier au lieu de la contourner pour passer directement à la couche d’accès aux données, nous devons créer une méthode BLL qui appelle la méthode DAL que nous venons de créer (`InsertWithPicture`). Pour ce didacticiel, créez une méthode dans la classe `CategoriesBLL` nommée `InsertWithPicture` qui accepte comme entrée trois `string` s et un tableau de `byte`. Les paramètres d’entrée `string` sont pour le nom de la catégorie, la description et le chemin d’accès au fichier de la brochure, tandis que le tableau de `byte` correspond au contenu binaire de l’image de la catégorie. Comme le montre le code suivant, cette méthode BLL appelle la méthode DAL correspondante :

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Assurez-vous que vous avez enregistré le DataSet typé avant d’ajouter la méthode `InsertWithPicture` à la couche BLL. Étant donné que le code de la classe `CategoriesTableAdapter` est généré automatiquement en fonction du DataSet typé, si vous n’enregistrez pas d’abord vos modifications dans le DataSet typé, la propriété `Adapter` n’est pas connue de la méthode `InsertWithPicture`.

## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Étape 3 : énumération des catégories existantes et de leurs données binaires

Dans ce didacticiel, nous allons créer une page qui permet à un utilisateur final d’ajouter une nouvelle catégorie au système, en fournissant une image et une brochure pour la nouvelle catégorie. Dans le [didacticiel précédent](displaying-binary-data-in-the-data-web-controls-cs.md) , nous avons utilisé un GridView avec TemplateField et ImageField pour afficher le nom, la description, l’image et le lien de chaque catégorie pour télécharger sa brochure. Essayons de répliquer cette fonctionnalité pour ce didacticiel, en créant une page qui répertorie toutes les catégories existantes et autorise la création de nouvelles.

Commencez par ouvrir la page `DisplayOrDownload.aspx` dans le dossier `BinaryData`. Accédez à la vue source et copiez la syntaxe déclarative de GridView et ObjectDataSource, en la collant dans l’élément `<asp:Content>` dans `UploadInDetailsView.aspx`. Par ailleurs, Don t oublie de copier la méthode `GenerateBrochureLink` de la classe code-behind de `DisplayOrDownload.aspx` à `UploadInDetailsView.aspx`.

[![copier et coller la syntaxe déclarative de DisplayOrDownload. aspx à UploadInDetailsView. aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Figure 3**: copier et coller la syntaxe déclarative de `DisplayOrDownload.aspx` à `UploadInDetailsView.aspx` ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))

Après avoir copié la syntaxe déclarative et la méthode `GenerateBrochureLink` sur la page `UploadInDetailsView.aspx`, affichez la page dans un navigateur pour vous assurer que tout a été copié correctement. Vous devez voir un GridView qui répertorie les huit catégories incluant un lien pour télécharger la brochure, ainsi que l’image des catégories.

[![vous devez maintenant voir chaque catégorie et ses données binaires](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Figure 4**: vous devez maintenant voir chaque catégorie et ses données binaires ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))

## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Étape 4 : configuration de l'`CategoriesDataSource`pour prendre en charge l’insertion

Le `CategoriesDataSource` ObjectDataSource utilisé par le GridView `Categories` ne permet pas d’insérer des données. Afin de prendre en charge l’insertion via ce contrôle de source de données, nous devons mapper sa méthode `Insert` à une méthode dans son objet sous-jacent, `CategoriesBLL`. En particulier, nous voulons le mapper à la méthode `CategoriesBLL` que nous avons rajoutée à l’étape 2, `InsertWithPicture`.

Commencez par cliquer sur le lien configurer la source de données dans la balise active ObjectDataSource s. Le premier écran montre l’objet avec lequel la source de données est configurée pour fonctionner, `CategoriesBLL`. Laissez ce paramètre défini sur, puis cliquez sur suivant pour passer à l’écran définir les méthodes de données. Accédez à l’onglet Insérer et sélectionnez la méthode `InsertWithPicture` dans la liste déroulante. Cliquez sur Terminer pour terminer l'Assistant.

[![configurer ObjectDataSource pour utiliser la méthode InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Figure 5**: configurer ObjectDataSource pour utiliser la méthode `InsertWithPicture` ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))

> [!NOTE]
> À la fin de l’Assistant, Visual Studio peut vous demander si vous souhaitez actualiser les champs et les clés, ce qui permet de régénérer les champs de contrôles Web de données. Choisissez non, car si vous choisissez Oui, vous remplacerez toutes les personnalisations de champ que vous avez effectuées.

Une fois l’exécution de l’Assistant terminée, l’ObjectDataSource inclut désormais une valeur pour sa propriété `InsertMethod`, ainsi que `InsertParameters` pour les quatre colonnes de catégorie, comme le montre le balisage déclaratif suivant :

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Étape 5 : création de l’interface d’insertion

Comme tout d’abord abordé dans [une vue d’ensemble de l’insertion, de la mise à jour et de la suppression de données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), le contrôle DetailsView fournit une interface d’insertion intégrée qui peut être utilisée lors de l’utilisation d’un contrôle de source de données qui prend en charge l’insertion. Ajoutez un contrôle DetailsView à cette page au-dessus du contrôle GridView qui restituera définitivement son interface d’insertion, ce qui permettra à un utilisateur d’ajouter rapidement une nouvelle catégorie. Lors de l’ajout d’une nouvelle catégorie dans le contrôle DetailsView, le contrôle GridView situé sous celui-ci s’actualise automatiquement et affiche la nouvelle catégorie.

Commencez par faire glisser un DetailsView de la boîte à outils vers le concepteur au-dessus du GridView, en affectant à sa propriété `ID` la valeur `NewCategory` et en effaçant les valeurs de propriété `Height` et `Width`. À partir de la balise active DetailsView s, liez-le à la `CategoriesDataSource` existante, puis activez la case à cocher Activer l’insertion.

[![lier le DetailsView à CategoriesDataSource et activer l’insertion](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Figure 6**: lier le contrôle DetailsView au `CategoriesDataSource` et activer l’insertion ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))

Pour restituer de façon permanente le DetailsView dans son interface d’insertion, définissez sa propriété `DefaultMode` sur `Insert`.

Notez que le contrôle DetailsView a cinq BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`et `BrochurePath` même si le `CategoryID` BoundField n’est pas restitué dans l’interface d’insertion, car sa propriété `InsertVisible` est définie sur `false`. Ces BoundFields existent parce qu’il s’agit des colonnes retournées par la méthode `GetCategories()`, qui est celle qui est appelée par ObjectDataSource pour récupérer ses données. Toutefois, pour l’insertion, nous ne souhaitons pas permettre à l’utilisateur de spécifier une valeur pour `NumberOfProducts`. En outre, nous devons leur permettre de télécharger une image pour la nouvelle catégorie et de télécharger un fichier PDF pour la brochure.

Supprimez complètement le `NumberOfProducts` BoundField du contrôle DetailsView, puis mettez à jour les propriétés de `HeaderText` du `CategoryName` et `BrochurePath` BoundFields en catégorie et à la brochure, respectivement. Ensuite, convertissez le `BrochurePath` BoundField en TemplateField et ajoutez un nouveau TemplateField pour l’image, en donnant à ce nouveau TemplateField une `HeaderText` valeur de Picture. Déplacez le `Picture` TemplateField afin qu’il se trouve entre les `BrochurePath` TemplateField et CommandField.

![Lier le DetailsView à CategoriesDataSource et activer l’insertion](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Figure 7**: lier le DetailsView au `CategoriesDataSource` et activer l’insertion

Si vous avez converti le `BrochurePath` BoundField en TemplateField via la boîte de dialogue Modifier les champs, le TemplateField comprend une `ItemTemplate`, `EditItemTemplate`et `InsertItemTemplate`. Toutefois, seul le `InsertItemTemplate` est nécessaire. n’hésitez donc pas à supprimer les deux autres modèles. À ce stade, la syntaxe déclarative de DetailsView s doit ressembler à ce qui suit :

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Ajout de contrôles FileUpload pour les champs brochure et image

Actuellement, le `BrochurePath` TemplateField s `InsertItemTemplate` contient une zone de texte, tandis que le `Picture` TemplateField ne contient aucun modèle. Nous devons mettre à jour ces deux TemplateField s `InsertItemTemplate` s pour utiliser les contrôles FileUpload.

Dans la balise active de DetailsView s, choisissez l’option modifier les modèles, puis sélectionnez le `BrochurePath` TemplateField s `InsertItemTemplate` dans la liste déroulante. Supprimez la zone de texte, puis faites glisser un contrôle FileUpload de la boîte à outils vers le modèle. Définissez les `ID` du contrôle FileUpload sur `BrochureUpload`. De même, ajoutez un contrôle FileUpload au `Picture` TemplateField s `InsertItemTemplate`. Définissez ce contrôle FileUpload s `ID` sur `PictureUpload`.

[![ajouter un contrôle FileUpload à InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Figure 8**: ajouter un contrôle FileUpload au `InsertItemTemplate` ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))

Après avoir apporté ces ajouts, la syntaxe déclarative des deux TemplateField est :

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Quand un utilisateur ajoute une nouvelle catégorie, nous voulons vérifier que la brochure et l’image sont du type de fichier correct. Pour la brochure, l’utilisateur doit fournir un fichier PDF. Pour l’image, nous avons besoin de l’utilisateur pour télécharger un fichier image, mais autorisez-vous tout fichier image ou fichier image d’un type particulier, tel que *les* fichiers GIF ou jpgs ? Afin d’autoriser différents types de fichiers, nous devons étendre le schéma `Categories` pour inclure une colonne qui capture le type de fichier afin que ce type puisse être envoyé au client par le biais de `Response.ContentType` dans `DisplayCategoryPicture.aspx`. Étant donné que nous ne disposons pas d’une telle colonne, il serait prudent de limiter les utilisateurs à la fourniture d’un type de fichier image spécifique. Les images existantes de la table `Categories` sont des bitmaps, mais les JPGs sont un format de fichier plus approprié pour les images gérées sur le Web.

Si un utilisateur charge un type de fichier incorrect, vous devez annuler l’insertion et afficher un message indiquant le problème. Ajoutez un contrôle Web Label sous le contrôle DetailsView. Affectez à sa propriété `ID` la valeur `UploadWarning`, effacez sa propriété `Text`, affectez à la propriété `CssClass` la valeur Warning et aux propriétés `Visible` et `EnableViewState` la valeur `false`. La classe CSS `Warning` est définie dans `Styles.css` et restitue le texte dans une police large, rouge, en italique et en gras.

> [!NOTE]
> Idéalement, les `CategoryName` et `Description` BoundFields seraient convertis en TemplateFields et leurs interfaces d’insertion personnalisées. Le `Description` l’insertion d’une interface, par exemple, serait probablement mieux adapté par le biais d’une zone de texte multiligne. Et étant donné que la colonne `CategoryName` n’accepte pas les valeurs `NULL`, un RequiredFieldValidator doit être ajouté pour s’assurer que l’utilisateur fournit une valeur pour le nouveau nom de catégorie. Ces étapes sont laissées à l’exercice du lecteur. Reportez-vous à [la rubrique Personnalisation de l’interface de modification des données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) pour une analyse approfondie de l’augmentation des interfaces de modification des données.

## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Étape 6 : enregistrement de la brochure téléchargée dans le système de fichiers du serveur Web s

Lorsque l’utilisateur entre les valeurs d’une nouvelle catégorie et clique sur le bouton Insérer, une publication (postback) se produit et le flux de travail d’insertion est déplié. Tout d’abord, l' [événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) DetailsView s`ItemInserting` se déclenche. Ensuite, la méthode d' `Insert()` ObjectDataSource s est appelée, ce qui entraîne l’ajout d’un nouvel enregistrement à la table `Categories`. Après cela, l’événement DetailsView s [`ItemInserted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) se déclenche.

Avant d’appeler la méthode ObjectDataSource s `Insert()`, nous devons tout d’abord vous assurer que les types de fichiers appropriés ont été téléchargés par l’utilisateur, puis enregistrer le fichier PDF de la brochure dans le système de fichiers du serveur Web s. Créez un gestionnaire d’événements pour l’événement DetailsView s `ItemInserting` et ajoutez le code suivant :

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Le gestionnaire d’événements commence par référencer le contrôle FileUpload `BrochureUpload` à partir des modèles DetailsView s. Ensuite, si une brochure a été téléchargée, l’extension des fichiers téléchargés est examinée. Si l’extension n’est pas. PDF, un avertissement s’affiche, l’insertion est annulée et l’exécution du gestionnaire d’événements se termine.

> [!NOTE]
> Le fait de s’appuyer sur l’extension des fichiers téléchargés n’est pas une technique de veille permettant de s’assurer que le fichier téléchargé est un document PDF. L’utilisateur peut avoir un document PDF valide avec l’extension `.Brochure`, ou peut avoir pris un document non PDF et lui donner une extension de `.pdf`. Le contenu binaire des fichiers doit être examiné par programme afin de vérifier de manière plus concluant le type de fichier. Ces approches approfondies, cependant, sont souvent excessives ; la vérification de l’extension est suffisante pour la plupart des scénarios.

Comme indiqué dans le didacticiel sur le téléchargement de [fichiers](uploading-files-cs.md) , il convient de veiller à ce que les fichiers soient enregistrés dans le système de fichiers, de sorte que le téléchargement d’un utilisateur ne remplace pas les autres. Pour ce didacticiel, nous allons essayer d’utiliser le même nom que le fichier téléchargé. Toutefois, s’il existe déjà un fichier dans le répertoire `~/Brochures` portant le même nom de fichier, nous ajouterons un nombre à la fin jusqu’à ce qu’un nom unique soit trouvé. Par exemple, si l’utilisateur charge un fichier de brochure nommé `Meats.pdf`, mais qu’il existe déjà un fichier nommé `Meats.pdf` dans le dossier `~/Brochures`, nous allons remplacer le nom de fichier enregistré par `Meats-1.pdf`. Si cela existe, nous essaierons `Meats-2.pdf`, et ainsi de suite, jusqu’à ce qu’un nom de fichier unique soit trouvé.

Le code suivant utilise la [méthode`File.Exists(path)`](https://msdn.microsoft.com/library/system.io.file.exists.aspx) pour déterminer si un fichier existe déjà avec le nom de fichier spécifié. Dans ce cas, il continue d’essayer de nouveaux noms de fichiers pour la brochure jusqu’à ce qu’aucun conflit ne soit trouvé.

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Une fois qu’un nom de fichier valide a été trouvé, le fichier doit être enregistré dans le système de fichiers et la valeur de l' `brochurePath``InsertParameter` ObjectDataSource doit être mise à jour pour que ce nom de fichier soit écrit dans la base de données. Comme nous l’avons vu dans le didacticiel sur le *chargement des fichiers* , le fichier peut être enregistré à l’aide de la méthode de `SaveAs(path)` du contrôle FileUpload. Pour mettre à jour le paramètre ObjectDataSource s `brochurePath`, utilisez la collection `e.Values`.

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Étape 7 : enregistrement de l’image chargée dans la base de données

Pour stocker l’image chargée dans le nouvel enregistrement de `Categories`, nous devons attribuer le contenu binaire chargé au paramètre ObjectDataSource s `picture` dans l’événement DetailsView s `ItemInserting`. Avant de procéder à cette attribution, toutefois, nous devons tout d’abord vous assurer que l’image chargée est un JPG et non un autre type d’image. Comme à l’étape 6, Let utilise l’extension de fichier image s chargée pour déterminer son type.

Tandis que la table `Categories` autorise `NULL` valeurs pour la colonne `Picture`, toutes les catégories ont actuellement une image. Let s force l’utilisateur à fournir une image lors de l’ajout d’une nouvelle catégorie via cette page. Le code suivant vérifie qu’une image a été téléchargée et qu’elle a une extension appropriée.

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Ce code doit être placé *avant* le code de l’étape 6. ainsi, en cas de problème avec le chargement de l’image, le gestionnaire d’événements se termine avant l’enregistrement du fichier de brochure dans le système de fichiers.

En supposant qu’un fichier approprié a été chargé, attribuez le contenu binaire téléchargé à la valeur s du paramètre image à l’aide de la ligne de code suivante :

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>Gestionnaire d’événements`ItemInserting`complet

À des fins d’exhaustivité, voici le `ItemInserting` gestionnaire d’événements dans son intégralité :

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Étape 8 : correction de la page de`DisplayCategoryPicture.aspx`

Essayons quelques instants pour tester l’interface d’insertion et `ItemInserting` gestionnaire d’événements qui a été créé au cours des dernières étapes. Accédez à la page `UploadInDetailsView.aspx` via un navigateur et essayez d’ajouter une catégorie, mais omettez l’image, ou spécifiez une image non JPG ou une brochure non PDF. Dans l’un de ces cas, un message d’erreur s’affiche et le flux de travail d’insertion a été annulé.

[![un message d’avertissement s’affiche si un type de fichier non valide est chargé](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Figure 9**: un message d’avertissement s’affiche si un type de fichier non valide est chargé ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))

Une fois que vous avez vérifié que la page requiert une image à charger et qu’elle n’accepte pas les fichiers non PDF ou non JPG, ajoutez une nouvelle catégorie avec une image JPG valide, en laissant le champ de brochure vide. Une fois que vous avez cliqué sur le bouton Insérer, la page est postback et un nouvel enregistrement est ajouté à la table `Categories` avec le contenu binaire d’image s téléchargé stocké directement dans la base de données. Le GridView est mis à jour et affiche une ligne pour la catégorie nouvellement ajoutée, mais comme le montre la figure 10, l’image de la nouvelle catégorie n’est pas rendue correctement.

[![l’image de la nouvelle catégorie ne s’affiche pas](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Figure 10**: l’image de la nouvelle catégorie ne s’affiche pas ([cliquez pour afficher l’image en plein écran](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))

La raison pour laquelle la nouvelle image n’est pas affichée est que la page `DisplayCategoryPicture.aspx` qui retourne une image de catégorie s spécifiée est configurée pour traiter les bitmaps qui ont un en-tête OLE. Cet en-tête de 78 octets est supprimé du contenu binaire de la colonne s `Picture` avant d’être renvoyé au client. Toutefois, le fichier JPG que nous venons de charger pour la nouvelle catégorie n’a pas cet en-tête OLE ; par conséquent, les octets nécessaires valides sont supprimés des données binaires de l’image.

Étant donné qu’il existe à présent des bitmaps avec des en-têtes OLE et JPGs dans la table `Categories`, nous devons mettre à jour `DisplayCategoryPicture.aspx` afin que l’en-tête OLE soit supprimé pour les huit catégories d’origine et ignore cette suppression pour les enregistrements de catégorie plus récents. Dans notre prochain didacticiel, nous allons examiner comment mettre à jour une image d’enregistrement existante, et nous allons mettre à jour toutes les anciennes photos de catégorie afin qu’elles soient JPGs. Pour l’instant, cependant, utilisez le code suivant dans `DisplayCategoryPicture.aspx` pour supprimer les en-têtes OLE des huit catégories d’origine :

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Avec cette modification, l’image JPG s’affiche à présent correctement dans le GridView.

[![les images JPG pour les nouvelles catégories sont correctement rendues](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Figure 11**: rendu correct des images jpg pour les nouvelles catégories ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))

## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Étape 9 : suppression de la brochure en face d’une exception

L’un des défis du stockage de données binaires sur le système de fichiers du serveur Web est qu’il introduit une déconnexion entre le modèle de données et ses données binaires. Par conséquent, chaque fois qu’un enregistrement est supprimé, les données binaires correspondantes du système de fichiers doivent également être supprimées. Cela peut également être joué lors de l’insertion de. Prenons le scénario suivant : un utilisateur ajoute une nouvelle catégorie, en spécifiant une image et une brochure valides. Lorsque vous cliquez sur le bouton Insérer, une publication (postback) se produit et l’événement DetailsView s `ItemInserting` se déclenche, ce qui enregistre la brochure dans le système de fichiers du serveur Web s. Ensuite, la méthode d' `Insert()` ObjectDataSource s est appelée, ce qui appelle la méthode `CategoriesBLL` classe s `InsertWithPicture`, qui appelle la méthode `InsertWithPicture` `CategoriesTableAdapter` s.

Maintenant, que se passe-t-il si la base de données est hors connexion ou si une erreur se produit dans l’instruction SQL `INSERT` ? Bien que l’insertion échoue, aucune nouvelle ligne de catégorie ne sera ajoutée à la base de données. Toutefois, nous avons toujours le fichier de brochure chargé sur le système de fichiers du serveur Web s ! Ce fichier doit être supprimé en cas d’exception pendant le flux de travail d’insertion.

Comme nous l’avons vu précédemment dans la [gestion des exceptions de niveau BLL et dal dans un didacticiel de Page ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , quand une exception est levée à partir des profondeurs de l’architecture, elle est propagée dans les différentes couches. Au niveau de la couche de présentation, nous pouvons déterminer si une exception s’est produite à partir de l’événement DetailsView s `ItemInserted`. Ce gestionnaire d’événements fournit également les valeurs du `InsertParameters`ObjectDataSource s. Par conséquent, nous pouvons créer un gestionnaire d’événements pour l’événement `ItemInserted` qui vérifie s’il existe une exception et, le cas échéant, supprime le fichier spécifié par le paramètre ObjectDataSource s `brochurePath` :

[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Récapitulatif

Un certain nombre d’étapes doivent être effectuées pour fournir une interface basée sur le Web afin d’ajouter des enregistrements qui incluent des données binaires. Si les données binaires sont stockées directement dans la base de données, il est probable que vous deviez mettre à jour l’architecture, en ajoutant des méthodes spécifiques pour gérer le cas dans lequel les données binaires sont insérées. Une fois l’architecture mise à jour, l’étape suivante consiste à créer l’interface d’insertion, qui peut être accomplie à l’aide d’un DetailsView qui a été personnalisé pour inclure un contrôle FileUpload pour chaque champ de données binaire. Les données chargées peuvent ensuite être enregistrées dans le système de fichiers du serveur Web ou être affectées à un paramètre de source de données dans le gestionnaire d’événements DetailsView s `ItemInserting`.

L’enregistrement de données binaires dans le système de fichiers nécessite davantage de planification que l’enregistrement de données directement dans la base de données. Vous devez choisir un modèle d’affectation de noms afin d’éviter qu’un utilisateur ait à remplacer un autre. En outre, des étapes supplémentaires doivent être effectuées pour supprimer le fichier chargé en cas d’échec de l’insertion de la base de données.

Nous avons maintenant la possibilité d’ajouter de nouvelles catégories au système avec une brochure et une image, mais nous avons encore vu comment mettre à jour les données binaires d’une catégorie existante ou comment supprimer correctement les données binaires d’une catégorie supprimée. Nous allons explorer ces deux rubriques dans le prochain didacticiel.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel sont Dave Gardner, Teresa Murphy et Bernadette Leigh. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](displaying-binary-data-in-the-data-web-controls-cs.md)
> [Suivant](updating-and-deleting-existing-binary-data-cs.md)
