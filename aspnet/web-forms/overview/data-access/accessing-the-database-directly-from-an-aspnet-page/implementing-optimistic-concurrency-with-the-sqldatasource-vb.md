---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: Implémentation de l’accès concurrentiel optimiste avec le SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner les notions fondamentales du contrôle d’accès concurrentiel optimiste, puis découvrir comment l’implémenter à l’aide du contrôle SqlDataSource.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 431734b5245c20ac840147cf0827fa7f8d1e4d17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553017"
---
# <a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implémentation de l’accès concurrentiel optimiste avec SqlDataSource (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) ou [Télécharger le PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> Dans ce didacticiel, nous allons examiner les notions fondamentales du contrôle d’accès concurrentiel optimiste, puis découvrir comment l’implémenter à l’aide du contrôle SqlDataSource.

## <a name="introduction"></a>Introduction

Dans le didacticiel précédent, nous avons examiné comment ajouter des fonctionnalités d’insertion, de mise à jour et de suppression au contrôle SqlDataSource. En résumé, pour fournir ces fonctionnalités, nous avons besoin de spécifier l’instruction SQL `INSERT`, `UPDATE`ou `DELETE` dans les propriétés Control s `InsertCommand`, `UpdateCommand`ou `DeleteCommand`, ainsi que les paramètres appropriés dans les collections `InsertParameters`, `UpdateParameters`et `DeleteParameters`. Si ces propriétés et ces regroupements peuvent être spécifiés manuellement, le bouton Paramètres avancés de l’Assistant Configuration de la source de données offre une case à cocher générer des instructions `INSERT`, `UPDATE`et `DELETE` qui crée automatiquement ces instructions en fonction de l’instruction de `SELECT`.

Avec la case à cocher générer `INSERT`, `UPDATE`et `DELETE` des instructions, la boîte de dialogue Options de génération SQL avancées comprend une option utiliser l’accès concurrentiel optimiste (voir figure 1). Lorsque cette option est activée, les clauses `WHERE` dans les instructions `UPDATE` et `DELETE` générées automatiquement sont modifiées pour effectuer uniquement la mise à jour ou la suppression si les données de la base de données sous-jacente n’ont pas été modifiées depuis le dernier chargement des données dans la grille par l’utilisateur.

![Vous pouvez ajouter la prise en charge de l’accès concurrentiel optimiste à partir de la boîte de dialogue Options de génération SQL avancées](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Figure 1**: vous pouvez ajouter la prise en charge de l’accès concurrentiel optimiste à partir de la boîte de dialogue Options de génération SQL avancées

De retour dans le didacticiel sur l’implémentation de l' [accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) , nous avons examiné les principes de base du contrôle d’accès concurrentiel optimiste et comment l’ajouter à ObjectDataSource. Dans ce didacticiel, nous allons examiner les notions fondamentales du contrôle d’accès concurrentiel optimiste, puis découvrir comment l’implémenter à l’aide du SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Récapitulatif de l’accès concurrentiel optimiste

Pour les applications Web qui permettent à plusieurs utilisateurs simultanés de modifier ou de supprimer les mêmes données, il existe un risque qu’un utilisateur remplace accidentellement d’autres modifications. Dans le didacticiel implémentation de l' [accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) , j’ai fourni l’exemple suivant :

Imaginez que deux utilisateurs, Jisun et Sam, visitaient une page dans une application qui permettait aux visiteurs de mettre à jour et de supprimer des produits via un contrôle GridView. Les deux cliquez sur le bouton modifier pour Chai en même temps. Jisun modifie le nom du produit en thé chai et clique sur le bouton mettre à jour. Le résultat net est une instruction `UPDATE` qui est envoyée à la base de données, qui définit *tous* les champs modifiables du produit (même si Jisun a mis à jour un seul champ, `ProductName`). À ce stade, la base de données a les valeurs Chai thé, les boissons de catégorie, les fournisseurs exotiques et ainsi de suite pour ce produit particulier. Toutefois, l’écran GridView sur le Sam s affiche toujours le nom du produit dans la ligne de GridView modifiable comme Chai. Quelques secondes après la validation des modifications de Jisun, Sam met à jour la catégorie en condiments et clique sur mettre à jour. Il en résulte une instruction `UPDATE` envoyée à la base de données qui définit le nom du produit sur Chai, le `CategoryID` à l’ID de catégorie des condiments correspondants, et ainsi de suite. Jisun s les modifications apportées au nom du produit ont été remplacées.

La figure 2 illustre cette interaction.

[![lorsque deux utilisateurs mettent à jour simultanément un enregistrement, il est possible que les modifications d’un utilisateur remplacent les autres](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Figure 2**: lorsque deux utilisateurs mettent à jour simultanément un enregistrement, il peut y avoir des modifications de l’utilisateur pour remplacer les autres s ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))

Pour éviter le dépliage de ce scénario, vous devez implémenter une forme de [contrôle d’accès concurrentiel](http://en.wikipedia.org/wiki/Concurrency_control) . [Accès concurrentiel optimiste](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) l’objectif de ce didacticiel fonctionne en partant du principe que, s’il peut y avoir des conflits d’accès concurrentiel à chaque instant, puis, la grande majorité du temps de tels conflits ne se produira pas. Par conséquent, en cas de conflit, le contrôle d’accès concurrentiel optimiste informe simplement l’utilisateur que ses modifications peuvent être enregistrées parce qu’un autre utilisateur a modifié les mêmes données.

> [!NOTE]
> Pour les applications pour lesquelles il est supposé qu’il y a de nombreux conflits d’accès concurrentiel ou si ces conflits ne sont pas acceptables, le contrôle d’accès concurrentiel pessimiste peut être utilisé à la place. Reportez-vous au didacticiel sur l’implémentation de l' [accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) pour une discussion plus approfondie sur le contrôle d’accès concurrentiel pessimiste.

Le contrôle d’accès concurrentiel optimiste fonctionne en s’assurant que l’enregistrement en cours de mise à jour ou de suppression a les mêmes valeurs que lors du démarrage de la mise à jour ou de la suppression. Par exemple, quand vous cliquez sur le bouton modifier dans un GridView modifiable, les valeurs de l’enregistrement sont lues à partir de la base de données et affichées dans des zones de texte et d’autres contrôles Web. Ces valeurs d’origine sont enregistrées par le contrôle GridView. Plus tard, une fois que l’utilisateur a effectué ses modifications et cliqué sur le bouton mettre à jour, l’instruction `UPDATE` utilisée doit prendre en compte les valeurs d’origine et les nouvelles valeurs et mettre à jour l’enregistrement de base de données sous-jacent uniquement si les valeurs d’origine que l’utilisateur a commencé à modifier sont identiques aux valeurs figurant toujours dans la base de données. La figure 3 illustre cette séquence d’événements.

[![pour que la mise à jour ou la suppression aboutisse, les valeurs d’origine doivent être égales aux valeurs de la base de données actuelle](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Figure 3**: pour que la mise à jour ou la suppression aboutisse, les valeurs d’origine doivent être égales aux valeurs de la base de données actuelle ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))

Il existe différentes approches pour implémenter l’accès concurrentiel optimiste (consultez la logique de [mise à jour de la concurrence optimiste](http://www.eggheadcafe.com/articles/20050719.asp) de [Peter A. Bromberg](http://peterbromberg.net/)pour obtenir un bref aperçu d’un certain nombre d’options). La technique utilisée par le SqlDataSource (ainsi que par les DataSets typés ADO.NET utilisés dans notre couche d’accès aux données) augmente la clause `WHERE` pour inclure une comparaison de toutes les valeurs d’origine. Par exemple, l’instruction `UPDATE` suivante met à jour le nom et le prix d’un produit uniquement si les valeurs de la base de données actuelle sont égales aux valeurs récupérées à l’origine lors de la mise à jour de l’enregistrement dans le GridView. Les paramètres `@ProductName` et `@UnitPrice` contiennent les nouvelles valeurs entrées par l’utilisateur, tandis que `@original_ProductName` et `@original_UnitPrice` contiennent les valeurs qui ont été chargées à l’origine dans le contrôle GridView lors d’un clic sur le bouton modifier :

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Comme nous le verrons dans ce didacticiel, l’activation du contrôle d’accès concurrentiel optimiste avec le SqlDataSource est aussi simple que la vérification d’une case à cocher.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Étape 1 : création d’un SqlDataSource qui prend en charge l’accès concurrentiel optimiste

Commencez par ouvrir la page `OptimisticConcurrency.aspx` dans le dossier `SqlDataSource`. Faites glisser un contrôle SqlDataSource de la boîte à outils vers le concepteur, en définissant sa propriété `ID` sur `ProductsDataSourceWithOptimisticConcurrency`. Ensuite, cliquez sur le lien configurer la source de données dans la balise active contrôle s. Dans le premier écran de l’Assistant, choisissez de travailler avec le `NORTHWINDConnectionString`, puis cliquez sur suivant.

[![choisir de travailler avec NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Figure 4**: choisir de travailler avec le `NORTHWINDConnectionString` ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))

Pour cet exemple, nous allons ajouter un GridView qui permet aux utilisateurs de modifier la table `Products`. Par conséquent, à partir de l’écran configurer l’instruction SELECT, sélectionnez la table `Products` dans la liste déroulante et sélectionnez les colonnes `ProductID`, `ProductName`, `UnitPrice`et `Discontinued`, comme illustré à la figure 5.

[![à partir de la table Products, retournez les colonnes ProductID, ProductName, UnitPrice et Discontinued](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Figure 5**: dans la table `Products`, retourner les colonnes `ProductID`, `ProductName`, `UnitPrice`et `Discontinued` ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))

Après avoir choisi les colonnes, cliquez sur le bouton avancé pour afficher la boîte de dialogue Options de génération SQL avancées. Cochez les instructions Generate `INSERT`, `UPDATE`et `DELETE` et utilisez les cases à cocher accès concurrentiel optimiste, puis cliquez sur OK (reportez-vous à la figure 1 pour obtenir une capture d’écran). Terminez l’Assistant en cliquant sur suivant, puis sur Terminer.

Après avoir terminé l’Assistant Configuration de la source de données, prenez un moment pour examiner les propriétés `DeleteCommand` et `UpdateCommand` qui en résultent, ainsi que les collections `DeleteParameters` et `UpdateParameters`. Pour ce faire, le plus simple est de cliquer sur l’onglet source dans le coin inférieur gauche pour afficher la syntaxe déclarative de la page. Vous y trouverez une valeur `UpdateCommand` de :

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Avec sept paramètres dans la collection `UpdateParameters` :

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

De même, les `DeleteCommand` propriété et `DeleteParameters` collection doivent ressembler à ce qui suit :

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Outre l’augmentation des clauses `WHERE` des propriétés `UpdateCommand` et `DeleteCommand` (et l’ajout des paramètres supplémentaires aux collections de paramètres respectives), la sélection de l’option utiliser l’accès concurrentiel optimiste ajuste deux autres propriétés :

- Remplace la [propriété`ConflictDetection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) de `OverwriteChanges` (valeur par défaut) par `CompareAllValues`
- Remplace la [propriété`OldValuesParameterFormatString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) de {0} (valeur par défaut) par la valeur d’origine\_{0}.

Lorsque le contrôle Web de données appelle la méthode SqlDataSource s `Update()` ou `Delete()`, il passe les valeurs d’origine. Si la propriété SqlDataSource s `ConflictDetection` est définie sur `CompareAllValues`, ces valeurs d’origine sont ajoutées à la commande. La propriété `OldValuesParameterFormatString` fournit le modèle d’affectation de noms utilisé pour ces paramètres de valeur d’origine. L’Assistant Configuration de la source de données utilise l'\_d’origine {0} et nomme chaque paramètre d’origine dans les propriétés `UpdateCommand` et `DeleteCommand`, ainsi que les collections `UpdateParameters` et `DeleteParameters` en conséquence.

> [!NOTE]
> Étant donné que nous n’utilisons pas les capacités d’insertion du contrôle SqlDataSource, n’hésitez pas à supprimer la propriété `InsertCommand` et sa collection `InsertParameters`.

## <a name="correctly-handlingnullvalues"></a>Gestion correcte des valeurs de`NULL`

Malheureusement, les instructions `UPDATE` et `DELETE` augmentées générées automatiquement par l’Assistant Configuration de la source de données lors de l’utilisation de l’accès concurrentiel optimiste ne fonctionnent *pas* avec les enregistrements qui contiennent des valeurs `NULL`. Pour en savoir plus, examinez nos `UpdateCommand`SqlDataSource :

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

La `UnitPrice` colonne de la table `Products` peut avoir des valeurs `NULL`. Si un enregistrement particulier a une valeur `NULL` pour `UnitPrice`, la partie de la clause `WHERE` `[UnitPrice] = @original_UnitPrice` prend *toujours* la valeur false, car `NULL = NULL` retourne toujours false. Par conséquent, les enregistrements qui contiennent des valeurs `NULL` ne peuvent pas être modifiés ou supprimés, car les instructions `UPDATE` et `DELETE` `WHERE` clauses ne renvoient aucune ligne à mettre à jour ou à supprimer.

> [!NOTE]
> Ce bogue a été signalé pour la première fois à Microsoft en juin 2004 dans le [SqlDataSource génère des instructions SQL incorrectes](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) et il est prévu qu’il soit corrigé dans la prochaine version de ASP.net.

Pour résoudre ce problème, nous devons mettre à jour manuellement les clauses `WHERE` dans les propriétés `UpdateCommand` et `DeleteCommand` pour **toutes les** colonnes qui peuvent avoir des valeurs `NULL`. En général, remplacez `[ColumnName] = @original_ColumnName` par :

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Cette modification peut être effectuée directement via le balisage déclaratif, via les options UpdateQuery ou DeleteQuery du Fenêtre Propriétés, ou via les onglets mettre à jour et supprimer de l’option spécifier une instruction SQL personnalisée ou une procédure stockée dans configurer les données. Assistant source. Là encore, cette modification doit être effectuée pour *chaque* colonne de la clause `UpdateCommand` et `DeleteCommand` s `WHERE` qui peut contenir des valeurs `NULL`.

L’application de cet exemple à l’exemple aboutit aux valeurs `UpdateCommand` et `DeleteCommand` suivantes :

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Étape 2 : ajout d’un contrôle GridView avec les options modifier et supprimer

Avec le SqlDataSource configuré pour prendre en charge l’accès concurrentiel optimiste, il ne reste plus qu’à ajouter un contrôle Web de données à la page qui utilise ce contrôle d’accès concurrentiel. Pour ce didacticiel, vous allez ajouter un GridView qui fournit des fonctionnalités de modification et de suppression. Pour ce faire, faites glisser un contrôle GridView de la boîte à outils vers le concepteur et affectez à son `ID` la valeur `Products`. À partir de la balise active GridView s, liez-le au contrôle `ProductsDataSourceWithOptimisticConcurrency` SqlDataSource ajouté à l’étape 1. Enfin, cochez les options activer la modification et activer la suppression de la balise active.

[![lier le contrôle GridView au SqlDataSource et activer la modification et la suppression](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Figure 6**: lier le contrôle GridView au SqlDataSource et activer la modification et la suppression ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))

Après avoir ajouté le contrôle GridView, configurez son apparence en supprimant le `ProductID` BoundField, en remplaçant la propriété `ProductName` BoundField `HeaderText` s par Product, et en mettant à jour le `UnitPrice` BoundField afin que sa propriété `HeaderText` soit simplement Price. Dans l’idéal, nous avons amélioré l’interface de modification pour inclure un RequiredFieldValidator pour la valeur `ProductName` et un CompareValidator pour la valeur `UnitPrice` (pour s’assurer qu’il s’agit d’une valeur numérique correctement mise en forme). Reportez-vous au didacticiel [Personnalisation de l’interface de modification des données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) pour une analyse plus approfondie de la personnalisation de l’interface de modification de GridView.

> [!NOTE]
> L’état d’affichage de GridView s doit être activé, car les valeurs d’origine transmises du contrôle GridView au SqlDataSource sont stockées dans l’état d’affichage.

Après avoir apporté ces modifications au GridView, les balises déclaratives GridView et SqlDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Pour voir le contrôle d’accès concurrentiel optimiste en action, ouvrez deux fenêtres de navigateur et chargez la page `OptimisticConcurrency.aspx` dans les deux. Cliquez sur les boutons Modifier pour le premier produit dans les deux navigateurs. Dans un navigateur, modifiez le nom du produit, puis cliquez sur mettre à jour. Le navigateur effectue une publication et le GridView retourne à son mode antérieur à la modification, en présentant le nouveau nom de produit de l’enregistrement que vous venez de modifier.

Dans la deuxième fenêtre du navigateur, modifiez le prix (mais laissez le nom du produit comme valeur d’origine), puis cliquez sur mettre à jour. Lors de la publication (postback), la grille retourne à son mode antérieur à la modification, mais la modification apportée au prix n’est pas enregistrée. Le second navigateur affiche la même valeur que la première du nouveau nom de produit avec l’ancien prix. Les modifications apportées dans la deuxième fenêtre de navigateur ont été perdues. En outre, les modifications ont été perdues plutôt en mode silencieux, car aucune exception ou message n’indique qu’une violation d’accès concurrentiel s’est produite.

[![les modifications apportées à la deuxième fenêtre du navigateur ont été perdues en mode silencieux](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Figure 7**: les modifications de la deuxième fenêtre du navigateur ont été perdues en mode silencieux ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))

La raison pour laquelle les modifications du second navigateur n’ont pas été validées était parce que la clause `UPDATE` instruction s `WHERE` a filtré tous les enregistrements et n’a donc affecté aucune ligne. Observons à nouveau l’instruction `UPDATE` :

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Lorsque la deuxième fenêtre de navigateur met à jour l’enregistrement, le nom de produit d’origine spécifié dans la clause `WHERE` ne correspond pas au nom de produit existant (dans la mesure où il a été modifié par le premier navigateur). Par conséquent, l’instruction `[ProductName] = @original_ProductName` retourne la valeur false et la `UPDATE` n’affecte pas les enregistrements.

> [!NOTE]
> La suppression fonctionne de la même manière. Avec deux fenêtres de navigateur ouvertes, commencez par modifier un produit donné avec l’un d’eux, puis enregistrez ses modifications. Après avoir enregistré les modifications dans un navigateur, cliquez sur le bouton Supprimer pour le même produit dans l’autre. Étant donné que les valeurs d’origine Don t correspondent dans la clause `DELETE` instruction s `WHERE`, la suppression silencieuse échoue.

Du point de vue de l’utilisateur final dans la deuxième fenêtre du navigateur, après avoir cliqué sur le bouton mettre à jour, la grille revient au mode antérieur à la modification, mais ses modifications ont été perdues. Toutefois, il n’y a aucun commentaire visuel indiquant que leurs modifications n’ont pas été respectées. Idéalement, si les modifications apportées par un utilisateur sont perdues à une violation d’accès concurrentiel, nous les avertissons et, éventuellement, nous conservons la grille en mode édition. Voyons comment y parvenir.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Étape 3 : détermination du moment où une violation d’accès concurrentiel s’est produite

Comme une violation d’accès concurrentiel rejette les modifications apportées, il serait intéressant d’alerter l’utilisateur lorsqu’une violation d’accès concurrentiel s’est produite. Pour alerter l’utilisateur, ajoutez un contrôle Web label en haut de la page nommée `ConcurrencyViolationMessage` dont la propriété `Text` affiche le message suivant : vous avez tenté de mettre à jour ou de supprimer un enregistrement qui a été mis à jour simultanément par un autre utilisateur. Passez en revue les modifications de l’autre utilisateur, puis réexécutez la mise à jour ou la suppression. Affectez à la propriété label Control s `CssClass` la valeur Warning, qui est une classe CSS définie dans `Styles.css` qui affiche le texte dans une police rouge, italique, gras et grande. Enfin, définissez les propriétés label s `Visible` et `EnableViewState` sur `False`. Cela masquera l’étiquette, à l’exception uniquement des publications (postbacks) où nous définissons explicitement sa propriété `Visible` sur `True`.

[![ajouter un contrôle Label à la page pour afficher l’avertissement](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Figure 8**: ajouter un contrôle Label à la page pour afficher l’avertissement ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))

Lorsque vous effectuez une mise à jour ou une suppression, les gestionnaires d’événements `RowUpdated` GridView et `RowDeleted` se déclenchent après que le contrôle de source de données a effectué la mise à jour ou la suppression demandée. Nous pouvons déterminer le nombre de lignes affectées par l’opération à partir de ces gestionnaires d’événements. Si aucune ligne n’a été affectée, nous souhaitons afficher le `ConcurrencyViolationMessage` étiquette.

Créez un gestionnaire d’événements pour les événements `RowUpdated` et `RowDeleted`, puis ajoutez le code suivant :

[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

Dans les deux gestionnaires d’événements, nous vérifions la propriété `e.AffectedRows` et, si elle est égale à 0, nous définissons la propriété `ConcurrencyViolationMessage` étiquette s `Visible` sur `True`. Dans le gestionnaire d’événements `RowUpdated`, nous indiquons également au GridView de rester en mode édition en affectant à la propriété `KeepInEditMode` la valeur true. Dans ce cas, nous devons relier les données à la grille afin que les autres données utilisateur soient chargées dans l’interface de modification. Pour ce faire, appelez la méthode GridView s `DataBind()`.

Comme le montre la figure 9, avec ces deux gestionnaires d’événements, un message très visible s’affiche chaque fois qu’une violation d’accès concurrentiel se produit.

[![un message est affiché en cas de violation d’accès concurrentiel](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Figure 9**: un message s’affiche en face d’une violation d’accès concurrentiel ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))

## <a name="summary"></a>Récapitulatif

Lors de la création d’une application Web où plusieurs utilisateurs simultanés peuvent modifier les mêmes données, il est important de prendre en compte les options de contrôle d’accès concurrentiel. Par défaut, les contrôles Web de données et les contrôles de source de données ASP.NET n’utilisent pas de contrôle d’accès concurrentiel. Comme nous l’avons vu dans ce didacticiel, l’implémentation du contrôle d’accès concurrentiel optimiste avec le SqlDataSource est relativement rapide et facile. Le SqlDataSource gère la plupart des problèmes pour l’ajout de clauses de `WHERE` augmentées aux instructions `UPDATE` et de `DELETE` générées automatiquement, mais il existe quelques subtilités pour gérer les colonnes de valeur `NULL`, comme indiqué dans la section gestion correcte des valeurs de `NULL`.

Ce didacticiel conclut notre examen du SqlDataSource. Nos autres didacticiels retourneront à l’utilisation des données à l’aide de l’ObjectDataSource et de l’architecture à plusieurs niveaux.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
