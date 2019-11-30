---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Création d’une couche de logiqueC#métier () | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir comment centraliser vos règles d’entreprise dans une couche BLL (Business Logic Layer) qui sert d’intermédiaire pour l’échange de données entre t...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: df96f3e7422a0537bf1b003a33fe8d71a671ac33
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586207"
---
# <a name="creating-a-business-logic-layer-c"></a>Création d’une couche de logique métier (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) ou [Télécharger le PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> Dans ce didacticiel, nous allons voir comment centraliser vos règles d’entreprise dans une couche BLL (Business Logic Layer) qui sert d’intermédiaire pour l’échange de données entre la couche de présentation et la couche DAL.

## <a name="introduction"></a>Introduction

La couche d’accès aux données (DAL) créée dans le [premier didacticiel](creating-a-data-access-layer-cs.md) sépare correctement la logique d’accès aux données de la logique de présentation. Toutefois, si la couche DAL sépare correctement les détails d’accès aux données de la couche de présentation, elle ne met pas en vigueur les règles d’entreprise qui peuvent s’appliquer. Par exemple, pour notre application, nous pouvons interdire la modification des champs `CategoryID` ou `SupplierID` de la table `Products` lorsque le champ `Discontinued` est défini sur 1, ou appliquer des règles d’ancienneté, en interdisant les situations dans lesquelles un employé est géré par une personne qui a été embauchée après. Un autre scénario courant est l’autorisation. peut-être seuls les utilisateurs d’un rôle particulier peuvent supprimer des produits ou modifier la valeur de `UnitPrice`.

Dans ce didacticiel, nous allons voir comment centraliser ces règles d’entreprise dans une couche BLL (Business Logic Layer) qui sert d’intermédiaire pour l’échange de données entre la couche de présentation et la couche DAL. Dans une application réelle, la couche BLL doit être implémentée en tant que projet de bibliothèque de classes distinct ; Toutefois, pour ces didacticiels, nous allons implémenter la couche BLL comme une série de classes dans notre dossier `App_Code` afin de simplifier la structure du projet. La figure 1 illustre les relations architecturales entre la couche de présentation, la couche BLL et la couche DAL.

![La couche BLL sépare la couche de présentation de la couche d’accès aux données et impose des règles d’entreprise](creating-a-business-logic-layer-cs/_static/image1.png)

**Figure 1**: la couche BLL sépare la couche de présentation de la couche d’accès aux données et impose des règles d’entreprise

## <a name="step-1-creating-the-bll-classes"></a>Étape 1 : création des classes BLL

Notre couche BLL sera composée de quatre classes, une pour chaque TableAdapter de la couche DAL. chacune de ces classes BLL aura des méthodes pour récupérer, insérer, mettre à jour et supprimer du TableAdapter respectif dans la couche DAL, en appliquant les règles d’entreprise appropriées.

Pour séparer plus clairement les classes DAL et BLL, nous allons créer deux sous-dossiers dans le dossier `App_Code`, `DAL` et `BLL`. Il vous suffit de cliquer avec le bouton droit sur le dossier `App_Code` dans le Explorateur de solutions et de choisir nouveau dossier. Après avoir créé ces deux dossiers, déplacez le DataSet typé créé dans le premier didacticiel dans le sous-dossier `DAL`.

Ensuite, créez les quatre fichiers de classe BLL dans le sous-dossier `BLL`. Pour ce faire, cliquez avec le bouton droit sur le sous-dossier `BLL`, choisissez Ajouter un nouvel élément, puis choisissez le modèle de classe. Nommez les quatre classes `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`et `EmployeesBLL`.

![Ajouter quatre nouvelles classes au dossier App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Figure 2**: ajouter quatre nouvelles classes au dossier `App_Code`

Ensuite, nous allons ajouter des méthodes à chacune des classes pour inclure simplement dans un wrapper les méthodes définies pour les TableAdapters dans le premier didacticiel. Pour le moment, ces méthodes appellent simplement directement dans la couche DAL. Nous reviendrons plus tard pour ajouter la logique métier nécessaire.

> [!NOTE]
> Si vous utilisez Visual Studio Standard Edition ou une version ultérieure (autrement dit, si vous *n’utilisez pas* Visual Web Developer), vous pouvez éventuellement concevoir vos classes visuellement à l’aide de l' [Concepteur de classes](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Pour plus d’informations sur cette nouvelle fonctionnalité dans Visual Studio, consultez le [Blog concepteur de classes](https://blogs.msdn.com/classdesigner/default.aspx) .

Pour la classe `ProductsBLL`, nous devons ajouter un total de sept méthodes :

- `GetProducts()` retourne tous les produits
- `GetProductByProductID(productID)` retourne le produit avec l’ID de produit spécifié
- `GetProductsByCategoryID(categoryID)` retourne tous les produits de la catégorie spécifiée
- `GetProductsBySupplier(supplierID)` retourne tous les produits du fournisseur spécifié
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` insère un nouveau produit dans la base de données à l’aide des valeurs transmises ; retourne la valeur `ProductID` de l’enregistrement qui vient d’être inséré.
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` met à jour un produit existant dans la base de données à l’aide des valeurs transmises ; retourne `true` si une ligne a été mise à jour avec précision, `false` dans le cas contraire
- `DeleteProduct(productID)` supprime le produit spécifié de la base de données.

ProductsBLL.cs

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

Les méthodes qui renvoient simplement des données `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`et `GetProductBySuppliersID` sont relativement simples, car elles appellent simplement la couche DAL. Dans certains scénarios, il peut y avoir des règles métier qui doivent être implémentées à ce niveau (par exemple, des règles d’autorisation basées sur l’utilisateur actuellement connecté ou le rôle auquel appartient l’utilisateur), mais nous allons simplement conserver ces méthodes. Pour ces méthodes, le BLL sert simplement de proxy par le biais duquel la couche de présentation accède aux données sous-jacentes de la couche d’accès aux données.

Les méthodes `AddProduct` et `UpdateProduct` prennent toutes les deux en tant que paramètres les valeurs des différents champs de produit et ajoutent un nouveau produit ou en mettent un à jour, respectivement. Étant donné que la plupart des colonnes de la table `Product` peuvent accepter des valeurs `NULL` (`CategoryID`, `SupplierID`et `UnitPrice`, pour en nommer quelques-unes), ces paramètres d’entrée pour `AddProduct` et `UpdateProduct` qui mappent à ces colonnes utilisent des [types Nullable](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Les types Nullable sont nouveaux dans .NET 2,0 et fournissent une technique pour indiquer si un type valeur doit, à la place, être `null`. Dans C# , vous pouvez marquer un type valeur comme un type Nullable en ajoutant `?` après le type (comme `int? x;`). Pour plus d’informations, reportez-vous à la section [types Nullable](https://msdn.microsoft.com/library/1t3y8s4s.aspx) dans le [ C# Guide de programmation](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) .

Les trois méthodes retournent une valeur booléenne indiquant si une ligne a été insérée, mise à jour ou supprimée depuis que l’opération risque de ne pas aboutir à une ligne affectée. Par exemple, si le développeur de pages appelle `DeleteProduct` passant une `ProductID` pour un produit inexistant, l’instruction `DELETE` émise pour la base de données n’aura aucun effet et la méthode `DeleteProduct` renverra donc `false`.

Notez que lors de l’ajout d’un nouveau produit ou de la mise à jour d’un produit existant, nous prenons les valeurs de champ du produit nouveau ou modifié comme une liste de scalaires au lieu d’accepter une instance de `ProductsRow`. Cette approche a été choisie, car la classe `ProductsRow` dérive de la classe ADO.NET `DataRow`, qui n’a pas de constructeur sans paramètre par défaut. Pour créer une instance de `ProductsRow`, nous devons d’abord créer une instance de `ProductsDataTable`, puis appeler sa méthode `NewProductRow()` (que nous faisons dans `AddProduct`). Cette lacune est à l’arrière de la tête lors de l’insertion et de la mise à jour des produits à l’aide de ObjectDataSource. En bref, ObjectDataSource essaiera de créer une instance des paramètres d’entrée. Si la méthode BLL attend une `ProductsRow` instance, ObjectDataSource essaiera d’en créer une, mais échouera en raison de l’absence d’un constructeur sans paramètre par défaut. Pour plus d’informations sur ce problème, consultez les deux publications de Forums ASP.NET suivantes : [mise à jour de ObjectDataSources avec des DataSets fortement typés](https://forums.asp.net/1098630/ShowPost.aspx)et [problème avec ObjectDataSource et DataSet fortement typé](https://forums.asp.net/1048212/ShowPost.aspx).

Ensuite, dans `AddProduct` et `UpdateProduct`, le code crée une instance `ProductsRow` et la remplit avec les valeurs qui viennent d’être transmises. Lorsque vous assignez des valeurs à DataColumns d’un DataRow, plusieurs contrôles de validation au niveau du champ peuvent se produire. Par conséquent, le fait de replacer manuellement les valeurs transmises dans un DataRow permet de garantir la validité des données transmises à la méthode BLL. Malheureusement, les classes DataRow fortement typées générées par Visual Studio n’utilisent pas de types Nullable. Au lieu de cela, pour indiquer qu’un DataColumn particulier dans un DataRow doit correspondre à une valeur de base de données `NULL` nous devons utiliser la méthode `SetColumnNameNull()`.

Dans `UpdateProduct` nous chargeons tout d’abord le produit pour la mise à jour à l’aide de `GetProductByProductID(productID)`. Bien que cela puisse sembler un déplacement inutile vers la base de données, ce voyage supplémentaire s’avérera utile dans les prochains didacticiels qui explorent l’accès concurrentiel optimiste. L’accès concurrentiel optimiste est une technique qui permet de s’assurer que deux utilisateurs qui travaillent simultanément sur les mêmes données ne remplacent pas accidentellement les modifications d’une autre. La saisie de la totalité de l’enregistrement facilite également la création de méthodes de mise à jour dans la couche BLL qui modifie uniquement un sous-ensemble des colonnes du DataRow. Lorsque nous explorons la classe `SuppliersBLL`, nous verrons un tel exemple.

Enfin, Notez que l' [attribut DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) est appliqué à la classe `ProductsBLL` (la syntaxe `[System.ComponentModel.DataObject]` juste avant l’instruction class près du début du fichier) et que les méthodes ont des [attributs DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). L’attribut `DataObject` marque la classe comme étant un objet approprié pour la liaison à un [contrôle ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), tandis que le `DataObjectMethodAttribute` indique l’objectif de la méthode. Comme nous le verrons dans les prochains didacticiels, ObjectDataSource de ASP.NET 2.0 facilite l’accès aux données de façon déclarative. Pour filtrer la liste des classes possibles à lier dans l’Assistant d’ObjectDataSource, par défaut, seules les classes marquées comme `DataObjects` sont affichées dans la liste déroulante de l’Assistant. La classe `ProductsBLL` fonctionnera tout aussi bien sans ces attributs, mais son ajout facilite son utilisation dans l’Assistant d’ObjectDataSource.

## <a name="adding-the-other-classes"></a>Ajout des autres classes

Une fois la classe `ProductsBLL` terminée, nous devons toujours ajouter les classes pour travailler avec des catégories, des fournisseurs et des employés. Prenez un moment pour créer les classes et méthodes suivantes à l’aide des concepts de l’exemple ci-dessus :

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

L’une des méthodes à noter est la méthode `UpdateSupplierAddress` de la classe `SuppliersBLL`. Cette méthode fournit une interface pour mettre à jour uniquement les informations d’adresse du fournisseur. En interne, cette méthode lit l’objet `SupplierDataRow` pour le `supplierID` spécifié (à l’aide de `GetSupplierBySupplierID`), définit ses propriétés relatives à l’adresse, puis appelle la méthode `Update` de `SupplierDataTable`. La méthode `UpdateSupplierAddress` suit :

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Reportez-vous au téléchargement de cet article pour obtenir une implémentation complète des classes BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Étape 2 : accès aux DataSets typés via les classes BLL

Dans le premier didacticiel, nous avons vu des exemples de travail directement avec le DataSet typé par programme, mais avec l’ajout de nos classes BLL, la couche présentation devrait fonctionner sur la couche BLL à la place. Dans l’exemple de `AllProducts.aspx` du premier didacticiel, le `ProductsTableAdapter` a été utilisé pour lier la liste de produits à un GridView, comme illustré dans le code suivant :

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Pour utiliser les nouvelles classes BLL, vous devez modifier la première ligne de code en remplaçant simplement l’objet `ProductsTableAdapter` par un objet `ProductBLL` :

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

Vous pouvez également accéder de façon déclarative aux classes BLL (comme le peut le DataSet typé) à l’aide de ObjectDataSource. Nous discuterons de l’ObjectDataSource plus en détail dans les didacticiels suivants.

[![la liste des produits s’affiche dans un GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Figure 3**: la liste des produits s’affiche dans un GridView ([cliquez pour afficher l’image en taille réelle](creating-a-business-logic-layer-cs/_static/image5.png))

## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Étape 3 : ajout de la validation au niveau du champ aux classes DataRow

La validation au niveau des champs s’applique aux valeurs des propriétés des objets d’entreprise lors de l’insertion ou de la mise à jour. Certaines règles de validation au niveau du champ pour les produits sont les suivantes :

- La longueur du champ de `ProductName` doit être de 40 caractères ou moins
- La longueur du champ de `QuantityPerUnit` doit être inférieure ou égale à 20 caractères
- Les champs `ProductID`, `ProductName`et `Discontinued` sont requis, mais tous les autres champs sont facultatifs
- Les champs `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`et `ReorderLevel` doivent être supérieurs ou égaux à zéro

Ces règles peuvent et doivent être exprimées au niveau de la base de données. La limite de caractères sur les champs `ProductName` et `QuantityPerUnit` est capturée par les types de données de ces colonnes dans la table `Products` (`nvarchar(40)` et `nvarchar(20)`, respectivement). Indique si les champs sont obligatoires et facultatifs sont exprimés par si la colonne de la table de base de données autorise `NULL` s. Il existe quatre [contraintes CHECK](https://msdn.microsoft.com/library/ms188258.aspx) qui garantissent que seules les valeurs supérieures ou égales à zéro peuvent être contenues dans les colonnes `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`ou `ReorderLevel`.

En plus d’appliquer ces règles à la base de données, elles doivent également être appliquées au niveau du jeu de données. En fait, la longueur de champ et si une valeur est obligatoire ou facultative sont déjà capturées pour chaque ensemble de DataColumns de chaque DataTable. Pour voir la validation existante au niveau du champ fournie automatiquement, accédez au concepteur de DataSet, sélectionnez un champ dans l’un des DataTables, puis accédez au Fenêtre Propriétés. Comme le montre la figure 4, le `QuantityPerUnit` DataColumn dans le `ProductsDataTable` a une longueur maximale de 20 caractères et autorise les valeurs de `NULL`. Si nous tentons de définir la propriété de `QuantityPerUnit` de l' `ProductsDataRow`sur une valeur de chaîne de plus de 20 caractères, une `ArgumentException` sera levée.

[![le DataColumn fournit une validation au niveau du champ de base](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Figure 4**: le DataColumn fournit une validation au niveau des champs de base ([cliquez pour afficher l’image en taille réelle](creating-a-business-logic-layer-cs/_static/image8.png))

Malheureusement, nous ne pouvons pas spécifier de contrôles de limites, tels que la valeur de `UnitPrice` doit être supérieure ou égale à zéro, par le Fenêtre Propriétés. Pour fournir ce type de validation au niveau du champ, nous devons créer un gestionnaire d’événements pour l’événement [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) du DataTable. Comme mentionné dans le [didacticiel précédent](creating-a-data-access-layer-cs.md), le DataSet, les DataTables et les objets DataRow créés par le DataSet typé peuvent être étendus à l’aide de classes partielles. À l’aide de cette technique, nous pouvons créer un gestionnaire d’événements `ColumnChanging` pour la classe `ProductsDataTable`. Commencez par créer une classe dans le dossier `App_Code` nommé `ProductsDataTable.ColumnChanging.cs`.

[![ajouter une nouvelle classe au dossier App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Figure 5**: ajouter une nouvelle classe au dossier `App_Code` ([cliquez pour afficher l’image en taille réelle](creating-a-business-logic-layer-cs/_static/image11.png))

Ensuite, créez un gestionnaire d’événements pour l’événement `ColumnChanging` qui permet de s’assurer que les valeurs de colonne `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`et `ReorderLevel` (si ce n’est pas `NULL`) sont supérieures ou égales à zéro. Si une colonne de ce type est hors limites, levez une `ArgumentException`.

ProductsDataTable.ColumnChanging.cs

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Étape 4 : ajout de règles métier personnalisées aux classes de la couche BLL

Outre la validation au niveau des champs, il peut y avoir des règles métier personnalisées de haut niveau qui impliquent des entités ou des concepts non exprimables au niveau de la colonne unique, par exemple :

- Si un produit n’est plus disponible, son `UnitPrice` ne peut pas être mis à jour
- Le pays de résidence d’un employé doit être le même que le pays de résidence de son responsable.
- Un produit ne peut pas être supprimé s’il s’agit du seul produit fourni par le fournisseur

Les classes BLL doivent contenir des vérifications pour garantir le respect des règles d’entreprise de l’application. Ces contrôles peuvent être ajoutés directement aux méthodes auxquelles ils s’appliquent.

Imaginez que nos règles d’entreprise stipulent qu’un produit n’a pas pu être marqué comme étant abandonné s’il s’agissait du seul produit d’un fournisseur donné. Autrement dit, si le produit *x* était le seul produit que nous avons acheté auprès du fournisseur *Y*, nous n’avons pas pu marquer *X* comme abandonné. Toutefois, si *le*fournisseur *Y* a fourni trois produits, A, *B*et *C*, nous pourrions marquer tout et partie de ceux-ci comme abandonnés. Règle d’entreprise étrange, mais les règles d’entreprise et le bon sens ne sont pas toujours alignés !

Pour appliquer cette règle d’entreprise dans la méthode `UpdateProducts`, nous commençons par vérifier si `Discontinued` a été défini sur `true` et, si c’est le cas, nous appelons `GetProductsBySupplierID` pour déterminer le nombre de produits que nous avons achetés auprès du fournisseur de ce produit. Si un seul produit est acheté auprès de ce fournisseur, nous levez une `ApplicationException`.

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Réponse aux erreurs de validation dans la couche présentation

Lors de l’appel de la couche BLL à partir de la couche présentation, nous pouvons décider s’il faut tenter de gérer toutes les exceptions susceptibles d’être déclenchées ou les laisser se propager à ASP.NET (ce qui déclenche l’événement `Error` du `HttpApplication`). Pour gérer une exception lors de l’utilisation de la couche BLL par programmation, nous pouvons utiliser un [bloc try... bloc catch](https://msdn.microsoft.com/library/0yd65esw.aspx) , comme le montre l’exemple suivant :

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Comme nous le verrons dans les prochains didacticiels, la gestion des exceptions qui se propagent à partir de la couche BLL lors de l’utilisation d’un contrôle Web de données pour l’insertion, la mise à jour ou la suppression de données peut être gérée directement dans un gestionnaire d’événements, par opposition à l’encapsulation du code dans des blocs `try...catch`.

## <a name="summary"></a>Récapitulatif

Une application bien conçue est créée dans des couches distinctes, chacune encapsulant un rôle particulier. Dans le premier didacticiel de cette série d’articles, nous avons créé une couche d’accès aux données à l’aide de DataSets typés. dans ce didacticiel, nous avons créé une couche de logique métier sous la forme d’une série de classes dans le dossier de `App_Code` de l’application qui fait appel à notre couche DAL. La couche BLL implémente la logique au niveau du champ et au niveau de l’entreprise pour notre application. En plus de créer une couche BLL distincte, comme nous l’avons fait dans ce didacticiel, une autre option consiste à étendre les méthodes des TableAdapters par le biais de l’utilisation de classes partielles. Toutefois, l’utilisation de cette technique ne nous permet pas de remplacer les méthodes existantes ni de séparer notre couche DAL et notre couche BLL comme une approche que nous avons adoptée dans cet article.

Une fois les couches DAL et BLL terminées, nous sommes prêts à démarrer sur notre couche de présentation. Dans le [didacticiel suivant](master-pages-and-site-navigation-cs.md) , nous allons suivre brièvement les rubriques relatives à l’accès aux données et définir une mise en page cohérente à utiliser dans tous les didacticiels.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Liz Shulok, Denis Patterson, Carlos Santos et Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-a-data-access-layer-cs.md)
> [Suivant](master-pages-and-site-navigation-cs.md)
