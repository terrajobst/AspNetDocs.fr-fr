---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Exécution des mises à jour par lots (VB) | Microsoft Docs
author: rick-anderson
description: Apprenez à créer un contrôle DataList entièrement modifiable dans lequel tous ses éléments sont en mode édition et dont les valeurs peuvent être enregistrées en cliquant sur le bouton « mettre à jour tout » dans le...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: e54c9fc12da278492b54164cf657eea142a3dae6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74632603"
---
# <a name="performing-batch-updates-vb"></a>Réalisation de mises à jour par lots (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) ou [Télécharger le PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Apprenez à créer un contrôle DataList entièrement modifiable dans lequel tous ses éléments sont en mode édition et dont les valeurs peuvent être enregistrées en cliquant sur un bouton « mettre à jour tout » sur la page.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) , nous avons examiné comment créer un contrôle DataList au niveau élément. À l’instar du contrôle GridView modifiable standard, chaque élément du contrôle DataList comprenait un bouton modifier qui, lorsque vous cliquez dessus, rend l’élément modifiable. Bien que cette modification au niveau de l’élément fonctionne bien pour les données qui sont uniquement mises à jour occasionnellement, certains scénarios de cas d’usage requièrent que l’utilisateur modifie de nombreux enregistrements. Si un utilisateur doit modifier des dizaines d’enregistrements et s’il est forcé de cliquer sur modifier, apporter les modifications nécessaires, puis cliquer sur mettre à jour pour chacun d’entre eux, la quantité de clics peut en nuire à la productivité. Dans ce cas, une meilleure option consiste à fournir un contrôle DataList entièrement modifiable, où *tous* ses éléments sont en mode édition et dont les valeurs peuvent être modifiées en cliquant sur un bouton mettre à jour tout sur la page (voir figure 1).

[![chaque élément d’un contrôle DataList entièrement modifiable peut être modifié](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Figure 1**: chaque élément d’un contrôle DataList entièrement modifiable peut être modifié ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-vb/_static/image3.png))

Dans ce didacticiel, nous allons examiner comment permettre aux utilisateurs de mettre à jour les informations d’adresse des fournisseurs à l’aide d’un contrôle DataList entièrement modifiable.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Étape 1 : créer l’interface utilisateur modifiable dans le ItemTemplate de DataList

Dans le didacticiel précédent, où nous créons une DataList modifiable standard au niveau élément, nous avons utilisé deux modèles :

- `ItemTemplate` contenait l’interface utilisateur en lecture seule (les contrôles Web d’étiquette permettant d’afficher chaque nom et prix du produit).
- `EditItemTemplate` contenait l’interface utilisateur du mode édition (les deux contrôles Web TextBox).

La propriété de `EditItemIndex` DataList détermine ce que `DataListItem` (le cas échéant) est restitué à l’aide du `EditItemTemplate`. En particulier, le `DataListItem` dont la valeur `ItemIndex` correspond à la propriété DataList s `EditItemIndex` est rendue à l’aide de la `EditItemTemplate`. Ce modèle fonctionne bien lorsque vous ne pouvez modifier qu’un seul élément à la fois, mais en cas de création d’un contrôle DataList entièrement modifiable.

Pour un contrôle DataList entièrement modifiable, nous voulons que *tous* les `DataListItem` s s’affichent à l’aide de l’interface modifiable. La façon la plus simple d’y parvenir consiste à définir l’interface modifiable dans le `ItemTemplate`. Pour modifier les informations d’adresse des fournisseurs, l’interface modifiable contient le nom du fournisseur sous forme de texte, puis les zones de texte pour les valeurs adresse, ville et pays.

Commencez par ouvrir la page `BatchUpdate.aspx`, ajoutez un contrôle DataList et affectez à sa propriété `ID` la valeur `Suppliers`. À partir de la balise active de DataList s, choisissez d’ajouter un nouveau contrôle ObjectDataSource nommé `SuppliersDataSource`.

[![créer un ObjectDataSource nommé SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Figure 2**: créer un ObjectDataSource nommé `SuppliersDataSource` ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-vb/_static/image6.png))

Configurez l’ObjectDataSource pour récupérer des données à l’aide de la méthode de `GetSuppliers()` `SuppliersBLL` classe s (voir figure 3). Comme dans le didacticiel précédent, au lieu de mettre à jour les informations sur les fournisseurs via ObjectDataSource, nous allons travailler directement avec la couche de logique métier. Par conséquent, définissez la liste déroulante sur (aucune) sous l’onglet mise à jour (voir figure 4).

[![récupérer les informations de fournisseur à l’aide de la méthode GetSuppliers ()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Figure 3**: récupérer les informations de fournisseur à l’aide de la méthode `GetSuppliers()` ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-vb/_static/image9.png))

[![définir la liste déroulante sur (aucun) dans l’onglet mettre à jour](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Figure 4**: définir la liste déroulante sur (aucune) sous l’onglet mise à jour ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-vb/_static/image12.png))

Une fois l’Assistant terminé, Visual Studio génère automatiquement les `ItemTemplate` DataList pour afficher chaque champ de données retourné par la source de données dans un contrôle Web étiquette. Nous devons modifier ce modèle pour qu’il fournisse plutôt l’interface d’édition. La `ItemTemplate` peut être personnalisée par le biais du concepteur à l’aide de l’option modifier les modèles de la balise active de DataList s ou directement par le biais de la syntaxe déclarative.

Prenez un moment pour créer une interface de modification qui affiche le nom du fournisseur sous forme de texte, mais il comprend des zones de texte pour les valeurs adresse du fournisseur, ville et pays. Après avoir apporté ces modifications, la syntaxe déclarative de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Comme pour le didacticiel précédent, l’état d’affichage de la DataList dans ce didacticiel doit être activé.

Dans le `ItemTemplate` j’utilise deux nouvelles classes CSS, `SupplierPropertyLabel` et `SupplierPropertyValue`, qui ont été ajoutées à la classe `Styles.css` et configurées pour utiliser les mêmes paramètres de style que les classes CSS `ProductPropertyLabel` et `ProductPropertyValue`.

[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Après avoir apporté ces modifications, visitez cette page via un navigateur. Comme le montre la figure 5, chaque élément DataList affiche le nom du fournisseur sous forme de texte et utilise les zones de texte pour afficher l’adresse, la ville et le pays.

[![chaque fournisseur du contrôle DataList est modifiable](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Figure 5**: chaque fournisseur du contrôle DataList est modifiable ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-vb/_static/image15.png))

## <a name="step-2-adding-an-update-all-button"></a>Étape 2 : ajout d’un bouton mettre à jour tout

Alors que les champs adresse, ville et pays de chaque fournisseur de la figure 5 sont affichés dans une zone de texte, aucun bouton de mise à jour n’est actuellement disponible. Au lieu d’avoir un bouton de mise à jour par élément, avec des DataLists entièrement modifiables, il existe généralement un bouton de mise à jour unique sur la page qui, lorsque vous cliquez dessus, met à jour *tous* les enregistrements dans le contrôle DataList. Pour ce didacticiel, nous allons ajouter deux boutons mettre à jour tous les boutons-l’un en haut de la page et l’autre en bas (bien que le fait de cliquer sur l’un des deux boutons aura le même effet).

Commencez par ajouter un contrôle Web Button au-dessus de DataList et affectez à sa propriété `ID` la valeur `UpdateAll1`. Ensuite, ajoutez le deuxième contrôle Web sous le contrôle DataList, en affectant à son `ID` la valeur `UpdateAll2`. Définissez les propriétés de `Text` des deux boutons pour mettre à jour tout. Enfin, créez des gestionnaires d’événements pour les deux boutons `Click` événements. Au lieu de dupliquer la logique de mise à jour dans chacun des gestionnaires d’événements, vous pouvez Refactoriser cette logique vers une troisième méthode, `UpdateAllSupplierAddresses`, en faisant en sorte que les gestionnaires d’événements appellent simplement cette troisième méthode.

[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

La figure 6 affiche la page une fois que les boutons mettre à jour tous ont été ajoutés.

[![deux boutons mettre à jour tous les boutons ont été ajoutés à la page](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Figure 6**: deux boutons mettre à jour tous les boutons ont été ajoutés à la page ([cliquez pour afficher l’image en taille réelle](performing-batch-updates-vb/_static/image18.png))

## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Étape 3 : mise à jour de toutes les informations d’adresse des fournisseurs

Avec tous les éléments de DataList qui affichent l’interface d’édition et avec l’ajout des boutons mettre à jour tout, il est tout à fait l’écriture du code pour effectuer la mise à jour par lot. Plus précisément, nous devons effectuer une boucle sur les éléments de DataList et appeler la méthode de `SuppliersBLL` classe s `UpdateSupplierAddress` pour chacun d’entre eux.

Vous pouvez accéder à la collection d’instances `DataListItem` qui configurent la DataList via la propriété DataList s [`Items`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Avec une référence à un `DataListItem`, nous pouvons récupérer les `SupplierID` correspondants de la collection `DataKeys` et référencer par programmation les contrôles Web TextBox dans le `ItemTemplate` comme l’illustre le code suivant :

[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Quand l’utilisateur clique sur l’un des boutons mettre à jour tout, la méthode `UpdateAllSupplierAddresses` itère au sein de chaque `DataListItem` de la `Suppliers` DataList et appelle la méthode `SuppliersBLL` classe s `UpdateSupplierAddress`, en passant les valeurs correspondantes. Une valeur non entrée pour l’adresse, la ville ou le pays passe est une valeur de `Nothing` à `UpdateSupplierAddress` (au lieu d’une chaîne vide), ce qui aboutit à une `NULL` de base de données pour les champs de l’enregistrement sous-jacent.

> [!NOTE]
> En guise d’amélioration, vous souhaiterez peut-être ajouter un contrôle Web étiquette d’État à la page qui fournit un message de confirmation après l’exécution de la mise à jour par lot.

## <a name="updating-only-those-addresses-that-have-been-modified"></a>Mise à jour uniquement des adresses qui ont été modifiées

L’algorithme de mise à jour par lot utilisé pour ce didacticiel appelle la méthode `UpdateSupplierAddress` pour *chaque* fournisseur du contrôle DataList, que leurs informations d’adresse aient été modifiées ou non. Bien que ces mises à jour aveugles ne soient généralement pas un problème de performance, elles peuvent entraîner des enregistrements superflus si vous renouvelez l’audit des modifications apportées à la table de base de données. Par exemple, si vous utilisez des déclencheurs pour enregistrer tous les `UPDATE` s dans la table `Suppliers` dans une table d’audit, chaque fois qu’un utilisateur clique sur le bouton mettre à jour tout, un nouvel enregistrement d’audit est créé pour chaque fournisseur du système, que l’utilisateur ait apporté des modifications ou non.

Les classes DataTable et DataAdapter de ADO.NET sont conçues pour prendre en charge les mises à jour par lot lorsque seuls les enregistrements modifiés, supprimés et nouveaux entraînent la communication entre les bases de données. Chaque ligne du DataTable a une [propriété`RowState`](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) qui indique si la ligne a été ajoutée au DataTable, si elle a été supprimée, modifiée ou reste inchangée. Quand un DataTable est initialement rempli, toutes les lignes sont marquées comme inchangées. La modification de la valeur de l’une des colonnes de la ligne s marque la ligne comme modifiée.

Dans la classe `SuppliersBLL`, nous mettons à jour les informations d’adresse du fournisseur spécifié en lisant d’abord dans l’enregistrement de fournisseur unique dans un `SuppliersDataTable` puis en définissant les valeurs de colonne `Address`, `City`et `Country` à l’aide du code suivant :

[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Ce code naïvement affecte les valeurs de l’adresse, de la ville et du pays passés au `SuppliersRow` dans le `SuppliersDataTable` que les valeurs aient ou non été modifiées. Ces modifications entraînent le marquage de la propriété `SuppliersRow` s `RowState` comme modifiée. Lorsque la méthode `Update` de la couche d’accès aux données est appelée, elle constate que le `SupplierRow` a été modifié et envoie donc une commande `UPDATE` à la base de données.

Imaginez, toutefois, que nous avons ajouté du code à cette méthode pour affecter uniquement les valeurs d’adresse, de ville et de pays transmises, si elles diffèrent des valeurs existantes de `SuppliersRow`. Dans le cas où l’adresse, la ville et le pays sont les mêmes que les données existantes, aucune modification ne sera apportée et le `SupplierRow` s `RowState` sera marqué comme inchangé. Le résultat net est que lorsque la méthode `Update` DAL est appelée, aucun appel à la base de données n’est effectué car le `SuppliersRow` n’a pas été modifié.

Pour mettre en œuvre cette modification, remplacez les instructions qui attribuent les valeurs de l’adresse, de la ville et du pays transmises à l’aide du code suivant :

[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Avec ce code ajouté, la méthode DAL s `Update` envoie une instruction `UPDATE` à la base de données uniquement pour les enregistrements dont les valeurs relatives à l’adresse ont été modifiées.

En guise d’alternative, nous pourrions savoir s’il existe des différences entre les champs d’adresse transmis et les données de la base de données et, s’il n’y en a pas, contourner simplement l’appel à la méthode `Update` DAL. Cette approche fonctionne bien si vous utilisez la méthode de base de données directe, car la méthode de base de données directe n’a pas passé une instance de `SuppliersRow` dont les `RowState` peuvent être vérifiées pour déterminer si un appel de base de données est réellement nécessaire.

> [!NOTE]
> Chaque fois que la méthode `UpdateSupplierAddress` est appelée, un appel est fait à la base de données pour récupérer des informations sur l’enregistrement mis à jour. Ensuite, si des modifications sont apportées aux données, un autre appel à la base de données est effectué pour mettre à jour la ligne de la table. Ce flux de travail peut être optimisé en créant une surcharge de méthode `UpdateSupplierAddress` qui accepte une instance `EmployeesDataTable` qui a *toutes* les modifications de la page `BatchUpdate.aspx`. Ensuite, il peut effectuer un appel à la base de données pour récupérer tous les enregistrements de la table `Suppliers`. Les deux jeux de résultats peuvent ensuite être énumérés et seuls les enregistrements pour lesquels des modifications ont été apportées peuvent être mis à jour.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment créer un contrôle DataList entièrement modifiable, ce qui permet à un utilisateur de modifier rapidement les informations d’adresse de plusieurs fournisseurs. Nous avons commencé par définir l’interface de modification dans un contrôle Web TextBox pour les valeurs adresse, ville et pays du fournisseur dans le `ItemTemplate`DataList. Ensuite, nous avons ajouté la mise à jour de tous les boutons au-dessus et en dessous du contrôle DataList. Une fois que l’utilisateur a apporté ses modifications et cliqué sur l’un des boutons mettre à jour tout, les `DataListItem` s sont énumérés et un appel à la méthode `SuppliersBLL` classe s `UpdateSupplierAddress` est effectué.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Zack Jones et Ken Pespisa. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [Suivant](handling-bll-and-dal-level-exceptions-vb.md)
