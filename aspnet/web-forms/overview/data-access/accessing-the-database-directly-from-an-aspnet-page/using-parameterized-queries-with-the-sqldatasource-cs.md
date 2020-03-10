---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: Utilisation de requêtes paramétrables avec le SqlDataSourceC#() | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous continuons notre examen du contrôle SqlDataSource et voyons comment définir des requêtes paramétrables. Les paramètres peuvent être spécifiés à la fois dans decla...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 241dc8c089d4faa9eb95a63684e8a56756bb302c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78552478"
---
# <a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Utilisation de requêtes paramétrables avec SqlDataSource (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) ou [Télécharger le PDF](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> Dans ce didacticiel, nous continuons notre examen du contrôle SqlDataSource et voyons comment définir des requêtes paramétrables. Les paramètres peuvent être spécifiés à la fois de façon déclarative et par programme, et peuvent être extraits d’un certain nombre d’emplacements tels que QueryString, état de session, autres contrôles, etc.

## <a name="introduction"></a>Introduction

Dans le didacticiel précédent, nous avons vu comment utiliser le contrôle SqlDataSource pour récupérer des données directement à partir d’une base de données. À l’aide de l’Assistant Configuration de la source de données, nous pourrions choisir la base de données, puis : sélectionner les colonnes à retourner à partir d’une table ou d’une vue ; Entrez une instruction SQL personnalisée ; ou utilisez une procédure stockée. Qu’il s’agisse de sélectionner des colonnes à partir d’une table ou d’une vue ou d’entrer une instruction SQL personnalisée, la propriété des `SelectCommand` du contrôle SqlDataSource est assignée à l’instruction `SELECT` SQL ad hoc résultante et c’est cette instruction `SELECT` qui est exécutée lorsque la méthode SqlDataSource s `Select()` est appelée (par programmation ou automatiquement à partir d’un contrôle Web de données).

Les instructions SQL `SELECT` utilisées dans les démonstrations précédentes du didacticiel s n’ont pas été `WHERE` clauses. Dans une instruction `SELECT`, la clause `WHERE` peut être utilisée pour limiter les résultats retournés. Par exemple, pour afficher les noms des produits dont le coût est supérieur à $50,00, nous pourrions utiliser la requête suivante :

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

En règle générale, les valeurs utilisées dans une clause `WHERE` sont définies par une source externe, telle qu’une valeur QueryString, une variable de session ou une entrée utilisateur à partir d’un contrôle Web sur la page. Dans l’idéal, ces entrées sont spécifiées à l’aide de *paramètres*. Avec Microsoft SQL Server, les paramètres sont dénotés à l’aide de `@parameterName`, comme dans :

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

Le SqlDataSource prend en charge les requêtes paramétrables, à la fois pour les instructions `SELECT` et les instructions `INSERT`, `UPDATE`et `DELETE`. En outre, les valeurs des paramètres peuvent être extraites automatiquement à partir de diverses sources, la chaîne de recherche, l’état de session, les contrôles de la page, etc. ou peuvent être assignés par programme. Dans ce didacticiel, nous allons voir comment définir des requêtes paramétrables et comment spécifier les valeurs de paramètres de façon déclarative et par programme.

> [!NOTE]
> Dans le didacticiel précédent, nous avons comparé l’ObjectDataSource qui est notre outil de choix sur les 46 premiers didacticiels avec le SqlDataSource, en notant les similitudes conceptuelles. Ces similarités s’étendent également aux paramètres. Les paramètres ObjectDataSource s sont mappés aux paramètres d’entrée pour les méthodes de la couche de logique métier. Avec le SqlDataSource, les paramètres sont définis directement dans la requête SQL. Les deux contrôles ont des collections de paramètres pour leurs méthodes `Select()`, `Insert()`, `Update()`et `Delete()`, et les deux peuvent avoir ces valeurs de paramètre remplies à partir de sources prédéfinies (valeurs QueryString, variables de session, etc.) ou assignées par programme.

## <a name="creating-a-parameterized-query"></a>Création d'une requête paramétrée

L’Assistant contrôle de la source de données de l’Assistant Configuration de la source de données offre trois moyens de définir la commande à exécuter pour récupérer les enregistrements de base de données :

- En sélectionnant les colonnes d’une table ou d’une vue existante,
- En entrant une instruction SQL personnalisée ou
- En choisissant une procédure stockée

Lorsque vous choisissez des colonnes dans une table ou une vue existante, vous devez spécifier les paramètres de la clause `WHERE` via la boîte de dialogue Ajouter une clause de `WHERE`. Toutefois, lors de la création d’une instruction SQL personnalisée, vous pouvez entrer les paramètres directement dans la clause `WHERE` (à l’aide de `@parameterName` pour indiquer chaque paramètre). Une [procédure stockée](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) se compose d’une ou de plusieurs instructions SQL, et ces instructions peuvent être paramétrables. Toutefois, les paramètres utilisés dans les instructions SQL doivent être transmis en tant que paramètres d’entrée à la procédure stockée.

Dans la mesure où la création d’une requête paramétrable dépend de la spécification de la `SelectCommand` du SqlDataSource, voyons les trois approches. Pour commencer, ouvrez la page `ParameterizedQueries.aspx` dans le dossier `SqlDataSource`, faites glisser un contrôle SqlDataSource de la boîte à outils vers le concepteur et définissez sa `ID` sur `Products25BucksAndUnderDataSource`. Ensuite, cliquez sur le lien configurer la source de données dans la balise active contrôle s. Sélectionnez la base de données à utiliser (`NORTHWINDConnectionString`), puis cliquez sur suivant.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Étape 1 : ajout d’une clause`WHERE`lors de la sélection des colonnes d’une table ou d’une vue

Lorsque vous sélectionnez les données à retourner à partir de la base de données avec le contrôle SqlDataSource, l’Assistant Configuration de la source de données nous permet de sélectionner simplement les colonnes à retourner à partir d’une table ou d’une vue existante (voir la figure 1). Cela génère automatiquement une instruction SQL `SELECT`, qui est celle qui est envoyée à la base de données lors de l’appel de la méthode SqlDataSource s `Select()`. Comme nous l’avons fait dans le didacticiel précédent, sélectionnez la table Products dans la liste déroulante et vérifiez les colonnes `ProductID`, `ProductName`et `UnitPrice`.

[![sélectionner les colonnes à retourner à partir d’une table ou d’une vue](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Figure 1**: sélectionner les colonnes à retourner à partir d’une table ou d’une vue ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))

Pour inclure une clause `WHERE` dans l’instruction `SELECT`, cliquez sur le bouton `WHERE`, qui affiche la boîte de dialogue Ajouter une clause de `WHERE` (voir figure 2). Pour ajouter un paramètre afin de limiter les résultats retournés par la requête `SELECT`, commencez par choisir la colonne par laquelle filtrer les données. Ensuite, choisissez l’opérateur à utiliser pour le filtrage (=, &lt;, &lt;=, &gt;, etc.). Enfin, choisissez la source de la valeur du paramètre, par exemple, à partir de la chaîne de connexion ou de l’état de session. Après avoir configuré le paramètre, cliquez sur le bouton Ajouter pour l’inclure dans la requête `SELECT`.

Pour cet exemple, Let ne retourne que les résultats où la valeur `UnitPrice` est inférieure ou égale à $25,00. Par conséquent, sélectionnez `UnitPrice` dans la liste déroulante colonne et &lt;= dans la liste déroulante opérateur. Lors de l’utilisation d’une valeur de paramètre codée en dur (telle que $25,00) ou si la valeur du paramètre doit être spécifiée par programme, sélectionnez aucun dans la liste déroulante source. Ensuite, entrez la valeur de paramètre codée en dur dans la zone de texte valeur 25,00 et terminez le processus en cliquant sur le bouton Ajouter.

[![limiter les résultats retournés à partir de la boîte de dialogue Ajouter une clause WHERE](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Figure 2**: limiter les résultats retournés à partir de la boîte de dialogue Ajouter une clause `WHERE` ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))

Après avoir ajouté le paramètre, cliquez sur OK pour revenir à l’Assistant Configuration de la source de données. L’instruction `SELECT` au bas de l’Assistant doit maintenant inclure une clause `WHERE` avec un paramètre nommé `@UnitPrice`:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> Si vous spécifiez plusieurs conditions dans la clause `WHERE` de la boîte de dialogue Ajouter une clause `WHERE`, l’Assistant les joint avec l’opérateur `AND`. Si vous devez inclure une `OR` dans la clause `WHERE` (telle que `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), vous devez générer l’instruction `SELECT` via l’écran instruction SQL personnalisée.

Terminez la configuration du SqlDataSource (cliquez sur suivant, puis sur Terminer), puis examinez le balisage déclaratif s. Le balisage comprend maintenant une collection `<SelectParameters>`, qui affiche les sources des paramètres dans le `SelectCommand`.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Lorsque la méthode SqlDataSource s `Select()` est appelée, la valeur du paramètre `UnitPrice` (25,00) est appliquée au paramètre `@UnitPrice` dans le `SelectCommand` avant d’être envoyé à la base de données. Le résultat net est que seuls les produits inférieurs ou égaux à $25,00 sont retournés à partir de la table `Products`. Pour confirmer cela, ajoutez un GridView à la page, liez-le à cette source de données, puis affichez la page via un navigateur. Vous ne devriez voir que les produits répertoriés qui sont inférieurs ou égaux à $25,00, comme le confirme la figure 3.

[![seuls les produits inférieurs ou égaux à $25,00 sont affichés](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Figure 3**: seuls les produits inférieurs ou égaux à $25,00 sont affichés ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))

## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Étape 2 : ajout de paramètres à une instruction SQL personnalisée

Lorsque vous ajoutez une instruction SQL personnalisée, vous pouvez entrer explicitement la clause `WHERE` ou spécifier une valeur dans la cellule de filtre du Générateur de requêtes. Pour illustrer cela, nous allons afficher uniquement les produits d’un GridView dont les prix sont inférieurs à un certain seuil. Commencez par ajouter une zone de texte à la page `ParameterizedQueries.aspx` pour collecter cette valeur de seuil auprès de l’utilisateur. Affectez à la propriété TextBox s `ID` la valeur `MaxPrice`. Ajoutez un contrôle Web Button et définissez sa propriété `Text` pour afficher les produits correspondants.

Ensuite, faites glisser un contrôle GridView sur la page et, à partir de sa balise active, choisissez de créer un SqlDataSource nommé `ProductsFilteredByPriceDataSource`. À partir de l’Assistant Configuration de la source de données, passez à l’écran spécifier une instruction SQL personnalisée ou une procédure stockée (voir figure 4), puis entrez la requête suivante :

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

Après avoir entré la requête (manuellement ou via le Générateur de requêtes), cliquez sur suivant.

[![retourner uniquement les produits inférieurs ou égaux à une valeur de paramètre](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Figure 4**: retourner uniquement les produits inférieurs ou égaux à une valeur de paramètre ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))

Étant donné que la requête comprend des paramètres, l’écran suivant de l’Assistant nous invite à entrer la source des valeurs des paramètres. Sélectionnez Control dans la liste déroulante source du paramètre et `MaxPrice` (la valeur du contrôle TextBox s `ID`) dans la liste déroulante ControlID. Vous pouvez également entrer une valeur par défaut facultative à utiliser dans le cas où l’utilisateur n’a saisi aucun texte dans la zone de texte `MaxPrice`. Pour l’instant, n’entrez pas de valeur par défaut.

[![la propriété Text de la zone de texte MaxPrice est utilisée comme source du paramètre](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Figure 5**: la propriété `MaxPrice` TextBox s `Text` est utilisée comme source de paramètre ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))

Terminez l’Assistant Configuration de la source de données en cliquant sur suivant, puis sur Terminer. Le balisage déclaratif pour les contrôles GridView, TextBox, Button et SqlDataSource est le suivant :

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Notez que le paramètre dans la section SqlDataSource s `<SelectParameters>` est un `ControlParameter`, qui comprend des propriétés supplémentaires telles que `ControlID` et `PropertyName`. Lorsque la méthode SqlDataSource s `Select()` est appelée, le `ControlParameter` récupère la valeur de la propriété de contrôle Web spécifiée et l’assigne au paramètre correspondant dans le `SelectCommand`. Dans cet exemple, la propriété de texte `MaxPrice` s est utilisée comme valeur de paramètre `@MaxPrice`.

Prenez une minute pour afficher cette page dans un navigateur. Lors de la première visite de la page ou à chaque fois que la zone de texte `MaxPrice` n’a pas de valeur, aucun enregistrement n’est affiché dans le GridView.

[![aucun enregistrement n’est affiché lorsque la zone de texte MaxPrice est vide](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Figure 6**: aucun enregistrement n’est affiché lorsque la zone de texte `MaxPrice` est vide ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))

La raison pour laquelle aucun produit n’est affiché est que, par défaut, une chaîne vide pour une valeur de paramètre est convertie en une valeur de `NULL` de base de données. Étant donné que la comparaison de `[UnitPrice] <= NULL` est toujours évaluée comme false, aucun résultat n’est retourné.

Entrez une valeur dans la zone de texte, par exemple 5,00, puis cliquez sur le bouton afficher les produits correspondants. Lors de la publication (postback), le SqlDataSource informe le GridView que l’une de ses sources de paramètres a changé. Par conséquent, le GridView se reconnecte au SqlDataSource, en affichant les produits inférieurs ou égaux à $5,00.

[![produits inférieurs ou égaux à $5,00 sont affichés](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Figure 7**: affichage des produits inférieurs ou égaux à $5,00 ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))

## <a name="initially-displaying-all-products"></a>Affichage initial de tous les produits

Au lieu d’afficher aucun produit lorsque la page est chargée pour la première fois, vous pouvez afficher *tous les* produits. L’une des façons de répertorier tous les produits chaque fois que la zone de texte `MaxPrice` est vide consiste à définir la valeur par défaut du paramètre sur une valeur élevée extrêmement, par exemple 1 million, car il est peu probable que Northwind Traders ait toujours un inventaire dont le prix unitaire dépasse $1 million. Toutefois, cette approche est shortsighted et peut ne pas fonctionner dans d’autres situations.

Dans les didacticiels précédents, les [paramètres déclaratifs](../basic-reporting/declarative-parameters-cs.md) et le [filtrage maître/détail avec un contrôle DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) ont été confrontés à un problème similaire. Notre solution consistait à placer cette logique dans la couche de logique métier. Plus précisément, la couche BLL a examiné la valeur entrante et, si elle a été `NULL` ou une valeur réservée, l’appel a été routé vers la méthode DAL qui a renvoyé tous les enregistrements. Si la valeur entrante était une valeur de filtrage normale, un appel a été passé à la méthode DAL qui a exécuté une instruction SQL qui utilisait une clause `WHERE` paramétrable avec la valeur fournie.

Malheureusement, nous ignorons l’architecture lors de l’utilisation du SqlDataSource. Au lieu de cela, nous devons personnaliser l’instruction SQL pour récupérer intelligemment tous les enregistrements si le paramètre `@MaximumPrice` est `NULL` ou une valeur réservée. Dans le cadre de cet exercice, nous allons faire en sorte que si le paramètre `@MaximumPrice` est égal à `-1.0`, *tous* les enregistrements doivent être retournés (`-1.0` fonctionne comme une valeur réservée, car aucun produit ne peut avoir une valeur `UnitPrice` négative). Pour ce faire, nous pouvons utiliser l’instruction SQL suivante :

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

Cette clause `WHERE` retourne *tous les* enregistrements si le paramètre `@MaximumPrice` est égal à `-1.0`. Si la valeur du paramètre n’est pas `-1.0`, seuls les produits dont le `UnitPrice` est inférieur ou égal à la valeur du paramètre `@MaximumPrice` sont retournés. En définissant la valeur par défaut du paramètre `@MaximumPrice` sur `-1.0`, lors du premier chargement de la page (ou à chaque fois que la zone de texte `MaxPrice` est vide), `@MaximumPrice` aura la valeur `-1.0` et tous les produits seront affichés.

[![maintenant, tous les produits s’affichent lorsque la zone de texte MaxPrice est vide](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Figure 8**: tous les produits sont désormais affichés lorsque la zone de texte `MaxPrice` est vide ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))

Il existe quelques inconvénients à noter avec cette approche. Tout d’abord, sachez que le type de données du paramètre est déduit par l’utilisation des s dans la requête SQL. Si vous remplacez la clause `WHERE` `@MaximumPrice = -1.0` par `@MaximumPrice = -1`, le runtime traite le paramètre comme un entier. Si vous tentez ensuite d’affecter la zone de texte `MaxPrice` à une valeur décimale (par exemple, 5,00), une erreur se produit, car elle ne peut pas convertir 5,00 en entier. Pour y remédier, assurez-vous que vous utilisez `@MaximumPrice = -1.0` dans la clause `WHERE` ou, mieux encore, définissez la propriété `ControlParameter` objet s `Type` sur décimal.

Deuxièmement, en ajoutant l' `OR @MaximumPrice = -1.0` à la clause `WHERE`, le moteur de requête ne peut pas utiliser un index sur `UnitPrice` (en supposant qu’il en existe un), ce qui entraîne une analyse de table. Cela peut avoir un impact sur les performances si le nombre d’enregistrements dans la table `Products` est suffisamment important. Une meilleure approche consisterait à déplacer cette logique vers une procédure stockée où une instruction `IF` exécuterait une requête `SELECT` à partir de la table `Products` sans clause `WHERE` lorsque tous les enregistrements doivent être retournés ou un dont la clause `WHERE` contient uniquement les critères de `UnitPrice`, afin qu’un index puisse être utilisé.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Étape 3 : création et utilisation de procédures stockées paramétrables

Les procédures stockées peuvent inclure un ensemble de paramètres d’entrée qui peuvent ensuite être utilisés dans la ou les instructions SQL définies dans la procédure stockée. Quand vous configurez le SqlDataSource pour utiliser une procédure stockée qui accepte les paramètres d’entrée, ces valeurs de paramètre peuvent être spécifiées à l’aide des mêmes techniques que les instructions SQL ad hoc.

Pour illustrer l’utilisation de procédures stockées dans le SqlDataSource, créons une nouvelle procédure stockée dans la base de données Northwind nommée `GetProductsByCategory`, qui accepte un paramètre nommé `@CategoryID` et retourne toutes les colonnes des produits dont la colonne `CategoryID` correspond à `@CategoryID`. Pour créer une procédure stockée, accédez à la Explorateur de serveurs et explorez la base de données `NORTHWND.MDF`. (Si vous ne voyez pas la Explorateur de serveurs, affichez-la en accédant au menu Affichage et en sélectionnant l’option Explorateur de serveurs.)

Dans la base de données `NORTHWND.MDF`, cliquez avec le bouton droit sur le dossier procédures stockées, choisissez Ajouter une nouvelle procédure stockée, puis entrez la syntaxe suivante :

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Cliquez sur l’icône Enregistrer (ou CTRL + S) pour enregistrer la procédure stockée. Vous pouvez tester la procédure stockée en cliquant dessus avec le bouton droit dans le dossier procédures stockées et en choisissant exécuter. Vous serez invité à entrer les paramètres des procédures stockées (`@CategoryID`, dans cette instance), après quoi les résultats s’afficheront dans la fenêtre sortie.

[![la procédure stockée GetProductsByCategory lorsqu’elle est exécutée avec un @CategoryID de 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Figure 9**: procédure stockée `GetProductsByCategory` lorsqu’elle est exécutée avec un `@CategoryID` de 1 ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))

Let utilise cette procédure stockée pour afficher tous les produits de la catégorie boissons dans un GridView. Ajoutez un nouveau contrôle GridView à la page et liez-le à un nouveau SqlDataSource nommé `BeverageProductsDataSource`. Passez à l’écran spécifier une instruction SQL personnalisée ou une procédure stockée, sélectionnez la case d’option procédure stockée, puis choisissez la `GetProductsByCategory` procédure stockée dans la liste déroulante.

[![sélectionnez la procédure stockée GetProductsByCategory dans la liste déroulante.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Figure 10**: sélectionner la procédure stockée `GetProductsByCategory` dans la liste déroulante ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))

Étant donné que la procédure stockée accepte un paramètre d’entrée (`@CategoryID`), le fait de cliquer sur suivant vous invite à spécifier la source pour cette valeur de paramètre s. Le `CategoryID` boissons est 1. laissez la liste déroulante source du paramètre sur None et entrez 1 dans la zone de texte DefaultValue.

[![utiliser une valeur codée en dur de 1 pour retourner les produits de la catégorie boissons](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Figure 11**: utiliser une valeur codée en dur égale à 1 pour retourner les produits de la catégorie boissons ([cliquez pour afficher l’image en plein écran](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))

Comme le montre le balisage déclaratif suivant, lors de l’utilisation d’une procédure stockée, la propriété SqlDataSource s `SelectCommand` est définie sur le nom de la procédure stockée et la [propriété`SelectCommandType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) est définie sur `StoredProcedure`, ce qui indique que le `SelectCommand` est le nom d’une procédure stockée plutôt qu’une instruction SQL ad hoc.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Testez la page dans un navigateur. Seuls les produits appartenant à la catégorie boissons sont affichés, bien que *tous* les champs de produit soient affichés puisque la procédure stockée `GetProductsByCategory` retourne toutes les colonnes de la table `Products`. Nous pourrions, bien sûr, limiter ou personnaliser les champs affichés dans le contrôle GridView à partir de la boîte de dialogue GridView s Edit Columns.

[![toutes les boissons sont affichées](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Figure 12**: toutes les boissons sont affichées ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))

## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Étape 4 : appel par programmation d’une instruction SqlDataSource s`Select()`

Les exemples que nous avons vus dans le didacticiel précédent et ce didacticiel jusqu’à présent ont des contrôles SqlDataSource liés directement à un GridView. Toutefois, les données du contrôle SqlDataSource peuvent être accessibles par programmation et énumérées dans le code. Cela peut être particulièrement utile lorsque vous devez interroger des données pour les inspecter, mais que vous n’avez pas besoin de les afficher. Plutôt que d’avoir à écrire tout le code ADO.NET pour se connecter à la base de données, spécifier la commande et récupérer les résultats, vous pouvez laisser le SqlDataSource gérer ce code monotone.

Pour illustrer l’utilisation des données du SqlDataSource s par programme, imaginez que votre patron vous a fait part de votre demande de création d’une page Web qui affiche le nom d’une catégorie sélectionnée de façon aléatoire et de ses produits associés. Autrement dit, lorsqu’un utilisateur visite cette page, nous voulons choisir une catégorie de manière aléatoire dans la table `Categories`, afficher le nom de la catégorie, puis répertorier les produits appartenant à cette catégorie.

Pour ce faire, nous avons besoin de deux contrôles SqlDataSource pour extraire une catégorie aléatoire de la table `Categories` et une autre pour obtenir les produits de la catégorie. Nous allons créer le SqlDataSource qui récupère un enregistrement de catégorie aléatoire à cette étape. L’étape 5 examine l’élaboration du SqlDataSource qui récupère les produits de la catégorie.

Commencez par ajouter un SqlDataSource à `ParameterizedQueries.aspx` et définissez ses `ID` sur `RandomCategoryDataSource`. Configurez-la pour qu’elle utilise la requête SQL suivante :

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()` retourne les enregistrements triés dans l’ordre aléatoire (consultez [utilisation de `NEWID()` pour trier des enregistrements de façon aléatoire](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` retourne le premier enregistrement du jeu de résultats. Cette requête retourne la `CategoryID` et `CategoryName` valeurs de colonne à partir d’une catégorie unique et sélectionnée de manière aléatoire.

Pour afficher la valeur de la catégorie s `CategoryName`, ajoutez un contrôle Web Label à la page, affectez à sa propriété `ID` la valeur `CategoryNameLabel`et effacez sa propriété `Text`. Pour récupérer les données d’un contrôle SqlDataSource par programmation, nous devons appeler sa méthode `Select()`. La [méthode`Select()`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) attend un paramètre d’entrée unique de type [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), qui spécifie la façon dont les données doivent être messageées avant d’être retournées. Cela peut inclure des instructions sur le tri et le filtrage des données, et est utilisé par les contrôles Web de données lors du tri ou de la pagination des données à partir d’un contrôle SqlDataSource. Pour notre exemple, nous n’avons pas besoin de modifier les données avant d’être retournées, et donc de transmettre l’objet `DataSourceSelectArguments.Empty`.

La méthode `Select()` retourne un objet qui implémente `IEnumerable`. Le type précis retourné dépend de la valeur de la [propriété`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)du contrôle SqlDataSource. Comme indiqué dans le didacticiel précédent, cette propriété peut être définie sur la valeur `DataSet` ou `DataReader`. Si la valeur est `DataSet`, la méthode `Select()` retourne un objet [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) ; Si la valeur est `DataReader`, elle retourne un objet qui implémente [`IDataReader`](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Étant donné que la propriété `DataSourceMode` de la `RandomCategoryDataSource` SqlDataSource est définie sur `DataSet` (valeur par défaut), nous travaillons avec un objet DataView.

Le code suivant montre comment récupérer les enregistrements à partir du `RandomCategoryDataSource` SqlDataSource en tant que DataView et comment lire la valeur de colonne `CategoryName` à partir de la première ligne DataView :

[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]` retourne la première `DataRowView` dans le DataView. `randomCategoryView[0]["CategoryName"]` retourne la valeur de la colonne `CategoryName` de cette première ligne. Notez que le DataView est faiblement typé. Pour référencer une valeur de colonne particulière, nous devons passer le nom de la colonne sous forme de chaîne (CategoryName, dans ce cas). La figure 13 montre le message affiché dans le `CategoryNameLabel` lors de l’affichage de la page. Bien entendu, le nom réel de la catégorie affiché est sélectionné de manière aléatoire par le `RandomCategoryDataSource` SqlDataSource à chaque visite de la page (y compris les publications).

[![le nom des catégories sélectionnées de manière aléatoire est affiché](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Figure 13**: le nom des catégories sélectionnées de manière aléatoire s’affiche ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))

> [!NOTE]
> Si la propriété `DataSourceMode` du contrôle SqlDataSource avait été définie sur `DataReader`, la valeur de retour de la méthode `Select()` aurait dû être castée en `IDataReader`. Pour lire la valeur de colonne `CategoryName` à partir de la première ligne, nous utilisons du code comme :

[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

Avec le SqlDataSource sélectionnant de façon aléatoire une catégorie, nous sommes prêts à ajouter le contrôle GridView qui répertorie les produits de la catégorie.

> [!NOTE]
> Au lieu d’utiliser un contrôle Web label pour afficher le nom de la catégorie, nous aurions pu ajouter un contrôle FormView ou DetailsView à la page, en le liant au SqlDataSource. Toutefois, l’utilisation de l’étiquette nous a permis d’explorer l’appel de l’instruction SqlDataSource s `Select()` et d’utiliser ses données résultantes dans le code.

## <a name="step-5-assigning-parameter-values-programmatically"></a>Étape 5 : assignation de valeurs de paramètre par programmation

Tous les exemples que nous avons vus jusqu’à présent dans ce didacticiel ont utilisé une valeur de paramètre codée en dur ou une issue d’une des sources de paramètres prédéfinies (une valeur QueryString, un contrôle Web sur la page, etc.). Toutefois, les paramètres s du contrôle SqlDataSource peuvent également être définis par programmation. Pour compléter notre exemple actuel, nous avons besoin d’un SqlDataSource qui retourne tous les produits appartenant à une catégorie spécifiée. Ce SqlDataSource aura un paramètre `CategoryID` dont la valeur doit être définie en fonction de la valeur de colonne `CategoryID` retournée par l' `RandomCategoryDataSource` SqlDataSource dans le gestionnaire d’événements `Page_Load`.

Commencez par ajouter un GridView à la page et liez-le à un nouveau SqlDataSource nommé `ProductsByCategoryDataSource`. Comme nous l’avons fait à l’étape 3, configurez le SqlDataSource afin qu’il appelle la procédure stockée `GetProductsByCategory`. Laissez la liste déroulante source de paramètres définie sur aucun, mais n’entrez pas de valeur par défaut, car nous allons définir cette valeur par défaut.

[![ne spécifiez pas de source de paramètre ou de valeur par défaut](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Figure 14**: ne pas spécifier de source de paramètre ou de valeur par défaut ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))

À l’issue de l’Assistant SqlDataSource, le balisage déclaratif obtenu doit ressembler à ce qui suit :

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

Nous pouvons affecter la `DefaultValue` du paramètre `CategoryID` par programmation dans le gestionnaire d’événements `Page_Load` :

[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

Avec cet ajout, la page contient un GridView qui affiche les produits associés à la catégorie sélectionnée de manière aléatoire.

[![ne spécifiez pas de source de paramètre ou de valeur par défaut](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Figure 15**: ne pas spécifier de source de paramètre ou de valeur par défaut ([cliquez pour afficher l’image en taille réelle](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))

## <a name="summary"></a>Récapitulatif

Le SqlDataSource permet aux développeurs de pages de définir des requêtes paramétrables dont les valeurs de paramètres peuvent être codées en dur, extraites de sources de paramètres prédéfinies ou assignées par programme. Dans ce didacticiel, nous avons vu comment créer une requête paramétrable à partir de l’Assistant Configuration de la source de données pour les requêtes SQL et les procédures stockées ad hoc. Nous avons également examiné l’utilisation de sources de paramètres codées en dur, d’un contrôle Web comme source de paramètre et de la spécification par programmation de la valeur de paramètre.

Comme avec ObjectDataSource, le SqlDataSource offre également des fonctionnalités permettant de modifier ses données sous-jacentes. Dans le didacticiel suivant, nous allons examiner comment définir des instructions `INSERT`, `UPDATE`et `DELETE` avec le SqlDataSource. Une fois ces instructions ajoutées, nous pouvons utiliser les fonctionnalités intégrées d’insertion, de modification et de suppression inhérentes aux contrôles GridView, DetailsView et FormView.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Scott Clyde, Randell Schmidt et Ken Pespisa. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](querying-data-with-the-sqldatasource-control-cs.md)
> [Suivant](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
