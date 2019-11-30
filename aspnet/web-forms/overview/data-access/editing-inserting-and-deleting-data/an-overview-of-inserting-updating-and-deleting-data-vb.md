---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: Vue d’ensemble de l’insertion, de la mise à jour et de la suppression de données (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir comment mapper les méthodes Insert (), Update () et Delete () d’ObjectDataSource aux méthodes des classes BLL, ainsi qu’à la configuration...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 79491118ba1cbbc8c1b67ca9646a817d941f17ba
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74630927"
---
# <a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>Vue d’ensemble de l’insertion, de la mise à jour et de la suppression de données (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe) ou [Télécharger le PDF](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> Dans ce didacticiel, nous allons apprendre à mapper les méthodes Insert (), Update () et Delete () d’ObjectDataSource aux méthodes des classes BLL, ainsi qu’à configurer les contrôles GridView, DetailsView et FormView pour fournir des fonctionnalités de modification de données.

## <a name="introduction"></a>Introduction

Au cours des quelques didacticiels précédents, nous avons examiné comment afficher des données dans une page ASP.NET à l’aide des contrôles GridView, DetailsView et FormView. Ces contrôles fonctionnent simplement avec les données qui leur sont fournies. En règle générale, ces contrôles accèdent aux données via l’utilisation d’un contrôle de source de données, tel que ObjectDataSource. Nous avons vu comment l’ObjectDataSource agit comme un proxy entre la page ASP.NET et les données sous-jacentes. Quand un GridView doit afficher des données, il appelle la méthode de `Select()` ObjectDataSource, qui à son tour appelle une méthode à partir de notre couche BLL (Business Logic Layer), qui appelle une méthode dans le TableAdapter de la couche d’accès aux données (DAL) approprié, qui à son tour envoie une requête `SELECT` à la base de données Northwind.

Rappelez-vous que lorsque nous avons créé les TableAdapters dans la couche DAL de [notre premier didacticiel](../introduction/creating-a-data-access-layer-cs.md), Visual Studio ajoutait automatiquement des méthodes pour insérer, mettre à jour et supprimer des données de la table de base de données sous-jacente. En outre, lors de la [création d’une couche de logique métier](../introduction/creating-a-business-logic-layer-vb.md) , nous avons conçu des méthodes dans la couche BLL qui étaient appelées dans ces méthodes de modification de données.

En plus de sa méthode `Select()`, ObjectDataSource possède également des méthodes `Insert()`, `Update()`et `Delete()`. À l’instar de la méthode `Select()`, ces trois méthodes peuvent être mappées à des méthodes dans un objet sous-jacent. Lorsqu’ils sont configurés pour insérer, mettre à jour ou supprimer des données, les contrôles GridView, DetailsView et FormView fournissent une interface utilisateur pour modifier les données sous-jacentes. Cette interface utilisateur appelle les méthodes `Insert()`, `Update()`et `Delete()` de l’ObjectDataSource, qui appelle ensuite les méthodes associées à l’objet sous-jacent (voir la figure 1).

[![les méthodes Insert (), Update () et Delete () de ObjectDataSource servent de proxy dans la couche BLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**Figure 1**: les méthodes `Insert()`, `Update()`et `Delete()` de l’ObjectDataSource servent de proxy dans la couche BLL ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))

Dans ce didacticiel, nous allons apprendre à mapper les méthodes `Insert()`, `Update()`et `Delete()` de l’ObjectDataSource aux méthodes des classes de la couche BLL, ainsi qu’à configurer les contrôles GridView, DetailsView et FormView pour fournir des fonctionnalités de modification de données.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Étape 1 : création des pages Web des didacticiels d’insertion, de mise à jour et de suppression

Avant de commencer à explorer l’insertion, la mise à jour et la suppression des données, commençons par prendre un moment pour créer les pages ASP.NET dans notre projet de site Web dont nous aurons besoin pour ce didacticiel et les autres. Commencez par ajouter un nouveau dossier nommé `EditInsertDelete`. Ajoutez ensuite les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Ajouter les pages ASP.NET pour les didacticiels relatifs à la modification des données](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image4.png)

**Figure 2**: ajouter les pages ASP.net pour les didacticiels relatifs à la modification des données

Comme dans les autres dossiers, `Default.aspx` dans le dossier `EditInsertDelete` répertorie les didacticiels de la section. N’oubliez pas que le contrôle utilisateur `SectionLevelTutorialListing.ascx` fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur le Mode Création de la page.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**Figure 3**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))

Enfin, ajoutez les pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après la `<siteMapNode>`de mise en forme personnalisée :

[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels de modification, d’insertion et de suppression.

![Le plan de site comprend désormais des entrées pour les didacticiels de modification, d’insertion et de suppression](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**Figure 4**: le plan de site contient maintenant des entrées pour les didacticiels de modification, d’insertion et de suppression

## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Étape 2 : ajout et configuration du contrôle ObjectDataSource

Étant donné que GridView, DetailsView et FormView diffèrent chacun dans leurs fonctionnalités de modification de données et leur disposition, examinons chacune d’elles individuellement. Plutôt que de faire en sorte que chaque contrôle utilise son propre ObjectDataSource, nous allons simplement créer un ObjectDataSource unique que les trois exemples de contrôle peuvent partager.

Ouvrez la page `Basics.aspx`, faites glisser un ObjectDataSource de la boîte à outils vers le concepteur, puis cliquez sur le lien configurer la source de données à partir de sa balise active. Étant donné que le `ProductsBLL` est la seule classe BLL qui fournit des méthodes d’édition, d’insertion et de suppression, configurez le ObjectDataSource pour utiliser cette classe.

[![configurer ObjectDataSource pour utiliser la classe ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**Figure 5**: configurer ObjectDataSource pour utiliser la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))

Dans l’écran suivant, nous pouvons spécifier les méthodes de la classe `ProductsBLL` qui sont mappées à l' `Select()`, `Insert()`, `Update()`et `Delete()` de l’ObjectDataSource en sélectionnant l’onglet approprié et en choisissant la méthode dans la liste déroulante. Figure 6, qui devrait vous paraître familier à présent, mappe la méthode de `Select()` ObjectDataSource à la méthode `GetProducts()` de la classe `ProductsBLL`. Les méthodes `Insert()`, `Update()`et `Delete()` peuvent être configurées en sélectionnant l’onglet approprié dans la liste située en haut.

[![que ObjectDataSource retournent tous les produits](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**Figure 6**: faire en sorte que ObjectDataSource retourne tous les produits ([cliquez pour afficher l’image en plein écran](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))

Les figures 7, 8 et 9 affichent les onglets mise à jour, insertion et suppression de l’ObjectDataSource. Configurez ces onglets afin que les méthodes `Insert()`, `Update()`et `Delete()` appellent respectivement les méthodes `UpdateProduct`, `AddProduct`et `DeleteProduct` de la classe `ProductsBLL`.

[![mapper la méthode Update () de l’ObjectDataSource à la méthode UpdateProduct de la classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**Figure 7**: mapper la méthode de `Update()` ObjectDataSource à la méthode `UpdateProduct` de la classe `ProductBLL` ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))

[![mapper la méthode Insert () de ObjectDataSource à la méthode AddProduct de la classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**Figure 8**: mapper la méthode de `Insert()` ObjectDataSource à la méthode Add `Product` de la classe `ProductBLL` ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))

[![mapper la méthode Delete () de ObjectDataSource à la méthode DeleteProduct de la classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**Figure 9**: mapper la méthode de `Delete()` ObjectDataSource à la méthode `DeleteProduct` de la classe `ProductBLL` ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))

Vous avez peut-être remarqué que ces méthodes ont déjà été sélectionnées dans les listes déroulantes des onglets mettre à jour, insérer et supprimer. C’est grâce à notre utilisation du `DataObjectMethodAttribute` qui décore les méthodes de `ProductsBLL`. Par exemple, la méthode DeleteProduct a la signature suivante :

[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

L’attribut `DataObjectMethodAttribute` indique l’objectif de chaque méthode, qu’il s’agisse de la sélection, de l’insertion, de la mise à jour ou de la suppression, et du fait qu’il s’agisse ou non de la valeur par défaut. Si vous avez omis ces attributs lors de la création de vos classes BLL, vous devez sélectionner manuellement les méthodes dans les onglets mettre à jour, insérer et supprimer.

Après vous être assuré que les méthodes de `ProductsBLL` appropriées sont mappées aux méthodes de `Insert()`, `Update()`et `Delete()` de l’ObjectDataSource, cliquez sur Terminer pour terminer l’Assistant.

## <a name="examining-the-objectdatasources-markup"></a>Examen de la balise ObjectDataSource

Après avoir configuré l’ObjectDataSource via son assistant, accédez à la vue source pour examiner le balisage déclaratif généré. La balise `<asp:ObjectDataSource>` spécifie l’objet sous-jacent et les méthodes à appeler. En outre, il existe `DeleteParameters`, `UpdateParameters`et `InsertParameters` qui mappent aux paramètres d’entrée pour les méthodes `AddProduct`, `UpdateProduct`et `DeleteProduct` de la classe `ProductsBLL` :

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

ObjectDataSource comprend un paramètre pour chacun des paramètres d’entrée de ses méthodes associées, tout comme une liste de `SelectParameter` s est présente lorsque ObjectDataSource est configuré pour appeler une méthode Select qui attend un paramètre d’entrée (comme `GetProductsByCategoryID(categoryID)`). Comme nous le verrons bientôt, les valeurs de ces `DeleteParameters`, `UpdateParameters`et `InsertParameters` sont définies automatiquement par GridView, DetailsView et FormView avant d’appeler la méthode `Insert()`, `Update()`ou `Delete()` de l’ObjectDataSource. Ces valeurs peuvent également être définies par programmation en fonction des besoins, comme nous le verrons dans un prochain didacticiel.

L’un des effets secondaires de l’utilisation de l’Assistant pour configurer sur ObjectDataSource est que Visual Studio définit la [propriété OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) sur `original_{0}`. Cette valeur de propriété est utilisée pour inclure les valeurs d’origine des données en cours de modification et est utile dans deux scénarios :

- Si, lors de la modification d’un enregistrement, les utilisateurs sont en mesure de modifier la valeur de la clé primaire. Dans ce cas, la nouvelle valeur de clé primaire et la valeur de clé primaire d’origine doivent être fournies afin que l’enregistrement avec la valeur de clé primaire d’origine puisse être trouvé et que sa valeur soit mise à jour en conséquence.
- Lors de l’utilisation de l’accès concurrentiel optimiste. L’accès concurrentiel optimiste est une technique qui permet de s’assurer que deux utilisateurs simultanés ne remplacent pas les modifications d’une autre, et est la rubrique pour un futur didacticiel.

La propriété `OldValuesParameterFormatString` indique le nom des paramètres d’entrée dans les méthodes Update et Delete de l’objet sous-jacent pour les valeurs d’origine. Nous aborderons cette propriété et son objectif plus en détail lorsque nous explorons l’accès concurrentiel optimiste. Je l’affiche maintenant, car les méthodes de la couche BLL n’attendent pas les valeurs d’origine et, par conséquent, il est important de supprimer cette propriété. Le fait de laisser la propriété `OldValuesParameterFormatString` définie sur une valeur autre que la valeur par défaut (`{0}`) génère une erreur lorsqu’un contrôle Web de données tente d’appeler les méthodes `Update()` ou `Delete()` de l’ObjectDataSource, car ObjectDataSource tente de transmettre à la fois le `UpdateParameters` ou le `DeleteParameters` spécifié, ainsi que les paramètres de valeur d’origine.

Si ce n’est pas très clair, ne vous inquiétez pas, nous examinerons cette propriété et son utilitaire dans un prochain didacticiel. Pour le moment, il vous suffit de supprimer cette déclaration de propriété de la syntaxe déclarative ou de définir la valeur par défaut ({0}).

> [!NOTE]
> Si vous supprimez simplement la valeur de la propriété `OldValuesParameterFormatString` de la Fenêtre Propriétés dans le Mode Création, la propriété existera toujours dans la syntaxe déclarative, mais elle sera définie sur une chaîne vide. Malheureusement, il en résultera toujours le même problème que celui décrit ci-dessus. Par conséquent, supprimez la propriété de la syntaxe déclarative ou, à partir de la Fenêtre Propriétés, affectez la valeur par défaut, `{0}`.

## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Étape 3 : ajout d’un contrôle Web de données et configuration de celui-ci pour la modification des données

Une fois que ObjectDataSource a été ajouté à la page et configuré, nous sommes prêts à ajouter des contrôles Web de données à la page pour afficher les données et fournir à l’utilisateur final la possibilité de le modifier. Nous examinerons séparément GridView, DetailsView et FormView, car ces contrôles Web de données diffèrent dans leurs fonctionnalités de modification de données et leur configuration.

Comme nous le verrons dans le reste de cet article, l’ajout de la prise en charge de l’édition, de l’insertion et de la suppression de base via les contrôles GridView, DetailsView et FormView est aussi simple que la vérification de deux cases à cocher. Dans le monde réel, il y a de nombreuses subtilités et cas de périphérie qui rendent les fonctionnalités de ce type plus impliquées qu’un simple pointage et un clic. Toutefois, ce didacticiel se concentre uniquement sur la démonstration des fonctionnalités de modification de données simplistes. Les prochains didacticiels examinent les problèmes qui se produiront sans aucun doute dans un environnement réel.

## <a name="deleting-data-from-the-gridview"></a>Suppression de données du contrôle GridView

Commencez par faire glisser un contrôle GridView de la boîte à outils vers le concepteur. Ensuite, liez l’ObjectDataSource au GridView en le sélectionnant dans la liste déroulante de la balise active de GridView. À ce stade, le balisage déclaratif de GridView sera :

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

La liaison du contrôle GridView au ObjectDataSource par le biais de sa balise active présente deux avantages :

- BoundFields et CheckBoxFields sont créés automatiquement pour chacun des champs retournés par l’ObjectDataSource. En outre, les propriétés BoundField et CheckBoxField sont définies en fonction des métadonnées du champ sous-jacent. Par exemple, les champs `ProductID`, `CategoryName`et `SupplierName` sont marqués en lecture seule dans le `ProductsDataTable` et ne peuvent donc pas être mis à jour lors de la modification. Pour ce faire, ces [propriétés ReadOnly](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) BoundFields sont définies sur `True`.
- La [propriété DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) est assignée au (x) champ (s) de clé primaire de l’objet sous-jacent. Cela est essentiel lors de l’utilisation de GridView pour la modification ou la suppression de données, car cette propriété indique le champ (ou l’ensemble de champs) qui identifie chaque enregistrement de manière unique. Pour plus d’informations sur la propriété `DataKeyNames`, reportez-vous à la rubrique [maître/détail à l’aide d’un GridView maître sélectionnable avec un didacticiel détails DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) .

Alors que le GridView peut être lié à l’ObjectDataSource par le biais de l’Fenêtre Propriétés ou de la syntaxe déclarative, vous devez ajouter manuellement le balisage BoundField et `DataKeyNames` approprié.

Le contrôle GridView offre une prise en charge intégrée de la modification et de la suppression au niveau des lignes. La configuration d’un contrôle GridView pour prendre en charge la suppression ajoute une colonne de boutons supprimer. Lorsque l’utilisateur final clique sur le bouton Supprimer pour une ligne particulière, une publication est effectuée et le GridView effectue les étapes suivantes :

1. La ou les valeurs de `DeleteParameters` de l’ObjectDataSource sont affectées
2. La méthode de `Delete()` de l’ObjectDataSource est appelée, supprimant l’enregistrement spécifié
3. Le GridView se lie de nouveau à ObjectDataSource en appelant sa méthode `Select()`

Les valeurs assignées à la `DeleteParameters` sont les valeurs des champs `DataKeyNames` pour la ligne dont l’utilisateur a cliqué sur le bouton supprimer. Par conséquent, il est vital que la propriété `DataKeyNames` d’un GridView soit correctement définie. Si elle est manquante, la `DeleteParameters` se verra attribuer une valeur de `Nothing` à l’étape 1, qui, à son tour, n’entraîne pas de suppression des enregistrements à l’étape 2.

> [!NOTE]
> La collection `DataKeys` est stockée dans l’état du contrôle GridView s, ce qui signifie que les valeurs `DataKeys` seront mémorisées sur la publication même si l’état d’affichage de GridView s a été désactivé. Toutefois, il est très important que l’état d’affichage reste activé pour les GridViews qui prennent en charge la modification ou la suppression (comportement par défaut). Si vous affectez à la propriété GridView s `EnableViewState` la valeur `false`, le comportement de modification et de suppression fonctionnera correctement pour un seul utilisateur, mais s’il existe des utilisateurs simultanés qui suppriment des données, il est possible que ces utilisateurs simultanés suppriment ou modifient accidentellement des enregistrements qu’ils n’avaient pas prévus. Pour plus d’informations, consultez mon blog, [Avertissement : problème d’accès concurrentiel avec ASP.NET 2,0 GridViews/DetailsView/FormViews qui prennent en charge l’édition et/ou la suppression et dont l’état d’affichage est désactivé](http://scottonwriting.net/sowblog/posts/10054.aspx).

Ce même avertissement s’applique également à DetailsViews et FormViews.

Pour ajouter des fonctionnalités de suppression à un GridView, il vous suffit d’accéder à sa balise active et de cocher la case Activer la suppression.

![Cochez la case Activer la suppression](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**Figure 10**: activer la case à cocher Activer la suppression

L’activation de la case à cocher Activer la suppression de la balise active ajoute un CommandField au GridView. CommandField restitue une colonne dans le GridView avec des boutons permettant d’effectuer une ou plusieurs des tâches suivantes : sélection d’un enregistrement, modification d’un enregistrement et suppression d’un enregistrement. Nous avons précédemment vu l’CommandField en action avec la sélection des enregistrements dans le [maître/détail à l’aide d’un GridView maître sélectionnable avec un didacticiel détails DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) .

CommandField contient un certain nombre de propriétés de `ShowXButton` qui indiquent la série de boutons affichée dans CommandField. En activant la case à cocher Activer la suppression, un CommandField dont la propriété `ShowDeleteButton` est `True` a été ajouté à la collection Columns du contrôle GridView.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

À ce stade, croyez-le ou non, nous avons terminé l’ajout de la prise en charge de la suppression au contrôle GridView ! Comme le montre la figure 11, lorsque vous visitez cette page via un navigateur, une colonne de boutons supprimer est présente.

[![la méthode CommandField ajoute une colonne de boutons supprimer](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**Figure 11**: CommandField ajoute une colonne de boutons supprimer ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))

Si vous avez créé ce didacticiel dès le départ, lorsque vous testez cette page, le fait de cliquer sur le bouton supprimer lève une exception. Poursuivez la lecture pour savoir pourquoi ces exceptions ont été générées et comment les corriger.

> [!NOTE]
> Si vous suivez le téléchargement accompagnant ce didacticiel, ces problèmes ont déjà été associés à. Toutefois, je vous encourage à lire les détails ci-dessous pour vous aider à identifier les problèmes qui peuvent survenir et les solutions de contournement appropriées.

Si, lors de la tentative de suppression d’un produit, vous obtenez une exception dont le message est similaire à «*ObjectDataSource’ObjectDataSource1 'n’a pas pu trouver une méthode’DeleteProduct’non générique qui a des paramètres : ProductID, original\_ProductID*», vous avez probablement oublié de supprimer la propriété `OldValuesParameterFormatString` de ObjectDataSource. Avec la propriété `OldValuesParameterFormatString` spécifiée, ObjectDataSource tente de transmettre les paramètres d’entrée `productID` et `original_ProductID` à la méthode `DeleteProduct`. Toutefois, `DeleteProduct`n’accepte qu’un seul paramètre d’entrée, par conséquent l’exception. La suppression de la propriété `OldValuesParameterFormatString` (ou sa définition sur `{0}`) fait en sorte que ObjectDataSource ne tente pas de passer le paramètre d’entrée d’origine.

[![vérifier que la propriété OldValuesParameterFormatString a été supprimée](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**Figure 12**: vérifier que la propriété `OldValuesParameterFormatString` a été effacée ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))

Même si vous avez supprimé la propriété `OldValuesParameterFormatString`, vous obtiendrez toujours une exception lors de la tentative de suppression d’un produit avec le message : «*l’instruction DELETE est en conflit avec la contrainte de référence «FK\_Order\_details\_Products »* . La base de données Northwind contient une contrainte de clé étrangère entre la `Order Details` et la table de `Products`, ce qui signifie qu’un produit ne peut pas être supprimé du système s’il y a un ou plusieurs enregistrements dans la table `Order Details`. Étant donné que chaque produit de la base de données Northwind a au moins un enregistrement dans `Order Details`, nous ne pouvons pas supprimer les produits tant que nous n’avons pas supprimé au préalable les enregistrements de détails de commande associés au produit.

[![une contrainte de clé étrangère interdit la suppression des produits](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**Figure 13**: une contrainte de clé étrangère interdit la suppression de produits ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))

Pour notre didacticiel, nous allons simplement supprimer tous les enregistrements de la table `Order Details`. Dans une application réelle, nous aurions besoin des éléments suivants :

- Avoir un autre écran pour gérer les informations sur les détails des commandes
- Augmentez la méthode `DeleteProduct` pour inclure la logique permettant de supprimer les détails de la commande du produit spécifié.
- Modifier la requête SQL utilisée par le TableAdapter pour inclure la suppression des détails de commande du produit spécifié

Nous allons simplement supprimer tous les enregistrements de la table `Order Details` pour contourner la contrainte de clé étrangère. Accédez à la Explorateur de serveurs dans Visual Studio, cliquez avec le bouton droit sur le nœud `NORTHWND.MDF`, puis choisissez nouvelle requête. Ensuite, dans la fenêtre de requête, exécutez l’instruction SQL suivante : `DELETE FROM [Order Details]`

[![supprimer tous les enregistrements de la table Order Details](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**Figure 14**: supprimer tous les enregistrements de la table `Order Details` ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))

Une fois le tableau de `Order Details` effacé, si vous cliquez sur le bouton supprimer, le produit est supprimé sans erreur. Si le fait de cliquer sur le bouton supprimer ne supprime pas le produit, assurez-vous que la propriété `DataKeyNames` du contrôle GridView est définie sur le champ de clé primaire (`ProductID`).

> [!NOTE]
> Lorsque vous cliquez sur le bouton supprimer, une publication est effectuée et l’enregistrement est supprimé. Cela peut être dangereux, car il est facile de cliquer accidentellement sur le bouton supprimer de la ligne incorrecte. Dans un prochain didacticiel, nous verrons comment ajouter une confirmation côté client lors de la suppression d’un enregistrement.

## <a name="editing-data-with-the-gridview"></a>Modification de données avec le contrôle GridView

En plus de la suppression, le contrôle GridView fournit également une prise en charge intégrée de la modification au niveau des lignes. La configuration d’un contrôle GridView pour prendre en charge la modification ajoute une colonne de boutons de modification. Du point de vue de l’utilisateur final, le fait de cliquer sur le bouton modifier d’une ligne permet de rendre cette ligne modifiable, en transformant les cellules en zones de texte contenant les valeurs existantes et en remplaçant le bouton modifier par les boutons mettre à jour et annuler. Après avoir apporté les modifications souhaitées, l’utilisateur final peut cliquer sur le bouton mettre à jour pour valider les modifications ou sur le bouton Annuler pour les ignorer. Dans les deux cas, après avoir cliqué sur mettre à jour ou annuler, le GridView revient à son état antérieur à la modification.

Du point de vue du développeur de pages, lorsque l’utilisateur final clique sur le bouton modifier pour une ligne particulière, une publication est effectuée et le GridView effectue les étapes suivantes :

1. La propriété `EditItemIndex` du GridView est assignée à l’index de la ligne dont le bouton modifier a été cliqué
2. Le GridView se lie de nouveau à ObjectDataSource en appelant sa méthode `Select()`
3. L’index de ligne qui correspond au `EditItemIndex` est restitué en « mode édition ». Dans ce mode, le bouton modifier est remplacé par les boutons mettre à jour et annuler et BoundFields dont les propriétés `ReadOnly` sont false (valeur par défaut) sont rendues sous la forme de contrôles Web TextBox dont les propriétés de `Text` sont affectées aux valeurs des champs de données.

À ce stade, le balisage est retourné au navigateur, ce qui permet à l’utilisateur final d’apporter des modifications aux données de la ligne. Quand l’utilisateur clique sur le bouton mettre à jour, une publication (postback) se produit et le GridView effectue les étapes suivantes :

1. Les valeurs entrées par l’utilisateur final dans l’interface de modification du contrôle GridView sont affectées à la ou aux valeurs de l' `UpdateParameters` ObjectDataSource.
2. La méthode de `Update()` ObjectDataSource est appelée, mettant à jour l’enregistrement spécifié
3. Le GridView se lie de nouveau à ObjectDataSource en appelant sa méthode `Select()`

Les valeurs de clé primaire assignées à la `UpdateParameters` à l’étape 1 proviennent des valeurs spécifiées dans la propriété `DataKeyNames`, alors que les valeurs de clé non primaire proviennent du texte des contrôles Web TextBox pour la ligne modifiée. Comme pour la suppression, il est vital que la propriété `DataKeyNames` d’un GridView soit correctement définie. Si elle est manquante, la valeur de la clé primaire `UpdateParameters` sera affectée à la valeur `Nothing` à l’étape 1, qui, à son tour, n’entraîne pas d’enregistrements mis à jour à l’étape 2.

La fonctionnalité d’édition peut être activée simplement en activant la case à cocher Activer la modification dans la balise active de GridView.

![Cochez la case Activer la modification](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**Figure 15**: activer la case à cocher Activer la modification

Si vous cochez la case Activer la modification, vous ajoutez un CommandField (si nécessaire) et définissez sa propriété `ShowEditButton` sur `True`. Comme nous l’avons vu précédemment, le CommandField contient un certain nombre de propriétés de `ShowXButton` qui indiquent la série de boutons affichée dans CommandField. Cochez la case Activer la modification pour ajouter la propriété `ShowEditButton` à la propriété CommandField existante :

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

C’est tout ce qu’il faut pour ajouter une prise en charge de la modification rudimentaire. Comme le montre Figure16, l’interface de modification est plutôt grossière pour chaque BoundField dont la propriété `ReadOnly` est définie sur `False` (la valeur par défaut) est rendue sous la forme d’une zone de texte. Cela comprend des champs comme `CategoryID` et `SupplierID`, qui sont des clés pour d’autres tables.

[![cliquant sur le bouton de modification Chai s affiche la ligne en mode édition](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**Figure 16**: cliquer sur le bouton de modification de l’option Chai s affiche la ligne en mode édition ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))

En plus de demander aux utilisateurs de modifier directement les valeurs de clé étrangère, l’interface de l’interface de modification est absente des façons suivantes :

- Si l’utilisateur entre une `CategoryID` ou `SupplierID` qui n’existe pas dans la base de données, le `UPDATE` viole une contrainte de clé étrangère, provoquant la levée d’une exception.
- L’interface de modification n’inclut aucune validation. Si vous ne fournissez pas de valeur requise (par exemple `ProductName`), ou si vous entrez une valeur de chaîne dans laquelle une valeur numérique est attendue (par exemple, en entrant « trop ! » dans la zone de texte `UnitPrice`), une exception est levée. Un futur didacticiel examinera comment ajouter des contrôles de validation à l’interface utilisateur de modification.
- Actuellement, *tous les* champs de produit qui ne sont pas en lecture seule doivent être inclus dans le GridView. Si nous devions supprimer un champ du GridView, par exemple `UnitPrice`, lors de la mise à jour des données, le contrôle GridView ne définit pas la valeur de la `UnitPrice` `UpdateParameters`, ce qui modifie le `UnitPrice` de l’enregistrement de base de données en `NULL` valeur. De même, si un champ obligatoire, tel que `ProductName`, est supprimé du GridView, la mise à jour échouera avec la même « la*colonne «ProductName » n’autorise pas les valeurs NULL*».
- La mise en forme de l’interface de modification laisse beaucoup à désirer. La `UnitPrice` est affichée avec quatre points décimaux. Dans l’idéal, les valeurs `CategoryID` et `SupplierID` contiennent des DropDownList qui répertorient les catégories et les fournisseurs dans le système.

Il s’agit de toutes les lacunes que nous devrons utiliser pour l’instant, mais elles seront abordées dans les prochains didacticiels.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Insertion, modification et suppression de données à l’aide du contrôle DetailsView

Comme nous l’avons vu dans les didacticiels précédents, le contrôle DetailsView affiche un enregistrement à la fois et, comme le GridView, permet la modification et la suppression de l’enregistrement actuellement affiché. L’expérience de l’utilisateur final avec la modification et la suppression d’éléments d’un DetailsView et du flux de travail du côté ASP.NET est identique à celle du contrôle GridView. Dans le cas où le DetailsView diffère de GridView, il fournit également une prise en charge de l’insertion intégrée.

Pour illustrer les fonctionnalités de modification de données du contrôle GridView, commencez par ajouter un DetailsView à la `Basics.aspx` page au-dessus du GridView existant et liez-le à l’ObjectDataSource existant via la balise active du contrôle DetailsView. Ensuite, effacez les propriétés `Height` et `Width` du contrôle DetailsView, puis activez l’option Activer la pagination dans la balise active. Pour activer la prise en charge de la modification, de l’insertion et de la suppression, cochez simplement les cases activer la modification, activer l’insertion et activer la suppression dans la balise active.

![Configurer le contrôle DetailsView pour prendre en charge la modification, l’insertion et la suppression](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**Figure 17**: configurer le contrôle DetailsView pour prendre en charge la modification, l’insertion et la suppression

Comme avec le contrôle GridView, l’ajout de la prise en charge de la modification, de l’insertion ou de la suppression ajoute une CommandField au DetailsView, comme le montre la syntaxe déclarative suivante :

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

Notez que pour le DetailsView, le CommandField apparaît à la fin de la collection Columns par défaut. Étant donné que les champs du contrôle DetailsView sont rendus sous forme de lignes, le CommandField apparaît sous la forme d’une ligne avec les boutons Insérer, modifier et supprimer en bas du contrôle DetailsView.

[![configurer le contrôle DetailsView pour prendre en charge la modification, l’insertion et la suppression](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**Figure 18**: configurer le contrôle DetailsView pour prendre en charge la modification, l’insertion et la suppression ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))

Le fait de cliquer sur le bouton supprimer démarre la même séquence d’événements qu’avec le GridView : une publication (postback); suivi du DetailsView qui remplit le `DeleteParameters` de son ObjectDataSource en fonction des valeurs de `DataKeyNames` ; et s’achèvent par un appel de la méthode de `Delete()` de ObjectDataSource, qui supprime le produit de la base de données. La modification dans le contrôle DetailsView fonctionne également de manière identique à celle du contrôle GridView.

Pour l’insertion, l’utilisateur final reçoit un nouveau bouton qui, lorsque vous cliquez dessus, restitue le DetailsView en « mode insertion ». Avec « mode insertion », le nouveau bouton est remplacé par les boutons Insérer et annuler et uniquement les BoundFields dont la propriété `InsertVisible` est définie sur `True` (valeur par défaut). Les champs de données identifiés comme des champs d’incrémentation automatique, tels que `ProductID`, ont leur [propriété InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) définie sur `False` lors de la liaison du contrôle DetailsView à la source de données via la balise active.

Lors de la liaison d’une source de données à un contrôle DetailsView via la balise active, Visual Studio affecte à la propriété `InsertVisible` la valeur `False` uniquement pour les champs à incrémentation automatique. Les champs en lecture seule, comme `CategoryName` et `SupplierName`, s’affichent dans l’interface utilisateur « mode insertion », sauf si leur propriété `InsertVisible` est explicitement définie sur `False`. Prenez un moment pour définir ces deux propriétés de `InsertVisible` de champs sur `False`, soit via la syntaxe déclarative du contrôle DetailsView, soit via le lien modifier les champs de la balise active. La figure 19 montre comment définir les propriétés de `InsertVisible` à `False` en cliquant sur le lien modifier les champs.

[![Northwind Traders propose désormais Acme Tea](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**Figure 19**: Northwind Traders propose désormais Acme Tea ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))

Après avoir défini les propriétés de `InsertVisible`, affichez la page `Basics.aspx` dans un navigateur, puis cliquez sur le bouton nouveau. La figure 20 illustre le DetailsView lors de l’ajout d’une nouvelle boisson, le thé Acme, à notre gamme de produits.

[![Northwind Traders propose désormais Acme Tea](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**Figure 20**: Northwind Traders propose désormais Acme Tea ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))

Après avoir entré les détails pour Acme Tea et cliqué sur le bouton Insérer, une publication est effectuée et le nouvel enregistrement est ajouté à la table de base de données `Products`. Étant donné que ce contrôle DetailsView répertorie les produits dans l’ordre dans lequel ils se trouvent dans la table de base de données, nous devons effectuer une pagination vers le dernier produit afin de voir le nouveau produit.

[Détails ![pour Acme thé](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**Figure 21**: détails pour Acme Tea ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))

> [!NOTE]
> La [propriété CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) du contrôle DetailsView indique l’interface affichée et peut prendre l’une des valeurs suivantes : `Edit`, `Insert`ou `ReadOnly`. La [propriété DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indique le mode vers lequel le DetailsView retourne une fois qu’une opération de modification ou d’insertion est terminée et est utile pour afficher un DetailsView qui est définitivement en mode édition ou insertion.

Le point et le clic sur les fonctionnalités d’insertion et de modification du contrôle DetailsView sont soumis aux mêmes limitations que le GridView : l’utilisateur doit entrer les valeurs `CategoryID` et `SupplierID` existantes dans une zone de texte. l’interface n’a pas de logique de validation ; tous les champs de produit qui n’autorisent pas les valeurs `NULL` ou qui n’ont pas de valeur par défaut spécifiée au niveau de la base de données doivent être inclus dans l’interface d’insertion, et ainsi de suite.

Les techniques que nous examinerons pour l’extension et l’amélioration de l’interface de modification de GridView dans les prochains articles peuvent également être appliquées aux interfaces de modification et d’insertion du contrôle DetailsView.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Utilisation du FormView pour une interface utilisateur de modification de données plus flexible

Le contrôle FormView offre une prise en charge intégrée de l’insertion, de la modification et de la suppression des données, mais parce qu’il utilise des modèles au lieu de champs, il n’y a pas d’emplacement pour ajouter le BoundFields ou le CommandField utilisé par les contrôles GridView et DetailsView pour fournir les données. interface de modification. Au lieu de cela, cette interface permet d’ajouter manuellement les contrôles Web pour la collecte de l’entrée d’utilisateur lors de l’ajout d’un nouvel élément ou de la modification d’un élément existant, ainsi que des boutons nouveau, modifier, supprimer, insérer, mettre à jour et annuler. Heureusement, Visual Studio crée automatiquement l’interface nécessaire lors de la liaison du FormView à une source de données via la liste déroulante de sa balise active.

Pour illustrer ces techniques, commencez par ajouter un contrôle FormView à la page de `Basics.aspx` et, à partir de la balise active FormView, liez-le à l’ObjectDataSource déjà créé. Cela génère un `EditItemTemplate`, `InsertItemTemplate`et `ItemTemplate` pour le FormView avec les contrôles Web TextBox pour la collecte des contrôles Web d’entrée et de bouton de l’utilisateur pour les boutons nouveau, modifier, supprimer, insérer, mettre à jour et annuler. En outre, la propriété `DataKeyNames` du FormView est définie sur le champ de clé primaire (`ProductID`) de l’objet retourné par ObjectDataSource. Enfin, activez l’option Activer la pagination dans la balise active du FormView.

L’exemple suivant montre le balisage déclaratif pour le `ItemTemplate` du FormView une fois que le FormView a été lié à ObjectDataSource. Par défaut, chaque champ de valeur non Boolean est lié à la propriété `Text` d’un contrôle Web label lorsque chaque champ de valeur booléenne (`Discontinued`) est lié à la propriété `Checked` d’un contrôle Web CheckBox désactivé. Pour que les boutons nouveau, modifier et supprimer déclenchent certains comportements FormView lorsque vous cliquez dessus, il est impératif que leurs valeurs `CommandName` soient définies respectivement sur `New`, `Edit`et `Delete`.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

La figure 22 illustre le `ItemTemplate` du FormView lorsqu’il est affiché dans un navigateur. Chaque champ de produit est listé avec les boutons nouveau, modifier et supprimer en bas.

[![le ItemTemplate défaut FormView répertorie chaque champ produit ainsi que les boutons nouveau, modifier et supprimer](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**Figure 22**: le `ItemTemplate` FormView défaut répertorie chaque champ produit ainsi que les boutons nouveau, modifier et supprimer ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))

Comme avec les contrôles GridView et DetailsView, le fait de cliquer sur le bouton supprimer ou sur un bouton, LinkButton ou ImageButton dont la propriété `CommandName` est définie sur Delete provoque une publication (postback), remplit le `DeleteParameters` de l’ObjectDataSource en fonction de la valeur de `DataKeyNames` du FormView, puis appelle la méthode de `Delete()` de l’ObjectDataSource.

Quand l’utilisateur clique sur le bouton modifier, une publication est effectuée et les données sont reliées à la `EditItemTemplate`, qui est chargée du rendu de l’interface de modification. Cette interface comprend les contrôles Web pour la modification des données, ainsi que les boutons de mise à jour et d’annulation. Le `EditItemTemplate` par défaut généré par Visual Studio contient une étiquette pour tous les champs d’incrémentation automatique (`ProductID`), une zone de texte pour chaque champ de valeur non booléen et une case à cocher pour chaque champ de valeur booléenne. Ce comportement est très similaire au BoundFields généré automatiquement dans les contrôles GridView et DetailsView.

> [!NOTE]
> L’un des petits problèmes avec la génération automatique du FormView du `EditItemTemplate` est qu’il affiche les contrôles Web TextBox pour les champs qui sont en lecture seule, tels que `CategoryName` et `SupplierName`. Nous verrons comment nous en tenir compte.

Les contrôles TextBox dans le `EditItemTemplate` ont leur propriété `Text` liée à la valeur de leur champ de données correspondant à l’aide de la liaison de données *bidirectionnelle*. La liaison de données bidirectionnelle, dénotée par `<%# Bind("dataField") %>`, effectue une liaison de données à la fois lors de la liaison de données au modèle et lors du remplissage des paramètres d’ObjectDataSource pour l’insertion ou la modification d’enregistrements. Autrement dit, lorsque l’utilisateur clique sur le bouton modifier dans le `ItemTemplate`, la méthode `Bind()` retourne la valeur de champ de données spécifiée. Une fois que l’utilisateur a effectué ses modifications et cliqué sur mettre à jour, les valeurs publiées qui correspondent aux champs de données spécifiés à l’aide de `Bind()` sont appliquées au `UpdateParameters`de l’ObjectDataSource. Sinon, la liaison de données unidirectionnelle, dénotée par `<%# Eval("dataField") %>`, récupère uniquement les valeurs de champ de données lors de la liaison de données au modèle et ne retourne *pas* les valeurs entrées par l’utilisateur aux paramètres de la source de données lors de la publication (postback).

Le balisage déclaratif suivant montre le `EditItemTemplate`du FormView. Notez que la méthode `Bind()` est utilisée dans la syntaxe DataBinding ici et que les propriétés `CommandName` des contrôles Web des boutons Update et Cancel sont définies en conséquence.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

À ce stade, notre `EditItemTemplate`entraîne la levée d’une exception si nous tentons de l’utiliser. Le problème est que les champs `CategoryName` et `SupplierName` sont rendus sous forme de contrôles Web TextBox dans le `EditItemTemplate`. Nous devons soit modifier ces zones de texte en étiquettes, soit les supprimer entièrement. Nous allons simplement les supprimer de la `EditItemTemplate`.

La figure 23 montre le FormView dans un navigateur après que l’utilisateur a cliqué sur le bouton modifier pour Chai. Notez que les champs `SupplierName` et `CategoryName` affichés dans le `ItemTemplate` ne sont plus présents, car nous les avons simplement supprimés de la `EditItemTemplate`. Quand l’utilisateur clique sur le bouton mettre à jour, le FormView passe par la même séquence d’étapes que les contrôles GridView et DetailsView.

[![par défaut, EditItemTemplate affiche chaque champ produit modifiable sous la forme d’une zone de texte ou d’une case à cocher](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**Figure 23**: par défaut, le `EditItemTemplate` affiche chaque champ de produit modifiable sous la forme d’une zone de texte ou d’une case à cocher ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))

Quand le bouton Insérer est cliqué sur le `ItemTemplate` du FormView, une publication est effectuée. Toutefois, aucune donnée n’est liée au FormView car un nouvel enregistrement est ajouté. L’interface `InsertItemTemplate` comprend les contrôles Web permettant d’ajouter un nouvel enregistrement avec les boutons d’insertion et d’annulation. Le `InsertItemTemplate` par défaut généré par Visual Studio contient une zone de texte pour chaque champ de valeur non booléen et une case à cocher pour chaque champ de valeur booléenne, similaire à l’interface de `EditItemTemplate`générée automatiquement. La propriété `Text` des contrôles TextBox est liée à la valeur de leur champ de données correspondant à l’aide de la liaison de données bidirectionnelle.

Le balisage déclaratif suivant montre le `InsertItemTemplate`du FormView. Notez que la méthode `Bind()` est utilisée dans la syntaxe DataBinding ici et que les propriétés `CommandName` des contrôles Web Insert et Cancel sont définies en conséquence.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

Il y a une subtilité avec la génération automatique du FormView du `InsertItemTemplate`. Plus précisément, les contrôles Web TextBox sont créés même pour les champs qui sont en lecture seule, par exemple `CategoryName` et `SupplierName`. Comme avec le `EditItemTemplate`, nous devons supprimer ces zones de texte du `InsertItemTemplate`.

La figure 24 montre le FormView dans un navigateur lors de l’ajout d’un nouveau produit, Acme Coffee. Notez que les champs `SupplierName` et `CategoryName` affichés dans le `ItemTemplate` ne sont plus présents, car nous venons de les supprimer. Quand l’utilisateur clique sur le bouton Insérer, le FormView passe par la même séquence d’étapes que le contrôle DetailsView, en ajoutant un nouvel enregistrement à la table `Products`. La figure 25 montre les détails du produit de café Acme dans le FormView après son insertion.

[![InsertItemTemplate dicte l’interface d’insertion du FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**Figure 24**: le `InsertItemTemplate` dicte l’interface d’insertion du FormView ([cliquez pour afficher l’image en taille réelle](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))

[![les détails pour le nouveau produit, Acme Coffee, s’affichent dans le FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**Figure 25**: les détails pour le nouveau produit, Acme Coffee, s’affichent dans le FormView ([cliquez pour afficher l’image en plein écran](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))

En séparant les interfaces en lecture seule, en modifiant et en insérant dans trois modèles distincts, le FormView offre un degré de contrôle plus fin sur ces interfaces que sur DetailsView et GridView.

> [!NOTE]
> À l’instar de DetailsView, la propriété `CurrentMode` du FormView indique l’interface affichée et sa propriété `DefaultMode` indique le mode vers lequel le FormView retourne une valeur après la fin d’une opération de modification ou d’insertion.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons examiné les principes fondamentaux de l’insertion, de la modification et de la suppression de données à l’aide de GridView, DetailsView et FormView. Ces trois contrôles fournissent un certain niveau de fonctionnalités de modification de données intégrées qui peuvent être utilisées sans écrire une seule ligne de code dans la page ASP.NET grâce aux contrôles Web de données et à ObjectDataSource. Toutefois, les techniques simples et simples permettent d’afficher une interface utilisateur de modification de données Frail et naïve. Pour fournir la validation, injecter des valeurs de programmation, gérer correctement les exceptions, personnaliser l’interface utilisateur, et ainsi de suite, nous devons nous appuyer sur un multitude de techniques qui seront abordées dans les prochains didacticiels.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](limiting-data-modification-functionality-based-on-the-user-cs.md)
> [Suivant](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
