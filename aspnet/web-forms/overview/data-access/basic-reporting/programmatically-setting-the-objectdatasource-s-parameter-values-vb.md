---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Définition par programmation des valeurs de paramètre de ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner comment ajouter une méthode à nos DAL et BLL qui accepte un paramètre d’entrée unique et retourne des données. L’exemple définit ce paramètre...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f1dd50f46528e8dd51f85e503604d3f0dbc21ad2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74601840"
---
# <a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Définition par programmation des valeurs des paramètres d’ObjectDataSource (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) ou [Télécharger le PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> Dans ce didacticiel, nous allons examiner comment ajouter une méthode à nos DAL et BLL qui accepte un paramètre d’entrée unique et retourne des données. L’exemple définit ce paramètre par programmation.

## <a name="introduction"></a>Introduction

Comme nous l’avons vu dans le [didacticiel précédent](declarative-parameters-vb.md), un certain nombre d’options sont disponibles pour transmettre de manière déclarative des valeurs de paramètre aux méthodes de l’ObjectDataSource. Si la valeur du paramètre est codée en dur, provient d’un contrôle Web sur la page ou se trouve dans une autre source lisible par une source de données `Parameter` objet, par exemple, cette valeur peut être liée au paramètre d’entrée sans écrire une ligne de code.

Toutefois, il peut arriver que la valeur du paramètre provient d’une source qui n’est pas encore prise en compte par une des objets de la source de données intégrée `Parameter`. Si notre site prend en charge les comptes d’utilisateur, nous pourrions souhaiter définir le paramètre en fonction de l’ID utilisateur du visiteur actuellement connecté. Ou nous pouvons être amenés à personnaliser la valeur du paramètre avant de l’envoyer à la méthode de l’objet sous-jacent de l’ObjectDataSource.

Chaque fois que la méthode de `Select` ObjectDataSource est appelée, ObjectDataSource commence par déclencher son [événement Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). La méthode de l’objet sous-jacent de ObjectDataSource est ensuite appelée. Une fois cette opération terminée, l' [événement sélectionné](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) de l’ObjectDataSource se déclenche (la figure 1 illustre cette séquence d’événements). Les valeurs de paramètre passées dans la méthode de l’objet sous-jacent de ObjectDataSource peuvent être définies ou personnalisées dans un gestionnaire d’événements pour l’événement `Selecting`.

[![les événements sélectionnés et de sélection de l’ObjectDataSource se déclenchent avant et après l’appel de la méthode de son objet sous-jacent](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figure 1**: les événements `Selected` et `Selecting` de l’ObjectDataSource se déclenchent avant et après l’appel de la méthode de l’objet sous-jacent ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))

Dans ce didacticiel, nous allons examiner l’ajout d’une méthode à notre DAL et BLL qui accepte un paramètre d’entrée unique `Month`, de type `Integer` et retourne un objet `EmployeesDataTable` rempli avec les employés ayant leur anniversaire d’embauche dans le `Month`spécifié. Notre exemple définit ce paramètre par programmation en fonction du mois en cours, en présentant la liste des « anniversaires employés ce mois-ci ».

Commençons !

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Étape 1 : ajout d’une méthode à`EmployeesTableAdapter`

Pour notre premier exemple, nous devons ajouter un moyen de récupérer les employés dont le `HireDate` s’est produit dans un mois donné. Pour fournir cette fonctionnalité conformément à notre architecture, nous devons d’abord créer une méthode dans `EmployeesTableAdapter` qui correspond à l’instruction SQL appropriée. Pour ce faire, commencez par ouvrir le DataSet typé Northwind. Cliquez avec le bouton droit sur l’étiquette `EmployeesTableAdapter`, puis choisissez Ajouter une requête.

[![ajouter une nouvelle requête au EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figure 2**: ajouter une nouvelle requête au `EmployeesTableAdapter` ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))

Choisissez d’ajouter une instruction SQL qui retourne des lignes. Quand vous atteignez l’écran spécifier une `SELECT` instruction, l’instruction par défaut `SELECT` pour l' `EmployeesTableAdapter` est déjà chargée. Ajoutez simplement la clause `WHERE` : `WHERE DATEPART(m, HireDate) = @Month`. [DatePart](https://msdn.microsoft.com/library/ms174420.aspx) est une fonction T-SQL qui retourne une partie de date particulière d’un type de `datetime` ; dans ce cas, nous utilisons `DATEPART` pour retourner le mois de la colonne `HireDate`.

[![ne retourner que les lignes où la colonne HireDate est inférieure ou égale au paramètre @HiredBeforeDate](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figure 3**: retourner uniquement les lignes où la `HireDate` colonne est inférieure ou égale au paramètre `@HiredBeforeDate` ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))

Enfin, modifiez les noms des méthodes `FillBy` et `GetDataBy` en `FillByHiredDateMonth` et `GetEmployeesByHiredDateMonth`, respectivement.

[![choisir des noms de méthode plus appropriés que FillBy et GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Figure 4**: choisir des noms de méthode plus appropriés que `FillBy` et `GetDataBy` ([Cliquer pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))

Cliquez sur Terminer pour terminer l’Assistant et revenir à l’aire de conception du jeu de données. Le `EmployeesTableAdapter` doit à présent inclure un nouvel ensemble de méthodes pour accéder aux employés embauchés au cours d’un mois donné.

[![les nouvelles méthodes apparaissent dans le Aire de conception du jeu de données](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figure 5**: les nouvelles méthodes apparaissent dans le aire de conception du jeu de données ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))

## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Étape 2 : ajout de la méthode`GetEmployeesByHiredDateMonth(month)`à la couche de logique métier

Étant donné que l’architecture de l’application utilise une couche distincte pour la logique métier et la logique d’accès aux données, nous devons ajouter une méthode à notre BLL qui appelle la couche DAL pour récupérer les employés recrutés avant une date spécifiée. Ouvrez le fichier `EmployeesBLL.vb` et ajoutez la méthode suivante :

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Comme avec les autres méthodes de cette classe, `GetEmployeesByHiredDateMonth(month)` appelle simplement la couche DAL et retourne les résultats.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Étape 3 : affichage des employés dont l’anniversaire d’embauche est ce mois-ci

L’étape finale de cet exemple consiste à afficher les employés dont l’anniversaire d’embauche est ce mois-ci. Commencez par ajouter un GridView à la page `ProgrammaticParams.aspx` dans le dossier `BasicReporting` et ajoutez un nouvel ObjectDataSource comme source de données. Configurez l’ObjectDataSource pour utiliser la classe `EmployeesBLL` avec la `SelectMethod` définie sur `GetEmployeesByHiredDateMonth(month)`.

[![utiliser la classe EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figure 6**: utiliser la classe `EmployeesBLL` ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))

[![Select à partir de la méthode GetEmployeesByHiredDateMonth (month)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figure 7**: sélectionner dans la méthode `GetEmployeesByHiredDateMonth(month)` ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))

L’écran final nous invite à fournir la source de la valeur de paramètre `month`. Étant donné que nous allons définir cette valeur par programmation, laissez la source de paramètres définie sur l’option None par défaut, puis cliquez sur Terminer.

[![laissez le paramètre source défini sur None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figure 8**: conserver la source de paramètres définie sur aucun ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))

Cette opération crée un objet `Parameter` dans la collection de `SelectParameters` de l’ObjectDataSource qui n’a pas de valeur spécifiée.

[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Pour définir cette valeur par programmation, nous devons créer un gestionnaire d’événements pour l’événement de `Selecting` de l’ObjectDataSource. Pour ce faire, accédez à la Mode Création et double-cliquez sur ObjectDataSource. Vous pouvez également sélectionner l’ObjectDataSource, accéder au Fenêtre Propriétés, puis cliquer sur l’icône représentant un éclair. Ensuite, double-cliquez dans la zone de texte à côté de l’événement `Selecting` ou tapez le nom du gestionnaire d’événements que vous souhaitez utiliser. En guise de troisième option, vous pouvez créer le gestionnaire d’événements en sélectionnant ObjectDataSource et son événement `Selecting` dans les deux listes déroulantes en haut de la classe code-behind de la page.

![Cliquez sur l’icône représentant un éclair dans la fenêtre Propriétés pour répertorier les événements d’un contrôle Web.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figure 9**: cliquer sur l’icône représentant un éclair dans la fenêtre Propriétés pour répertorier les événements d’un contrôle Web

Les trois approches ajoutent un nouveau gestionnaire d’événements pour l’événement de `Selecting` ObjectDataSource à la classe code-behind de la page. Dans ce gestionnaire d’événements, nous pouvons lire et écrire dans les valeurs de paramètre à l’aide de `e.InputParameters(parameterName)`, où *`parameterName`* est la valeur de l’attribut `Name` dans la balise `<asp:Parameter>` (la collection `InputParameters` peut également être indexée de manière ordinale, comme dans `e.InputParameters(index)`). Pour définir le paramètre `month` sur le mois actuel, ajoutez ce qui suit au gestionnaire d’événements `Selecting` :

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Lorsque vous accédez à cette page par le biais d’un navigateur, nous voyons que seul un employé a été embauché ce mois-ci (mars) Laura Callahan, qui fait partie de l’entreprise depuis 1994.

[![les employés dont les anniversaires sont affichés](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figure 10**: les employés dont les anniversaires sont affichés par mois ([cliquez pour afficher l’image en taille réelle](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))

## <a name="summary"></a>Récapitulatif

Alors que les valeurs des paramètres de l’ObjectDataSource peuvent généralement être définies de façon déclarative, sans qu’il soit nécessaire de définir une ligne de code, il est facile de définir les valeurs des paramètres par programmation. Il nous suffit de créer un gestionnaire d’événements pour l’événement `Selecting` de l’ObjectDataSource, qui se déclenche avant l’appel de la méthode de l’objet sous-jacent, et de définir manuellement les valeurs d’un ou de plusieurs paramètres via la collection `InputParameters`.

Ce didacticiel conclut la section Création de rapports de base. Le [didacticiel suivant](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) lance la section relative aux scénarios de filtrage et de détails maîtres, dans laquelle nous allons examiner les techniques permettant au visiteur de filtrer les données et d’effectuer un zoom avant d’un rapport maître dans un rapport détaillé.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](declarative-parameters-vb.md)
