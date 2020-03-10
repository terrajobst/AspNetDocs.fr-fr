---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Contrôles liés aux données | Microsoft Docs
author: microsoft
description: La plupart des applications ASP.NET reposent sur un certain degré de présentation des données à partir d’une source de données principale. Les contrôles liés aux données ont été une partie pivotante de l’interaction w...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 1154b38e0fa3d9d56cb407ae659c3b0d69871fda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603837"
---
# <a name="data-bound-controls"></a>Contrôles liés aux données

par [Microsoft](https://github.com/microsoft)

> La plupart des applications ASP.NET reposent sur un certain degré de présentation des données à partir d’une source de données principale. Les contrôles liés aux données ont été une partie pivotante de l’interaction avec les données dans les applications Web dynamiques. ASP.NET 2,0 introduit des améliorations significatives pour les contrôles liés aux données, notamment une nouvelle classe BaseDataBoundControl et une syntaxe déclarative.

La plupart des applications ASP.NET reposent sur un certain degré de présentation des données à partir d’une source de données principale. Les contrôles liés aux données ont été une partie pivotante de l’interaction avec les données dans les applications Web dynamiques. ASP.NET 2,0 introduit des améliorations significatives pour les contrôles liés aux données, notamment une nouvelle classe BaseDataBoundControl et une syntaxe déclarative.

BaseDataBoundControl agit comme classe de base pour la classe pour DataBoundControl et la classe HierarchicalDataBoundControl. Dans ce module, nous allons aborder les classes suivantes qui dérivent de pour DataBoundControl :

- AdRotator
- Contrôles de liste
- Affichage de grille
- FormView
- Détails

Nous aborderons également les classes suivantes qui dérivent de la classe HierarchicalDataBoundControl :

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Pour DataBoundControl, classe

La classe pour DataBoundControl est une classe abstraite (marquée MustInherit en VB) qui permet d’interagir avec des données tabulaires ou de style liste. Les contrôles suivants sont certains des contrôles qui dérivent de pour DataBoundControl.

## <a name="adrotator"></a>AdRotator

Le contrôle AdRotator vous permet d’afficher une bannière graphique sur une page Web qui est liée à une URL spécifique. Le graphique affiché est pivoté à l’aide des propriétés du contrôle. La fréquence d’une publicité particulière affichée sur une page peut être configurée à l’aide de la propriété **impressions** et les publicités peuvent être filtrées à l’aide d’un filtrage par mot clé.

Les contrôles AdRotator utilisent un fichier XML ou une table dans une base de données pour les données. Les attributs suivants sont utilisés dans les fichiers XML pour configurer le contrôle AdRotator.

### <a name="imageurl"></a>ImageUrl
URL d’une image à afficher pour la publicité.

### <a name="navigateurl"></a>NavigateUrl
URL à laquelle l’utilisateur doit se contenter lorsque l’utilisateur clique sur la publicité. Il doit s’agir d’un encodage d’URL.

### <a name="alternatetext"></a>AlternateText
Texte de remplacement affiché dans une info-bulle et lu par les lecteurs d’écran. S’affiche également lorsque l’image spécifiée par ImageUrl n’est pas disponible.

### <a name="keyword"></a>Mot clé
Définit un mot clé qui peut être utilisé lors de l’utilisation du filtrage par mot clé. S’il est spécifié, seules les publicités dont le mot clé correspond au filtre sont affichées.

### <a name="impressions"></a>Empreinte
Numéro de pondération qui détermine la fréquence à laquelle une publicité particulière est susceptible d’apparaître. Il est relatif à l’impression d’autres publicités dans le même fichier. La valeur maximale des impressions collectives pour toutes les publicités dans un fichier XML est 2,048 milliards 1.

### <a name="height"></a>Hauteur
Hauteur de la publicité en pixels.

### <a name="width"></a>Largeur
Largeur de la publicité, en pixels.

> [!NOTE]
> Les attributs de hauteur et de largeur remplacent la hauteur et la largeur du contrôle AdRotator lui-même.

Un fichier XML standard peut se présenter comme suit :

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Dans l’exemple ci-dessus, la publicité pour Contoso est deux fois plus susceptible d’apparaître comme AD pour le site Web ASP.NET en raison de la valeur de l’attribut impressions.

Pour afficher les publicités du fichier XML ci-dessus, ajoutez un contrôle AdRotator à une page et définissez la propriété **AdvertisementFile** de manière à ce qu’elle pointe vers le fichier XML comme indiqué ci-dessous :

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Si vous choisissez d’utiliser une table de base de données comme source de données pour votre contrôle AdRotator, vous devez d’abord configurer une base de données en utilisant le schéma suivant :

| **Nom de la colonne** | **Type de données** | **Description** |
| --- | --- | --- |
| ID | int | Clé primaire Cette colonne peut avoir n’importe quel nom. |
| ImageUrl | nvarchar (*longueur*) | URL relative ou absolue de l’image à afficher pour la publicité. |
| NavigateUrl | nvarchar (*longueur*) | URL cible de la publicité. Si vous ne fournissez pas de valeur, la publicité n’est pas un lien hypertexte. |
| AlternateText | nvarchar (*longueur*) | Texte affiché si l’image est introuvable. Dans certains navigateurs, le texte est affiché sous la forme d’une info-bulle. Le texte de remplacement est également utilisé pour l’accessibilité afin que les utilisateurs qui ne peuvent pas voir le graphique puissent entendre sa description lue à haute voix. |
| Mot clé | nvarchar (*longueur*) | Catégorie de la publicité sur laquelle la page peut être filtrée. |
| Empreinte | int(4) | Nombre qui indique la probabilité de la fréquence à laquelle la publicité est affichée. Plus le nombre est élevé, plus la publicité sera affichée. Le total de toutes les valeurs d’impressions dans le fichier XML ne peut pas dépasser 2,048 milliards-1. |
| Largeur | int(4) | Largeur de l’image en pixels. |
| Hauteur | int(4) | Hauteur de l’image en pixels. |

Dans les cas où vous disposez déjà d’une base de données avec un schéma différent, vous pouvez utiliser les propriétés **AlternateTextField**, **ImageUrlField**et **NavigateUrlField** pour mapper les attributs AdRotator à votre base de données existante. Pour afficher les données de la base de données dans le contrôle AdRotator, ajoutez un contrôle de source de données à la page, configurez la chaîne de connexion pour que le contrôle de source de données pointe vers votre base de données, puis affectez à la propriété **DataSourceID** du contrôle ADROTATOR l’ID du contrôle de source de données. Dans les cas où vous avez besoin de configurer des annonces AdRotator par programme, utilisez l’événement AdCreated. L’événement AdCreated accepte deux paramètres : un objet et l’autre instance de AdCreatedEventArgs. Le AdCreatedEventArgs est une référence à la publicité en cours de création.

L’extrait de code suivant définit les paramètres ImageUrl, NavigateUrl et AlternateText pour une annonce par programme :

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Contrôles de liste

Les contrôles List incluent ListBox, DropDownList, CheckBoxList, RadioButtonList et BulletedList. Chacun de ces contrôles peut être lié aux données d’une source de données. Ils utilisent un champ dans la source de données comme texte d’affichage et peuvent éventuellement utiliser un deuxième champ comme valeur d’un élément. Les éléments peuvent également être ajoutés statiquement au moment de la conception, et vous pouvez mélanger des éléments statiques et des éléments dynamiques ajoutés à partir d’une source de données.

Pour lier des données à un contrôle de liste, ajoutez un contrôle de source de données à la page. Spécifiez une commande SELECT pour le contrôle de source de données, puis affectez à la propriété DataSourceID du contrôle de liste l’ID du contrôle de source de données. Utilisez les propriétés **DataTextField** et **DataValueField** pour définir le texte à afficher et la valeur du contrôle. En outre, vous pouvez utiliser la propriété **DataTextFormatString** pour contrôler l’apparence du texte affiché comme suit :

| **Expression** | **Description** |
| --- | --- |
| Prix : {0:C} | Pour les données numériques/décimales. Affiche le littéral « Price : » suivi de nombres au format monétaire. Le format monétaire dépend du paramètre de culture spécifié dans l’attribut culture sur la directive **page** ou dans le fichier Web. config. |
| {0:D4} | Pour les données de type Integer. Ne peut pas être utilisé avec des nombres décimaux. Les entiers sont affichés dans un champ rempli à zéro qui a une largeur de quatre caractères. |
| {0:N2}% | Pour les données numériques. Affiche le nombre avec une précision de 2 décimales suivie du littéral « % ». |
| {0:000.0} | Pour les données numériques/décimales. Les nombres sont arrondis à une décimale. Les nombres de moins de trois chiffres sont remplis de zéros. |
| {0:D} | Pour les données de date/heure. Affiche le format de date longue (« jeudi 06 août, 1996 »). Le format de date dépend des paramètres de culture de la page ou du fichier Web.config. |
| {0:d} | Pour les données de date/heure. Affiche le format de date abrégée (« 12/31/99 »). |
| {0:yy-MM-dd} | Pour les données de date/heure. Affiche la date au format numérique année-mois-jour (96-08-06) |

## <a name="gridview"></a>Affichage de grille

Le contrôle GridView permet l’affichage et la modification des données tabulaires à l’aide d’une approche déclarative et est le successeur du contrôle DataGrid. Les fonctionnalités suivantes sont disponibles dans le contrôle GridView.

- Liaison à des contrôles de source de données, tels que SqlDataSource.
- Fonctionnalités de tri intégrées.
- Fonctionnalités intégrées de mise à jour et de suppression.
- Fonctionnalités de pagination intégrées.
- Fonctionnalités de sélection de ligne intégrées.
- Accès par programme au modèle objet GridView pour définir dynamiquement des propriétés, gérer des événements, et ainsi de suite.
- Plusieurs champs clés.
- Plusieurs champs de données pour les colonnes de lien hypertexte.
- Apparence personnalisable via les thèmes et les styles.

**Champs de colonne**

Chaque colonne du contrôle GridView est représentée par un objet DataControlField. Par défaut, la propriété AutoGenerateColumns est définie sur **true**, ce qui crée un objet AutoGeneratedField pour chaque champ de la source de données. Chaque champ est ensuite rendu sous la forme d’une colonne dans le contrôle GridView dans l’ordre dans lequel chaque champ apparaît dans la source de données. Vous pouvez également contrôler manuellement les champs de colonne qui s’affichent dans le contrôle **GridView** en affectant à la propriété **AutoGenerateColumns** la **valeur false** , puis en définissant votre propre collection de champs de colonne. Les différents types de champs de colonne déterminent le comportement des colonnes dans le contrôle.

Le tableau suivant répertorie les différents types de champs de colonne qui peuvent être utilisés.

| **Type de champ de colonne** | **Description** |
| --- | --- |
| BoundField | Affiche la valeur d’un champ dans une source de données. Il s’agit du type de colonne par défaut du contrôle GridView. |
| ButtonField | Affiche un bouton de commande pour chaque élément dans le contrôle GridView. Cela vous permet de créer une colonne de contrôles bouton personnalisés, tels que le bouton Ajouter ou supprimer. |
| CheckBoxField | Affiche une case à cocher pour chaque élément dans le contrôle GridView. Ce type de champ de colonne est couramment utilisé pour afficher des champs avec une valeur booléenne. |
| CommandField | Affiche des boutons de commande prédéfinis pour effectuer des opérations de sélection, de modification ou de suppression. |
| HyperLinkField | Affiche la valeur d’un champ dans une source de données sous la forme d’un lien hypertexte. Ce type de champ de colonne vous permet de lier un deuxième champ à l’URL du lien hypertexte. |
| ImageField | Affiche une image pour chaque élément dans le contrôle GridView. |
| TemplateField | Affiche le contenu défini par l’utilisateur pour chaque élément dans le contrôle GridView en fonction d’un modèle spécifié. Ce type de champ de colonne vous permet de créer un champ de colonne personnalisé. |

Pour définir une collection de champs de colonne de façon déclarative, commencez par ajouter des balises d’ouverture et de fermeture **&lt;les colonnes&gt;** entre les balises d’ouverture et de fermeture du contrôle GridView. Ensuite, répertoriez les champs de colonne que vous souhaitez inclure entre les balises d’ouverture et de fermeture **&lt;des colonnes&gt;** . Les colonnes spécifiées sont ajoutées à la collection Columns dans l’ordre indiqué. La collection **Columns** stocke tous les champs de colonne dans le contrôle et vous permet de gérer par programmation les champs de colonne dans le contrôle GridView.

Les champs de colonne déclarés explicitement peuvent être affichés en combinaison avec les champs de colonne générés automatiquement. Quand les deux sont utilisés, les champs de colonne déclarés explicitement sont affichés en premier, suivis des champs de colonne générés automatiquement.

## <a name="binding-to-data"></a>Lier à des données

Le contrôle GridView peut être lié à un contrôle de source de données (par exemple, **SqlDataSource**, **ObjectDataSource**, etc.), ainsi qu’à toute source de données qui implémente l’interface System. Collections. IEnumerable (telle que System. Data. DataView, System. Collections. ArrayList ou System. Collections. Hashtable). Utilisez l’une des méthodes suivantes pour lier le contrôle GridView au type de source de données approprié :

- Pour effectuer une liaison à un contrôle de source de données, affectez à la propriété DataSourceID du contrôle GridView la valeur d’ID du contrôle de source de données. Le contrôle GridView est automatiquement lié au contrôle de source de données spécifié et peut tirer parti des fonctionnalités du contrôle de source de données pour effectuer des fonctionnalités de tri, de mise à jour, de suppression et de pagination. Il s’agit de la méthode recommandée pour la liaison aux données.
- Pour effectuer une liaison à une source de données qui implémente l’interface System. Collections. IEnumerable, définissez par programmation la propriété DataSource du contrôle GridView sur la source de données, puis appelez la méthode DataBind. Lors de l’utilisation de cette méthode, le contrôle GridView ne fournit pas de fonctionnalités de tri, de mise à jour, de suppression et de pagination intégrées. Vous devez fournir cette fonctionnalité vous-même.

## <a name="data-operations"></a>Opérations de données

Le contrôle GridView fournit de nombreuses fonctionnalités intégrées qui permettent à l’utilisateur de trier, mettre à jour, supprimer, sélectionner et paginer des éléments dans le contrôle. Quand le contrôle GridView est lié à un contrôle de source de données, le contrôle GridView peut tirer parti des fonctionnalités du contrôle de source de données et fournir des fonctionnalités de tri, de mise à jour et de suppression automatiques.

> [!NOTE]
> Le contrôle GridView peut assurer la prise en charge du tri, de la mise à jour et de la suppression avec d’autres types de sources de données ; Toutefois, vous devrez fournir un gestionnaire d’événements approprié avec l’implémentation pour ces opérations.

Le tri permet à l’utilisateur de trier les éléments du contrôle GridView par rapport à une colonne spécifique en cliquant sur l’en-tête de la colonne. Pour activer le tri, affectez la valeur **true**à la propriété AllowSorting.

Les fonctionnalités de mise à jour, de suppression et de sélection automatiques sont activées lorsqu’un bouton d’un champ de colonne **ButtonField** ou **TemplateField** , avec pour nom de commande « modifier », « supprimer » et « sélectionner », est activé. Le contrôle GridView peut ajouter automatiquement un champ de colonne **CommandField** avec un bouton modifier, supprimer ou sélectionner si la propriété AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateSelectButton a la valeur **true**, respectivement.

> [!NOTE]
> L’insertion d’enregistrements dans la source de données n’est pas prise en charge directement par le contrôle GridView. Toutefois, il est possible d’insérer des enregistrements à l’aide du contrôle GridView conjointement au contrôle DetailsView ou FormView.

Au lieu d’afficher tous les enregistrements de la source de données en même temps, le contrôle GridView peut automatiquement fractionner les enregistrements en pages. Pour activer la pagination, affectez la valeur **true**à la propriété AllowPaging.

## <a name="customizing-the-user-interface"></a>Personnalisation de l’interface utilisateur

Vous pouvez personnaliser l’apparence du contrôle GridView en définissant les propriétés de style pour les différentes parties du contrôle. Le tableau suivant répertorie les différentes propriétés de style.

| **Propriété style** | **Description** |
| --- | --- |
| AlternatingRowStyle | Paramètres de style pour les lignes de données en alternance dans le contrôle GridView. Lorsque cette propriété est définie, les lignes de données s’affichent en alternance entre les paramètres RowStyle et les paramètres **AlternatingRowStyle** . |
| EditRowStyle | Paramètres de style de la ligne en cours de modification dans le contrôle GridView. |
| EmptyDataRowStyle | Paramètres de style de la ligne de données vide affichée dans le contrôle GridView lorsque la source de données ne contient pas d’enregistrements. |
| FooterStyle | Paramètres de style de la ligne de pied de page du contrôle GridView. |
| HeaderStyle | Paramètres de style de la ligne d’en-tête du contrôle GridView. |
| PagerStyle | Paramètres de style de la ligne de pagineur du contrôle GridView. |
| RowStyle | Paramètres de style pour les lignes de données dans le contrôle GridView. Lorsque la propriété **AlternatingRowStyle** est également définie, les lignes de données s’affichent en alternance entre les paramètres **RowStyle** et les paramètres **AlternatingRowStyle** . |
| SelectedRowStyle | Paramètres de style de la ligne sélectionnée dans le contrôle GridView. |

Vous pouvez également afficher ou masquer différentes parties du contrôle. Le tableau suivant répertorie les propriétés qui contrôlent les parties qui sont affichées ou masquées.

| **Propriété** | **Description** |
| --- | --- |
| ShowFooter | Affiche ou masque la section de pied de page du contrôle GridView. |
| ShowHeader | Affiche ou masque la section d’en-tête du contrôle GridView. |

### <a name="events"></a>événements

Le contrôle GridView fournit plusieurs événements que vous pouvez programmer. Cela vous permet d’exécuter une routine personnalisée à chaque fois qu’un événement se produit. Le tableau suivant répertorie les événements pris en charge par le contrôle GridView.

| **Event** | **Description** |
| --- | --- |
| PageIndexChanged | Se produit lors d’un clic sur l’un des boutons du pagineur, mais après que le contrôle GridView a géré l’opération de pagination. Cet événement est couramment utilisé lorsque vous devez effectuer une tâche une fois que l’utilisateur a accédé à une page différente dans le contrôle. |
| PageIndexChanging | Se produit lors d’un clic sur l’un des boutons du pagineur, mais avant que le contrôle GridView ne gère l’opération de pagination. Cet événement est souvent utilisé pour annuler l’opération de pagination. |
| RowCancelingEdit | Se produit lorsque l’utilisateur clique sur le bouton Annuler d’une ligne, mais avant que le contrôle GridView ne quitte le mode édition. Cet événement est souvent utilisé pour arrêter l’opération d’annulation. |
| RowCommand | Se produit lorsque l’utilisateur clique sur un bouton dans le contrôle GridView. Cet événement est souvent utilisé pour exécuter une tâche lorsque l’utilisateur clique sur un bouton dans le contrôle. |
| RowCreated | Se produit lorsqu’une nouvelle ligne est créée dans le contrôle GridView. Cet événement est souvent utilisé pour modifier le contenu d’une ligne lors de la création de la ligne. |
| RowDataBound | Se produit lorsqu’une ligne de données est liée à des données dans le contrôle GridView. Cet événement est souvent utilisé pour modifier le contenu d’une ligne lorsque la ligne est liée à des données. |
| RowDeleted | Se produit lorsque l’utilisateur clique sur le bouton supprimer d’une ligne, mais après que le contrôle GridView a supprimé l’enregistrement de la source de données. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de suppression. |
| RowDeleting | Se produit lorsque l’utilisateur clique sur le bouton supprimer d’une ligne, mais avant que le contrôle GridView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour annuler l’opération de suppression. |
| RowEditing | Se produit lorsque l’utilisateur clique sur le bouton modifier d’une ligne, mais avant que le contrôle GridView ne passe en mode édition. Cet événement est souvent utilisé pour annuler l’opération de modification. |
| RowUpdated | Se produit lorsque l’utilisateur clique sur le bouton mettre à jour d’une ligne, mais après que le contrôle GridView a mis à jour la ligne. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de mise à jour. |
| RowUpdating | Se produit lorsque l’utilisateur clique sur le bouton mettre à jour d’une ligne, mais avant que le contrôle GridView ne met à jour la ligne. Cet événement est souvent utilisé pour annuler l’opération de mise à jour. |
| SelectedIndexChanged | Se produit lorsque l’utilisateur clique sur le bouton de sélection d’une ligne, mais après que le contrôle GridView a géré l’opération de sélection. Cet événement est souvent utilisé pour effectuer une tâche une fois qu’une ligne est sélectionnée dans le contrôle. |
| SelectedIndexChanging | Se produit lorsque l’utilisateur clique sur le bouton de sélection d’une ligne, mais avant que le contrôle GridView ne gère l’opération de sélection. Cet événement est souvent utilisé pour annuler l’opération de sélection. |
| Triées | Se produit lors d’un clic sur le lien hypertexte pour trier une colonne, mais après que le contrôle GridView a géré l’opération de tri. Cet événement est couramment utilisé pour effectuer une tâche une fois que l’utilisateur a cliqué sur un lien hypertexte pour trier une colonne. |
| Tri | Se produit lorsque l’utilisateur clique sur le lien hypertexte pour trier une colonne, mais avant que le contrôle GridView ne gère l’opération de tri. Cet événement est souvent utilisé pour annuler l’opération de tri ou pour effectuer une routine de tri personnalisée. |

## <a name="formview"></a>FormView

Le contrôle FormView est utilisé pour afficher un enregistrement unique à partir d’une source de données. Elle est similaire au contrôle DetailsView, à la différence qu’elle affiche des modèles définis par l’utilisateur au lieu de champs de ligne. La création de vos propres modèles vous offre une plus grande souplesse pour contrôler la façon dont les données sont affichées. Le contrôle FormView prend en charge les fonctionnalités suivantes :

- Liaison à des contrôles de source de données, tels que SqlDataSource et ObjectDataSource.
- Fonctionnalités intégrées d’insertion.
- Fonctionnalités intégrées de mise à jour et de suppression.
- Fonctionnalités de pagination intégrées.
- Accès par programme au modèle objet FormView pour définir dynamiquement des propriétés, gérer des événements, et ainsi de suite.
- Apparence personnalisable par le biais de modèles, thèmes et styles définis par l’utilisateur.

## <a name="templates"></a>Modèles

Pour que le contrôle FormView affiche du contenu, vous devez créer des modèles pour les différentes parties du contrôle. La plupart des modèles sont facultatifs. Toutefois, vous devez créer un modèle pour le mode dans lequel le contrôle est configuré. Par exemple, un contrôle FormView qui prend en charge l’insertion d’enregistrements doit avoir un modèle d’insertion d’élément défini. Le tableau suivant répertorie les différents modèles que vous pouvez créer.

| **Type de modèle** | **Description** |
| --- | --- |
| EditItemTemplate | Définit le contenu de la ligne de données lorsque le contrôle FormView est en mode édition. Ce modèle contient généralement des contrôles d’entrée et des boutons de commande avec lesquels l’utilisateur peut modifier un enregistrement existant. |
| EmptyDataTemplate | Définit le contenu de la ligne de données vide affichée lorsque le contrôle FormView est lié à une source de données qui ne contient aucun enregistrement. Ce modèle contient généralement du contenu pour alerter l’utilisateur que la source de données ne contient aucun enregistrement. |
| FooterTemplate | Définit le contenu de la ligne de pied de page. Ce modèle contient généralement le contenu supplémentaire que vous souhaitez afficher dans la ligne de pied de page. En guise d’alternative, vous pouvez simplement spécifier le texte à afficher dans la ligne de pied de page en définissant la propriété FooterText. |
| HeaderTemplate | Définit le contenu de la ligne d’en-tête. Ce modèle contient généralement tout contenu supplémentaire que vous souhaitez afficher dans la ligne d’en-tête. En guise d’alternative, vous pouvez simplement spécifier le texte à afficher dans la ligne d’en-tête en définissant la propriété HeaderText. |
| ItemTemplate | Définit le contenu de la ligne de données lorsque le contrôle FormView est en mode lecture seule. Ce modèle contient généralement du contenu permettant d’afficher les valeurs d’un enregistrement existant. |
| InsertItemTemplate | Définit le contenu de la ligne de données lorsque le contrôle FormView est en mode insertion. Ce modèle contient généralement des contrôles d’entrée et des boutons de commande avec lesquels l’utilisateur peut ajouter un nouvel enregistrement. |
| PagerTemplate | Définit le contenu de la ligne de pagineur affichée lorsque la fonctionnalité de pagination est activée (lorsque la propriété AllowPaging a la valeur **true**). Ce modèle contient généralement des contrôles avec lesquels l’utilisateur peut accéder à un autre enregistrement. |

Les contrôles d’entrée dans le modèle d’élément de modification et le modèle d’insertion d’élément peuvent être liés aux champs d’une source de données à l’aide d’une expression de liaison bidirectionnelle. Cela permet au contrôle FormView d’extraire automatiquement les valeurs du contrôle d’entrée pour une opération de mise à jour ou d’insertion. Les expressions de liaison bidirectionnelle permettent également aux contrôles d’entrée dans un modèle d’élément de modification d’afficher automatiquement les valeurs de champ d’origine.

### <a name="binding-to-data"></a>Lier à des données

Le contrôle FormView peut être lié à un contrôle de source de données (par exemple, **SqlDataSource**, AccessDataSource, **ObjectDataSource** , etc.) ou à n’importe quelle source de données qui implémente l’interface System. Collections. IEnumerable (telle que System. Data. DataView, System. Collections. ArrayList et System. Collections. Hashtable). Utilisez l’une des méthodes suivantes pour lier le contrôle FormView au type de source de données approprié :

- Pour effectuer une liaison à un contrôle de source de données, affectez à la propriété DataSourceID du contrôle FormView la valeur d’ID du contrôle de source de données. Le contrôle FormView est automatiquement lié au contrôle de source de données spécifié et peut tirer parti des fonctionnalités du contrôle de source de données pour effectuer des fonctionnalités d’insertion, de mise à jour, de suppression et de pagination. Il s’agit de la méthode recommandée pour la liaison aux données.
- Pour effectuer une liaison à une source de données qui implémente l’interface **System. Collections. IEnumerable** , définissez par programmation la propriété DataSource du contrôle FormView sur la source de données, puis appelez la méthode DataBind. Lors de l’utilisation de cette méthode, le contrôle FormView ne fournit pas de fonctionnalités intégrées d’insertion, de mise à jour, de suppression et de pagination. Vous devez fournir cette fonctionnalité à l’aide de l’événement approprié.

## <a name="data-operations"></a>Opérations de données

Le contrôle FormView fournit de nombreuses fonctionnalités intégrées qui permettent à l’utilisateur de mettre à jour, supprimer, insérer et paginer des éléments dans le contrôle. Quand le contrôle FormView est lié à un contrôle de source de données, le contrôle FormView peut tirer parti des fonctionnalités du contrôle de source de données et fournir la fonctionnalité de mise à jour, de suppression, d’insertion et de pagination automatique. Le contrôle FormView peut assurer la prise en charge des opérations de mise à jour, de suppression, d’insertion et de pagination avec d’autres types de sources de données ; Toutefois, vous devez fournir un gestionnaire d’événements approprié avec l’implémentation pour ces opérations.

Étant donné que le contrôle FormView utilise des modèles, il ne fournit pas de moyen de générer automatiquement des boutons de commande pour effectuer des opérations de mise à jour, de suppression ou d’insertion. Vous devez inclure manuellement ces boutons de commande dans le modèle approprié. Le contrôle FormView reconnaît certains boutons dont les propriétés **CommandName** ont des valeurs spécifiques. Le tableau suivant répertorie les boutons de commande que le contrôle FormView reconnaît.

| **Button** | **Valeur CommandName** | **Description** |
| --- | --- | --- |
| Annuler | Cancel | Utilisé dans les opérations de mise à jour ou d’insertion pour annuler l’opération et ignorer les valeurs entrées par l’utilisateur. Le contrôle FormView retourne ensuite au mode spécifié par la propriété DefaultMode. |
| Supprimer | "Delete" | Utilisé lors de la suppression des opérations pour supprimer l’enregistrement affiché de la source de données. Déclenche les événements ItemDeleting et ItemDeleted. |
| Éditer | Modifiés | Utilisé dans les opérations de mise à jour pour mettre le contrôle FormView en mode édition. Le contenu spécifié dans la propriété **EditItemTemplate** s’affiche pour la ligne de données. |
| Insérer | Insérer | Utilisé lors de l’insertion d’opérations pour tenter d’insérer un nouvel enregistrement dans la source de données à l’aide des valeurs fournies par l’utilisateur. Déclenche les événements ItemInserting et ItemInserted. |
| Nouveau | Nouveaux | Utilisé dans les opérations d’insertion pour placer le contrôle FormView en mode insertion. Le contenu spécifié dans la propriété **InsertItemTemplate** est affiché pour la ligne de données. |
| Page | Pagination | Utilisé dans les opérations de pagination pour représenter un bouton dans la ligne de pagineur qui effectue la pagination. Pour spécifier l’opération de pagination, définissez la propriété **CommandArgument** du bouton sur « Next », « prev », « First », « Last » ou l’index de la page vers lequel naviguer. Déclenche les événements PageIndexChanging et PageIndexChanged. |
| Mise à jour | Mise à jour | Utilisé dans les opérations de mise à jour pour tenter de mettre à jour l’enregistrement affiché dans la source de données avec les valeurs fournies par l’utilisateur. Déclenche les événements ItemUpdating et ItemUpdated. |

Contrairement au bouton supprimer (qui supprime immédiatement l’enregistrement affiché), lorsque l’utilisateur clique sur le bouton modifier ou nouveau, le contrôle FormView passe respectivement en mode édition ou insertion. En mode édition, le contenu de la propriété **EditItemTemplate** est affiché pour l’élément de données actuel. En règle générale, le modèle modifier l’élément est défini de sorte que le bouton modifier est remplacé par un bouton mettre à jour et annuler. Les contrôles d’entrée appropriés pour le type de données du champ (par exemple, une zone de texte ou un contrôle de case à cocher) sont également généralement affichés avec la valeur d’un champ que l’utilisateur doit modifier. Le fait de cliquer sur le bouton mettre à jour met à jour l’enregistrement dans la source de données, tandis que le fait de cliquer sur le bouton Annuler abandonne les modifications.

De même, le contenu de la propriété **InsertItemTemplate** est affiché pour l’élément de données lorsque le contrôle est en mode insertion. Le modèle d’insertion d’élément est généralement défini de façon à ce que le nouveau bouton soit remplacé par un bouton d’insertion et d’annulation, tandis que des contrôles d’entrée vides sont affichés pour que l’utilisateur entre les valeurs du nouvel enregistrement. Cliquez sur le bouton Insérer pour insérer l’enregistrement dans la source de données, puis cliquez sur le bouton Annuler pour abandonner les modifications.

Le contrôle FormView fournit une fonctionnalité de pagination qui permet à l’utilisateur d’accéder à d’autres enregistrements dans la source de données. Lorsqu’elle est activée, une ligne de pagineur est affichée dans le contrôle FormView qui contient les contrôles de navigation entre les pages. Pour activer la pagination, affectez la valeur **true**à la propriété **AllowPaging** . Vous pouvez personnaliser la ligne de pagineur en définissant les propriétés des objets contenus dans les propriétés PagerStyle et PagerSettings. Au lieu d’utiliser l’interface utilisateur de ligne de pagineur intégrée, vous pouvez créer votre propre interface utilisateur à l’aide de la propriété **PagerTemplate** .

## <a name="customizing-the-user-interface"></a>Personnalisation de l’interface utilisateur

Vous pouvez personnaliser l’apparence du contrôle FormView en définissant les propriétés de style pour les différentes parties du contrôle. Le tableau suivant répertorie les différentes propriétés de style.

| **Propriété style** | **Description** |
| --- | --- |
| EditRowStyle | Paramètres de style de la ligne de données lorsque le contrôle FormView est en mode édition. |
| EmptyDataRowStyle | Paramètres de style de la ligne de données vide affichée dans le contrôle FormView lorsque la source de données ne contient pas d’enregistrements. |
| FooterStyle | Paramètres de style de la ligne de pied de page du contrôle FormView. |
| HeaderStyle | Paramètres de style de la ligne d’en-tête du contrôle FormView. |
| InsertRowStyle | Paramètres de style de la ligne de données lorsque le contrôle FormView est en mode insertion. |
| PagerStyle | Paramètres de style de la ligne de pagineur affichée dans le contrôle FormView lorsque la fonctionnalité de pagination est activée. |
| RowStyle | Paramètres de style de la ligne de données lorsque le contrôle FormView est en mode lecture seule. |

## <a name="events"></a>événements

Le contrôle FormView fournit plusieurs événements que vous pouvez programmer. Cela vous permet d’exécuter une routine personnalisée à chaque fois qu’un événement se produit. Le tableau suivant répertorie les événements pris en charge par le contrôle FormView.

| **Event** | **Description** |
| --- | --- |
| Événement | Se produit lors d’un clic sur un bouton dans un contrôle FormView. Cet événement est souvent utilisé pour exécuter une tâche lorsque l’utilisateur clique sur un bouton dans le contrôle. |
| ItemCreated | Se produit après la création de tous les objets FormViewRow dans le contrôle FormView. Cet événement est souvent utilisé pour modifier les valeurs d’un enregistrement avant qu’il ne soit affiché. |
| ItemDeleted | Se produit lorsqu’un clic est effectué sur un bouton supprimer (bouton dont la propriété **CommandName** a la valeur « Delete »), mais après que le contrôle FormView a supprimé l’enregistrement de la source de données. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de suppression. |
| ItemDeleting | Se produit lorsqu’un clic est effectué sur un bouton supprimer, mais avant que le contrôle FormView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour annuler l’opération de suppression. |
| ItemInserted | Se produit lorsqu’un clic est effectué sur un bouton d’insertion (bouton dont la propriété **CommandName** a la valeur « Insert »), mais après l’insertion de l’enregistrement par le contrôle FormView. Cet événement est souvent utilisé pour vérifier les résultats de l’opération d’insertion. |
| ItemInserting | Se produit lorsqu’un clic est effectué sur un bouton d’insertion, mais avant que le contrôle FormView n’insère l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération d’insertion. |
| ItemUpdated | Se produit lorsqu’un clic est effectué sur un bouton mettre à jour (un bouton dont la propriété **CommandName** a la valeur « Update »), mais après que le contrôle FormView a mis à jour la ligne. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de mise à jour. |
| ItemUpdating | Se produit lorsqu’un clic est effectué sur un bouton mettre à jour, mais avant que le contrôle FormView ne met à jour l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération de mise à jour. |
| ModeChanged | Se produit après que le contrôle FormView a modifié les modes (en mode édition, insertion ou lecture seule). Cet événement est souvent utilisé pour exécuter une tâche lorsque le contrôle FormView change de mode. |
| ModeChanging | Se produit avant que le contrôle FormView ne change de mode (en mode édition, insertion ou lecture seule). Cet événement est souvent utilisé pour annuler une modification de mode. |
| PageIndexChanged | Se produit lors d’un clic sur l’un des boutons du pagineur, mais après que le contrôle FormView a géré l’opération de pagination. Cet événement est couramment utilisé lorsque vous devez effectuer une tâche une fois que l’utilisateur a accédé à un autre enregistrement dans le contrôle. |
| PageIndexChanging | Se produit lors d’un clic sur l’un des boutons du pagineur, mais avant que le contrôle FormView ne gère l’opération de pagination. Cet événement est souvent utilisé pour annuler l’opération de pagination. |

## <a name="detailsview"></a>Détails

Le contrôle DetailsView est utilisé pour afficher un enregistrement unique à partir d’une source de données dans une table, où chaque champ de l’enregistrement est affiché dans une ligne de la table. Il peut être utilisé en association avec un contrôle GridView pour les scénarios maître/détail. Le contrôle DetailsView prend en charge les fonctionnalités suivantes :

- Liaison à des contrôles de source de données, tels que SqlDataSource.
- Fonctionnalités intégrées d’insertion.
- Fonctionnalités intégrées de mise à jour et de suppression.
- Fonctionnalités de pagination intégrées.
- Accès par programme au modèle objet DetailsView pour définir dynamiquement les propriétés, gérer les événements, etc.
- Apparence personnalisable via les thèmes et les styles.

## <a name="row-fields"></a>Champs de ligne

Chaque ligne de données du contrôle DetailsView est créée en déclarant un contrôle de champ. Les différents types de champ de ligne déterminent le comportement des lignes dans le contrôle. Les contrôles de champ dérivent de DataControlField. Le tableau suivant répertorie les différents types de champ de ligne qui peuvent être utilisés.

| **Type de champ de colonne** | **Description** |
| --- | --- |
| BoundField | Affiche la valeur d’un champ dans une source de données sous forme de texte. |
| ButtonField | Affiche un bouton de commande dans le contrôle DetailsView. Cela vous permet d’afficher une ligne avec un contrôle bouton personnalisé, tel qu’un bouton Ajouter ou supprimer. |
| CheckBoxField | Affiche une case à cocher dans le contrôle DetailsView. Ce type de champ de ligne est couramment utilisé pour afficher des champs avec une valeur booléenne. |
| CommandField | Affiche des boutons de commande intégrés pour effectuer des opérations de modification, d’insertion ou de suppression dans le contrôle DetailsView. |
| HyperLinkField | Affiche la valeur d’un champ dans une source de données sous la forme d’un lien hypertexte. Ce type de champ de ligne vous permet de lier un deuxième champ à l’URL du lien hypertexte. |
| ImageField | Affiche une image dans le contrôle DetailsView. |
| TemplateField | Affiche le contenu défini par l’utilisateur pour une ligne dans le contrôle DetailsView en fonction d’un modèle spécifié. Ce type de champ de ligne vous permet de créer un champ de ligne personnalisé. |

Par défaut, la propriété AutoGenerateRows est définie sur **true**, ce qui génère automatiquement un objet de champ de ligne lié pour chaque champ d’un type pouvant être lié dans la source de données. Les types valides pouvant être liés sont String, DateTime, Decimal, Guid et le jeu de types primitifs. Chaque champ est ensuite affiché dans une ligne sous forme de texte, dans l’ordre dans lequel chaque champ apparaît dans la source de données.

La génération automatique des lignes offre un moyen simple et rapide d’afficher tous les champs de l’enregistrement. Toutefois, pour utiliser les fonctionnalités avancées du contrôle DetailsView, vous devez déclarer explicitement les champs de ligne à inclure dans le contrôle DetailsView. Pour déclarer les champs de ligne, définissez d’abord la propriété **AutoGenerateRows** sur **false**. Ensuite, ajoutez des champs de **&lt;** ouvrant et fermant&gt;des balises entre les balises d’ouverture et de fermeture du contrôle DetailsView. Enfin, répertoriez les champs de ligne que vous souhaitez inclure entre les balises d’ouverture et de fermeture **&lt;&gt;** balises. Les champs de ligne spécifiés sont ajoutés à la collection Fields dans l’ordre indiqué. La collection **Fields** vous permet de gérer par programmation les champs de ligne dans le contrôle DetailsView.

> [!NOTE]
> Les champs de ligne générés automatiquement ne sont pas ajoutés à la collection de champs.

## <a name="binding-to-data"></a>Lier à des données

Le contrôle DetailsView peut être lié à un contrôle de source de données, tel que **SqlDataSource** ou AccessDataSource, ou à n’importe quelle source de données qui implémente l’interface System. Collections. IEnumerable, telle que System. Data. DataView, System. Collections. ArrayList et System. Collections. Hashtable.

Utilisez l’une des méthodes suivantes pour lier le contrôle DetailsView au type de source de données approprié :

- Pour effectuer une liaison à un contrôle de source de données, affectez à la propriété DataSourceID du contrôle DetailsView la valeur d’ID du contrôle de source de données. Le contrôle DetailsView est automatiquement lié au contrôle de source de données spécifié. Il s’agit de la méthode recommandée pour la liaison aux données.
- Pour effectuer une liaison à une source de données qui implémente l’interface **System. Collections. IEnumerable** , définissez par programmation la propriété DataSource du contrôle DetailsView sur la source de données, puis appelez la méthode DataBind.

## <a name="security"></a>Sécurité

Ce contrôle peut être utilisé pour afficher les entrées d’utilisateur, qui peuvent inclure un script client malveillant. Vérifiez les informations envoyées à partir d’un client pour le script exécutable, les instructions SQL ou un autre code avant de l’afficher dans votre application. ASP.NET fournit une fonctionnalité de validation de demande d’entrée pour bloquer le script et le HTML dans l’entrée utilisateur.

## <a name="data-operations"></a>Opérations de données

Le contrôle DetailsView fournit des fonctionnalités intégrées qui permettent à l’utilisateur de mettre à jour, supprimer, insérer et paginer des éléments dans le contrôle. Quand le contrôle DetailsView est lié à un contrôle de source de données, le contrôle DetailsView peut tirer parti des fonctionnalités du contrôle de source de données et fournir la fonctionnalité de mise à jour, de suppression, d’insertion et de pagination automatique.

Le contrôle DetailsView peut fournir une prise en charge pour les opérations de mise à jour, de suppression, d’insertion et de pagination avec d’autres types de sources de données ; Toutefois, vous devez fournir l’implémentation pour ces opérations dans un gestionnaire d’événements approprié.

Le contrôle DetailsView peut ajouter automatiquement un champ de ligne **CommandField** avec un bouton modifier, supprimer ou nouveau en affectant aux propriétés AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateInsertButton la **valeur true**, respectivement. Contrairement au bouton supprimer (qui supprime immédiatement l’enregistrement sélectionné), lorsque l’utilisateur clique sur le bouton modifier ou nouveau, le contrôle DetailsView passe en mode édition ou insertion, respectivement. En mode édition, le bouton modifier est remplacé par un bouton mettre à jour et annuler. Les contrôles d’entrée appropriés pour le type de données du champ (par exemple, une zone de texte ou un contrôle de case à cocher) sont affichés avec la valeur d’un champ que l’utilisateur doit modifier. Le fait de cliquer sur le bouton mettre à jour met à jour l’enregistrement dans la source de données, tandis que le fait de cliquer sur le bouton Annuler abandonne les modifications. De même, en mode insertion, le bouton nouveau est remplacé par un bouton Insérer et annuler, et des contrôles d’entrée vides sont affichés pour que l’utilisateur entre les valeurs du nouvel enregistrement.

Le contrôle DetailsView fournit une fonctionnalité de pagination qui permet à l’utilisateur d’accéder à d’autres enregistrements dans la source de données. Quand cette option est activée, les contrôles de navigation entre les pages s’affichent dans une ligne de pagineur. Pour activer la pagination, affectez la valeur **true**à la propriété AllowPaging. La ligne de pagineur peut être personnalisée à l’aide des propriétés PagerStyle et PagerSettings.

## <a name="customizing-the-user-interface"></a>Personnalisation de l’interface utilisateur

Vous pouvez personnaliser l’apparence du contrôle DetailsView en définissant les propriétés de style pour les différentes parties du contrôle. Le tableau suivant répertorie les différentes propriétés de style.

| **Propriété style** | **Description** |
| --- | --- |
| AlternatingRowStyle | Paramètres de style pour les lignes de données en alternance dans le contrôle DetailsView. Lorsque cette propriété est définie, les lignes de données s’affichent en alternance entre les paramètres RowStyle et les paramètres **AlternatingRowStyle** . |
| CommandRowStyle | Paramètres de style de la ligne contenant les boutons de commande intégrés dans le contrôle DetailsView. |
| EditRowStyle | Paramètres de style pour les lignes de données lorsque le contrôle DetailsView est en mode édition. |
| EmptyDataRowStyle | Paramètres de style de la ligne de données vide affichée dans le contrôle DetailsView lorsque la source de données ne contient pas d’enregistrements. |
| FooterStyle | Paramètres de style de la ligne de pied de page du contrôle DetailsView. |
| HeaderStyle | Paramètres de style de la ligne d’en-tête du contrôle DetailsView. |
| InsertRowStyle | Paramètres de style pour les lignes de données lorsque le contrôle DetailsView est en mode insertion. |
| PagerStyle | Paramètres de style de la ligne de pagineur du contrôle DetailsView. |
| RowStyle | Paramètres de style pour les lignes de données dans le contrôle DetailsView. Lorsque la propriété **AlternatingRowStyle** est également définie, les lignes de données s’affichent en alternance entre les paramètres **RowStyle** et les paramètres **AlternatingRowStyle** . |
| FieldHeaderStyle | Paramètres de style de la colonne d’en-tête du contrôle DetailsView. |

## <a name="events"></a>événements

Le contrôle DetailsView fournit plusieurs événements que vous pouvez programmer. Cela vous permet d’exécuter une routine personnalisée à chaque fois qu’un événement se produit. Le tableau suivant répertorie les événements pris en charge par le contrôle DetailsView. Le contrôle DetailsView hérite également de ces événements de ses classes de base : DataBinding, DataBound, Disposed, init, chargement, PreRender et Render.

| **Event** | **Description** |
| --- | --- |
| Événement | Se produit lorsque l’utilisateur clique sur un bouton dans le contrôle DetailsView. |
| ItemCreated | Se produit après la création de tous les objets DetailsViewRow dans le contrôle DetailsView. Cet événement est souvent utilisé pour modifier les valeurs d’un enregistrement avant qu’il ne soit affiché. |
| ItemDeleted | Se produit lorsqu’un clic est effectué sur un bouton supprimer, mais après que le contrôle DetailsView a supprimé l’enregistrement de la source de données. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de suppression. |
| ItemDeleting | Se produit lorsqu’un clic est effectué sur un bouton supprimer, mais avant que le contrôle DetailsView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour annuler l’opération de suppression. |
| ItemInserted | Se produit lorsqu’un clic est effectué sur un bouton d’insertion, mais après que le contrôle DetailsView a inséré l’enregistrement. Cet événement est souvent utilisé pour vérifier les résultats de l’opération d’insertion. |
| ItemInserting | Se produit lorsqu’un clic est effectué sur un bouton d’insertion, mais avant que le contrôle DetailsView n’insère l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération d’insertion. |
| ItemUpdated | Se produit lorsqu’un clic est effectué sur un bouton mettre à jour, mais après que le contrôle DetailsView a mis à jour la ligne. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de mise à jour. |
| ItemUpdating | Se produit lorsqu’un clic est effectué sur un bouton mettre à jour, mais avant que le contrôle DetailsView ne met à jour l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération de mise à jour. |
| ModeChanged | Se produit après que le contrôle DetailsView a modifié les modes (mode édition, insertion ou lecture seule). Cet événement est souvent utilisé pour exécuter une tâche lorsque le contrôle DetailsView change de mode. |
| ModeChanging | Se produit avant que le contrôle DetailsView ne change de mode (mode édition, insertion ou lecture seule). Cet événement est souvent utilisé pour annuler une modification de mode. |
| PageIndexChanged | Se produit lors d’un clic sur l’un des boutons du pagineur, mais après que le contrôle DetailsView a géré l’opération de pagination. Cet événement est couramment utilisé lorsque vous devez effectuer une tâche une fois que l’utilisateur a accédé à un autre enregistrement dans le contrôle. |
| PageIndexChanging | Se produit lors d’un clic sur l’un des boutons du pagineur, mais avant que le contrôle DetailsView ne gère l’opération de pagination. Cet événement est souvent utilisé pour annuler l’opération de pagination. |

## <a name="the-menu-control"></a>Contrôle Menu

Le contrôle menu dans ASP.NET 2,0 est conçu pour être un système de navigation complet. Il peut être facilement lié aux données dans des sources de données hiérarchiques telles que SiteMapDataSource.

Une structure de contrôles de menu peut être définie de façon déclarative ou dynamique et se compose d’un nœud racine unique et de n’importe quel nombre de sous-nœuds. Le code suivant définit de manière déclarative un menu pour le contrôle Menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Dans l’exemple ci-dessus, le nœud root. aspx est le nœud racine. Tous les autres nœuds sont imbriqués dans le nœud racine à différents niveaux.

Il existe deux types de menus que le contrôle de menu peut restituer ; menus statiques et menus dynamiques. Les menus statiques se composent d’éléments de menu qui sont toujours visibles. Les menus dynamiques sont constitués d’éléments de menu qui ne sont visibles que lorsque l’utilisateur pointe dessus avec la souris. Les clients peuvent souvent confondre des menus statiques avec des menus définis de façon déclarative et dynamique avec des menus qui sont liés aux DataBound au moment de l’exécution. En fait, les menus dynamiques et statiques ne sont pas liés à la méthode de remplissage. Les termes *statique* et *dynamique* font uniquement référence au fait que le menu s’affiche ou non de manière statique par défaut ou affiché uniquement lorsque l’utilisateur effectue une action.

La propriété **StaticDisplayLevels** est utilisée pour configurer le nombre de niveaux du menu statique qui sont donc affichés par défaut. Dans l’exemple ci-dessus, l’affectation de la valeur 2 à la propriété **StaticDisplayLevels** entraînerait l’affichage statique du nœud de démarrage, du nœud musique et du nœud films dans le menu. Tous les autres nœuds s’affichent dynamiquement lorsque l’utilisateur pointe sur le nœud parent.

La propriété **MaximumDynamicDisplayLevels** configure le nombre maximal de niveaux dynamiques que le menu peut afficher. Les menus dynamiques d’un niveau supérieur à la valeur spécifiée par la propriété **MaximumDynamicDisplayLevels** sont ignorés.

> [!NOTE]
> Il est presque certain que vous pouvez rencontrer des situations où les menus n’apparaissent pas en raison de la propriété MaximumDynamicDisplayLevels. Dans ce cas, assurez-vous que la propriété est suffisamment définie pour permettre l’affichage des menus clients.

## <a name="data-binding-the-menu-control"></a>Liaison de données avec le contrôle Menu

Le contrôle menu peut être lié à toute source de données hiérarchique telle que SiteMapDataSource ou XMLDataSource. SiteMapDataSource est la méthode la plus couramment utilisée pour la liaison de données à un contrôle de menu, car il est alimenté par le fichier Web. sitemap et son schéma fournit une API connue pour le contrôle Menu. La liste ci-dessous montre un fichier Web. sitemap simple.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Notez qu’il n’existe qu’un seul élément siteMapNode racine, dans ce cas, l’élément de début. Plusieurs attributs peuvent être configurés pour chaque siteMapNode. Les attributs les plus couramment utilisés sont les suivants :

- **URL** Spécifie l’URL à afficher lorsqu’un utilisateur clique sur l’élément de menu. Si cet attribut n’est pas présent, le nœud est simplement publié lorsque l’utilisateur clique dessus.
- **titre** Spécifie le texte affiché sur l’élément de menu.
- **Description** Utilisé comme documentation pour le nœud. S’affiche également sous la forme d’une info-bulle lorsque la souris pointe sur le nœud.
- **siteMapFile** Autorise les TabStrip imbriqués. Cet attribut doit pointer vers un fichier sitemap ASP.NET correctement formé.
- **rôles** Permet de contrôler l’apparence d’un nœud par le filtrage de sécurité ASP.NET.

Notez que bien que ces attributs soient tous facultatifs, le comportement du menu peut ne pas être celui qui est attendu s’ils ne sont pas spécifiés. Par exemple, si l’attribut *URL* est spécifié mais que l’attribut *Description* ne l’est pas, le nœud ne sera pas visible et il n’y aura aucun moyen d’accéder à l’URL spécifiée.

## <a name="controlling-a-menus-operation"></a>Contrôle d’une opération de menus

Il existe plusieurs propriétés qui affectent le fonctionnement d’un contrôle de menu ASP.NET ; la propriété **orientation** , la propriété **DisappearAfter** , la propriété **StaticItemFormatString** et la propriété **StaticPopoutImageUrl** en sont quelques-unes.

- L' **orientation** peut être *horizontale* ou *verticale* et détermine si les éléments de menu statiques sont disposés horizontalement dans une ligne ou verticalement et empilés les uns sur les autres. Cette propriété n’affecte pas les menus dynamiques.
- La propriété **DisappearAfter** configure la durée pendant laquelle un menu dynamique doit rester visible une fois que la souris a été déplacée. La valeur est spécifiée en millisecondes et est définie par défaut sur 500. Si vous affectez la valeur-1 à cette propriété, le menu ne disparaîtra jamais automatiquement. Dans ce cas, le menu ne disparaît que lorsque l’utilisateur clique en dehors du menu.
- La propriété **StaticItemFormatString** facilite la gestion des formulations de manière cohérente dans votre système de menus. Lorsque vous spécifiez cette propriété, *{0}* doit être entré à la place de la description qui apparaît dans la source de données. Par exemple, pour que l’élément de menu de l’exercice 1 soit visiter la page de nos produits, etc., vous devez spécifier visitez notre page de {0} pour StaticItemFormatString. Lors de l’exécution, ASP.NET remplace toute occurrence de {0} par la description correcte de l’élément de menu.
- La propriété **StaticPopoutImageUrl** spécifie l’image utilisée pour indiquer qu’un nœud de menu particulier a des nœuds enfants accessibles en pointant dessus. Les menus dynamiques continuent à utiliser l’image par défaut.

## <a name="templated-menu-controls"></a>Contrôles de menu basés sur un modèle

Le contrôle Menu est un contrôle basé sur un modèle et autorise deux éléments personnalisés différents. StaticItemTemplate et DynamicItemTemplate. À l’aide de ces modèles, vous pouvez facilement ajouter des contrôles serveur ou des contrôles utilisateur à vos menus.

Pour modifier les modèles dans Visual Studio .NET, cliquez sur le bouton balise active dans le menu, puis choisissez Modifier les modèles. Vous pouvez ensuite choisir entre la modification de StaticItemTemplate ou DynamicItemTemplate.

Les contrôles ajoutés au StaticItemTemplate s’affichent dans le menu statique lors du chargement de la page. Tous les contrôles ajoutés au DynamicItemTemplate s’affichent dans tous les menus contextuels.

## <a name="menu-events"></a>Événements de menu

Le contrôle Menu a deux événements qui lui sont propres ; **MenuItemClicked** et l’événement **MenuItemDatabound** .

L’événement MenuItemClicked est déclenché lorsqu’un utilisateur clique sur un élément de menu. L’événement MenuItemDatabound est déclenché lorsqu’un élément de menu est lié aux DataBound. Le **MenuEventArgs** passé au gestionnaire d’événements donne accès à l’élément de menu via la propriété Item.

## <a name="controlling-a-menus-appearance"></a>Contrôle de l’apparence des menus

Vous pouvez également affecter l’apparence d’un contrôle Menu à l’aide d’un ou plusieurs des nombreux styles disponibles pour les menus format. Parmi celles-ci figurent **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**et **DynamicHoverStyle**. Ces propriétés sont configurées à l’aide d’une chaîne de style HTML standard. Par exemple, les éléments suivants affectent le style des menus dynamiques.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Si vous utilisez l’un des styles de survol, vous devez ajouter un &lt;en-tête&gt; élément dans la page avec l’élément *runat* défini sur *Server*.

Les contrôles de menu prennent également en charge l’utilisation de thèmes ASP.NET 2,0.

## <a name="the-treeview-control"></a>Contrôle TreeView

Le contrôle TreeView affiche des données dans une structure de type arborescence. Comme avec le contrôle Menu, il peut être facilement lié aux données d’une source de données hiérarchique telle que SiteMapDataSource.

La première question que les clients sont susceptibles de poser sur le contrôle TreeView dans ASP.NET 2,0 est le fait qu’il soit lié au contrôle TreeView IE WebControl disponible pour ASP.NET 1. x. Ce n’est pas le fait. Le contrôle TreeView ASP.NET 2,0 a été écrit à partir de zéro et il offre une amélioration significative par rapport au WebControl TreeView d’Internet Explorer qui était précédemment disponible.

Je n’aborderai pas en détail comment lier un contrôle TreeView à un plan de site, car il est exécuté exactement de la même façon que le contrôle Menu. Toutefois, le contrôle TreeView présente des différences distinctes quant à son fonctionnement.

Par défaut, un contrôle TreeView apparaît entièrement développé. Pour modifier le niveau d’expansion lors du chargement initial, modifiez la propriété **ExpandDepth** du contrôle. Cela est particulièrement important dans les cas où l’arborescence est liée aux données lors du développement de nœuds particuliers.

## <a name="databinding-the-treeview-control"></a>Liaison de liaison du contrôle TreeView

Contrairement au contrôle Menu, l’arborescence TreeView se prête bien à la gestion de grandes quantités de données. Par conséquent, en plus de la liaison de données à un SiteMapDataSource ou à XMLDataSource, le TreeView est souvent lié aux données d’un jeu de données ou à d’autres données relationnelles. Dans les cas où le contrôle TreeView est lié à de grandes quantités de données, il est préférable de lier uniquement les données qui sont réellement visibles dans le contrôle. Vous pouvez ensuite lier des données à des données supplémentaires sous forme de nœuds TreeView.

Dans ce cas, la propriété **PopulateOnDemand** de l’arborescence doit avoir la valeur *true*. Vous devrez ensuite fournir une implémentation pour la méthode **TreeNodePopulate** .

## <a name="data-binding-without-postback"></a>Liaison de données sans publication (PostBack)

Notez que lorsque vous développez un nœud dans l’exemple précédent pour la première fois, la page est publiée et actualisée. Ce n’est pas un problème dans cet exemple, mais vous pouvez imaginer qu’il peut se trouver dans un environnement de production avec une grande quantité de données. Un scénario plus approprié serait celui dans lequel le TreeView remplirait toujours dynamiquement ses nœuds, mais sans une publication sur le serveur.

En définissant les propriétés **PopulateNodesFromClient** et **PopulateOnDemand** sur true, le contrôle TreeView ASP.net remplit dynamiquement les nœuds sans publication. Lorsque le nœud parent est développé, une demande XMLHttp est effectuée à partir du client et l’événement OnTreeNodePopulate est déclenché. Le serveur répond avec un îlot de données XML qui est ensuite utilisé pour lier les nœuds enfants.

ASP.NET crée dynamiquement le code client qui implémente cette fonctionnalité. Les balises de&gt; de script &lt;qui contiennent le script sont générées et pointant vers un fichier AXD. Par exemple, la liste ci-dessous montre les liens de script pour le code de script qui génère la demande XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Si vous parcourez le fichier AXD ci-dessus dans votre navigateur et que vous l’ouvrez, vous verrez le code qui implémente la demande XMLHttp. Cette méthode empêche les clients de modifier le fichier de script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Contrôle du fonctionnement du contrôle TreeView

Le contrôle TreeView possède plusieurs propriétés qui affectent le fonctionnement du contrôle. Les propriétés les plus évidentes sont **ShowCheckBoxes**, **ShowExpandCollapse**et **ShowLines**.

La propriété **ShowCheckBoxes** détermine si les nœuds affichent ou non une case à cocher lorsqu’ils sont rendus. Les valeurs valides pour cette propriété sont **None**, **root**, **parent**, **feuille**et **All**. Celles-ci affectent le contrôle TreeView comme suit :

| **Valeur de la propriété** | **Effect** |
| --- | --- |
| Aucun | Les cases à cocher ne sont pas affichées sur les nœuds. Il s'agit du paramètre par défaut. |
| Root | Une case à cocher s’affiche uniquement sur le nœud racine. |
| Parent | Une case à cocher s’affiche uniquement sur les nœuds qui ont des nœuds enfants. Ces nœuds enfants peuvent être des nœuds parents ou des nœuds terminaux. |
| Feuille | Une case à cocher s’affiche uniquement sur les nœuds qui n’ont pas de nœuds enfants. |
| Tout | Une case à cocher est affichée sur tous les nœuds. |

Lorsque les cases à cocher sont utilisées, la propriété **CheckedNodes** retourne une collection de nœuds TreeView vérifiés lors de la publication (postback).

La propriété **ShowExpandCollapse** contrôle l’apparence de l’image développer/réduire en regard des nœuds racine et parent. Si cette propriété est définie sur **false**, les nœuds TreeView sont rendus sous forme de liens hypertexte et sont développés/réduits en cliquant sur le lien.

La propriété **ShowLines** contrôle si les lignes sont affichées ou non pour connecter des nœuds parents à des nœuds enfants. Quand la **valeur est false** (valeur par défaut), aucune ligne n’est affichée. Lorsque la **valeur est true**, le contrôle TreeView utilise des images de lignes dans le dossier spécifié par la propriété **LineImagesFolder** .

Pour personnaliser l’apparence des lignes TreeView, Visual Studio .NET 2005 comprend un outil de concepteur de lignes. Vous pouvez accéder à cet outil à l’aide du bouton balise active du contrôle TreeView comme indiqué ci-dessous.

![](data-bound-controls/_static/image1.jpg)

**Figure 1**

Lorsque vous sélectionnez l’option de menu **personnaliser les images de ligne** , l’outil concepteur de lignes est lancé, ce qui vous permet de configurer l’apparence des lignes de l’arborescence.

## <a name="treeview-events"></a>Événements TreeView

Le contrôle TreeView contient les événements uniques suivants :

- SelectedNodeChanged se produit lorsqu’un nœud est sélectionné en fonction de la propriété **SelectAction** .
- TreeNodeCheckChanged se produit quand l’état d’un nœud cases à cocher est modifié.
- TreeNodeExpanded se produit lorsqu’un nœud est développé en fonction de la propriété **SelectAction** .
- TreeNodeCollapsed se produit lorsqu’un nœud est réduit.
- TreeNodeDataBound se produit lorsqu’un nœud est lié aux données.
- TreeNodePopulate se produit lorsqu’un nœud est rempli.

La propriété **SelectAction** vous permet de configurer l’événement qui est déclenché lorsqu’un nœud est sélectionné. La propriété SelectAction fournit les actions suivantes :

- TreeNodeSelectAction. Expand déclenche TreeNodeExpanded lorsque le nœud est sélectionné.
- TreeNodeSelectAction. None ne déclenche aucun événement lorsque le nœud est sélectionné.
- TreeNodeSelectAction. Select déclenche l’événement SelectedNodeChanged lorsque le nœud est sélectionné.
- TreeNodeSelectAction. SelectExpand déclenche à la fois l’événement SelectedNodeChanged et l’événement TreeNodeExpanded lorsque le nœud est sélectionné.

## <a name="controlling-appearance-with-styles"></a>Contrôle de l’apparence avec des styles

Le contrôle TreeView fournit de nombreuses propriétés pour contrôler l’apparence du contrôle avec des styles. Les propriétés suivantes sont disponibles.

| **Nom de la propriété** | **Contrôles** |
| --- | --- |
| HoverNodeStyle | Contrôle le style des nœuds lorsque la souris est positionnée dessus. |
| LeafNodeStyle | Contrôle le style des nœuds terminaux. |
| NodeStyle | Contrôle le style de tous les nœuds. Les styles de nœuds spécifiques (tels que LeafNodeStyle) remplacent ce style. |
| ParentNodeStyle | Contrôle le style de tous les nœuds parents. |
| RootNodeStyle | Contrôle le style du nœud racine. |
| SelectedNodeStyle | Contrôle le style du nœud sélectionné. |

Chacune de ces propriétés est en lecture seule. Toutefois, ils retournent chacun un objet **TreeNodeStyle** , et les propriétés de cet objet peuvent être modifiées à l’aide du format de *propriété-sous-propriété* . Par exemple, pour définir la propriété **ForeColor** de **SelectedNodeStyle**, vous devez utiliser la syntaxe suivante :

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Notez que la balise ci-dessus n’est pas fermée. En effet, lorsque vous utilisez la syntaxe déclarative indiquée ici, vous incluez également les nœuds d’arborescences dans le code HTML.

Les propriétés de style peuvent également être spécifiées dans le code à l’aide du format *Property. Subproperty* . Par exemple, pour définir la propriété **ForeColor** de **RootNodeStyle** dans le code, utilisez la syntaxe suivante :

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Pour obtenir une liste complète des différentes propriétés de style, consultez la documentation MSDN sur l’objet TreeNodeStyle.

## <a name="the-sitemappath-control"></a>Contrôle SiteMapPath

Le contrôle SiteMapPath fournit un contrôle de navigation de navigation de pain pour les développeurs ASP.NET. Comme les autres contrôles de navigation, elle peut être facilement liée à des sources de données hiérarchiques telles que SiteMapDataSource ou XmlDataSource.

Un contrôle SiteMapPath est constitué d’objets SiteMapNodeItem. Il existe trois types de nœuds : le nœud racine, les nœuds parents et le nœud actuel. Le nœud racine est le nœud situé en haut de la structure hiérarchique. Le nœud actuel représente la page actuelle. Tous les autres nœuds sont des nœuds parents.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Contrôle du fonctionnement du contrôle SiteMapPath

Les propriétés qui contrôlent le fonctionnement du contrôle SiteMapPath sont les suivantes :

| **Propriété** | **Description de la propriété** |
| --- | --- |
| ParentLevelsDisplayed | Contrôle le nombre de nœuds parents affichés. La valeur par défaut est-1, ce qui n’impose aucune restriction sur le nombre de nœuds parents affichés. |
| PathDirection | Contrôle la direction du SiteMapPath. Les valeurs valides sont RootToCurrent (par défaut) et CurrentToRoot. |
| PathSeparator | Chaîne qui contrôle le caractère qui sépare les nœuds dans un contrôle SiteMapPath. La valeur par défaut est :. |
| RenderCurrentNodeAsLink | Valeur booléenne qui contrôle si le nœud actuel est rendu sous forme de lien. La valeur par défaut est False. |
| SkipLinkText | Aide à l’accessibilité lorsque la page est affichée par les lecteurs d’écran. Cette propriété permet aux lecteurs d’écran d’ignorer le contrôle SiteMapPath. Pour désactiver cette fonctionnalité, affectez à la propriété la valeur String. Empty. |

## <a name="templated-sitemappath-controls"></a>Contrôles SiteMapPath basés sur un modèle

Le SiteMapControl est un contrôle basé sur un modèle et, par conséquent, vous pouvez définir différents modèles à utiliser pour afficher le contrôle. Pour modifier des modèles dans un contrôle SiteMapPath, cliquez sur le bouton balise active sur le contrôle, puis choisissez Modifier les modèles dans le menu. Le menu SiteMapTasks s’affiche, comme indiqué ci-dessous, où vous pouvez choisir entre les différents modèles disponibles.

![](data-bound-controls/_static/image2.jpg)

**Figure 2**

Le modèle **NodeTemplate** fait référence à n’importe quel nœud dans le SiteMapPath. Si le nœud est un nœud racine ou le nœud actuel et qu’un **RootNodeTemplate** ou **CurrentNodeTemplate** est configuré, le NodeTemplate est substitué.

## <a name="sitemappath-events"></a>Événements SiteMapPath

Le contrôle SiteMapPath a deux événements qui ne sont pas dérivés de la classe de contrôle ; l’événement **ItemCreated** et l’événement **ItemDataBound** . L’événement ItemCreated est déclenché lors de la création d’un élément SiteMapPath. ItemDataBound est déclenchée lorsque la méthode DataBind est appelée pendant la liaison de données d’un nœud SiteMapPath. Un objet **SiteMapNodeItemEventArgs** donne accès au SiteMapNodeItem spécifique via la propriété Item.

## <a name="controlling-appearance-with-styles"></a>Contrôle de l’apparence avec des styles

Les styles suivants sont disponibles pour la mise en forme d’un contrôle SiteMapPath.

| **Nom de la propriété** | **Contrôles** |
| --- | --- |
| CurrentNodeStyle | Contrôle le style du texte pour le nœud actuel. |
| RootNodeStyle | Contrôle le style du texte du nœud racine. |
| NodeStyle | Contrôle le style du texte pour tous les nœuds en supposant qu’un CurrentNodeStyle ou un RootNodeStyle ne s’applique pas. |

La propriété NodeStyle est remplacée par CurrentNodeStyle ou RootNodeStyle. Chacune de ces propriétés est en lecture seule et retourne un objet **style** . Pour affecter l’apparence d’un nœud à l’aide de l’une de ces propriétés, vous devez définir les propriétés de l’objet de style qui est retourné. Par exemple, le code ci-dessous modifie la propriété ForeColor du nœud actuel.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La propriété peut également être appliquée par programme comme suit :

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Si un modèle est appliqué, le style n’est pas appliqué.

## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Lab 1 : configuration d’un contrôle de menu ASP.NET

1. Créez un nouveau site Web.
2. Ajoutez un fichier de plan de site en sélectionnant fichier, nouveau, fichier, puis choisissez plan du site dans la liste des modèles de fichiers.
3. Ouvrez le plan de site (Web. sitemap par défaut) et modifiez-le afin qu’il ressemble à la liste ci-dessous. Les pages que vous liez dans le fichier de plan de site n’existent pas vraiment, mais ce n’est pas un problème pour cet exercice.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Ouvrez le formulaire Web par défaut dans Mode Création.
5. À partir de la section navigation de la boîte à outils, ajoutez un nouveau contrôle Menu à la page.
6. À partir de la section données de la boîte à outils, ajoutez un nouveau SiteMapDataSource. SiteMapDataSource utilisera automatiquement le fichier Web. sitemap dans votre site. (Le fichier Web. sitemap *doit* se trouver dans le dossier racine du site.)
7. Cliquez sur le contrôle Menu, puis sur le bouton balise active pour afficher la boîte de dialogue tâches de menu.
8. Dans la liste déroulante choisir la source de données, sélectionnez SiteMapDataSource1.
9. Cliquez sur le lien mise en forme automatique et choisissez un format pour le menu.
10. Dans le volet Propriétés, affectez à la propriété **StaticDisplayLevels** la valeur 2. Le contrôle menu doit maintenant afficher le nœud page d’hébergement, produits et services dans le concepteur.
11. Parcourez la page de votre navigateur pour utiliser le menu. (Étant donné que les pages que vous avez ajoutées à l’Explorateur de sites n’existent pas réellement, vous verrez une erreur lorsque vous tenterez de les parcourir.)

Expérimentez la modification des propriétés StaticDisplayLevels et MaximumDynamicDisplayLevels et observez comment elles affectent le mode de rendu du menu.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Lab 2 : liaison dynamique d’un contrôle TreeView

Cet exercice suppose que vous avez SQL Server exécuté localement et que la base de données Northwind est présente sur l’instance SQL Server. Si ces conditions ne sont pas remplies, modifiez la chaîne de connexion dans l’exemple. Notez que vous devrez peut-être également spécifier SQL Server authentification au lieu d’une connexion approuvée.

1. Créez un nouveau site Web.
2. Passez en mode code pour default. aspx et remplacez tout le code par le code ci-dessous. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Enregistrez la page sous TreeView. aspx.
4. Parcourez la page.
5. Lorsque la page s’affiche pour la première fois, affichez la source de la page dans votre navigateur. Notez que seuls les nœuds visibles ont été envoyés au client.
6. Cliquez sur le signe plus (+) en regard de n’importe quel nœud.
7. Affichez à nouveau la source sur la page. Notez que les nœuds qui viennent d’être affichés sont maintenant présents.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Lab 3 : affichage des détails et modification des données à l’aide d’un GridView et DetailsView

1. Créez un nouveau site Web.
2. Ajoutez un nouveau fichier Web. config au site Web.
3. Ajoutez une chaîne de connexion au fichier Web. config comme indiqué ci-dessous : 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Vous devrez peut-être modifier la chaîne de connexion en fonction de votre environnement.
4. Enregistrez et fermez le fichier web.config.
5. Ouvrez default. aspx et ajoutez un nouveau contrôle SqlDataSource.
6. Remplacez l’ID du contrôle SqlDataSource par **Products**.
7. Dans le menu **Tâches SqlDataSource** , cliquez sur **configurer la source de données**.
8. Sélectionnez **Northwind** dans la liste déroulante connexion, puis cliquez sur suivant.
9. Sélectionnez **Products** dans la liste déroulante **nom** et cochez les cases **ProductID**, **ProductName**, **UnitPrice**et **UnitsInStock** comme indiqué ci-dessous. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Cliquez sur **Next**.
11. Cliquez sur **Finish**.
12. Basculez en mode Source et examinez le code qui a été généré. Notez les **SelectCommand**, **DeleteCommand**, **InsertCommand**et **UpdateCommand** qui ont été ajoutés au contrôle SqlDataSource. Notez également les paramètres qui ont été ajoutés.
13. Basculez vers Mode Création et ajoutez un nouveau contrôle GridView à la page.
14. Sélectionnez **Products** dans la liste déroulante **choisir la source de données** .
15. Cochez la case **activer la pagination** et **activer la sélection** comme indiqué ci-dessous. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Cliquez sur le lien **modifier les colonnes** et assurez-vous que l' **option générer automatiquement les champs** est activée.
17. Cliquez sur **OK**.
18. Le contrôle GridView étant sélectionné, cliquez sur le bouton en regard de la propriété **DataKeyNames** dans le volet Propriétés.
19. Sélectionnez **ProductID** dans la liste **champs de données disponibles** , puis cliquez sur le bouton **&gt;** pour l’ajouter.
20. Cliquez sur OK.
21. Ajoutez un nouveau contrôle SqlDataSource à la page.
22. Remplacez l’ID du contrôle SqlDataSource par **Details**.
23. Dans le menu Tâches SqlDataSource, choisissez **configurer la source de données**.
24. Choisissez **Northwind** dans la liste déroulante, puis cliquez sur **suivant**.
25. Sélectionnez <strong>Products</strong> dans la liste déroulante <strong>nom</strong> et cochez la case <strong>\</strong > * dans la zone de liste <strong>colonnes</strong> .
26. Cliquez sur le bouton **Where** .
27. Sélectionnez **ProductID** dans la liste déroulante **colonne** .
28. Sélectionnez **=** dans la liste déroulante opérateur.
29. Sélectionnez **Control** dans la liste déroulante **source** .
30. Sélectionnez **GridView1** dans la liste déroulante **ID du contrôle** .
31. Cliquez sur le bouton **Ajouter** pour ajouter la clause WHERE.
32. Cliquez sur **OK**.
33. Cliquez sur le bouton **avancé** et cochez la case **générer des instructions INSERT, Update et Delete** .
34. Cliquez sur **OK**.
35. Cliquez sur **suivant** , puis sur **Terminer**.
36. Ajoutez un contrôle DetailsView à la page.
37. Dans la liste déroulante **choisir la source de données** , choisissez **Détails**.
38. Cochez la case **activer la modification** comme indiqué ci-dessous. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Enregistrez la page et parcourez default. aspx.
40. Cliquez sur le lien **Sélectionner** en regard de différents enregistrements pour afficher la mise à jour du contrôle DetailsView automatiquement.
41. Cliquez sur le lien **modifier** dans le contrôle DetailsView.
42. Apportez une modification à l’enregistrement, puis cliquez sur **mettre à jour**.
