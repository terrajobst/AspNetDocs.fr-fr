---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: Mise à jour et suppression de données binaires existantes (C#) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons vu comment le contrôle GridView facilite la modification et la suppression des données texte. Dans ce didacticiel, nous voyons comment le contrôle GridView effectue également...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 3e37381ee48fcda8e0e10374aa7a6ae53c3cc77c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587417"
---
# <a name="updating-and-deleting-existing-binary-data-c"></a>Mise à jour et suppression de données binaires existantes (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe) ou [Télécharger le PDF](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> Dans les didacticiels précédents, nous avons vu comment le contrôle GridView facilite la modification et la suppression des données texte. Dans ce didacticiel, nous voyons comment le contrôle GridView permet également de modifier et de supprimer des données binaires, que ces données binaires soient enregistrées dans la base de données ou stockées dans le système de fichiers.

## <a name="introduction"></a>Introduction

Au cours des trois derniers didacticiels, nous avons ajouté de nombreuses fonctionnalités pour travailler avec des données binaires. Nous avons commencé par ajouter une `BrochurePath` colonne au tableau `Categories` et mis à jour l’architecture en conséquence. Nous avons également ajouté la couche d’accès aux données et les méthodes de la couche de logique métier pour travailler avec la colonne `Picture` existante de la table des catégories, qui contient le contenu binaire d’un fichier image. Nous avons créé des pages Web pour présenter les données binaires dans un élément GridView un lien de téléchargement pour la brochure, avec l’image des catégories affichée dans un élément `<img>` et ajouté un DetailsView pour permettre aux utilisateurs d’ajouter une nouvelle catégorie et de charger la brochure et les données d’image.

Tout ce qui reste à implémenter est la possibilité de modifier et de supprimer des catégories existantes, que nous allons accomplir dans ce didacticiel à l’aide des fonctionnalités intégrées de modification et de suppression de GridView. Lorsque vous modifiez une catégorie, l’utilisateur est en mesure de télécharger une nouvelle image ou de faire en sorte que la catégorie continue à utiliser celle qui existe déjà. Pour la brochure, ils peuvent choisir d’utiliser la brochure existante, de télécharger une nouvelle brochure ou d’indiquer que la catégorie n’est plus associée à une brochure. Commençons !

## <a name="step-1-updating-the-data-access-layer"></a>Étape 1 : mise à jour de la couche d’accès aux données

La couche DAL contient des méthodes de `Insert`, `Update`et `Delete` générées automatiquement, mais ces méthodes ont été générées en fonction de la requête principale de `CategoriesTableAdapter` s, qui n’inclut pas la colonne `Picture`. Par conséquent, les méthodes `Insert` et `Update` n’incluent pas de paramètres pour la spécification des données binaires de l’image de la catégorie s. Comme nous l’avons fait dans le [didacticiel précédent](including-a-file-upload-option-when-adding-a-new-record-cs.md), nous devons créer une nouvelle méthode TableAdapter pour mettre à jour la table `Categories` lors de la spécification de données binaires.

Ouvrez le DataSet typé et, à partir du concepteur, cliquez avec le bouton droit sur l’en-tête `CategoriesTableAdapter` s et choisissez Ajouter une requête dans le menu contextuel pour lancer l’Assistant Configuration de requêtes TableAdapter. Cet Assistant commence par nous demander comment la requête TableAdapter doit accéder à la base de données. Choisissez utiliser des instructions SQL, puis cliquez sur suivant. L’étape suivante vous invite à entrer le type de requête à générer. Étant donné que nous avons recréé une requête pour ajouter un nouvel enregistrement à la table `Categories`, choisissez mettre à jour, puis cliquez sur suivant.

[![sélectionnez l’option mettre à jour](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**Figure 1**: sélectionner l’option mettre à jour ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image2.png))

Nous devons maintenant spécifier l’instruction SQL `UPDATE`. L’Assistant suggère automatiquement une instruction `UPDATE` correspondant à la requête principale du TableAdapter (une qui met à jour les valeurs `CategoryName`, `Description`et `BrochurePath`). Modifiez l’instruction de sorte que la colonne `Picture` soit incluse avec un paramètre `@Picture`, comme suit :

[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

Le dernier écran de l’Assistant nous invite à nommer la nouvelle méthode TableAdapter. Entrez `UpdateWithPicture`, puis cliquez sur Terminer.

[![nommez la nouvelle méthode TableAdapter UpdateWithPicture](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**Figure 2**: nommer la nouvelle méthode TableAdapter `UpdateWithPicture` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image4.png))

## <a name="step-2-adding-the-business-logic-layer-methods"></a>Étape 2 : ajout des méthodes de la couche de logique métier

Outre la mise à jour de la couche DAL, nous devons mettre à jour la couche BLL pour inclure des méthodes de mise à jour et de suppression d’une catégorie. Il s’agit des méthodes qui seront appelées à partir de la couche de présentation.

Pour supprimer une catégorie, nous pouvons utiliser la méthode de `Delete` générée automatiquement par `CategoriesTableAdapter` s. Ajoutez la méthode suivante à la classe `CategoriesBLL` :

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

Pour ce didacticiel, créons deux méthodes de mise à jour d’une catégorie : une qui attend les données d’image binaire et appelle la méthode `UpdateWithPicture` que nous venons d’ajouter au `CategoriesTableAdapter` et une autre qui accepte uniquement les valeurs `CategoryName`, `Description`et `BrochurePath` et utilise `CategoriesTableAdapter` instruction `Update` de classe s générée automatiquement. Le raisonnement derrière l’utilisation de deux méthodes est que dans certains cas, un utilisateur peut souhaiter mettre à jour l’image de catégorie s avec ses autres champs, auquel cas l’utilisateur devra Télécharger la nouvelle image. Les données binaires des images téléchargées peuvent ensuite être utilisées dans l’instruction `UPDATE`. Dans d’autres cas, l’utilisateur peut uniquement être intéressé par la mise à jour, par exemple, le nom et la description. Toutefois, si l’instruction `UPDATE` attend les données binaires de la colonne `Picture` également, nous devons également fournir ces informations. Cela nécessite un aller-retour supplémentaire à la base de données pour rétablir les données d’image de l’enregistrement en cours de modification. Par conséquent, nous souhaitons deux méthodes `UPDATE`. La couche de logique métier déterminera celle à utiliser selon que des données d’image sont fournies lors de la mise à jour de la catégorie.

Pour faciliter cette tâche, ajoutez deux méthodes à la classe `CategoriesBLL`, toutes deux nommées `UpdateCategory`. La première doit accepter trois `string` s, un tableau `byte` et un `int` comme paramètres d’entrée ; le deuxième, seulement trois `string` s et un `int`. Les paramètres d’entrée `string` sont pour le nom, la description et le chemin d’accès de fichier de la catégorie s, le tableau `byte` est pour le contenu binaire de l’image des catégories et le `int` identifie la `CategoryID` de l’enregistrement à mettre à jour. Notez que la première surcharge appelle la seconde si le tableau de `byte` passé est `null`:

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Étape 3 : copie sur les fonctionnalités d’insertion et de vue

Dans le [didacticiel précédent](including-a-file-upload-option-when-adding-a-new-record-cs.md) , nous avons créé une page nommée `UploadInDetailsView.aspx` qui répertorie toutes les catégories dans un GridView et fourni un DetailsView pour ajouter de nouvelles catégories au système. Dans ce didacticiel, nous allons étendre le contrôle GridView pour inclure la prise en charge de la modification et de la suppression. Plutôt que de continuer à travailler à partir de `UploadInDetailsView.aspx`, nous allons placer ce didacticiel sur les modifications apportées à la page `UpdatingAndDeleting.aspx` à partir du même dossier, `~/BinaryData`. Copiez et collez le balisage déclaratif et le code de `UploadInDetailsView.aspx` dans `UpdatingAndDeleting.aspx`.

Commencez par ouvrir la page `UploadInDetailsView.aspx`. Copiez toute la syntaxe déclarative dans l’élément `<asp:Content>`, comme illustré à la figure 3. Ensuite, ouvrez `UpdatingAndDeleting.aspx` et collez ce balisage dans son élément `<asp:Content>`. De même, copiez le code de la classe code-behind de la page `UploadInDetailsView.aspx` s vers `UpdatingAndDeleting.aspx`.

[![copier la balise déclarative à partir de UploadInDetailsView. aspx](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**Figure 3**: copier le balisage déclaratif d' `UploadInDetailsView.aspx` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image6.png))

Après la copie sur le balisage déclaratif et le code, visitez `UpdatingAndDeleting.aspx`. Vous devez voir la même sortie et avoir la même expérience utilisateur qu’avec `UploadInDetailsView.aspx` page du didacticiel précédent.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Étape 4 : ajout de la prise en charge de la suppression aux ObjectDataSource et GridView

Comme nous l’avons vu dans le didacticiel sur l' [insertion, la mise à jour et la suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , le contrôle GridView fournit des fonctionnalités de suppression intégrées et ces fonctionnalités peuvent être activées au niveau de la case à cocher si la source de données sous-jacente de la grille prend en charge la suppression. Actuellement, l’ObjectDataSource auquel le GridView est lié (`CategoriesDataSource`) ne prend pas en charge la suppression.

Pour y remédier, cliquez sur l’option configurer la source de données dans la balise active ObjectDataSource s pour lancer l’Assistant. Le premier écran montre que ObjectDataSource est configuré pour fonctionner avec la classe `CategoriesBLL`. Appuyez sur suivant. Actuellement, seules les propriétés ObjectDataSource s `InsertMethod` et `SelectMethod` sont spécifiées. Toutefois, l’Assistant a rempli automatiquement les listes déroulantes des onglets mettre à jour et supprimer avec les méthodes `UpdateCategory` et `DeleteCategory`, respectivement. En effet, dans la classe `CategoriesBLL`, nous avons marqué ces méthodes à l’aide du `DataObjectMethodAttribute` comme méthodes par défaut pour la mise à jour et la suppression.

Pour le moment, définissez la liste déroulante des onglets de mise à jour sur (aucun), mais laissez la liste déroulante onglet supprimer définie sur `DeleteCategory`. Nous reviendrons à cet Assistant à l’étape 6 pour ajouter la prise en charge de la mise à jour.

[![configurer ObjectDataSource pour utiliser la méthode DeleteCategory](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**Figure 4**: configurer ObjectDataSource pour utiliser la méthode `DeleteCategory` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image8.png))

> [!NOTE]
> À la fin de l’Assistant, Visual Studio peut vous demander si vous souhaitez actualiser les champs et les clés, ce qui permet de régénérer les champs de contrôles Web de données. Choisissez non, car si vous choisissez Oui, vous remplacerez toutes les personnalisations de champ que vous avez effectuées.

ObjectDataSource inclut désormais une valeur pour sa propriété `DeleteMethod`, ainsi qu’une `DeleteParameter`. Rappelez-vous que lorsque vous utilisez l’Assistant pour spécifier les méthodes, Visual Studio définit la propriété ObjectDataSource s `OldValuesParameterFormatString` sur `original_{0}`, ce qui entraîne des problèmes avec les appels de méthode Update et DELETE. Par conséquent, effacez complètement cette propriété ou réinitialisez-la sur la valeur par défaut, `{0}`. Si vous devez actualiser la mémoire sur cette propriété ObjectDataSource, consultez le didacticiel [vue d’ensemble de l’insertion, de la mise à jour et de la suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) .

À la fin de l’Assistant et en corrigeant le `OldValuesParameterFormatString`, le balisage déclaratif s doit ressembler à ce qui suit :

[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

Après avoir configuré l’ObjectDataSource, ajoutez des fonctionnalités de suppression au contrôle GridView en activant la case à cocher Activer la suppression dans la balise active GridView s. Cette opération ajoute un CommandField au GridView dont la propriété `ShowDeleteButton` a la valeur `true`.

[![activer la prise en charge de la suppression dans le GridView](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**Figure 5**: activer la prise en charge de la suppression dans le contrôle GridView ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image10.png))

Prenez un moment pour tester la fonctionnalité de suppression. Il existe une clé étrangère entre la `Products` table s `CategoryID` et la table `Categories` `CategoryID`. vous obtiendrez donc une exception de violation de contrainte de clé étrangère si vous tentez de supprimer l’une des huit premières catégories. Pour tester cette fonctionnalité, ajoutez une nouvelle catégorie, en fournissant à la fois une brochure et une image. Ma catégorie de test, illustrée à la figure 6, comprend un fichier de brochure de test nommé `Test.pdf` et une image de test. La figure 7 montre le GridView après l’ajout de la catégorie de test.

[![ajouter une catégorie de test avec une brochure et une image](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**Figure 6**: ajouter une catégorie de test avec une brochure et une image ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image12.png))

[![après avoir inséré la catégorie de test, elle est affichée dans le GridView](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**Figure 7**: après avoir inséré la catégorie de test, elle est affichée dans le GridView ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image14.png))

Dans Visual Studio, actualisez le Explorateur de solutions. Vous devez maintenant voir un nouveau fichier dans le dossier `~/Brochures`, `Test.pdf` (voir figure 8).

Ensuite, cliquez sur le lien supprimer dans la ligne catégorie de test, ce qui entraîne la publication de la page et la méthode de `CategoriesBLL` classe s `DeleteCategory`. Cela appellera la méthode DAL s `Delete`, provoquant l’envoi de l’instruction `DELETE` appropriée à la base de données. Les données sont ensuite reliées au GridView et le balisage est renvoyé au client avec la catégorie de test qui n’est plus présente.

Tandis que le flux de travail de suppression supprime correctement l’enregistrement de catégorie de test de la table `Categories`, il n’a pas supprimé son fichier de brochure du système de fichiers du serveur Web s. Actualisez le Explorateur de solutions et vous verrez que `Test.pdf` est toujours assis dans le dossier `~/Brochures`.

![Le fichier test. pdf n’a pas été supprimé du système de fichiers du serveur Web s](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**Figure 8**: le fichier `Test.pdf` n’a pas été supprimé du système de fichiers du serveur Web s

## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Étape 5 : suppression du fichier de brochure des catégories supprimées

L’un des inconvénients du stockage de données binaires externes à la base de données est que des étapes supplémentaires doivent être prises pour nettoyer ces fichiers lorsque l’enregistrement de base de données associé est supprimé. Les contrôles GridView et ObjectDataSource fournissent des événements qui se déclenchent à la fois avant et après l’exécution de la commande Delete. Nous devons en fait créer des gestionnaires d’événements pour les événements pré-et-action. Avant de supprimer le `Categories` enregistrement, nous devons déterminer son chemin d’accès aux fichiers PDF, mais nous ne voulons pas supprimer le fichier PDF avant la suppression de la catégorie en cas d’exception et la catégorie n’est pas supprimée.

L’événement GridView s [`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) se déclenche avant l’appel de la commande ObjectDataSource s Delete, tandis que son [événement`RowDeleted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) se déclenche après. Créez des gestionnaires d’événements pour ces deux événements à l’aide du code suivant :

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

Dans le gestionnaire d’événements `RowDeleting`, le `CategoryID` de la ligne en cours de suppression est extrait de la collection de `DataKeys` GridView s, accessible dans ce gestionnaire d’événements par le biais de la collection `e.Keys`. Ensuite, la classe `CategoriesBLL` s `GetCategoryByCategoryID(categoryID)` est appelée pour renvoyer des informations sur l’enregistrement en cours de suppression. Si l’objet `CategoriesDataRow` retourné a une valeur non`NULL``BrochurePath`, il est stocké dans la variable de page `deletedCategorysPdfPath` afin que le fichier puisse être supprimé dans le gestionnaire d’événements `RowDeleted`.

> [!NOTE]
> Au lieu de récupérer les détails de `BrochurePath` pour la suppression de l’enregistrement de `Categories` dans le gestionnaire d’événements `RowDeleting`, nous aurions pu ajouter le `BrochurePath` à la propriété GridView s `DataKeyNames` et accéder à la valeur des enregistrements par le biais de la collection `e.Keys`. Cela augmente légèrement la taille de l’état de la vue GridView s, mais elle réduit la quantité de code nécessaire et enregistre un voyage dans la base de données.

Après l’appel de la commande de suppression sous-jacente de ObjectDataSource, le gestionnaire d’événements `RowDeleted` GridView est déclenché. Si aucune exception n’a été levée lors de la suppression des données et qu’il y a une valeur pour `deletedCategorysPdfPath`, le fichier PDF est supprimé du système de fichiers. Notez que ce code supplémentaire n’est pas nécessaire pour nettoyer les données binaires de catégorie s associées à son image. Si les données de l’image sont stockées directement dans la base de données, la suppression de la `Categories` ligne supprime également les données d’image de cette catégorie.

Après avoir ajouté les deux gestionnaires d’événements, réexécutez ce cas de test. Lors de la suppression de la catégorie, son fichier PDF associé est également supprimé.

La mise à jour d’un enregistrement existant de données binaires associées présente des défis intéressants. Le reste de ce didacticiel explore l’ajout de fonctionnalités de mise à jour à la brochure et à l’image. L’étape 6 explore les techniques de mise à jour des informations sur les brochures, tandis que l’étape 7 examine la mise à jour de l’image.

## <a name="step-6-updating-a-category-s-brochure"></a>Étape 6 : mise à jour d’une brochure catégorie s

Comme indiqué dans le didacticiel [vue d’ensemble de l’insertion, de la mise à jour et de la suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , le contrôle GridView offre une prise en charge intégrée de la modification au niveau des lignes qui peut être implémentée par le cycle d’une case à cocher si sa source de données sous-jacente est correctement configurée. Actuellement, la `CategoriesDataSource` ObjectDataSource n’est pas encore configurée pour inclure la prise en charge de la mise à jour. nous allons donc ajouter cela dans.

Cliquez sur le lien configurer la source de données dans l’Assistant ObjectDataSource s et passez à la deuxième étape. En raison de l' `DataObjectMethodAttribute` utilisé dans `CategoriesBLL`, la liste déroulante mettre à jour doit être automatiquement renseignée avec la surcharge `UpdateCategory` qui accepte quatre paramètres d’entrée (pour toutes les colonnes, mais `Picture`). Modifiez ce paramètre afin qu’il utilise la surcharge avec cinq paramètres.

[![configurer ObjectDataSource pour utiliser la méthode UpdateCategory qui comprend un paramètre pour image](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**Figure 9**: configurer ObjectDataSource pour utiliser la méthode `UpdateCategory` qui comprend un paramètre pour `Picture` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image16.png))

ObjectDataSource inclut désormais une valeur pour sa propriété `UpdateMethod`, ainsi que les `UpdateParameter` s correspondants. Comme indiqué à l’étape 4, Visual Studio définit la propriété ObjectDataSource s `OldValuesParameterFormatString` sur `original_{0}` lors de l’utilisation de l’Assistant Configuration de la source de données. Cela entraîne des problèmes avec les appels de méthode Update et DELETE. Par conséquent, effacez complètement cette propriété ou réinitialisez-la sur la valeur par défaut, `{0}`.

À la fin de l’Assistant et en corrigeant le `OldValuesParameterFormatString`, le balisage déclaratif s doit ressembler à ce qui suit :

[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

Pour activer les fonctionnalités d’édition intégrées de GridView, cochez l’option Activer la modification dans la balise active GridView s. Cela permet de définir la propriété CommandField s `ShowEditButton` sur `true`, ce qui entraîne l’ajout d’un bouton modifier (et les boutons de mise à jour et d’annulation pour la ligne en cours de modification).

[![configurer le contrôle GridView pour prendre en charge la modification](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**Figure 10**: configurer le contrôle GridView pour prendre en charge la modification ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image18.png))

Accédez à la page via un navigateur et cliquez sur l’un des boutons de modification de la ligne. Les `CategoryName` et `Description` BoundFields sont rendus sous forme de zones de texte. Le `BrochurePath` TemplateField n’a pas de `EditItemTemplate`. il continue donc à afficher ses `ItemTemplate` un lien vers la brochure. La `Picture` ImageField est rendue sous la forme d’une zone de texte dont la valeur de la `DataImageUrlField` ImageField est assignée à la propriété `Text`, dans ce cas `CategoryID`.

[![le GridView ne dispose pas d’une interface de modification pour BrochurePath](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**Figure 11**: le GridView ne dispose pas d’une interface de modification pour `BrochurePath` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image20.png))

## <a name="customizing-thebrochurepaths-editing-interface"></a>Personnalisation de l’interface de modification des`BrochurePath`s

Nous devons créer une interface de modification pour le `BrochurePath` TemplateField, qui permet à l’utilisateur d’effectuer l’une des opérations suivantes :

- Laissez la brochure catégorie s,
- Mettez à jour la brochure catégorie s en téléchargeant une nouvelle brochure, ou
- Supprimez complètement la brochure catégorie s (dans le cas où la catégorie n’a plus de brochure associée).

Nous devons également mettre à jour l’interface de modification de `Picture` ImageField, mais nous allons le faire à l’étape 7.

À partir de la balise active GridView s, cliquez sur le lien modifier les modèles et sélectionnez le `BrochurePath` TemplateField s `EditItemTemplate` dans la liste déroulante. Ajoutez un contrôle Web RadioButtonList à ce modèle, en affectant à sa propriété `ID` la valeur `BrochureOptions` et à sa propriété `AutoPostBack` la valeur `true`. Dans la Fenêtre Propriétés, cliquez sur les ellipses dans la propriété `Items`, qui affichera l’éditeur de collections `ListItem`. Ajoutez les trois options suivantes avec `Value` s 1, 2 et 3, respectivement :

- Utiliser la brochure actuelle
- Supprimer la brochure actuelle
- Télécharger une nouvelle brochure

Définissez la propriété First `ListItem` s `Selected` sur `true`.

![Ajouter trois ListItems à RadioButtonList](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**Figure 12**: ajouter trois `ListItem` s à RadioButtonList

Sous RadioButtonList, ajoutez un contrôle FileUpload nommé `BrochureUpload`. Affectez à sa propriété `Visible` la valeur `false`.

[![ajouter un contrôle RadioButtonList et FileUpload à l’EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**Figure 13**: ajouter un contrôle RadioButtonList et FileUpload au `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image22.png))

Ce RadioButtonList fournit les trois options pour l’utilisateur. L’idée est que le contrôle FileUpload s’affiche uniquement si la dernière option, télécharger une nouvelle brochure, est sélectionnée. Pour ce faire, créez un gestionnaire d’événements pour l’événement RadioButtonList s `SelectedIndexChanged` et ajoutez le code suivant :

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

Étant donné que les contrôles RadioButtonList et FileUpload se trouvent dans un modèle, nous devons écrire un peu de code pour accéder à ces contrôles par programmation. Le gestionnaire d’événements `SelectedIndexChanged` reçoit une référence du RadioButtonList dans le paramètre d’entrée `sender`. Pour accéder au contrôle FileUpload, nous devons accéder au contrôle parent de RadioButtonList et utiliser la méthode `FindControl("controlID")` à partir de là. Une fois que nous avons une référence aux contrôles RadioButtonList et FileUpload, la propriété FileUpload Control s `Visible` est définie sur `true` uniquement si RadioButtonList s `SelectedValue` est égal à 3, ce qui correspond à la `Value` de l' `ListItem`Télécharger la nouvelle brochure.

Une fois ce code en place, prenez un moment pour tester l’interface de modification. Cliquez sur le bouton modifier pour une ligne. Au départ, l’option utiliser la brochure actuelle doit être sélectionnée. La modification de l’index sélectionné entraîne une publication (postback). Si la troisième option est sélectionnée, le contrôle FileUpload s’affiche ; sinon, il est masqué. La figure 14 illustre la première fois que l’utilisateur clique sur le bouton modifier. La figure 15 illustre l’interface après la sélection de l’option Télécharger une nouvelle brochure.

[![initialement, l’option utiliser la brochure actuelle est sélectionnée](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**Figure 14**: à l’origine, l’option utiliser la brochure actuelle est sélectionnée ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image24.png))

[![le choix de l’option Télécharger une nouvelle brochure affiche le contrôle FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**Figure 15**: le choix de l’option Télécharger une nouvelle brochure affiche le contrôle FileUpload ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image26.png))

## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Enregistrement du fichier de brochure et mise à jour de la colonne`BrochurePath`

Lorsque l’utilisateur clique sur le bouton de mise à jour de GridView, son événement `RowUpdating` se déclenche. La commande de mise à jour de ObjectDataSource s est appelée, puis le GridView s `RowUpdated` événement est déclenché. Comme avec le flux de travail de suppression, nous devons créer des gestionnaires d’événements pour ces deux événements. Dans le gestionnaire d’événements `RowUpdating`, nous devons déterminer l’action à entreprendre en fonction de la `SelectedValue` du `BrochureOptions` RadioButtonList :

- Si le `SelectedValue` a la valeur 1, nous voulons continuer à utiliser le même paramètre de `BrochurePath`. Par conséquent, nous devons définir le paramètre ObjectDataSource s `brochurePath` sur la valeur `BrochurePath` existante de l’enregistrement en cours de mise à jour. Le paramètre de `brochurePath` ObjectDataSource s peut être défini à l’aide de `e.NewValues["brochurePath"] = value`.
- Si le `SelectedValue` a la valeur 2, nous voulons définir la valeur de l’enregistrement de `BrochurePath` sur `NULL`. Pour ce faire, vous pouvez définir le paramètre ObjectDataSource s `brochurePath` sur `Nothing`, ce qui entraîne l’utilisation d’une base de données `NULL` dans l’instruction `UPDATE`. Si un fichier de brochure existant est en cours de suppression, vous devez supprimer le fichier existant. Toutefois, nous voulons uniquement effectuer cette opération si la mise à jour se termine sans lever d’exception.
- Si le `SelectedValue` est 3, nous voulons nous assurer que l’utilisateur a chargé un fichier PDF, puis l’enregistre dans le système de fichiers et met à jour la valeur de la colonne enregistrement s `BrochurePath`. En outre, s’il existe un fichier de brochure qui est remplacé, nous devons supprimer le fichier précédent. Toutefois, nous voulons uniquement effectuer cette opération si la mise à jour se termine sans lever d’exception.

Les étapes nécessaires à l’exécution des `SelectedValue` RadioButtonList sont virtuellement identiques à celles utilisées par le gestionnaire d’événements du `ItemInserting` DetailsView s. Ce gestionnaire d’événements est exécuté lorsqu’un nouvel enregistrement de catégorie est ajouté à partir du contrôle DetailsView que nous avons ajouté dans le [didacticiel précédent](including-a-file-upload-option-when-adding-a-new-record-cs.md). Par conséquent, il est nécessaire de refactoriser cette fonctionnalité dans des méthodes distinctes. En particulier, j’ai déplacé les fonctionnalités communes en deux méthodes :

- `ProcessBrochureUpload(FileUpload, out bool)` accepte comme entrée une instance de contrôle FileUpload et une valeur booléenne de sortie qui spécifie si l’opération de suppression ou de modification doit se poursuivre ou si elle doit être annulée en raison d’une erreur de validation. Cette méthode retourne le chemin d’accès au fichier enregistré ou `null` si aucun fichier n’a été enregistré.
- `DeleteRememberedBrochurePath` supprime le fichier spécifié par le chemin d’accès dans la variable de page `deletedCategorysPdfPath` si `deletedCategorysPdfPath` n’est pas `null`.

Le code de ces deux méthodes suit. Notez la similarité entre `ProcessBrochureUpload` et le gestionnaire d’événements DetailsView s `ItemInserting` du didacticiel précédent. Dans ce didacticiel, j’ai mis à jour les gestionnaires d’événements DetailsView s pour utiliser ces nouvelles méthodes. Téléchargez le code associé à ce didacticiel pour voir les modifications apportées aux gestionnaires d’événements du contrôle DetailsView s.

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

Les gestionnaires d’événements GridView s `RowUpdating` et `RowUpdated` utilisent les méthodes `ProcessBrochureUpload` et `DeleteRememberedBrochurePath`, comme le montre le code suivant :

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

Notez la manière dont le gestionnaire d’événements `RowUpdating` utilise une série d’instructions conditionnelles pour exécuter l’action appropriée en fonction de la valeur de propriété `BrochureOptions` RadioButtonList s `SelectedValue`.

Une fois ce code en place, vous pouvez modifier une catégorie et faire en sorte qu’elle utilise sa brochure actuelle, n’utiliser aucune brochure ou en télécharger une nouvelle. Poursuivez et testez-le. Définissez des points d’arrêt dans les gestionnaires d’événements `RowUpdating` et `RowUpdated` pour obtenir une idée du flux de travail.

## <a name="step-7-uploading-a-new-picture"></a>Étape 7 : chargement d’une nouvelle image

L’interface de modification `Picture` ImageField s est rendue sous la forme d’une zone de texte remplie avec la valeur de sa propriété `DataImageUrlField`. Pendant le workflow d’édition, le contrôle GridView passe un paramètre à l’ObjectDataSource avec le paramètre s Name la valeur de la propriété ImageField s `DataImageUrlField` et la valeur du paramètre s la valeur entrée dans la zone de texte de l’interface d’édition. Ce comportement est approprié lorsque l’image est enregistrée en tant que fichier sur le système de fichiers et que le `DataImageUrlField` contient l’URL complète de l’image. Dans ce cas, l’interface de modification affiche l’URL de l’image s dans la zone de texte, que l’utilisateur peut modifier et qu’il a enregistré dans la base de données. Cette interface par défaut n’autorise pas l’utilisateur à télécharger une nouvelle image, mais elle lui permet de modifier l’URL de l’image de la valeur actuelle à une autre. Pour ce didacticiel, toutefois, l’interface de modification par défaut de ImageField ne suffit pas, car les `Picture` données binaires sont stockées directement dans la base de données et la propriété `DataImageUrlField` contient uniquement les `CategoryID`.

Pour mieux comprendre ce qui se passe dans notre didacticiel lorsqu’un utilisateur modifie une ligne avec un ImageField, prenons l’exemple suivant : un utilisateur modifie une ligne avec `CategoryID` 10, ce qui entraîne le rendu de la `Picture` ImageField sous la forme d’une zone de texte avec la valeur 10. Imaginez que l’utilisateur modifie la valeur de cette zone de texte en 50 et clique sur le bouton mettre à jour. Une publication (postback) a lieu et le GridView crée initialement un paramètre nommé `CategoryID` avec la valeur 50. Toutefois, avant que le contrôle GridView n’envoie ce paramètre (et les paramètres `CategoryName` et `Description`), il ajoute les valeurs de la collection `DataKeys`. Par conséquent, il remplace le paramètre `CategoryID` par la ligne actuelle s sous-jacente `CategoryID` valeur, 10. En bref, l’interface de modification de ImageField n’a aucun effet sur le flux de travail d’édition pour ce didacticiel, car les noms de la propriété ImageField s `DataImageUrlField` et de la valeur Grid `DataKey` sont identiques.

Bien que le ImageField facilite l’affichage d’une image basée sur les données de la base de données, nous ne souhaitons pas fournir de zone de texte dans l’interface de modification. Au lieu de cela, nous souhaitons offrir un contrôle FileUpload que l’utilisateur final peut utiliser pour modifier l’image des catégories. Contrairement à la valeur `BrochurePath`, pour ces didacticiels, nous avons décidé de demander que chaque catégorie doive avoir une image. Par conséquent, nous n’avons pas besoin de laisser l’utilisateur indiquer qu’il n’y a pas d’image associée. l’utilisateur peut charger une nouvelle image ou laisser l’image actuelle telle quelle.

Pour personnaliser l’interface de modification ImageField s, nous devons la convertir en TemplateField. À partir de la balise active GridView s, cliquez sur le lien modifier les colonnes, sélectionnez le ImageField, puis cliquez sur le lien convertir ce champ en TemplateField.

![Convertir le ImageField en TemplateField](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**Figure 16**: convertir le ImageField en TemplateField

La conversion de ImageField en TemplateField de cette manière génère un TemplateField avec deux modèles. Comme le montre la syntaxe déclarative suivante, le `ItemTemplate` contient un contrôle Web image dont la propriété `ImageUrl` est assignée à l’aide de la syntaxe DataBinding basée sur les propriétés ImageField s `DataImageUrlField` et `DataImageUrlFormatString`. Le `EditItemTemplate` contient une zone de texte dont la propriété `Text` est liée à la valeur spécifiée par la propriété `DataImageUrlField`.

[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

Nous devons mettre à jour le `EditItemTemplate` pour utiliser un contrôle FileUpload. À partir de la balise active GridView s, cliquez sur le lien modifier les modèles, puis sélectionnez le `Picture` TemplateField s `EditItemTemplate` dans la liste déroulante. Dans le modèle, une zone de texte doit être supprimée. Ensuite, faites glisser un contrôle FileUpload de la boîte à outils vers le modèle, en affectant à son `ID` la valeur `PictureUpload`. Ajoutez également le texte pour modifier l’image des catégories, puis spécifiez une nouvelle image. Pour conserver la même image de catégorie, laissez le champ vide pour le modèle.

[![ajouter un contrôle FileUpload à EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**Figure 17**: ajouter un contrôle FileUpload au `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image28.png))

Une fois l’interface de modification personnalisée, affichez votre progression dans un navigateur. Lorsque vous affichez une ligne en lecture seule, l’image des catégories s’affiche comme avant, mais le fait de cliquer sur le bouton modifier rend la colonne d’image sous forme de texte avec un contrôle FileUpload.

[![l’interface de modification comprend un contrôle FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**Figure 18**: l’interface de modification comprend un contrôle FileUpload ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-cs/_static/image30.png))

Rappelez-vous que ObjectDataSource est configuré pour appeler la méthode `CategoriesBLL` classe s `UpdateCategory` qui accepte comme entrée les données binaires de l’image en tant que tableau de `byte`. Toutefois, si ce tableau a une valeur de `null`, la surcharge de `UpdateCategory` de remplacement est appelée, ce qui émet l’instruction SQL de `UPDATE` qui ne modifie pas la colonne `Picture`, laissant l’image actuelle de la catégorie intacte. Par conséquent, dans le gestionnaire d’événements `RowUpdating` GridView, nous devons référencer par programmation le contrôle `PictureUpload` FileUpload et déterminer si un fichier a été téléchargé. Si aucun n’a été téléchargé, nous ne souhaitons *pas* spécifier de valeur pour le paramètre `picture`. En revanche, si un fichier a été téléchargé dans le contrôle `PictureUpload` FileUpload, nous voulons nous assurer qu’il s’agit d’un fichier JPG. Si c’est le cas, nous pouvons envoyer son contenu binaire au ObjectDataSource par le biais du paramètre `picture`.

Comme avec le code utilisé à l’étape 6, la majeure partie du code nécessaire ici existe déjà dans le gestionnaire d’événements `ItemInserting` DetailsView s. Par conséquent, j’ai refactorisé la fonctionnalité commune dans une nouvelle méthode, `ValidPictureUpload`et mis à jour le gestionnaire d’événements `ItemInserting` pour utiliser cette méthode.

Ajoutez le code suivant au début du gestionnaire d’événements `RowUpdating` GridView s. Il est important que ce code précède le code qui enregistre le fichier de brochure puisque nous ne voulons pas enregistrer la brochure dans le système de fichiers du serveur Web si un fichier image non valide est téléchargé.

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

La méthode `ValidPictureUpload(FileUpload)` prend un contrôle FileUpload comme son seul paramètre d’entrée et vérifie l’extension des fichiers téléchargés pour s’assurer que le fichier téléchargé est un JPG ; elle est appelée uniquement si un fichier image est téléchargé. Si aucun fichier n’est téléchargé, le paramètre image n’est pas défini et utilise donc sa valeur par défaut de `null`. Si une image a été téléchargée et `ValidPictureUpload` retourne `true`, le paramètre `picture` reçoit les données binaires de l’image chargée ; Si la méthode retourne `false`, le flux de travail de mise à jour est annulé et le gestionnaire d’événements s’est arrêté.

Le code de la méthode `ValidPictureUpload(FileUpload)`, qui a été refactorisé à partir du gestionnaire d’événements DetailsView s `ItemInserting`, est le suivant :

[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Étape 8 : remplacement des images des catégories d’origine par JPGs

Rappelez-vous que les images d’origine de huit catégories sont des fichiers bitmap encapsulés dans un en-tête OLE. Maintenant que nous avons ajouté la fonctionnalité de modification d’une image d’enregistrement existante, prenez un moment pour remplacer ces bitmaps par JPGs. Si vous souhaitez continuer à utiliser les images de catégorie actuelles, vous pouvez les convertir en JPGs en procédant comme suit :

1. Enregistrez les images bitmap sur votre disque dur. Accédez à la page `UpdatingAndDeleting.aspx` de votre navigateur et pour chacune des huit premières catégories, cliquez avec le bouton droit sur l’image et choisissez d’enregistrer l’image.
2. Ouvrez l’image dans l’éditeur d’images de votre choix. Vous pouvez utiliser Microsoft Paint, par exemple.
3. Enregistrez la bitmap en tant qu’image JPG.
4. Mettez à jour l’image de catégorie s via l’interface de modification, à l’aide du fichier JPG.

Après la modification d’une catégorie et le chargement de l’image JPG, l’image n’est pas rendue dans le navigateur, car la page de `DisplayCategoryPicture.aspx` élimine les 78 premiers octets des images des huit premières catégories. Corrigez ce problème en supprimant le code qui effectue la suppression de l’en-tête OLE. Après cela, le gestionnaire d’événements `DisplayCategoryPicture.aspx``Page_Load` doit avoir simplement le code suivant :

[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> La `UpdatingAndDeleting.aspx` page s insertion et modification d’interfaces peut utiliser un peu plus de travail. Les `CategoryName` et `Description` BoundFields dans DetailsView et GridView doivent être converties en TemplateFields. Étant donné que `CategoryName` n’autorise pas les valeurs `NULL`, un RequiredFieldValidator doit être ajouté. Et la zone de texte `Description` doit probablement être convertie en zone de texte multiligne. Je laisse ces touches de finition en guise d’exercice pour vous.

## <a name="summary"></a>Récapitulatif

Ce didacticiel termine notre examen de l’utilisation des données binaires. Dans ce didacticiel et les trois précédents, nous avons vu comment les données binaires peuvent être stockées sur le système de fichiers ou directement dans la base de données. Un utilisateur fournit des données binaires au système en sélectionnant un fichier à partir de son disque dur et en le téléchargeant sur le serveur Web, où il peut être stocké dans le système de fichiers ou inséré dans la base de données. ASP.NET 2,0 comprend un contrôle FileUpload qui permet de fournir une interface de ce type aussi simple que glisser-déplacer. Toutefois, comme indiqué dans le didacticiel sur le [téléchargement de fichiers](uploading-files-cs.md) , le contrôle FileUpload est bien adapté aux chargements de fichiers relativement petits, idéalement au-delà de la limite d’un mégaoctet. Nous avons également abordé la manière d’associer des données téléchargées au modèle de données sous-jacent, ainsi que de modifier et de supprimer les données binaires des enregistrements existants.

Notre prochain ensemble de didacticiels explore différentes techniques de mise en cache. La mise en cache permet d’améliorer les performances globales d’une application en prenant les résultats des opérations coûteuses et en les stockant dans un emplacement qui peut être accédé plus rapidement.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](including-a-file-upload-option-when-adding-a-new-record-cs.md)
> [Suivant](uploading-files-vb.md)
