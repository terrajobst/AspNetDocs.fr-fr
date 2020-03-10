---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Vue d’ensemble de la modification et de la suppression de données dans le contrôle DataList (VB) | Microsoft Docs
author: rick-anderson
description: Alors que la DataList ne dispose pas de fonctionnalités de modification et de suppression intégrées, dans ce didacticiel, nous allons apprendre à créer un contrôle DataList qui prend en charge la modification et la suppression de...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 49b09cbf0f12f7c7233c3bb2a8b3b2c073bf117e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78611271"
---
# <a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Vue d’ensemble de la modification et de la suppression de données dans le contrôle DataList (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) ou [Télécharger le PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Alors que la DataList ne dispose pas de fonctionnalités de modification et de suppression intégrées, dans ce didacticiel, nous allons apprendre à créer un contrôle DataList qui prend en charge la modification et la suppression de ses données sous-jacentes.

## <a name="introduction"></a>Introduction

Dans le didacticiel sur l’insertion, la mise à jour [et la suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , nous avons vu comment insérer, mettre à jour et supprimer des données à l’aide de l’architecture de l’application, d’un ObjectDataSource et des contrôles GridView, DetailsView et FormView. Avec ObjectDataSource et ces trois contrôles Web de données, l’implémentation d’interfaces de modification de données simples était un jeu d’enfant et impliquait simplement la présence d’une case à cocher à partir d’une balise active. Aucun code n’a besoin d’être écrit.

Malheureusement, la DataList ne dispose pas des fonctionnalités intégrées de modification et de suppression inhérentes au contrôle GridView. Cette fonctionnalité manquante est due en partie au fait que la DataList est un Relic de la version précédente de ASP.NET, lorsque les contrôles de source de données déclaratives et les pages de modification de données sans code ne sont pas disponibles. Bien que la DataList dans ASP.NET 2,0 n’offre pas les mêmes fonctionnalités de modification de données prêtes à l’emploi que le GridView, nous pouvons utiliser les techniques ASP.NET 1. x pour inclure de telles fonctionnalités. Cette approche nécessite un peu de code mais, comme nous le verrons dans ce didacticiel, la DataList a des événements et des propriétés en place pour faciliter ce processus.

Dans ce didacticiel, nous allons apprendre à créer un contrôle DataList qui prend en charge la modification et la suppression de ses données sous-jacentes. Les prochains didacticiels examinent les scénarios de modification et de suppression plus avancés, y compris la validation des champs d’entrée, en gérant en douceur les exceptions déclenchées par l’accès aux données ou les couches de logique métier, etc.

> [!NOTE]
> Comme la DataList, le contrôle Repeater ne dispose pas de la fonctionnalité prête à l’emploi pour l’insertion, la mise à jour ou la suppression. Si de telles fonctionnalités peuvent être ajoutées, le contrôle DataList comprend des propriétés et des événements introuvables dans le Repeater qui simplifient l’ajout de telles fonctionnalités. Par conséquent, ce didacticiel et les futurs qui examinent la modification et la suppression se concentreront strictement sur la DataList.

## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Étape 1 : création des pages Web de modification et de suppression des didacticiels

Avant de commencer à explorer la manière de mettre à jour et de supprimer des données d’un contrôle DataList, commencez par prendre un moment pour créer les pages ASP.NET dans notre projet de site Web dont nous aurons besoin pour ce didacticiel et les autres. Commencez par ajouter un nouveau dossier nommé `EditDeleteDataList`. Ajoutez ensuite les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Ajouter les pages ASP.NET pour les didacticiels](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Figure 1**: ajouter les pages ASP.net pour les didacticiels

Comme dans les autres dossiers, `Default.aspx` dans le dossier `EditDeleteDataList` répertorie les didacticiels de la section. N’oubliez pas que le contrôle utilisateur `SectionLevelTutorialListing.ascx` fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur la page s Mode Création.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Figure 2**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))

Enfin, ajoutez les pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après les rapports maître/détail avec les `<siteMapNode>`DataList et Repeater :

[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels de modification et de suppression de DataList.

![Le plan de site comprend désormais des entrées pour les didacticiels de modification et de suppression de DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Figure 3**: le plan de site contient maintenant des entrées pour les didacticiels de modification et de suppression de DataList

## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Étape 2 : examen des techniques de mise à jour et de suppression des données

La modification et la suppression de données avec le contrôle GridView est si simple car, sous les couvertures, le GridView et ObjectDataSource fonctionnent de concert. Comme indiqué dans le didacticiel [examen des événements associés à l’insertion, à la mise à jour et à la suppression](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) , lorsque l’utilisateur clique sur un bouton de mise à jour d’une ligne, le GridView attribue automatiquement ses champs qui ont utilisé la liaison de liaison bidirectionnelle à la collection `UpdateParameters` de son ObjectDataSource, puis appelle cette méthode d' `Update()` ObjectDataSource.

Malheureusement, la DataList ne fournit aucune de ces fonctionnalités intégrées. Il est de notre responsabilité de s’assurer que les valeurs utilisateur sont affectées aux paramètres ObjectDataSource et que sa méthode `Update()` est appelée. Pour nous aider dans ce cadre, la DataList fournit les propriétés et les événements suivants :

- **La [propriété`DataKeyField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  lors de la mise à jour ou de la suppression, nous devons être en mesure d’identifier chaque élément de la DataList de manière unique. Définissez cette propriété sur le champ de clé primaire des données affichées. Cela remplit la [collection de`DataKeys`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) DataList avec la valeur de `DataKeyField` spécifiée pour chaque élément DataList.
- **L' [événement`EditCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  se déclenche lorsque l’utilisateur clique sur un bouton, LinkButton ou ImageButton dont la propriété `CommandName` a la valeur Edit.
- **L' [événement`CancelCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  se déclenche lorsqu’un clic est effectué sur un bouton, LinkButton ou ImageButton dont la propriété `CommandName` est définie sur Annuler.
- **L' [événement`UpdateCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  se déclenche lorsqu’un clic est effectué sur un bouton, LinkButton ou ImageButton dont la propriété `CommandName` a la valeur Update.
- **L' [événement`DeleteCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  se déclenche quand l’utilisateur clique sur un bouton, LinkButton ou ImageButton dont la propriété `CommandName` est définie sur supprimer.

À l’aide de ces propriétés et événements, il existe quatre approches que nous pouvons utiliser pour mettre à jour et supprimer des données de la DataList :

1. **À l’aide des Techniques ASP.net 1. x** , la DataList existait avant ASP.NET 2,0 et ObjectDataSources et a pu mettre à jour et supprimer des données entièrement par le biais de moyens par programmation. Cette technique permet d’abandonner l’ObjectDataSource et requiert que nous lions les données au contrôle DataList directement à partir de la couche de logique métier, à la fois pour récupérer les données à afficher et lors de la mise à jour ou de la suppression d’un enregistrement.
2. **À l’aide d’un seul contrôle ObjectDataSource sur la page pour la sélection, la mise à jour et la suppression** , tandis que le contrôle DataList ne dispose pas des fonctionnalités de modification et de suppression inhérentes à GridView, il n’y a aucune raison de les ajouter. Avec cette approche, nous utilisons un ObjectDataSource comme dans les exemples de GridView, mais vous devez créer un gestionnaire d’événements pour l’événement DataList s `UpdateCommand` où nous définissons les paramètres ObjectDataSource et appelez sa méthode `Update()`.
3. **À l’aide d’un contrôle ObjectDataSource pour la sélection, mais la mise à jour et la suppression directe par rapport à la couche BLL lors de** l’utilisation de l’option 2, nous devons écrire un peu de code dans le `UpdateCommand` événement, en affectant des valeurs de paramètre, etc. Au lieu de cela, nous pouvons utiliser l’ObjectDataSource pour sélectionner, mais effectuer les appels de mise à jour et de suppression directement par rapport à la couche BLL (comme avec l’option 1). À mon avis, la mise à jour des données en étant directement associées à la couche BLL amène un code plus lisible que l’affectation de l’ObjectDataSource s `UpdateParameters` et l’appel de sa méthode `Update()`.
4. L' **utilisation de moyens déclaratifs par le biais de plusieurs ObjectDataSources** les trois approches précédentes requiert un peu de code. Si vous avez plutôt utilisé le plus de syntaxe déclarative possible, une dernière option consiste à inclure plusieurs ObjectDataSources sur la page. Le premier ObjectDataSource récupère les données de la couche BLL et les lie au contrôle DataList. Pour la mise à jour, un autre ObjectDataSource est ajouté, mais ajouté directement dans le `EditItemTemplate`DataList. Pour inclure la prise en charge de la suppression, une autre ObjectDataSource serait nécessaire dans le `ItemTemplate`. Avec cette approche, ces ObjectDataSource incorporés utilisent `ControlParameters` pour lier de façon déclarative les paramètres de l’ObjectDataSource s aux contrôles d’entrée utilisateur (au lieu de devoir les spécifier par programmation dans le gestionnaire d’événements de la DataList `UpdateCommand`). Cette approche nécessite toujours un peu de code dont nous avons besoin pour appeler le `Update()` ObjectDataSource incorporé ou la commande `Delete()`, mais elle nécessite beaucoup moins que avec les trois autres approches. L’inconvénient, c’est que le ObjectDataSources multiple est trop encombré sur la page, ce qui est un détournement de la lisibilité globale.

Si l’utilisation de l’une de ces approches est forcée, j’ai choisi l’option 1, car elle offre la plus grande flexibilité et parce que la DataList a été conçue à l’origine pour prendre en charge ce modèle. Alors que la DataList a été étendue pour fonctionner avec les contrôles de source de données ASP.NET 2,0, elle n’a pas tous les points d’extensibilité ou les fonctionnalités des contrôles Web de données ASP.NET 2,0 officiels (GridView, DetailsView et FormView). Toutefois, les options 2 à 4 ne sont pas sans mérite.

Cela et les didacticiels de modification et de suppression ultérieurs utiliseront un ObjectDataSource pour récupérer les données à afficher et les appels directs à la couche BLL pour mettre à jour et supprimer des données (option 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Étape 3 : ajout de DataList et configuration de son ObjectDataSource

Dans ce didacticiel, nous allons créer un contrôle DataList qui répertorie les informations sur les produits et, pour chaque produit, permet à l’utilisateur de modifier le nom et le prix et de supprimer entièrement le produit. En particulier, nous récupérerons les enregistrements à afficher à l’aide d’ObjectDataSource, mais vous exécuterez les actions de mise à jour et de suppression en les interagissant directement avec la couche BLL. Avant de vous soucier de l’implémentation des fonctionnalités de modification et de suppression dans la DataList, vous devez d’abord obtenir la page pour afficher les produits dans une interface en lecture seule. Étant donné que nous avons examiné ces étapes dans les didacticiels précédents, je les examinerai rapidement.

Commencez par ouvrir la page `Basics.aspx` dans le dossier `EditDeleteDataList` et, à partir du Mode Création, ajoutez un contrôle DataList à la page. Ensuite, à partir de la balise active de DataList s, créez un nouvel ObjectDataSource. Étant donné que nous travaillons sur des données de produit, configurez-les pour utiliser la classe `ProductsBLL`. Pour récupérer *tous les* produits, choisissez la méthode `GetProducts()` dans l’onglet sélectionner.

[![configurer ObjectDataSource pour utiliser la classe ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Figure 4**: configurer ObjectDataSource pour utiliser la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))

[![retourner les informations sur le produit à l’aide de la méthode GetProducts ()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Figure 5**: renvoyer les informations sur le produit à l’aide de la méthode `GetProducts()` ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))

Le contrôle DataList, comme le contrôle GridView, n’est pas conçu pour l’insertion de nouvelles données ; par conséquent, sélectionnez l’option (aucune) dans la liste déroulante de l’onglet Insérer. Choisissez également (aucune) pour les onglets mettre à jour et supprimer puisque les mises à jour et les suppressions sont effectuées par programme via la couche BLL.

[![confirmer que les listes déroulantes des onglets insertion, mise à jour et suppression de ObjectDataSource ont la valeur (aucune)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Figure 6**: vérifier que les listes déroulantes des onglets insertion, mise à jour et suppression de l’ObjectDataSource sont définies sur (aucune) ([cliquez pour afficher l’image en plein écran](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))

Après avoir configuré l’ObjectDataSource, cliquez sur terminer, en revenant au concepteur. Comme nous l’avons vu dans les exemples précédents, lorsque vous terminez la configuration de ObjectDataSource, Visual Studio crée automatiquement un `ItemTemplate` pour le DropDownList, en affichant chacun des champs de données. Remplacez cette `ItemTemplate` par une autre qui affiche uniquement le nom et le prix du produit. En outre, affectez la valeur 2 à la propriété `RepeatColumns`.

> [!NOTE]
> Comme nous l’avons vu dans le didacticiel sur l' *insertion, la mise à jour et la suppression des données* , lors de la modification de données à l’aide de ObjectDataSource, notre architecture requiert que nous supprimions la propriété `OldValuesParameterFormatString` du balisage déclaratif de ObjectDataSource s (ou que vous la reconfiguriez à sa valeur par défaut, `{0}`). Dans ce didacticiel, toutefois, nous utilisons l’ObjectDataSource uniquement pour récupérer des données. Par conséquent, nous n’avons pas besoin de modifier la valeur de la propriété ObjectDataSource s `OldValuesParameterFormatString` (bien qu’elle n’ait pas de mal à le faire).

Après avoir remplacé le `ItemTemplate` DataList par défaut par un contrôle personnalisé, le balisage déclaratif sur votre page doit ressembler à ce qui suit :

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Prenez un moment pour voir notre progression dans un navigateur. Comme le montre la figure 7, le contrôle DataList affiche le nom de produit et le prix unitaire pour chaque produit en deux colonnes.

[![les noms des produits et les prix sont affichés dans un contrôle DataList à deux colonnes](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Figure 7**: les noms et les prix des produits s’affichent dans un contrôle DataList à deux colonnes ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))

> [!NOTE]
> La DataList possède un certain nombre de propriétés qui sont requises pour le processus de mise à jour et de suppression, et ces valeurs sont stockées dans l’état d’affichage. Par conséquent, lors de la création d’un contrôle DataList qui prend en charge la modification ou la suppression de données, il est essentiel que l’état d’affichage de DataList s soit activé.  
>   
> Le lecteur astucieux peut vous rappeler que nous avons pu désactiver l’état d’affichage lors de la création de GridViews modifiables, de DetailsViews et de FormViews. Cela est dû au fait que les contrôles Web ASP.NET 2,0 peuvent inclure l' *État du contrôle*, qui est l’état rendu persistant entre les publications telles que l’état d’affichage, mais est jugé essentiel.

La désactivation de l’état d’affichage dans le GridView ignore simplement les informations d’État trivial, mais conserve l’état du contrôle (qui comprend l’État nécessaire pour la modification et la suppression). La DataList, ayant été créée dans le ASP.NET 1. x, n’utilise pas l’état du contrôle et doit donc avoir activé l’état d’affichage. Pour plus d’informations sur l’objectif de l’état du contrôle et son différences par rapport à l’état d’affichage, consultez contrôler l’État et l' [État d’affichage](https://msdn.microsoft.com/library/1whwt1k7.aspx) .

## <a name="step-4-adding-an-editing-user-interface"></a>Étape 4 : ajout d’une interface utilisateur de modification

Le contrôle GridView est composé d’une collection de champs (BoundFields, CheckBoxFields, TemplateFields, etc.). Ces champs peuvent ajuster le balisage rendu en fonction de leur mode. Par exemple, en mode lecture seule, un BoundField affiche la valeur du champ de données sous forme de texte ; en mode édition, il restitue un contrôle Web TextBox dont la valeur du champ de données est assignée à la propriété `Text`.

La DataList, en revanche, restitue ses éléments à l’aide de modèles. Les éléments en lecture seule sont rendus à l’aide de la `ItemTemplate`, alors que les éléments en mode édition sont rendus via le `EditItemTemplate`. À ce stade, notre DataList n’a qu’une `ItemTemplate`. Pour prendre en charge la fonctionnalité de modification au niveau de l’élément, nous devons ajouter un `EditItemTemplate` qui contient le balisage à afficher pour l’élément modifiable. Pour ce didacticiel, nous allons utiliser les contrôles Web de zone de texte pour modifier le nom du produit et le prix unitaire.

Vous pouvez créer le `EditItemTemplate` de façon déclarative ou par le biais du concepteur (en sélectionnant l’option modifier les modèles dans la balise active de DataList s). Pour utiliser l’option modifier les modèles, cliquez d’abord sur le lien modifier les modèles dans la balise active, puis sélectionnez l’élément `EditItemTemplate` dans la liste déroulante.

[![choisir de travailler avec la DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Figure 8**: choisir de travailler avec le `EditItemTemplate` DataList s ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))

Ensuite, tapez Product Name : et Price :, puis faites glisser deux contrôles TextBox de la boîte à outils vers l’interface `EditItemTemplate` sur le concepteur. Définissez les zones de texte `ID` propriétés sur `ProductName` et `UnitPrice`.

[![ajouter une zone de texte pour le nom et le prix du produit](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Figure 9**: ajouter une zone de texte pour le nom et le prix du produit ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))

Nous devons lier les valeurs de champ de données de produit correspondantes aux propriétés de `Text` des deux zones de texte. À partir des balises actives des zones de texte, cliquez sur le lien modifier les DataBindings, puis associez le champ de données approprié à la propriété `Text`, comme illustré à la figure 10.

> [!NOTE]
> Lors de la liaison du champ de données `UnitPrice` au champ de `Text` de la zone de texte Price, vous pouvez le formater en tant que valeur monétaire (`{0:C}`), en nombre général (`{0:N}`) ou en le laissant non formaté.

![Lier les champs de données ProductName et UnitPrice aux propriétés Text des zones de texte](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Figure 10**: lier les champs de données `ProductName` et `UnitPrice` aux propriétés de `Text` des zones de texte

Notez que la boîte de dialogue Modifier les DataBindings de la figure 10 n’inclut *pas* la case à cocher liaison de liaison bidirectionnelle qui est présente lors de la modification d’un TemplateField dans le contrôle GridView ou DetailsView, ou un modèle dans le contrôle FormView. La fonctionnalité de liaison de données bidirectionnelle permettait d’affecter automatiquement la valeur entrée dans le contrôle Web d’entrée au `InsertParameters` ObjectDataSource correspondant ou `UpdateParameters` lors de l’insertion ou de la mise à jour des données. La DataList ne prend pas en charge la liaison de données bidirectionnelle, comme nous le verrons plus loin dans ce didacticiel, une fois que l’utilisateur a effectué ses modifications et est prêt à mettre à jour les données, nous devons accéder par programmation à ces zones de texte `Text` propriétés et transmettre leurs valeurs à la méthode de `UpdateProduct` appropriée dans la classe `ProductsBLL`.

Enfin, nous devons ajouter des boutons mettre à jour et annuler au `EditItemTemplate`. Comme nous l’avons vu dans le [maître/détail à l’aide d’une liste à puces d’enregistrements maîtres avec un didacticiel des détails DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) , quand vous cliquez sur un bouton, un LinkButton ou un ImageButton dont la propriété `CommandName` est définie sur un clic dans un Repeater ou un contrôle DataList, l’événement Repeater ou datalist s `ItemCommand` est déclenché. Pour la DataList, si la propriété `CommandName` est définie sur une certaine valeur, un événement supplémentaire peut également être déclenché. Les valeurs de propriété `CommandName` spéciales incluent, entre autres :

- Cancel déclenche l’événement `CancelCommand`
- La modification déclenche l’événement `EditCommand`
- Update déclenche l’événement `UpdateCommand`

Gardez à l’esprit que ces événements sont déclenchés *en plus de* l’événement `ItemCommand`.

Ajoutez à la `EditItemTemplate` deux contrôles Web Button, l’un dont `CommandName` a la valeur Update et l’autre est défini sur CANCEL. Après avoir ajouté ces deux contrôles Web Button, le concepteur doit ressembler à ce qui suit :

[![ajouter des boutons Update et Cancel à EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Figure 11**: ajouter des boutons mettre à jour et annuler au `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))

Une fois le `EditItemTemplate` terminé, le balisage déclaratif s doit ressembler à ce qui suit :

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Étape 5 : ajout de la plomberie pour passer en mode édition

À ce stade, notre DataList a une interface de modification définie par le biais de son `EditItemTemplate`; Toutefois, il n’existe actuellement aucun moyen pour un utilisateur visitant notre page pour indiquer qu’il souhaite modifier les informations d’un produit. Nous devons ajouter un bouton modifier à chaque produit qui, lorsque vous cliquez dessus, effectue le rendu de cet élément DataList en mode édition. Commencez par ajouter un bouton modifier à la `ItemTemplate`, par le biais du concepteur ou de façon déclarative. Veillez à définir la propriété modifier le bouton s `CommandName` sur modifier.

Après avoir ajouté ce bouton de modification, prenez un moment pour afficher la page dans un navigateur. Avec cet ajout, chaque produit doit inclure un bouton modifier.

[![ajouter des boutons Update et Cancel à EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Figure 12**: ajouter des boutons mettre à jour et annuler au `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))

Le fait de cliquer sur le bouton entraîne une publication, mais ne met *pas* la liste des produits en mode édition. Pour modifier le produit, nous devons :

1. Affectez à la [propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) DataList s`EditItemIndex` l’index du `DataListItem` dont l’utilisateur a cliqué sur le bouton modifier.
2. Reliez les données au contrôle DataList. Lorsque le contrôle DataList est restitué à nouveau, le `DataListItem` dont `ItemIndex` correspond à DataList s `EditItemIndex` s’affiche à l’aide de son `EditItemTemplate`.

Étant donné que l’événement DataList s `EditCommand` est déclenché lorsque l’utilisateur clique sur le bouton modifier, créez un gestionnaire d’événements `EditCommand` à l’aide du code suivant :

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

Le gestionnaire d’événements `EditCommand` est passé dans un objet de type `DataListCommandEventArgs` en tant que second paramètre d’entrée, qui comprend une référence au `DataListItem` dont l’utilisateur a cliqué sur le bouton modifier (`e.Item`). Le gestionnaire d’événements définit tout d’abord le `EditItemIndex` DataList sur la `ItemIndex` du `DataListItem` modifiable, puis relit les données au contrôle DataList en appelant la méthode DataList s `DataBind()`.

Après avoir ajouté ce gestionnaire d’événements, réexaminez la page dans un navigateur. Le fait de cliquer sur le bouton Modifier permet désormais de modifier le produit sur lequel vous avez cliqué (voir figure 13).

[![lorsque vous cliquez sur le bouton modifier, le produit est modifiable](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Figure 13**: le fait de cliquer sur le bouton Modifier permet de modifier le produit ([Cliquer pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))

## <a name="step-6-saving-the-user-s-changes"></a>Étape 6 : enregistrement des modifications apportées aux utilisateurs

Le fait de cliquer sur les boutons mettre à jour ou annuler du produit modifié ne fait rien à ce stade. pour ajouter cette fonctionnalité, nous devons créer des gestionnaires d’événements pour les événements DataList `UpdateCommand` et `CancelCommand`. Commencez par créer le `CancelCommand` gestionnaire d’événements, qui s’exécute quand l’utilisateur clique sur le bouton Annuler du produit édité et qu’il est chargé de rétablir l’état antérieur à la modification du contrôle DataList.

Pour que la DataList restitue tous ses éléments en mode lecture seule, nous devons :

1. Affectez à la [propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) DataList s`EditItemIndex` l’index d’un index `DataListItem` inexistant. `-1` est un choix sûr, puisque les index `DataListItem` commencent à `0`.
2. Reliez les données au contrôle DataList. Dans la mesure où aucun `DataListItem` `ItemIndex` es ne correspond aux `EditItemIndex`DataList, le contrôle DataList entier est restitué en mode lecture seule.

Vous pouvez effectuer ces étapes à l’aide du code de gestionnaire d’événements suivant :

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Avec cet ajout, le clic sur le bouton Annuler renvoie le contrôle DataList à son état antérieur à la modification.

Le dernier gestionnaire d’événements que nous devons terminer est le gestionnaire d’événements `UpdateCommand`. Ce gestionnaire d’événements doit :

1. Accédez par programmation au nom et au prix du produit entré par l’utilisateur, ainsi qu’au `ProductID`du produit modifié.
2. Initiez le processus de mise à jour en appelant la surcharge de `UpdateProduct` appropriée dans la classe `ProductsBLL`.
3. Affectez à la [propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) DataList s`EditItemIndex` l’index d’un index `DataListItem` inexistant. `-1` est un choix sûr, puisque les index `DataListItem` commencent à `0`.
4. Reliez les données au contrôle DataList. Dans la mesure où aucun `DataListItem` `ItemIndex` es ne correspond aux `EditItemIndex`DataList, le contrôle DataList entier est restitué en mode lecture seule.

Les étapes 1 et 2 sont responsables de l’enregistrement des modifications apportées aux utilisateurs. les étapes 3 et 4 retournent le contrôle DataList à son état antérieur à la modification après l’enregistrement des modifications et sont identiques aux étapes effectuées dans le gestionnaire d’événements `CancelCommand`.

Pour récupérer le nom et le prix du produit mis à jour, nous devons utiliser la méthode `FindControl` pour référencer par programmation les contrôles Web TextBox dans le `EditItemTemplate`. Nous devons également récupérer la valeur de `ProductID` du produit modifié. Lorsque nous avons initialement lié ObjectDataSource au contrôle DataList, Visual Studio a affecté la propriété DataList s `DataKeyField` à la valeur de clé primaire de la source de données (`ProductID`). Cette valeur peut ensuite être récupérée à partir de la collection de `DataKeys` DataList. Prenez un moment pour vous assurer que la propriété `DataKeyField` est effectivement définie sur `ProductID`.

Le code suivant implémente les quatre étapes :

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

Le gestionnaire d’événements commence par lire dans le `ProductID` du produit modifié à partir de la collection de `DataKeys`. Ensuite, les deux zones de texte de la `EditItemTemplate` sont référencées et leurs propriétés `Text` stockées dans des variables locales, `productNameValue` et `unitPriceValue`. Nous utilisons la méthode `Decimal.Parse()` pour lire la valeur de la zone de texte `UnitPrice` de sorte que si la valeur entrée comporte un symbole monétaire, elle peut toujours être convertie correctement en valeur `Decimal`.

> [!NOTE]
> Les valeurs des zones de texte `ProductName` et `UnitPrice` sont uniquement affectées aux variables productNameValue et unitPriceValue si les propriétés Text des zones de texte ont une valeur spécifiée. Dans le cas contraire, la valeur `Nothing` est utilisée pour les variables, ce qui a pour effet de mettre à jour les données avec une valeur de `NULL` de base de données. Autrement dit, notre Code traite la conversion des chaînes vides en valeurs de `NULL` de base de données, ce qui correspond au comportement par défaut de l’interface de modification des contrôles GridView, DetailsView et FormView.

Après avoir lu les valeurs, la méthode `ProductsBLL` classe s `UpdateProduct` est appelée, en passant le nom, le prix et l' `ProductID`du produit. Le gestionnaire d’événements se termine en retournant le contrôle DataList à son état antérieur à la modification, en utilisant exactement la même logique que dans le gestionnaire d’événements `CancelCommand`.

Avec les gestionnaires d’événements `EditCommand`, `CancelCommand`et `UpdateCommand` terminés, un visiteur peut modifier le nom et le prix d’un produit. Les figures 14-16 illustrent ce flux de travail d’édition en action.

[![lors de la première visite de la page, tous les produits sont en mode lecture seule](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Figure 14**: lors de la première visite de la page, tous les produits sont en mode lecture seule ([cliquez pour afficher l’image en plein écran](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))

[![pour mettre à jour le nom ou le prix d’un produit, cliquez sur le bouton modifier](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Figure 15**: pour mettre à jour le nom ou le prix d’un produit, cliquez sur le bouton modifier ([cliquez pour afficher l’image en taille réelle](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))

[![après avoir modifié la valeur, cliquez sur mettre à jour pour revenir au mode lecture seule.](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Figure 16**: après avoir modifié la valeur, cliquez sur mettre à jour pour revenir au mode lecture seule ([cliquez pour afficher l’image en plein écran](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))

## <a name="step-7-adding-delete-capabilities"></a>Étape 7 : ajout de fonctionnalités de suppression

Les étapes permettant d’ajouter des fonctionnalités de suppression à un contrôle DataList sont similaires à celles de l’ajout de fonctionnalités de modification. En bref, nous devons ajouter un bouton supprimer à la `ItemTemplate` qui, lorsque vous cliquez dessus :

1. Lit les `ProductID` du produit correspondant via la collection de `DataKeys`.
2. Exécute la suppression en appelant la méthode `ProductsBLL` de la classe s `DeleteProduct`.
3. Relit les données au contrôle DataList.

Commençons par ajouter un bouton supprimer à la `ItemTemplate`.

Lorsque vous cliquez sur un bouton dont le `CommandName` est modifier, mettre à jour ou annuler déclenche l’événement DataList `ItemCommand` avec un événement supplémentaire (par exemple, lors de l’utilisation de modifier l’événement `EditCommand` est également déclenché). De même, tout bouton, LinkButton ou ImageButton dans le contrôle DataList dont la propriété `CommandName` est définie sur Delete provoque le déclenchement de l’événement `DeleteCommand` (avec `ItemCommand`).

Ajoutez un bouton supprimer en regard du bouton modifier dans la `ItemTemplate`, en affectant à sa propriété `CommandName` la valeur Delete. Une fois que vous avez ajouté ce bouton, vous devez `ItemTemplate` syntaxe déclarative :

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

Ensuite, créez un gestionnaire d’événements pour l’événement DataList s `DeleteCommand`, à l’aide du code suivant :

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Le fait de cliquer sur le bouton supprimer entraîne une publication (postback) et déclenche l’événement DataList s `DeleteCommand`. Dans le gestionnaire d’événements, la valeur de `ProductID` du produit cliqué est accessible à partir de la collection de `DataKeys`. Ensuite, le produit est supprimé en appelant la méthode `ProductsBLL` classe s `DeleteProduct`.

Après la suppression du produit, il est important de relier les données au contrôle DataList (`DataList1.DataBind()`). dans le cas contraire, le contrôle DataList continue d’afficher le produit qui vient d’être supprimé.

## <a name="summary"></a>Récapitulatif

Alors que la DataList n’a pas le point et clique sur la modification et la suppression de la prise en charge par le GridView, avec un peu de code, il peut être amélioré pour inclure ces fonctionnalités. Dans ce didacticiel, nous avons vu comment créer une liste à deux colonnes de produits susceptibles d’être supprimés et dont le nom et le prix pouvaient être modifiés. L’ajout de la prise en charge de la modification et de la suppression consiste à inclure les contrôles Web appropriés dans le `ItemTemplate` et `EditItemTemplate`, à créer les gestionnaires d’événements correspondants, à lire les valeurs entrées par l’utilisateur et clés primaires et à se conformer à la couche de logique métier.

Alors que nous avons ajouté des fonctionnalités de modification et de suppression de base au contrôle DataList, il manque des fonctionnalités plus avancées. Par exemple, il n’y a aucune validation de champ d’entrée : si un utilisateur entre un prix trop onéreux, une exception est levée par `Decimal.Parse` lors d’une tentative de conversion trop coûteuse en `Decimal`. De même, en cas de problème lors de la mise à jour des données au niveau de la logique métier ou des couches d’accès aux données, l’écran d’erreur standard s’affiche. Sans aucune confirmation du bouton supprimer, la suppression accidentelle d’un produit est trop probable.

Dans les prochains didacticiels, nous verrons comment améliorer la modification de l’expérience utilisateur.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Zack Jones, Ken Pespisa et Randy Schmidt. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](customizing-the-datalist-s-editing-interface-cs.md)
> [Suivant](performing-batch-updates-vb.md)
