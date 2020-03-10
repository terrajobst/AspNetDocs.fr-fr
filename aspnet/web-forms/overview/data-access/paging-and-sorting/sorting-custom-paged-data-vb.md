---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: Tri de données paginées personnalisées (VB) | Microsoft Docs
author: rick-anderson
description: Dans le didacticiel précédent, nous avons appris à implémenter la pagination personnalisée lors de la présentation de données sur une page Web. Dans ce didacticiel, nous voyons comment étendre le précédent...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 934c7558d907611732ae6f04c553bc9e295c569b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78618831"
---
# <a name="sorting-custom-paged-data-vb"></a>Tri de données paginées personnalisées (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe) ou [Télécharger le PDF](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> Dans le didacticiel précédent, nous avons appris à implémenter la pagination personnalisée lors de la présentation de données sur une page Web. Dans ce didacticiel, nous voyons comment étendre l’exemple précédent pour inclure la prise en charge du tri de la pagination personnalisée.

## <a name="introduction"></a>Introduction

Par rapport à la pagination par défaut, la pagination personnalisée peut améliorer les performances de pagination de données de plusieurs ordres de grandeur, ce qui rend la pagination personnalisée le choix d’implémentation de la pagination de facto lors de la pagination de grandes quantités de données. L’implémentation de la pagination personnalisée est plus complexe que l’implémentation de la pagination par défaut, en particulier lors de l’ajout du tri à la combinaison. Dans ce didacticiel, nous allons étendre l’exemple du précédent pour inclure la prise en charge du tri *et* de la pagination personnalisée.

> [!NOTE]
> Dans la mesure où ce didacticiel s’appuie sur le précédent, avant de commencer, il est nécessaire de copier la syntaxe déclarative dans l’élément `<asp:Content>` à partir de la page Web du didacticiel précédent (`EfficientPaging.aspx`) et de la coller entre l’élément `<asp:Content>` de la page `SortParameter.aspx`. Pour plus d’informations sur la réplication des fonctionnalités d’une page ASP.NET vers une autre, reportez-vous à l’étape 1 du didacticiel sur l' [Ajout de contrôles de validation au didacticiel sur les interfaces de modification et d’insertion](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) .

## <a name="step-1-reexamining-the-custom-paging-technique"></a>Étape 1 : réexamen de la technique de pagination personnalisée

Pour que la pagination personnalisée fonctionne correctement, nous devons implémenter une technique capable de récupérer efficacement un sous-ensemble particulier d’enregistrements en fonction des paramètres index de ligne de début et lignes maximales. Il existe quelques techniques qui peuvent être utilisées pour atteindre cet objectif. Dans le didacticiel précédent, nous avons examiné la réalisation de cette fonction à l’aide de la nouvelle fonction de classement `ROW_NUMBER()` de Microsoft SQL Server 2005 s. En bref, la fonction de classement `ROW_NUMBER()` affecte un numéro de ligne à chaque ligne retournée par une requête classée selon un ordre de tri spécifié. Le sous-ensemble approprié d’enregistrements est ensuite obtenu en retournant une section particulière des résultats numérotés. La requête suivante illustre l’utilisation de cette technique pour retourner les produits numérotés de 11 à 20 lorsque les résultats sont classés par ordre alphabétique par la `ProductName`:

[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

Cette technique fonctionne bien pour la pagination à l’aide d’un ordre de tri spécifique (`ProductName` triées par ordre alphabétique, dans ce cas), mais la requête doit être modifiée pour afficher les résultats triés par une expression de tri différente. Dans l’idéal, la requête ci-dessus peut être réécrite pour utiliser un paramètre dans la clause `OVER`, comme suit :

[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

Malheureusement, les clauses `ORDER BY` paramétrées ne sont pas autorisées. Au lieu de cela, nous devons créer une procédure stockée qui accepte un paramètre d’entrée `@sortExpression`, mais utilise l’une des solutions de contournement suivantes :

- Écrire des requêtes codées en dur pour chacune des expressions de tri qui peuvent être utilisées ; Ensuite, utilisez `IF/ELSE` instructions T-SQL pour déterminer la requête à exécuter.
- Utilisez une instruction `CASE` pour fournir des expressions `ORDER BY` dynamiques basées sur le paramètre d’entrée `@sortExpressio` n ; Pour plus d’informations, consultez la section utilisation pour le tri dynamique des résultats de la requête dans [les instructions Power of SQL `CASE`](http://www.4guysfromrolla.com/webtech/102704-1.shtml) .
- Élaborez la requête appropriée sous forme de chaîne dans la procédure stockée, puis utilisez [la procédure stockée système `sp_executesql`](https://msdn.microsoft.com/library/ms188001.aspx) pour exécuter la requête dynamique.

Chacune de ces solutions de contournement présente certains inconvénients. La première option n’est pas aussi facile à gérer que les deux autres, car elle requiert la création d’une requête pour chaque expression de tri possible. Par conséquent, si vous décidez ultérieurement d’ajouter de nouveaux champs triables au contrôle GridView, vous devrez également revenir en arrière et mettre à jour la procédure stockée. La deuxième approche présente quelques subtilités qui introduisent des problèmes de performances lors du tri par colonnes de base de données non-chaîne et qui subissent également les mêmes problèmes de maintenance que le premier. Le troisième choix, qui utilise le SQL dynamique, introduit le risque d’une attaque par injection SQL si une personne malveillante est en mesure d’exécuter la procédure stockée en passant les valeurs des paramètres d’entrée de leur choix.

Bien qu’aucune de ces approches ne soit parfaite, je pense que la troisième option est la meilleure des trois. Grâce à son utilisation de SQL dynamique, il offre un niveau de flexibilité inégalé pour les deux autres. En outre, une attaque par injection de code SQL ne peut être exploitée que si une personne malveillante est en mesure d’exécuter la procédure stockée en passant les paramètres d’entrée de son choix. Étant donné que la couche DAL utilise des requêtes paramétrables, ADO.NET protège les paramètres envoyés à la base de données par le biais de l’architecture, ce qui signifie que la vulnérabilité d’attaque par injection SQL n’existe que si l’attaquant peut exécuter directement la procédure stockée.

Pour implémenter cette fonctionnalité, créez une procédure stockée dans la base de données Northwind nommée `GetProductsPagedAndSorted`. Cette procédure stockée doit accepter trois paramètres d’entrée : `@sortExpression`, un paramètre d’entrée de type `nvarchar(100`) qui spécifie la façon dont les résultats doivent être triés et sont injectés directement après le texte `ORDER BY` dans la clause `OVER` ; et `@startRowIndex` et `@maximumRows`, les deux mêmes paramètres d’entrée de type entier de la procédure stockée `GetProductsPaged` examinés dans le didacticiel précédent. Créez la procédure stockée `GetProductsPagedAndSorted` à l’aide du script suivant :

[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

La procédure stockée commence par s’assurer qu’une valeur a été spécifiée pour le paramètre `@sortExpression`. S’il est manquant, les résultats sont classés par `ProductID`. Ensuite, la requête SQL dynamique est construite. Notez que la requête SQL dynamique est légèrement différente de celle utilisée pour récupérer toutes les lignes de la table Products. Dans les exemples précédents, nous avons obtenu chaque produit et les noms des fournisseurs associés à l’aide d’une sous-requête. Cette décision a été reportée dans le didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) et a été effectuée à la place de l’utilisation de `JOIN` s, car le TableAdapter ne peut pas créer automatiquement les méthodes d’insertion, de mise à jour et de suppression associées pour ces requêtes. Toutefois, la procédure stockée `GetProductsPagedAndSorted` doit utiliser `JOIN` s pour que les résultats soient classés par catégorie ou par nom de fournisseur.

Cette requête dynamique est créée en concaténant les parties de requête statique et les paramètres `@sortExpression`, `@startRowIndex`et `@maximumRows`. Étant donné que `@startRowIndex` et `@maximumRows` sont des paramètres entiers, ils doivent être convertis en nvarchars afin d’être correctement concaténés. Une fois cette requête SQL dynamique construite, elle est exécutée via `sp_executesql`.

Prenez un moment pour tester cette procédure stockée avec des valeurs différentes pour les paramètres `@sortExpression`, `@startRowIndex`et `@maximumRows`. Dans le Explorateur de serveurs, cliquez avec le bouton droit sur le nom de la procédure stockée et choisissez Exécuter. La boîte de dialogue Exécuter la procédure stockée s’affiche, dans laquelle vous pouvez entrer les paramètres d’entrée (voir la figure 1). Pour trier les résultats en utilisant le nom de catégorie, utilisez CategoryName pour la valeur de paramètre `@sortExpression` ; pour effectuer un tri selon le nom de société du fournisseur, utilisez CompanyName. Après avoir fourni les valeurs des paramètres, cliquez sur OK. Les résultats s’affichent dans la fenêtre sortie. La figure 2 illustre les résultats obtenus lors du retour de produits classés 11 à 20 lorsque la `UnitPrice` dans l’ordre décroissant.

![Essayer différentes valeurs pour les trois paramètres d’entrée de la procédure stockée](sorting-custom-paged-data-vb/_static/image1.png)

**Figure 1**: essayer différentes valeurs pour les trois paramètres d’entrée de la procédure stockée

[![les résultats des procédures stockées sont affichés dans le Fenêtre Sortie](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**Figure 2**: les résultats des procédures stockées sont affichés dans le fenêtre sortie ([cliquez pour afficher l’image en taille réelle](sorting-custom-paged-data-vb/_static/image4.png))

> [!NOTE]
> Lors du classement des résultats par la colonne de `ORDER BY` spécifiée dans la clause `OVER`, SQL Server doit trier les résultats. Il s’agit d’une opération rapide s’il existe un index cluster sur la ou les colonnes dans lesquelles les résultats sont triés par ou s’il existe un index de couverture, mais qui peuvent être plus coûteux dans le cas contraire. Pour améliorer les performances des requêtes suffisamment volumineuses, envisagez d’ajouter un index non cluster pour la colonne par laquelle les résultats sont triés. Pour plus d’informations, reportez-vous à [fonctions de classement et performances dans SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) .

## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Étape 2 : augmentation des couches de logique métier et d’accès aux données

Avec la procédure stockée `GetProductsPagedAndSorted` créée, l’étape suivante consiste à fournir un moyen d’exécuter cette procédure stockée via notre architecture d’application. Cela implique l’ajout d’une méthode appropriée à la couche DAL et à la couche BLL. Commençons par ajouter une méthode à la couche DAL. Ouvrez le jeu de données typé `Northwind.xsd`, cliquez avec le bouton droit sur l' `ProductsTableAdapter`, puis choisissez l’option Ajouter une requête dans le menu contextuel. Comme nous l’avons fait dans le didacticiel précédent, nous voulons configurer cette nouvelle méthode DAL pour utiliser une procédure stockée existante-`GetProductsPagedAndSorted`, dans ce cas. Commencez par indiquer que vous souhaitez que la nouvelle méthode TableAdapter utilise une procédure stockée existante.

![Choisir d’utiliser une procédure stockée existante](sorting-custom-paged-data-vb/_static/image5.png)

**Figure 3**: choisir d’utiliser une procédure stockée existante

Pour spécifier la procédure stockée à utiliser, sélectionnez la procédure stockée `GetProductsPagedAndSorted` dans la liste déroulante de l’écran suivant.

![Utiliser la procédure stockée GetProductsPagedAndSorted](sorting-custom-paged-data-vb/_static/image6.png)

**Figure 4**: utiliser la procédure stockée GetProductsPagedAndSorted

Cette procédure stockée retourne un jeu d’enregistrements comme résultat, dans l’écran suivant, indiquez qu’elle retourne des données tabulaires.

![Indiquer que la procédure stockée retourne des données tabulaires](sorting-custom-paged-data-vb/_static/image7.png)

**Figure 5**: indiquer que la procédure stockée retourne des données tabulaires

Enfin, créez des méthodes DAL qui utilisent à la fois le remplissage d’un DataTable et retournent des modèles DataTable, en nommant les méthodes `FillPagedAndSorted` et `GetProductsPagedAndSorted`, respectivement.

![Choisir les noms des méthodes](sorting-custom-paged-data-vb/_static/image8.png)

**Figure 6**: choisir les noms des méthodes

Maintenant que nous avons étendu la couche DAL, nous sommes prêts à activer la couche BLL. Ouvrez le fichier de classe `ProductsBLL` et ajoutez une nouvelle méthode, `GetProductsPagedAndSorted`. Cette méthode doit accepter trois paramètres d’entrée `sortExpression`, `startRowIndex`et `maximumRows` et doit simplement appeler la méthode DAL s `GetProductsPagedAndSorted`, comme suit :

[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Étape 3 : configuration de l’ObjectDataSource pour passer le paramètre SortExpression

En ayant augmenté la couche DAL et la couche BLL pour inclure des méthodes qui utilisent la procédure stockée `GetProductsPagedAndSorted`, il ne reste plus qu’à configurer ObjectDataSource dans la page `SortParameter.aspx` pour utiliser la nouvelle méthode BLL et à passer le paramètre `SortExpression` en fonction de la colonne sur laquelle l’utilisateur a demandé le tri des résultats.

Commencez par modifier le `SelectMethod` ObjectDataSource des `GetProductsPaged` en `GetProductsPagedAndSorted`. Pour ce faire, vous pouvez utiliser l’Assistant Configuration de la source de données, à partir de la Fenêtre Propriétés ou directement à l’aide de la syntaxe déclarative. Ensuite, nous devons fournir une valeur pour la propriété ObjectDataSource s [`SortParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Si cette propriété est définie, ObjectDataSource tente de transmettre la propriété de `SortExpression` GridView s à l' `SelectMethod`. En particulier, ObjectDataSource recherche un paramètre d’entrée dont le nom est égal à la valeur de la propriété `SortParameterName`. Étant donné que la méthode BLL s `GetProductsPagedAndSorted` a le paramètre d’entrée d’expression de tri nommé `sortExpression`, définissez la propriété ObjectDataSource s `SortExpression` sur sortExpression.

Après avoir apporté ces deux modifications, la syntaxe déclarative de ObjectDataSource s doit ressembler à ce qui suit :

[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> Comme dans le didacticiel précédent, assurez-vous que ObjectDataSource n’inclut *pas* les paramètres d’entrée SortExpression, StartRowIndex ou MaximumRows dans sa collection SelectParameters.

Pour activer le tri dans le GridView, activez simplement la case à cocher Activer le tri dans la balise active GridView s, qui définit la propriété GridView s `AllowSorting` sur `true` et provoquant le rendu du texte d’en-tête pour chaque colonne en tant que LinkButton. Lorsque l’utilisateur final clique sur l’un des LinkButtons d’en-tête, une publication (postback) se déproduira et les étapes suivantes s’écouleront :

1. Le contrôle GridView met à jour sa [propriété`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) avec la valeur de la `SortExpression` du champ dont l’utilisateur a cliqué sur le lien
2. ObjectDataSource appelle la méthode BLL s `GetProductsPagedAndSorted`, en transmettant la propriété GridView s `SortExpression` en tant que valeur de la méthode s `sortExpression` paramètre d’entrée (ainsi que les valeurs de paramètre d’entrée `startRowIndex` et `maximumRows` appropriées)
3. La couche BLL appelle la méthode `GetProductsPagedAndSorted` DAL
4. La couche DAL exécute la procédure stockée `GetProductsPagedAndSorted`, en transmettant le paramètre `@sortExpression` (avec les valeurs de paramètres d’entrée `@startRowIndex` et `@maximumRows`).
5. La procédure stockée retourne le sous-ensemble approprié de données à la couche BLL, qui la renvoie à l’ObjectDataSource ; ces données sont ensuite liées au GridView, rendues en HTML et envoyées à l’utilisateur final.

La figure 7 illustre la première page de résultats lorsqu’elle est triée par `UnitPrice` dans l’ordre croissant.

[![les résultats sont triés par prix unitaire](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**Figure 7**: les résultats sont triés par prix unitaire ([cliquez pour afficher l’image en taille réelle](sorting-custom-paged-data-vb/_static/image11.png))

Tandis que l’implémentation actuelle peut Trier correctement les résultats par nom de produit, nom de catégorie, quantité par unité et prix unitaire, toute tentative de classement des résultats par le nom du fournisseur entraîne une exception d’exécution (voir figure 8).

![La tentative de tri des résultats par le fournisseur génère l’exception Runtime suivante](sorting-custom-paged-data-vb/_static/image12.png)

**Figure 8**: la tentative de tri des résultats par le fournisseur génère l’exception Runtime suivante

Cette exception se produit parce que la `SortExpression` du GridView s `SupplierName` BoundField a la valeur `SupplierName`. Toutefois, le nom du fournisseur dans la table `Suppliers` est appelé `CompanyName` nous avons fait l’alias de ce nom de colonne en tant que `SupplierName`. Toutefois, la clause `OVER` utilisée par la fonction `ROW_NUMBER()` ne peut pas utiliser l’alias et doit utiliser le nom réel de la colonne. Par conséquent, remplacez le `SupplierName` BoundField s `SortExpression` de NomFournisseur par CompanyName (voir figure 9). Comme le montre la figure 10, une fois cette modification apportée, les résultats peuvent être triés par le fournisseur.

![Remplacez la valeur de NomFournisseur BoundField s SortExpression par CompanyName](sorting-custom-paged-data-vb/_static/image13.png)

**Figure 9**: modifier le NomFournisseur BoundField s SortExpression en CompanyName

[![les résultats peuvent désormais être triés par fournisseur](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**Figure 10**: les résultats peuvent maintenant être triés par fournisseur ([cliquez pour afficher l’image en taille réelle](sorting-custom-paged-data-vb/_static/image16.png))

## <a name="summary"></a>Récapitulatif

L’implémentation personnalisée de la pagination que nous avons examinée dans le didacticiel précédent nécessitait que l’ordre de tri des résultats soit spécifié au moment de la conception. En bref, cela signifiait que l’implémentation de la pagination personnalisée que nous avons implémentée ne pouvait pas, en même temps, fournir des fonctionnalités de tri. Dans ce didacticiel, nous ont surmonté cette limitation en étendant la procédure stockée à partir de la première pour inclure un paramètre d’entrée `@sortExpression` en vue de trier les résultats.

Après avoir créé cette procédure stockée et créé de nouvelles méthodes dans la couche DAL et la couche BLL, nous avons pu implémenter un GridView qui offrait à la fois le tri et la pagination personnalisée en configurant ObjectDataSource pour transmettre la propriété GridView s Current `SortExpression` à la couche BLL `SelectMethod`.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Carlos Santos. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](efficiently-paging-through-large-amounts-of-data-vb.md)
> [Suivant](creating-a-customized-sorting-user-interface-vb.md)
